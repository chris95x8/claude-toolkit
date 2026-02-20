# Layer Guide

Layers are groupings of work that share a context or that must be sequenced before later work can start. Derive them from the actual product — do not use a fixed set of names.

---

## How to Identify Layers

Read the specs and ask:

1. **What must exist before anything else can be built?**
   Any human setup steps (accounts, native project scaffolding, design assets) form their own layer and always come first.

2. **What are the foundational technical pieces?**
   These are things every feature will depend on: data model, auth, project scaffold, core infrastructure. Group them by system boundary (e.g. separate backend and mobile scaffolding).

3. **What are the major user-facing features or flows?**
   Each significant feature or flow is a candidate for its own layer. If a feature has both backend and frontend work, decide whether they can be one layer or need two (split when the backend work is substantial and should be done before the frontend starts).

4. **What depends on what?**
   If Feature B can only start after Feature A ships, they are separate layers in that order.

5. **Is there a final hardening phase?**
   If the specs mention polish, QA, or launch prep, that is the last layer.

---

## Naming

Use the format `Layer: [Name]` so layers are easy to filter in Linear.

Name layers after **what they deliver**, not a generic role:
- Good: `Layer: Auth`, `Layer: Feed`, `Layer: Checkout`, `Layer: iOS Shell`
- Avoid: `Layer: Backend`, `Layer: Frontend` (too generic — be specific to this product)

If a product has both mobile and web clients, name them specifically: `Layer: iOS Shell`, `Layer: Web Shell`.

---

## Ordering Rules

1. Human setup layers always come first.
2. Foundational infrastructure (data model, auth, scaffolding) before features.
3. Feature layers in dependency order — if one feature consumes another's data, it goes after.
4. Polish / QA / launch prep always last.

---

## Size Check

If a layer would have more than ~6–8 issues, consider whether it should be split into two more focused layers.
