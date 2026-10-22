---
specfork: 1
name: "@specfork/teams"
summary: Teams with member/admin/owner roles, email invites, and permission rows other specs extend.
license: CC-BY-4.0
tags: [base, accounts]
requires:
  "@specfork/auth": "^0.3.0"
derived_from:
  repo: https://github.com/calcom/cal.com
  commit: 0f19cc46ef6b6426ee398c670575b3c96be7af9e
  url: https://cal.com
  license: MIT
---

# Teams

Users group into teams with roles. Roles and membership follow Cal.com; invites, ownership and removal are designed here.
Not affiliated with or endorsed by Cal.com, Inc. The name is used only to credit the source.

| Area | Prefix | Covers |
|---|---|---|
| [teams.md](specs/teams.md) | TEAM | Teams, memberships, invites, permissions, ownership |
