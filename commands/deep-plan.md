---
description: Deep planning for complete implementation - extracts ALL details from supporting docs, generates complement documents, asks non-obvious questions, and ensures production/compliance readiness across all system layers.
---

# Deep Plan - Comprehensive Implementation Planning

**Purpose**: Ensure thorough, production-ready implementation planning by extracting ALL details from supporting documents, generating complement materials, asking probing questions, and creating a complete implementation blueprint across all system layers (database, backend, frontend, migrations, UI/UX).

## Quick Start

**Usage**: `deep-plan [task description or supporting document reference]`

**Examples**:
- `deep-plan implement the user roles module per spec.md`
- `deep-plan add payment integration following payments-spec.md`
- `deep-plan build the dashboard feature`
- `deep-plan implement authentication system`

**What It Does** (7-phase deep analysis):
1. **Document Extraction**: Complete multi-pass reading of all supporting documents
2. **Gap Analysis**: Identify missing details and ambiguities
3. **Complement Generation**: Create supplementary documents with suffix `_complement.md`
4. **Cross-Layer Mapping**: Map requirements to DB, backend, frontend, migrations
5. **Probing Questions**: Ask non-obvious questions to surface hidden requirements
6. **Recommendations**: Suggest improvements and best practices
7. **Implementation Blueprint**: Detailed task breakdown with exact locations

**Key Differentiator**: Unlike standard planning, this command:
- Reads supporting documents FULLY (multiple passes, no skimming)
- Generates complement documents to fill gaps
- Ensures NO detail is overlooked
- Maps every requirement to specific files and line numbers
- Asks questions that aren't obvious from surface reading
- **Navigation**: Features must be accessible via existing navigation, menus, or linked from relevant pages - NO standalone/orphaned pages unless explicitly requested

## Instructions

When a user requests deep planning, execute ALL phases thoroughly. Do NOT skip steps or do partial analysis.

---

### Phase 1: Document Extraction - Complete Multi-Pass Reading

**Goal**: Extract 100% of information from all supporting documents. No partial reads.

#### 1.1 Identify All Source Documents

Search for and identify ALL relevant documents:
- **Explicit References**: Documents mentioned in the task
- **Related Specs**: Files matching `*spec*`, `*requirements*`, `*design*`
- **Existing Implementation**: Similar features already implemented
- **Configuration Files**: Environment, config, constants
- **Schema Files**: Database schemas, API schemas, type definitions

**Action**: Use Glob and Grep to find all potentially relevant documents:
```
Glob: **/*spec*.md, **/*requirement*.md, **/*design*.md
Grep: [feature name], [module name], [relevant keywords]
```

#### 1.2 Multi-Pass Document Reading

For EACH document, perform THREE reading passes:

**Pass 1 - Structure Extraction**:
- Section headings and hierarchy
- Lists of features/requirements
- Tables and matrices
- Diagrams and flowcharts mentioned

**Pass 2 - Detail Extraction**:
- Every individual requirement
- Field specifications (names, types, constraints)
- Business rules and validation logic
- Edge cases mentioned
- Error handling requirements
- Performance requirements

**Pass 3 - Implicit Requirements**:
- Dependencies implied but not stated
- Assumptions made by the document
- Integration points referenced
- Security implications
- Compliance requirements

**Action**: Read each document completely. Use Read tool with full file, NOT partial reads. Create structured notes for each pass.

#### 1.3 Cross-Reference Validation

After reading all documents:
- Identify contradictions between documents
- Find gaps where documents reference missing specifications
- Note version mismatches or outdated information
- List external dependencies mentioned

---

### Phase 2: Gap Analysis - Identify What's Missing

**Goal**: Find every missing piece of information before implementation begins.

#### 2.1 Requirements Completeness Check

For each requirement found, verify presence of:

