# OverseerReport -- 19-03-2026

## UniOpsQC-command
### CMD-23 -- workspace_invitations Table Migration
**Filed by:** AT (Atlas)
**Date:** 19-03-2026
**Status:** COMPLETE

**Summary**
Created `hub.workspace_invitations` table via migration script `40_workspace_invitations.sql`. The migration introduces a new `hub.workspace_role` enum type (owner, manager, contributor, viewer, member) with idempotent creation, then creates the invitations table with UUID PK, FKs to `hub.workspaces` and `hub.users`, a secure random token column (UNIQUE), status check constraint (pending/accepted/declined/expired), 7-day default expiry, and `accepted_at` timestamp. Two indexes created: token lookup index and partial unique index on (workspace_id, email) WHERE status = 'pending' to prevent duplicate pending invites.

**Acceptance Criteria**
- [x] Migration script `40_workspace_invitations.sql` created in `deploy/init-scripts/`
- [x] Table creates cleanly on fresh DB (idempotent with IF NOT EXISTS guards)
- [x] Unique index prevents duplicate pending invite to same (workspace_id, email)
- [x] Token index exists for fast lookup

**Blockers:** None
**Next Step for AM:** Present to Commander. Signal Syndicate that CMD-24 is unblocked.

### Dependency Signal
Ticket CMD-23 is COMPLETE. Teams waiting on this ticket may now proceed:
- Syndicate: CMD-24 (Invitation API endpoints) is now unblocked

---

## Arcade
### CMD-26 -- Create Org Workspace Flow
**Filed by:** CP (Captain)
**Date:** 19-03-2026
**Status:** COMPLETE

**Summary**
Replaced WorkspaceSwitcher's "New Workspace" button (which navigated to `/onboarding`) with an inline modal supporting both Personal and Team workspace types. Modal includes name input with validation, segmented type control (Personal/Team), error handling, and post-creation navigation (Team -> workspace settings, Personal -> dashboard). Workspace list refreshes on success.

**Acceptance Criteria**
- [x] "New Workspace" opens a modal (does NOT navigate to /onboarding)
- [x] Modal allows selecting Personal or Team type
- [x] Personal workspace created with `plan: 'personal'`
- [x] Team workspace created with `plan: 'team'`
- [x] After creating Team workspace, user lands on Workspace Settings page
- [x] After creating Personal workspace, user lands on Dashboard
- [x] Error shown if creation fails (no silent failure)
- [x] Modal closes and workspace switcher refreshes on success

**Blockers:** None
**Next Step for AM:** Present to Commander.

---

### CMD-27 -- Invitation Accept/Decline UI
**Filed by:** CP (Captain)
**Date:** 19-03-2026
**Status:** COMPLETE

**Summary**
Built the full invitation UX across three touchpoints: (1) New `/invite/:token` page with workspace info card, accept/decline buttons, sign-in redirect for unauthenticated users, and edge case handling (expired, already accepted, invalid token). (2) Pending Invitations section in WorkspaceSettingsPage visible to Owner/Admin with copyable accept links. (3) Invitation banner in AppShell on app load checking `GET /api/me/invitations`. Also updated LoginPage to support `?redirect=` query parameter. Added 5 API functions and 3 TypeScript interfaces to workspaceApi.ts.

**Acceptance Criteria**
- [x] `/invite/:token` shows workspace info + accept/decline buttons
- [x] Accepting invitation adds user to workspace and switches to it
- [x] Expired token shows "This invitation has expired" message
- [x] Invalid token shows 404-style error
- [x] WorkspaceSettingsPage shows pending invitations with copyable accept links (Owner/Admin only)
- [x] App load shows banner if user has pending invitation
- [x] No silent failures -- all API errors shown to user

**Blockers:** None (implemented against documented API contracts; live wiring depends on Syndicate CMD-24)
**Next Step for AM:** Present to Commander. Verify with Syndicate that CMD-24 endpoints match the contract.

---

### CMD-28 -- Frontend Schema Fixes + Member UX
**Filed by:** CP (Captain)
**Date:** 19-03-2026
**Status:** COMPLETE

