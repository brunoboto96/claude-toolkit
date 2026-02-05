# Example: Deep Plan - User Roles & Permissions Module

This example demonstrates the `/deep-plan` command output for implementing a complete user roles and permissions module.

---

## Deep Plan: User Roles & Permissions Module

**Status**: Ready for Implementation
**Confidence Level**: High
**Supporting Documents Analyzed**: 3
**Complement Documents Generated**: 2

---

### Executive Summary

Implement a comprehensive user roles and permissions system with RBAC (Role-Based Access Control), supporting predefined roles (Admin, Manager, User, Viewer), custom roles, and granular permissions. Includes database schema, backend API, frontend UI, and full audit logging.

**Scope**:
- Database: 4 new tables (roles, permissions, user_roles, role_permissions), 2 migrations
- Backend: 8 endpoints, 3 services, 2 middleware components
- Frontend: 5 pages/components, role management UI, permission guards
- Integration: Auth system integration, audit logging, navigation updates

---

### Document Analysis Summary

| Document | Path | Key Requirements | Gaps Found |
|----------|------|-----------------|------------|
| User Roles Spec | docs/specs/user-roles.md | 24 | 7 |
| Permissions Matrix | docs/specs/permissions-matrix.xlsx | 15 | 3 |
| Auth Architecture | docs/architecture/auth.md | 8 | 2 |

**Multi-Pass Reading Notes**:
- Pass 1: Identified 47 total requirements across 3 documents
- Pass 2: Extracted 156 specific field/validation details
- Pass 3: Found 12 implicit requirements from cross-references

---

### Generated Complement Documents

#### 1. user-roles_complement.md

**Purpose**: Fill specification gaps and document technical decisions

**Gaps Addressed**: 7
**Questions Raised**: 5

**Key Sections**:

```markdown
## Gap Resolution

### Gap 1: Role Hierarchy Not Specified
**Original Statement**: "Roles determine user access levels"
**Clarification Needed**: Is there role inheritance? Can Admin do everything Manager can?
**Proposed Resolution**: Implement flat role model (no inheritance) with explicit permission assignment
**Confidence**: Medium
**Action Required**: [x] Confirm with stakeholder

### Gap 2: Custom Roles - Field Constraints Missing
**Original Statement**: "Users can create custom roles"
**Clarification Needed**: Max roles per org? Max permissions per role? Name constraints?
**Proposed Resolution**:
- Max 50 custom roles per organization
- Max 100 permissions per role
- Role name: 3-50 chars, alphanumeric + spaces
**Confidence**: High (based on industry standards)

### Gap 3: Permission Granularity Undefined
**Original Statement**: "Permissions control access to features"
**Clarification Needed**: Resource-level vs action-level? Hierarchical?
**Proposed Resolution**: Action-level permissions with resource scope
- Format: `resource:action` (e.g., `users:read`, `users:write`, `users:delete`)
**Confidence**: High (matches existing auth patterns)

## Inferred Requirements

### From Context Analysis
1. **Soft delete for roles** - Users table uses soft delete, roles should match
2. **Audit trail required** - Existing features log changes, roles need same
3. **Multi-tenancy support** - Org-scoped roles based on existing architecture

### From Codebase Patterns
1. **Service layer pattern** - Found at src/services/userService.ts
2. **Validation with Zod** - Found at src/validators/userValidator.ts
3. **React Query for data fetching** - Found at src/hooks/useUsers.ts

## Technical Specifications

### Database Schema Additions

```sql
-- Timestamps not specified, adding based on convention
ALTER TABLE roles ADD COLUMN created_at TIMESTAMP DEFAULT NOW();
ALTER TABLE roles ADD COLUMN updated_at TIMESTAMP DEFAULT NOW();
ALTER TABLE roles ADD COLUMN deleted_at TIMESTAMP NULL;

-- Index for soft delete queries (not in spec)
CREATE INDEX idx_roles_deleted_at ON roles(deleted_at);
```

### API Endpoints Not in Spec

| Method | Path | Purpose | Rationale |
|--------|------|---------|-----------|
| GET | /api/roles/:id/users | List users with role | Common UI need |
| POST | /api/roles/:id/duplicate | Clone existing role | Requested in user research |

## Questions for Stakeholder Review

1. [ ] Should role deletion cascade to user_roles or soft-delete only?
2. [ ] Is there a "super admin" concept that bypasses all permissions?
3. [ ] Should permission checks be cached? What's the invalidation strategy?
4. [ ] Are there compliance requirements (SOX, HIPAA) affecting audit granularity?
5. [ ] Should role changes require approval workflow?
```