| Aspect | Question | Missing = Gap |
|--------|----------|---------------|
| **Data** | What data is needed? What's the schema? | Yes |
| **Validation** | What validation rules apply? | Yes |
| **Permissions** | Who can access/modify this? | Yes |
| **States** | What states can this be in? | Yes |
| **Transitions** | How do states change? | Yes |
| **Errors** | What errors can occur? | Yes |
| **Recovery** | How to recover from errors? | Yes |
| **Audit** | What needs logging? | Yes |
| **Performance** | Any performance requirements? | Yes |
| **Limits** | Any rate limits, size limits? | Yes |

#### 2.2 Layer-Specific Gap Detection

**Database Layer**:
- [ ] All entities defined?
- [ ] All relationships mapped?
- [ ] Indexes specified?
- [ ] Constraints defined?
- [ ] Migration strategy clear?
- [ ] Seed data needed?

**Backend Layer**:
- [ ] All endpoints defined?
- [ ] Request/response formats?
- [ ] Authentication requirements?
- [ ] Authorization rules?
- [ ] Error codes and messages?
- [ ] Rate limiting?
- [ ] Caching strategy?

**Frontend Layer**:
- [ ] All screens/pages defined?
- [ ] Component hierarchy?
- [ ] State management approach?
- [ ] Form validations?
- [ ] Loading states?
- [ ] Error displays?
- [ ] Empty states?
- [ ] Mobile responsiveness?
- [ ] **Navigation integration?** (How users reach this feature - NO orphaned pages)

**Integration Layer**:
- [ ] External API dependencies?
- [ ] Webhook requirements?
- [ ] Event/message queues?
- [ ] Third-party services?

#### 2.3 Document Gaps Found

Create a structured list of all gaps:
```markdown
### Gaps Identified

#### Critical Gaps (Blocking)
1. [Gap description] - [Why it's critical]

#### Important Gaps (Should Clarify)
1. [Gap description] - [Impact if not clarified]

#### Minor Gaps (Can Assume)
1. [Gap description] - [Reasonable assumption]
```

---

### Phase 3: Complement Document Generation

**Goal**: Generate supplementary documents to fill gaps and provide implementation clarity.

#### 3.1 When to Generate Complements

Generate a complement document (`_complement.md`) when:
- Original document has >3 critical gaps
- Implementation requires details not in spec
- Business logic needs explicit documentation
- Technical decisions need recording

#### 3.2 Complement Document Structure

For each supporting document `[name].md`, generate `[name]_complement.md`:

```markdown
# [Original Document Name] - Implementation Complement

**Source Document**: [path/to/original.md]
**Generated For**: [task description]
**Status**: Draft - Requires Review

## Gap Resolution

### Gap 1: [Gap Title]
**Original Statement**: "[Quote from original]"
**Clarification Needed**: [What's unclear]
**Proposed Resolution**: [Your recommendation]
**Confidence**: High/Medium/Low
**Action Required**: [ ] Confirm with stakeholder / [ ] Proceed with assumption

### Gap 2: [Gap Title]
...

## Inferred Requirements

### From Context Analysis
1. [Requirement inferred] - [Source of inference]

### From Codebase Patterns
1. [Requirement inferred] - [Similar implementation at path/to/file.ext]

## Technical Specifications

### Database Schema Additions
```sql
-- Tables/columns not in original spec
```

### API Endpoints Additions
| Method | Path | Purpose | Not in Spec |
|--------|------|---------|-------------|

### Frontend Components
| Component | Purpose | Not in Spec |
|-----------|---------|-------------|

## Implementation Notes

### Recommended Patterns
- [Pattern] - [Why recommended based on codebase]

### Potential Gotchas
- [Issue] - [How to avoid]

## Questions for Stakeholder Review
1. [ ] [Question]
2. [ ] [Question]
```

#### 3.3 Complement Types

Generate different complement types based on task:

**Feature Implementation** → `feature_complement.md`
- Missing user stories
- Edge case definitions
- Acceptance criteria

**API Development** → `api_complement.md`
- Request/response examples
- Error code catalog
- Rate limit specifications