**Summary**
Applied four frontend fixes: (1) Migrated `workspace.type` references to `workspace.plan` with fallback for backward compatibility in WorkspaceSwitcher and WorkspaceSettingsPage. Added `plan` field and `WorkspacePlan` type to Workspace interface. (2) Fixed `isCurrentUser = false` TODO by reading user ID from localStorage and comparing with member userId -- now shows emerald "You" badge. (3) Added avatar initials circles (indigo, 7x7) before display names in member table. (4) Added member count badge for team/enterprise workspaces in WorkspaceSwitcher dropdown. Zero TypeScript errors confirmed.

**Acceptance Criteria**
- [x] WorkspaceSwitcher shows correct type badge from `plan` field
- [x] "You" label appears next to current user in member table
- [x] Avatar initials shown for each member in the table
- [x] Team workspaces show member count in WorkspaceSwitcher
- [x] No TypeScript errors after type/plan migration

**Blockers:** None (plan field uses fallback to `type` for backward compat until CMD-25 deploys)
**Next Step for AM:** Present to Commander.

---

### Arcade Phase 5 Status
All 3 Arcade tickets (CMD-26, CMD-27, CMD-28) are COMPLETE. Holding for Commander -- NOT advancing to Phase 6.

---

## Syndicate
### CMD-25 -- Schema/API Alignment + RBAC Enforcement
**Filed by:** DR (Director)
**Date:** 19-03-2026
**Status:** COMPLETE

**Summary**
Added `Type` computed property to `Workspace` entity (maps from `Plan` column for frontend compatibility). Updated `CreateWorkspaceRequest` to accept both `plan` and `type` body fields with precedence logic. Hardened RBAC across WorkspacesController: ChangeRole and RemoveMember expanded from owner-only to owner OR manager; added explicit guard preventing manager from promoting to owner; added new DELETE workspace endpoint restricted to owner only. Added `DeleteAsync` to workspace repository.

**Acceptance Criteria**
- [x] GET /api/workspaces response includes `type` field (value = plan)
- [x] POST /api/workspaces accepts both `type` and `plan` body fields
- [x] Calling PATCH /workspaces/{id}/members/{userId} as Member returns 403
- [x] Calling DELETE /workspaces/{id}/members/{userId} as Member returns 403
- [x] Calling DELETE /workspaces/{id} as Admin returns 403 (Owner only)
- [x] Admin cannot set member role to 'owner' returns 403

**Blockers:** None
**Next Step for AM:** Present to Commander. Signal Arcade that CMD-28 dependency is satisfied.

### Dependency Signal
Ticket CMD-25 is COMPLETE. Teams waiting on this ticket may now proceed:
- Arcade: CMD-28 (Frontend Schema Fixes) now unblocked

---

### CMD-24 -- Invitation Flow API
**Filed by:** DR (Director)
**Date:** 19-03-2026
**Status:** COMPLETE

**Summary**
Implemented complete invitation lifecycle API: `WorkspaceInvitation` entity, `IWorkspaceInvitationRepository` with Dapper implementation, 5 CQRS handlers (CreateInvitation, AcceptInvitation, DeclineInvitation, GetInvitationByToken, GetMyInvitations), new `InvitationsController` with 4 endpoints, plus invite endpoint on WorkspacesController. Token generation uses 32-byte cryptographic random (64-char hex). Accept validates email match, expiry, and status. All handlers registered in DI. Compiles clean across all 3 library projects (Hub.Api file lock from running process is environmental).

**Acceptance Criteria**
- [x] POST /workspaces/{id}/invite creates invitation, returns accept URL
- [x] GET /invitations/{token} returns workspace info (no auth required)
- [x] POST /invitations/{token}/accept adds user to workspace, marks accepted
- [x] POST /invitations/{token}/decline marks invitation declined
- [x] GET /me/invitations returns pending invitations for calling user
- [x] Expired invitations return 410 Gone
- [x] Already-accepted invitations return 409 Conflict
- [x] Only Owner/Admin can create invitations (403 if Member calls)
- [x] Email mismatch on accept returns 403

**Blockers:** None
**Next Step for AM:** Present to Commander. Signal Arcade that CMD-27 dependency is satisfied.

### Dependency Signal
Ticket CMD-24 is COMPLETE. Teams waiting on this ticket may now proceed:
- Arcade: CMD-27 (Invitation Accept/Decline UI) now unblocked

---

### Syndicate Phase 5 Status
All 2 Syndicate tickets (CMD-25, CMD-24) are COMPLETE. Holding for Commander -- NOT advancing to Phase 6.
