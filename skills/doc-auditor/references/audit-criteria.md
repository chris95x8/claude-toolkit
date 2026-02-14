# Audit Criteria Reference

Use this reference to ensure all audit categories are applied systematically when cross-examining specification and planning documents.

---

## Category 1: Logical Gaps

Issues where the documentation describes a goal or outcome but omits the steps, mechanisms, or decisions needed to reach it.

**What to look for:**
- A feature is mentioned as a requirement but no implementation path or approach is described
- A user flow is described at the start and end but the middle steps are absent
- A data model or schema is referenced but never defined
- An integration or dependency is listed without explaining how it connects to the system
- A sequence of events where the cause-and-effect chain breaks down
- Inputs described without corresponding outputs (or vice versa)

**Probe questions:**
- "How exactly does X become Y?"
- "What triggers this state transition?"
- "Where does this data come from / where does it go?"

---

## Category 2: Inconsistencies and Contradictions

Issues where two or more documents (or sections within the same document) make conflicting statements about the same thing.

**What to look for:**
- Different data field names or types for the same concept across documents
- Conflicting ownership or responsibility assignments (e.g., two services claim to own the same data)
- Feature descriptions that differ between a PRD and a technical spec
- User stories that contradict acceptance criteria
- API contracts that disagree with the data model
- Timeline or sequencing conflicts (e.g., Feature B depends on Feature A but is scheduled first)
- Terminology used inconsistently (same word means different things in different docs)

**Probe questions:**
- "Does the PRD agree with the technical spec on this point?"
- "Is this field name / type consistent across all references?"
- "Who owns this? Does every document agree?"

---

## Category 3: Missing Specifications

Issues where a critical aspect of the system is entirely absent from the documentation — things an engineer would need to know but cannot infer.

**What to look for:**
- No error handling or failure modes defined for critical paths
- Authentication and authorization rules not specified
- No mention of rate limits, quotas, or performance targets
- Data retention, deletion, or privacy rules missing
- Edge cases and boundary conditions undefined (empty states, max values, null inputs)
- No definition of done / acceptance criteria for features
- Deployment or environment configuration not documented
- Migration strategy absent when a breaking change is described
- Accessibility, localization, or compliance requirements not addressed
- No rollback plan for risky changes

**Probe questions:**
- "What happens when this fails?"
- "What are the limits / constraints?"
- "How is access controlled?"
- "What does success look like for an engineer?"

---

## Category 4: Ambiguities

Issues where the language used is vague, subjective, or open to multiple valid interpretations, which would cause different engineers to build different things.

**What to look for:**
- Relative or unmeasurable terms: "fast," "soon," "large," "simple," "good UX," "scalable"
- Passive voice without a clear actor: "the data will be processed," "users will be notified"
- Undefined pronouns or references: "it," "this," "the system" without clear antecedents
- "Should" vs. "must" vs. "may" used interchangeably or without intent
- Multiple valid readings of a requirement sentence
- Scope words that are under-defined: "all users," "most cases," "typically"
- Feature toggles or conditions described without specifying the exact trigger

**Probe questions:**
- "What does 'fast' mean in measurable terms?"
- "Who specifically performs this action?"
- "Under what exact conditions does this apply?"

---

## Category 5: Critical Issues (Blockers)

Issues that would prevent a team from starting or completing the build — not just gaps in documentation, but fundamental unsolved problems in the design itself.

**What to look for:**
- A core user flow that has no viable technical path described
- A dependency on an external system or API that is not confirmed to exist or be accessible
- Two requirements that are mutually exclusive (cannot both be true)
- A security model that has an obvious bypass or hole
- A data architecture that cannot support the described query or access patterns
- A stated timeline that is impossible given the described scope
- A third-party constraint (legal, compliance, vendor) that has not been accounted for
- A required decision (e.g., "choose database") that has been deferred but blocks all downstream work

**Probe questions:**
- "Is this actually buildable as described?"
- "What has to be true for this to work, and is it guaranteed?"
- "Does anything here legally or technically prevent this?"

---

## Category 6: Dependency and Sequencing Issues

Issues related to the order in which things must be built, or external dependencies that are not surfaced.

**What to look for:**
- Features that depend on other features not yet planned or scoped
- Circular dependencies between components or services
- External services, APIs, or vendors referenced but not confirmed
- Infrastructure or platform prerequisites that are assumed but not stated
- Database migrations required before feature rollout but not sequenced
- Shared components depended on by multiple features without a clear owner or timeline

---

## Category 7: Scope and Completeness Issues

Issues where the boundaries of the work are unclear or where important topics are conspicuously absent.

**What to look for:**
- No clear definition of what is in scope vs. out of scope
- Features described at vastly different levels of detail (some over-specified, some bare)
- MVP vs. full product not distinguished
- Testing, monitoring, or observability requirements absent
- Documentation of the feature itself (user-facing help, admin docs) not planned
- Non-functional requirements (performance, reliability, security) entirely missing

---

## Priority Tiers for Issues

When reporting issues, assign a priority tier:

| Tier | Label | Criteria |
|------|-------|----------|
| P0 | **Critical / Blocker** | Prevents the team from starting or completing the feature. Must be resolved before any implementation begins. |
| P1 | **High** | Will likely cause significant rework, bugs, or misalignment if not resolved before implementation. Should be resolved in the planning phase. |
| P2 | **Medium** | Creates ambiguity or risk that should be addressed, but work could start with a documented assumption in the interim. |
| P3 | **Low** | Minor clarity improvements. Nice to resolve but unlikely to cause implementation problems. |