**Database Changes** → `schema_complement.md`
- Migration scripts
- Rollback procedures
- Data transformation rules

**UI Development** → `ui_complement.md`
- Interaction patterns
- Accessibility requirements
- Responsive breakpoints

---

### Phase 4: Cross-Layer Requirement Mapping

**Goal**: Map every requirement to specific implementation locations.

#### 4.1 Create Implementation Matrix

For EACH requirement, identify:

```markdown
| Requirement | DB | Backend | Frontend | Migration | Config |
|-------------|-----|---------|----------|-----------|--------|
| [Req 1] | Table: X | Endpoint: Y | Page: Z | Yes/No | Key: A |
```

#### 4.2 File Location Mapping

For each component, identify EXACT files:

**Database**:
- Schema file: `path/to/schema.prisma:L45` or `path/to/models/*.py`
- Migration: `migrations/YYYYMMDD_name.sql`
- Seed: `seeds/feature.ts`

**Backend**:
- Route: `routes/feature.ts:L23`
- Controller: `controllers/featureController.ts`
- Service: `services/featureService.ts`
- Validation: `validators/featureValidator.ts`
- Types: `types/feature.ts`

**Frontend**:
- Page: `pages/feature/index.tsx`
- Components: `components/feature/*.tsx`
- Hooks: `hooks/useFeature.ts`
- State: `store/featureSlice.ts`
- API client: `api/feature.ts`

**Configuration**:
- Environment: `.env.example` additions
- Constants: `constants/feature.ts`
- Feature flags: `config/features.ts`

#### 4.3 Dependency Graph

Create dependency order:
```
1. [Task] depends on nothing → Start here
2. [Task] depends on [1] → Then this
3. [Task] depends on [1, 2] → Then this
```

---

### Phase 5: Probing Questions - Surface Hidden Requirements

**Goal**: Ask questions that aren't obvious but are critical for implementation.

#### 5.1 Standard Probing Questions

Always ask these across all implementations:

**Data & State**:
- What happens to existing data during migration?
- Are there any soft-delete vs hard-delete requirements?
- What's the data retention policy?
- Is there historical data that needs preserving?

**Security & Compliance**:
- Is there PII involved? What's the handling policy?
- What audit logging is required?
- Are there compliance requirements (GDPR, HIPAA, SOC2)?
- What's the data encryption requirement (at rest, in transit)?

**Performance & Scale**:
- What's the expected data volume?
- Are there any SLAs for response time?
- What's the concurrent user expectation?
- Are there batch processing requirements?

**Integration & Dependencies**:
- Are there external systems that need notification?
- Are there downstream systems that consume this data?
- Are there webhooks or callbacks needed?
- Is there a queue/async processing requirement?

**User Experience**:
- What's the expected user journey?
- What feedback should users see during operations?
- How should errors be presented?
- Are there accessibility requirements?
- **How will users discover/access this feature?** (navigation, menu, link from existing page)

**Operations**:
- How will this be deployed?
- Is there a feature flag requirement?
- What monitoring/alerting is needed?
- What's the rollback strategy?

#### 5.2 Task-Type Specific Questions

**For Feature Implementation**:
- Is this replacing an existing feature or net new?
- Are there A/B testing requirements?
- What analytics events need tracking?

**For API Development**:
- Is this a public or internal API?
- What versioning strategy?
- Are there SDK/client considerations?

**For Database Changes**:
- Is zero-downtime migration required?
- Are there dependent reports or analytics?
- Is there an archive/purge strategy?

**For UI Development**:
- Are there design mockups?
- What's the internationalization requirement?
- Are there animation/transition requirements?

#### 5.3 Output Non-Obvious Questions

List questions with WHY they matter:

```markdown
### Questions Requiring Answers

#### Critical (Blocks Implementation)
1. **[Question]**
   - Why it matters: [Impact]
   - If not answered: [Risk]
   - Suggested default: [Assumption if no answer]

#### Important (Affects Quality)
1. **[Question]**
   - Why it matters: [Impact]
   - Suggested default: [Assumption if no answer]

#### Nice to Know (Optimization)
1. **[Question]**
   - Why it matters: [Impact]
```

