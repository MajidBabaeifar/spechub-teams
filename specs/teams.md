# Teams

**Status:** Draft · **Prefix:** TEAM

## Purpose

Users group into teams with roles; apps attach their own resources and permissions to a team.

## Requirements

| ID | Requirement | Verified by |
|---|---|---|
| TEAM-1 | Team: `id`, `name` (1–100), `slug` (`[a-z0-9-]`, 2–64, unique across teams and usernames), `logo?`, `bio?`, `private` (default false: member list visible on the public page). | slug equal to a username → refused |
| TEAM-2 | Membership `(user, team)` unique: `role`: `viewer` < `member` < `admin` < `owner`, `accepted` (bool). Unaccepted memberships grant nothing. | pending member reads team → 403 |
| TEAM-3 | Any verified user can create a team and becomes its `owner`. | create → creator is owner |
| TEAM-4 | Admins invite by email with a role ≤ their own; invite = single-use token valid 7 days; re-inviting replaces the token. | admin invites owner → refused |
| TEAM-5 | Accepting: a signed-in user with the invited email accepts; a new person signs up via the link (email counts as verified) and joins on signup. Wrong email → refused. | link used by other email → refused |
| TEAM-6 | Permissions: read team and members: viewer+; update team, invite, remove members (below own role): admin+; change roles, transfer ownership, delete team: owner. | member removes member → 403 |
| TEAM-7 | A team always has ≥ 1 owner: the last owner can't leave, be removed or be demoted. | sole owner leaves → refused |
| TEAM-8 | Members can leave; removal or leaving ends access immediately. | removed member's session → 403 on team |
| TEAM-9 | Deleting a team deletes its resources; deleting a user removes their memberships (refused while they're a sole owner). | sole owner deletes account → refused |
| TEAM-10 | Apps extend TEAM-6 with their own resource rows (resource, action, minimum role); the default for unlisted actions is admin+; a row's minimum role may be `viewer` only for reading or for actions the requiring spec names as viewer-safe (e.g. commenting). | manual: requiring spec lists its rows |
| TEAM-11 | `viewer` never creates, updates or deletes team resources unless a TEAM-10 row explicitly allows that action for viewers. | viewer edits a record → 403 |
| TEAM-12 | Team setting `require_2fa` (default false, owner sets it): members without 2FA (`@spechub/auth:AUTH-9`) get 403 `2fa_required` on every team resource until they enable it. | require_2fa on, member without 2FA → 403 |
| TEAM-13 | Team `time_zone` (IANA), default the creating owner's; requiring specs use it as the default for team resources. | new team → owner's zone |
| TEAM-14 | Moving a resource to another team needs admin+ in both teams; members of the old team lose access at once. | admin of source only → 403 |
| TEAM-15 | When a membership ends, every requiring spec's on-member-removed rules (e.g. unassign) run before access ends, in the same operation. | remove member → their assignments cleared |
| TEAM-16 | Reserved slugs (`app`, `api`, `admin`, `login`, `signup`, `settings`, `static`, `www`, plus any a requiring spec adds) are refused; a deleted team's slug is not reusable for 30 days. | slug `api` → refused |

## Out of scope

- Organizations with sub-teams and domain auto-join, custom roles, SCIM.

## Decisions

- Mostly designed here: the source (Cal.diy, enterprise code removed) kept only roles, membership and invite redemption.
- Slugs share one namespace with usernames so `/{slug}` URLs never clash.
- 0.2 adds `viewer` (read-only, the source's view-only and guest roles) but keeps fixed roles; per-resource access belongs to the requiring spec.
