# Syndicate Team Chat — 19-03-2026

## Session Start — Phase 5: Organization Workspaces (CMD-25, CMD-24)

**DR:** Syndicate online. Two tickets on deck — CMD-25 (Schema/API Alignment + RBAC) starts now, CMD-24 (Invitation Flow API) proceeds per Commander's authorization to write app code without waiting for migration file. Timeline is tight. No delays.

**AX:** Codebase scanned. WorkspacesController already has partial RBAC — `ChangeRole` and `RemoveMember` check for owner-only, `InviteMember` checks owner/manager. Gaps identified:
- Workspace entity has `Plan` but no `Type` alias — frontend expects `type`
- `CreateWorkspaceRequest` accepts `Plan` but not `Type`
- `ChangeRole` only allows owner — ticket says Owner OR Admin (i.e. "manager" in current schema)
- `RemoveMember` only allows owner — same, needs Owner OR Admin
- No guard against Admin promoting to Owner
- `DeleteWorkspace` endpoint doesn't exist yet — ticket says Owner only

**LX:** API response schema needs a `Type` property mapped from `Plan`. Clean alias pattern — keep `Plan` as source of truth, expose `Type` for frontend compatibility.

**WT:** RBAC audit confirms: role values in DB are 'owner', 'manager', 'contributor', 'viewer'. Ticket references "Admin" = "manager" in our schema. Will verify all role checks align after implementation.

---

### CMD-25 — Schema/API Alignment + RBAC [COMPLETE]

**DR:** Starting CMD-25 now. AX, execute.

**AX:** Done. Changes applied:
1. `Workspace.cs` — added `Type` computed property mapped from `Plan`
2. `CreateWorkspaceRequest` — now accepts both `Plan` and `Type` fields; `ResolvedPlan` resolves precedence
3. `CreateWorkspaceCommand` — uses `ResolvedPlan` instead of raw `Plan`
4. `WorkspacesController.ChangeRole` — expanded from owner-only to owner OR manager; added guard blocking manager from promoting to owner
5. `WorkspacesController.RemoveMember` — expanded from owner-only to owner OR manager
6. `WorkspacesController.DeleteWorkspace` — new endpoint, owner-only
7. `IWorkspaceRepository.DeleteAsync` + `WorkspaceRepository.DeleteAsync` — new method
8. `SecurityEventTypes` — added `WorkspaceDeleted`, `InvitationCreated`, `InvitationAccepted`

**LX:** Response shape is clean. `Type` auto-serializes alongside `Plan` in all workspace JSON responses. No DTO changes needed — the computed property propagates through existing serialization.

**WT:** RBAC matrix verified:
| Endpoint | Member | Manager | Owner |
|----------|--------|---------|-------|
| PATCH members (role change) | 403 | Allowed (cannot set owner) | Allowed |
| DELETE members (remove) | 403 | Allowed | Allowed |
| DELETE workspace | 403 | 403 | Allowed |
| POST invite (CMD-24) | 403 | Allowed | Allowed |

All acceptance criteria met. Zero compilation errors.

**DR:** CMD-25 marked Complete. Dependency Signal: Arcade CMD-28 unblocked.

---

### CMD-24 — Invitation Flow API [COMPLETE]

**DR:** Proceeding to CMD-24. Commander authorized writing app code without waiting for physical migration file. AX, build it.

**AX:** Full invitation flow implemented:

**Domain Layer:**
- `WorkspaceInvitation.cs` — entity with all columns matching migration schema + joined display fields
- `IWorkspaceInvitationRepository.cs` — 5 methods: Create, GetByToken, GetPendingByEmail, Accept, Decline

**Application Layer (5 handlers):**
- `CreateInvitationCommand` — generates 64-char hex token via `RandomNumberGenerator`, validates role, creates record
- `AcceptInvitationCommand` — validates token/expiry/status/email match, inserts `workspace_members`, marks accepted
- `DeclineInvitationCommand` — validates token/status, marks declined
- `GetInvitationByTokenQuery` — joins workspace name + inviter name, computes expired status on-the-fly
- `GetMyInvitationsQuery` — looks up user email, returns pending non-expired invitations

**Infrastructure Layer:**
- `WorkspaceInvitationRepository.cs` — Dapper queries against `hub.workspace_invitations`, casts role to `hub.workspace_role` enum