---

### Phase 6: Recommendations & Best Practices

**Goal**: Proactively suggest improvements beyond the stated requirements.

#### 6.1 Codebase Pattern Analysis

Analyze existing codebase for patterns to follow:

**Action**: Use Grep and Read to find similar implementations:
- How are similar features structured?
- What patterns are used for auth, validation, error handling?
- What conventions exist for naming, file organization?

**Output**:
```markdown
### Recommended Patterns (Based on Codebase)

#### Pattern: [Name]
- **Used in**: [path/to/example.ts]
- **Apply to**: [This feature]
- **Benefit**: [Why]

#### Anti-Pattern to Avoid
- **Seen in**: [path/to/bad-example.ts]
- **Issue**: [Problem]
- **Better approach**: [Solution]
```

#### 6.2 Improvement Suggestions

Suggest improvements in these categories:

**Architecture**:
- Better separation of concerns
- More testable structure
- Improved error handling

**Performance**:
- Caching opportunities
- Query optimization
- Lazy loading candidates

**Security**:
- Additional validation
- Input sanitization
- Permission hardening

**User Experience**:
- Optimistic updates
- Better loading states
- Helpful error messages

**Maintainability**:
- Type safety improvements
- Documentation needs
- Test coverage recommendations

#### 6.3 Risk Identification

Identify implementation risks:

```markdown
### Implementation Risks

| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| [Risk 1] | High/Med/Low | High/Med/Low | [Strategy] |
```

---

### Phase 7: Implementation Blueprint

**Goal**: Create a complete, unambiguous implementation plan.

#### 7.1 Task Breakdown Structure

Break down into atomic, actionable tasks:

```markdown
## Implementation Blueprint

### Layer 1: Database
- [ ] **DB-1**: Create migration for [table]
  - File: `migrations/YYYYMMDD_create_X.sql`
  - Fields: [list every field with type]
  - Indexes: [list indexes]
  - Constraints: [list constraints]
  - Verification: Run migration, check schema

- [ ] **DB-2**: Add seed data
  - File: `seeds/X.ts`
  - Data: [describe seed data]

### Layer 2: Backend
- [ ] **BE-1**: Create [model/entity]
  - File: `models/X.ts`
  - Extends: [if applicable]
  - Methods: [list methods needed]

- [ ] **BE-2**: Create [service]
  - File: `services/XService.ts`
  - Methods: [list with signatures]
  - Dependencies: [list injections]

- [ ] **BE-3**: Create [endpoint]
  - File: `routes/X.ts`
  - Method: GET/POST/PUT/DELETE
  - Path: `/api/v1/X`
  - Auth: [requirement]
  - Request: [schema]
  - Response: [schema]
  - Errors: [list error codes]

### Layer 3: Frontend
- [ ] **FE-1**: Create [page/component]
  - File: `pages/X/index.tsx` or `components/X.tsx`
  - Props: [list]
  - State: [list]
  - Hooks: [list]

- [ ] **FE-2**: Add to navigation (REQUIRED - no orphaned pages)
  - File: `components/Navigation.tsx` or relevant menu/sidebar
  - Entry point: [sidebar menu / header nav / link from parent page / dashboard card]
  - Position: [where in nav hierarchy]
  - Permissions: [who sees it]
  - Discoverability: [how users find this feature]

### Layer 4: Integration
- [ ] **INT-1**: Wire [frontend to backend]
  - API client: `api/X.ts`
  - Hook: `hooks/useX.ts`
  - State management: [approach]

### Layer 5: Testing
- [ ] **TEST-1**: Unit tests for [service]
- [ ] **TEST-2**: Integration tests for [endpoint]
- [ ] **TEST-3**: E2E tests for [flow]

### Layer 6: Documentation
- [ ] **DOC-1**: Update API docs
- [ ] **DOC-2**: Update README if needed
```