#### 2. permissions-matrix_complement.md

**Purpose**: Translate spreadsheet to implementable format

**Gaps Addressed**: 3
**Questions Raised**: 2

---

### Cross-Layer Mapping

#### Implementation Matrix

| Requirement | DB | Backend | Frontend | Migration | Config |
|-------------|-----|---------|----------|-----------|--------|
| Create role | roles table | POST /api/roles | RoleForm.tsx | Yes | - |
| Assign permissions | role_permissions | PUT /api/roles/:id/permissions | PermissionPicker.tsx | Yes | - |
| Check permissions | - | authMiddleware | PermissionGuard.tsx | - | - |
| Audit logging | audit_logs | AuditService | - | Yes | AUDIT_ENABLED |
| List user roles | user_roles | GET /api/users/:id/roles | UserRolesPanel.tsx | Yes | - |
| Default roles | roles (seed) | - | - | Seed | DEFAULT_ROLES |

#### File Location Mapping

**Database**:
- Schema: [prisma/schema.prisma:L145](prisma/schema.prisma#L145) - Add role models
- Migration: `prisma/migrations/YYYYMMDD_create_roles.sql` - New file
- Seed: [prisma/seed.ts:L89](prisma/seed.ts#L89) - Add default roles

**Backend**:
- Route: [src/routes/index.ts:L34](src/routes/index.ts#L34) - Register role routes
- Controller: `src/controllers/roleController.ts` - New file
- Service: `src/services/roleService.ts` - New file
- Middleware: [src/middleware/auth.ts:L78](src/middleware/auth.ts#L78) - Add permission check
- Validation: `src/validators/roleValidator.ts` - New file
- Types: `src/types/role.ts` - New file

**Frontend**:
- Pages: `src/pages/admin/roles/` - New directory
- Components: `src/components/roles/` - New directory
- Hooks: `src/hooks/useRoles.ts` - New file
- State: [src/store/index.ts:L23](src/store/index.ts#L23) - Add role slice
- Guards: `src/components/PermissionGuard.tsx` - New file
- Navigation: [src/components/Sidebar.tsx:L67](src/components/Sidebar.tsx#L67) - Add Roles menu item

**Configuration**:
- Environment: [.env.example:L45](.env.example#L45) - Add RBAC configs
- Constants: `src/constants/permissions.ts` - New file
- Feature flags: [src/config/features.ts:L12](src/config/features.ts#L12) - Add RBAC flag

#### Dependency Graph

```
1. DB-1: Create roles migration → Start here (no deps)
2. DB-2: Create permissions migration → Depends on DB-1
3. DB-3: Seed default roles → Depends on DB-1, DB-2
4. BE-1: Create types → Depends on DB-1
5. BE-2: Create validators → Depends on BE-1
6. BE-3: Create service → Depends on DB-3, BE-1
7. BE-4: Create controller → Depends on BE-3, BE-2
8. BE-5: Create middleware → Depends on BE-3
9. FE-1: Create types → Depends on BE-1
10. FE-2: Create API client → Depends on BE-4
11. FE-3: Create hooks → Depends on FE-2
12. FE-4: Create components → Depends on FE-3
13. FE-5: Create pages → Depends on FE-4
14. FE-6: Add to navigation → Depends on FE-5
```

---

### Critical Questions Requiring Answers

#### Critical (Blocks Implementation)

1. **What happens to users when their role is deleted?**
   - Why it matters: Data integrity and user access continuity
   - If not answered: Could lock users out or create orphaned records
   - Suggested default: Reassign to "User" role before deletion

2. **Is there a permission for "manage other users' roles"?**
   - Why it matters: Determines if managers can assign roles to their team
   - If not answered: Security gap - unclear who can modify roles
   - Suggested default: Only Admin can assign roles

3. **Should permission checks happen at API gateway or service level?**
   - Why it matters: Performance and architecture consistency
   - If not answered: Inconsistent implementation, potential security holes
   - Suggested default: Service level (matches existing patterns)

#### Important (Affects Quality)

1. **Should we cache user permissions?**
   - Why it matters: Performance at scale
   - Suggested default: Redis cache with 5-minute TTL, invalidate on role change

2. **What audit events need logging?**
   - Why it matters: Compliance and debugging
   - Suggested default: All CRUD operations + permission checks failures

#### Nice to Know (Optimization)

1. **Expected number of custom roles per organization?**
   - Benefit: Helps optimize query patterns and UI pagination

---

### Recommendations

#### Recommended Patterns (Based on Codebase)

| Pattern | Used In | Apply To | Benefit |
|---------|---------|----------|---------|
| Repository pattern | [src/repositories/userRepository.ts](src/repositories/userRepository.ts) | RoleRepository | Consistent data access |
| Zod validation | [src/validators/userValidator.ts](src/validators/userValidator.ts) | Role validation | Type-safe validation |
| React Query | [src/hooks/useUsers.ts](src/hooks/useUsers.ts) | Role hooks | Caching, refetching |
| Permission constants | [src/constants/auth.ts](src/constants/auth.ts) | Permission definitions | Single source of truth |

#### Anti-Patterns to Avoid

| Pattern | Seen In | Issue | Better Approach |
|---------|---------|-------|-----------------|
| Inline permission strings | [src/pages/admin/users.tsx:L45](src/pages/admin/users.tsx#L45) | Typo-prone, hard to refactor | Use constants |
| Client-side only permission checks | [src/components/DeleteButton.tsx](src/components/DeleteButton.tsx) | Security bypass possible | Server-side + client-side |

#### Improvement Suggestions

1. **Architecture**: Consider adding a PermissionService abstraction
   - Rationale: Centralizes permission logic, easier testing
   - Priority: Medium

2. **Performance**: Implement permission bitmap for fast checks
   - Rationale: O(1) permission checks vs O(n) array search
   - Priority: Low (optimize later if needed)

3. **Security**: Add rate limiting to role management endpoints
   - Rationale: Prevent privilege escalation attacks
   - Priority: High

4. **UX**: Add bulk role assignment for multiple users
   - Rationale: Common admin workflow, reduces clicks
   - Priority: Medium

5. **Maintainability**: Generate TypeScript types from permission constants
   - Rationale: Compile-time checking of permission strings
   - Priority: Medium

---

### Risk Assessment

| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| Permission cache gets stale | Medium | High | Short TTL + event-based invalidation |
| Migration breaks existing users | Low | Critical | Test with production data copy |
| Role deletion orphans users | Medium | High | Validate no users assigned before delete |
| Performance degradation | Low | Medium | Load test permission checks |
| UI complexity confuses admins | Medium | Medium | User testing, progressive disclosure |

---

### Implementation Blueprint

#### Layer 1: Database

- [ ] **DB-1**: Create roles table migration
  - File: `prisma/migrations/20240115_create_roles/migration.sql`
  - Fields:
    - `id` UUID PRIMARY KEY
    - `name` VARCHAR(50) NOT NULL UNIQUE
    - `description` VARCHAR(255)
    - `is_system` BOOLEAN DEFAULT false
    - `organization_id` UUID REFERENCES organizations(id)
    - `created_at` TIMESTAMP DEFAULT NOW()
    - `updated_at` TIMESTAMP DEFAULT NOW()
    - `deleted_at` TIMESTAMP NULL
  - Indexes: `idx_roles_org_id`, `idx_roles_name`
  - Constraints: UNIQUE(organization_id, name)
  - Verification: Run migration, query roles table

- [ ] **DB-2**: Create permissions and junction tables
  - File: `prisma/migrations/20240115_create_permissions/migration.sql`
  - Tables: permissions, role_permissions, user_roles
  - Verification: Run migration, check foreign keys

- [ ] **DB-3**: Seed default roles
  - File: [prisma/seed.ts](prisma/seed.ts) - Add to existing
  - Data: Admin, Manager, User, Viewer with default permissions
  - Verification: Run seed, verify 4 roles exist

#### Layer 2: Backend

- [ ] **BE-1**: Create role types
  - File: `src/types/role.ts`
  - Types: Role, Permission, RolePermission, UserRole
  - Enums: PermissionAction, PermissionResource

- [ ] **BE-2**: Create role validators
  - File: `src/validators/roleValidator.ts`
  - Schemas: createRoleSchema, updateRoleSchema, assignPermissionsSchema
  - Validation: name length, permission format, organization scope

- [ ] **BE-3**: Create RoleService
  - File: `src/services/roleService.ts`
  - Methods:
    - `createRole(data: CreateRoleDto): Promise<Role>`
    - `updateRole(id: string, data: UpdateRoleDto): Promise<Role>`
    - `deleteRole(id: string): Promise<void>`
    - `assignPermissions(roleId: string, permissionIds: string[]): Promise<void>`
    - `getUserPermissions(userId: string): Promise<Permission[]>`
    - `checkPermission(userId: string, permission: string): Promise<boolean>`
  - Dependencies: PrismaClient, CacheService

- [ ] **BE-4**: Create RoleController
  - File: `src/controllers/roleController.ts`
  - Endpoints:
    - `GET /api/roles` - List roles
    - `GET /api/roles/:id` - Get role details
    - `POST /api/roles` - Create role
    - `PUT /api/roles/:id` - Update role
    - `DELETE /api/roles/:id` - Delete role
    - `PUT /api/roles/:id/permissions` - Assign permissions
    - `GET /api/users/:id/roles` - Get user roles
    - `PUT /api/users/:id/roles` - Assign user roles

- [ ] **BE-5**: Create permission middleware
  - File: [src/middleware/auth.ts](src/middleware/auth.ts) - Extend existing
  - Method: `requirePermission(permission: string)`
  - Caching: Redis with 5-min TTL
  - Verification: Test protected endpoints

#### Layer 3: Frontend

- [ ] **FE-1**: Create role types
  - File: `src/types/role.ts`
  - Match backend types

- [ ] **FE-2**: Create API client
  - File: `src/api/roles.ts`
  - Methods: getRoles, getRole, createRole, updateRole, deleteRole, etc.

- [ ] **FE-3**: Create React Query hooks
  - File: `src/hooks/useRoles.ts`
  - Hooks: useRoles, useRole, useCreateRole, useUpdateRole, useDeleteRole
  - Include optimistic updates

- [ ] **FE-4**: Create components
  - Files:
    - `src/components/roles/RoleList.tsx`
    - `src/components/roles/RoleForm.tsx`
    - `src/components/roles/PermissionPicker.tsx`
    - `src/components/roles/UserRoleAssignment.tsx`
    - `src/components/PermissionGuard.tsx`

- [ ] **FE-5**: Create pages
  - Files:
    - `src/pages/admin/roles/index.tsx` - Role list
    - `src/pages/admin/roles/[id].tsx` - Role details
    - `src/pages/admin/roles/new.tsx` - Create role
  - Include: Loading states, error handling, empty states

- [ ] **FE-6**: Add to navigation (REQUIRED - no orphaned pages)
  - File: [src/components/Sidebar.tsx:L67](src/components/Sidebar.tsx#L67)
  - Entry point: Sidebar menu under Admin section
  - Menu item: "Roles" with icon `ShieldCheck`
  - Position: After "Users" in Admin section
  - Permission: `roles:read`
  - Discoverability: Also linked from User details page ("Manage Roles" button)

#### Layer 4: Integration

- [ ] **INT-1**: Integrate with auth context
  - File: [src/contexts/AuthContext.tsx](src/contexts/AuthContext.tsx)
  - Add: userPermissions, hasPermission method

- [ ] **INT-2**: Add audit logging
  - File: [src/services/auditService.ts](src/services/auditService.ts)
  - Events: role_created, role_updated, role_deleted, permissions_changed

#### Layer 5: Testing

- [ ] **TEST-1**: Unit tests for RoleService
- [ ] **TEST-2**: Integration tests for role endpoints
- [ ] **TEST-3**: E2E tests for role management flow
- [ ] **TEST-4**: Permission middleware tests

#### Layer 6: Documentation

- [ ] **DOC-1**: API documentation for role endpoints
- [ ] **DOC-2**: Update permissions matrix document

---

### Verification Checklist

#### Pre-Implementation
- [x] All supporting documents read completely
- [x] Complement documents generated and reviewed
- [ ] Critical questions answered
- [x] Dependencies identified (Auth system, existing user management)
- [ ] Feature flag configured (ENABLE_RBAC)

#### Post-Implementation
- [ ] All 25 tasks completed
- [ ] TypeScript compiles with no errors
- [ ] Linting passes
- [ ] All tests passing
- [ ] Manual testing completed
- [ ] Migration tested (up and down)
- [ ] Performance tested (permission check < 50ms)
- [ ] Security review passed
- [ ] Accessibility checked (WCAG 2.1 AA)
- [ ] **Navigation verified** (Roles accessible from sidebar + linked from User page)
- [ ] Documentation updated

---

### Rollback Plan

**If deployment fails**:
1. Disable feature flag `ENABLE_RBAC`
2. Revert to previous deployment
3. Investigate logs

**Database rollback**:
- Migration down: `npx prisma migrate revert`
- Data restoration: Restore from pre-deployment backup

**Feature disable**:
- Feature flag: `ENABLE_RBAC`
- Set to: `false`
- Effect: All role checks bypass, default to basic access

---

### Next Steps

1. [ ] Review complement documents with stakeholder
2. [ ] Answer 3 critical questions
3. [ ] Configure feature flag
4. [ ] Begin implementation with DB-1 (roles migration)
5. [ ] Schedule security review for BE-5 (permission middleware)