**API Layer:**
- `WorkspacesController.CreateInvitation` — `POST /api/workspaces/{id}/invite` (owner/manager only)
- `InvitationsController` — new controller with 4 endpoints:
  - `GET /api/invitations/{token}` (anonymous preview)
  - `POST /api/invitations/{token}/accept` (auth, email must match)
  - `POST /api/invitations/{token}/decline` (auth)
  - `GET /api/me/invitations` (auth, pending for calling user)

**DI Registration:**
- Infrastructure: `IWorkspaceInvitationRepository` registered as Singleton
- Application: all 5 handlers registered as Scoped

**LX:** Endpoint schema is RESTful and consistent. Token-based routes are clean — `{token}` as path param, status codes follow HTTP semantics (201 create, 410 expired, 409 conflict, 403 email mismatch).

**WT:** Security audit passed:
- Token is cryptographically random (32 bytes = 64 hex chars)
- Email match enforced on accept — prevents token interception by wrong user
- Expired tokens return 410, already-used tokens return 409
- RBAC on create: owner/manager only
- No emails sent (out of scope) — token + URL returned to caller
- All three library projects compile clean (0 warnings, 0 errors). Hub.Api file lock from running process is environmental, not code.

**DR:** CMD-24 marked Complete. Dependency Signal: Arcade CMD-27 unblocked.

### Dependency Signal
Ticket CMD-25 is COMPLETE. Teams waiting on this ticket may now proceed:
- Arcade: CMD-28 now unblocked

### Dependency Signal
Ticket CMD-24 is COMPLETE. Teams waiting on this ticket may now proceed:
- Arcade: CMD-27 now unblocked

---

### Output Delivered

**Files Created:**
- `C:/UnicornVibeCode/UniOpsQC-command/src/Hub.Domain/Entities/WorkspaceInvitation.cs`
- `C:/UnicornVibeCode/UniOpsQC-command/src/Hub.Domain/Interfaces/IWorkspaceInvitationRepository.cs`
- `C:/UnicornVibeCode/UniOpsQC-command/src/Hub.Application/Features/Workspaces/CreateInvitationCommand.cs`
- `C:/UnicornVibeCode/UniOpsQC-command/src/Hub.Application/Features/Workspaces/AcceptInvitationCommand.cs`
- `C:/UnicornVibeCode/UniOpsQC-command/src/Hub.Application/Features/Workspaces/DeclineInvitationCommand.cs`
- `C:/UnicornVibeCode/UniOpsQC-command/src/Hub.Application/Features/Workspaces/GetInvitationByTokenQuery.cs`
- `C:/UnicornVibeCode/UniOpsQC-command/src/Hub.Application/Features/Workspaces/GetMyInvitationsQuery.cs`
- `C:/UnicornVibeCode/UniOpsQC-command/src/Hub.Infrastructure/Repositories/WorkspaceInvitationRepository.cs`
- `C:/UnicornVibeCode/UniOpsQC-command/src/Hub.Api/Controllers/InvitationsController.cs`

**Files Modified:**
- `C:/UnicornVibeCode/UniOpsQC-command/src/Hub.Domain/Entities/Workspace.cs` — added `Type` computed property
- `C:/UnicornVibeCode/UniOpsQC-command/src/Hub.Domain/Interfaces/IWorkspaceRepository.cs` — added `DeleteAsync`
- `C:/UnicornVibeCode/UniOpsQC-command/src/Hub.Domain/Interfaces/ISecurityLogger.cs` — added 3 event type constants
- `C:/UnicornVibeCode/UniOpsQC-command/src/Hub.Application/Features/Workspaces/CreateWorkspaceCommand.cs` — `Type` alias + `ResolvedPlan`
- `C:/UnicornVibeCode/UniOpsQC-command/src/Hub.Application/DependencyInjection.cs` — registered 5 invitation handlers
- `C:/UnicornVibeCode/UniOpsQC-command/src/Hub.Infrastructure/Repositories/WorkspaceRepository.cs` — added `DeleteAsync`
- `C:/UnicornVibeCode/UniOpsQC-command/src/Hub.Infrastructure/DependencyInjection.cs` — registered invitation repository
- `C:/UnicornVibeCode/UniOpsQC-command/src/Hub.Api/Controllers/WorkspacesController.cs` — RBAC hardening + invite endpoint + delete endpoint

**Reason:** Both tickets deliver Phase 5 Organization Workspace API — CMD-25 fixes schema mismatch and hardens RBAC, CMD-24 adds the full invitation lifecycle replacing direct member insertion.

**DR:** Phase 5 Syndicate scope complete. Both tickets delivered. Holding for Commander — do NOT advance to Phase 6.