#### 7.2 Verification Checklist

Create a checklist for implementation verification:

```markdown
### Pre-Implementation Verification
- [ ] All questions answered or defaults accepted
- [ ] Complement documents reviewed
- [ ] Dependencies identified and available
- [ ] Feature flag configured (if needed)

### Post-Implementation Verification
- [ ] All tasks completed
- [ ] Tests passing
- [ ] No TypeScript/lint errors
- [ ] Manual testing completed
- [ ] Documentation updated
- [ ] Migration tested (up and down)
- [ ] Performance acceptable
- [ ] Security review passed
- [ ] Accessibility checked
- [ ] **Navigation verified** (feature accessible via menus/links, not orphaned)
```

#### 7.3 Rollback Plan

Always include rollback strategy:

```markdown
### Rollback Plan

**If deployment fails**:
1. [Step 1]
2. [Step 2]

**Database rollback**:
- Migration down command: `[command]`
- Data restoration: [procedure]

**Feature disable**:
- Feature flag: `[flag name]`
- Set to: `false`
```

---

## Output Format

After completing all phases, provide this comprehensive output:

```markdown
## Deep Plan: [Task Name]

**Status**: Ready for Implementation / Needs Clarification / Blocked
**Confidence Level**: High / Medium / Low
**Supporting Documents Analyzed**: [count]
**Complement Documents Generated**: [count]

---

### Executive Summary

[2-3 sentence overview of what will be implemented]

**Scope**:
- Database: [changes summary]
- Backend: [changes summary]
- Frontend: [changes summary]
- Integration: [changes summary]

---

### Document Analysis Summary

| Document | Path | Key Requirements | Gaps Found |
|----------|------|-----------------|------------|
| [Name] | [path] | [count] | [count] |

---

### Generated Complement Documents

1. **[name]_complement.md** - [purpose]
   - Gaps addressed: [count]
   - Questions raised: [count]

---

### Cross-Layer Mapping

[Implementation Matrix from Phase 4]

---

### Critical Questions Requiring Answers

[Questions from Phase 5 that are blocking]

---

### Recommendations

[Key recommendations from Phase 6]

---

### Implementation Blueprint

[Complete task breakdown from Phase 7]

---

### Risk Assessment

| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|

---

### Verification Checklist

[Combined checklist]

---

### Next Steps

1. [ ] Review complement documents
2. [ ] Answer critical questions
3. [ ] Begin implementation with [first task]
```

---

## Execution Strategy

1. **Never skim documents** - Read everything fully
2. **Generate complements proactively** - Don't wait for failures
3. **Ask questions early** - Before implementation, not during
4. **Map everything to files** - No vague references
5. **Consider all layers** - DB, backend, frontend, config
6. **Think production** - Security, performance, compliance
7. **Plan for failure** - Rollback, error handling, edge cases
8. **No orphaned pages** - Every feature MUST be accessible via navigation/menus/links

## Verification Before Completing

Before marking plan as complete, verify:
- [ ] All supporting documents read completely (not partial)
- [ ] All gaps identified and documented
- [ ] Complement documents generated for significant gaps
- [ ] All requirements mapped to specific files
- [ ] All non-obvious questions listed
- [ ] Recommendations provided
- [ ] Complete task breakdown with file paths
- [ ] Verification checklist included
- [ ] Rollback plan included
- [ ] **Navigation plan included** (how users access the feature - no orphaned pages)

## Adaptive Behavior

Adapt the depth based on task type:

| Task Type | Focus Areas |
|-----------|-------------|
| New Feature | All phases equally |
| Bug Fix | Phase 2, 5, 7 (gaps, questions, blueprint) |
| Refactor | Phase 4, 6, 7 (mapping, recommendations, blueprint) |
| Migration | Phase 1, 3, 7 (docs, complements, blueprint) |
| Integration | Phase 2, 4, 5 (gaps, mapping, questions) |
| UI/UX | Phase 1, 5, 6 (docs, questions, recommendations) |
