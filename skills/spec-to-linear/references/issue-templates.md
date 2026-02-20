# Issue Templates

Examples of well-formed issues for both human and agent executors.

---

## Human Issue Template

```
Title: [Imperative action the human must take]
Executor: Human
Layer: Layer: [Name]
Dependencies: [None] or [Issue titles that must be done first]

Description:
[Explain what the human needs to do, why it's needed, and any
account/access details. Reference docs if relevant.]

Acceptance criteria:
- [ ] [Verifiable outcome 1]
- [ ] [Verifiable outcome 2]
```

### Example: Account Setup

**Title:** Create Convex account and project

**Executor:** Human

**Layer:** Layer: Project Setup

**Dependencies:** None

**Description:**
The backend uses Convex for the database and serverless functions. Before any backend code can be deployed, a Convex account and project must be created.

Steps:
1. Go to https://dashboard.convex.dev and sign up or log in
2. Create a new project named `[app-name]-prod`
3. Note the deployment URL (format: `https://[slug].convex.cloud`) — share it in the team Slack channel or add it to `.env.example`

See `docs/architecture.md` → "Backend Infrastructure" for context.

**Acceptance criteria:**
- [ ] Convex account created
- [ ] Project created and named correctly
- [ ] Deployment URL documented and shared

---

### Example: Design Assets

**Title:** Provide Figma designs for onboarding flow

**Executor:** Human

**Layer:** Layer: Project Setup

**Dependencies:** None

**Description:**
The onboarding flow (screens: Welcome, Sign Up, Email Verification, Profile Setup) needs Figma designs before the frontend implementation can begin.

Designs should cover:
- All screens listed in `docs/prd.md` → "Onboarding Flow" section
- Both light and dark mode variants
- Error states for form inputs
- Mobile (375px) and tablet (768px) breakpoints

Share the Figma link in the project Notion page and tag the engineering lead.

**Acceptance criteria:**
- [ ] All onboarding screens designed
- [ ] Light and dark modes covered
- [ ] Error states designed
- [ ] Figma link shared

---

## Agent Issue Template

```
Title: [Imperative action the agent must implement]
Executor: Agent
Layer: Layer: [Name]
Dependencies: [None] or [Issue titles that must be done first]

Description:
[Explain what to build, the expected behavior, and key constraints.
Reference specific sections of spec/doc files. Include data models,
API contracts, or logic details from the specs.]

Acceptance criteria:
- [ ] [Testable outcome 1]
- [ ] [Testable outcome 2]
```

### Example: Database Schema

**Title:** Implement Convex database schema

**Executor:** Agent

**Layer:** Layer: Backend Setup

**Dependencies:**
- Create Convex account and project (human issue)

**Description:**
Define the full database schema in `convex/schema.ts` based on `docs/schema.md`.

Tables to implement:
- `users` — see `docs/schema.md` → "Users Table"
- `posts` — see `docs/schema.md` → "Posts Table"
- `comments` — see `docs/schema.md` → "Comments Table"
- `notifications` — see `docs/schema.md` → "Notifications Table"

Key constraints:
- All tables require `createdAt` and `updatedAt` timestamps
- Foreign keys should use Convex `Id<"tableName">` types
- Add indexes as specified in `docs/schema.md` → "Indexes" section

Refer to `docs/architecture.md` → "Data Layer" for field-level decisions and rationale.

**Acceptance criteria:**
- [ ] All tables defined with correct field types
- [ ] All indexes added as specified
- [ ] Schema passes `npx convex dev` type check with no errors

---

### Example: API Endpoint

**Title:** Implement user profile API endpoints

**Executor:** Agent

**Layer:** Layer: Backend–Frontend Integration

**Dependencies:**
- Implement Convex database schema
- Set up authentication

**Description:**
Implement the following Convex functions for user profile management, as specified in `docs/api-contracts.md` → "User Profile":

1. `getUserProfile(userId)` — query, returns public profile fields
2. `updateUserProfile(fields)` — mutation, updates the calling user's profile (auth required)
3. `uploadProfilePhoto(storageId)` — mutation, links a Convex storage file to the user record

Business logic rules (from `docs/prd.md` → "Profile"):
- Display name must be 2–50 characters
- Bio max 300 characters
- Profile photo must be stored via Convex file storage (see `docs/architecture.md` → "File Storage")
- Users can only update their own profile — throw `ConvexError("Unauthorized")` otherwise

**Acceptance criteria:**
- [ ] All three functions implemented and exported
- [ ] Auth checks in place for mutations
- [ ] Input validation matches spec constraints
- [ ] Returns correct shape as documented in `docs/api-contracts.md`

---

### Example: UI Component

**Title:** Build onboarding flow screens

**Executor:** Agent

**Layer:** Layer: Onboarding Flow

**Dependencies:**
- Provide Figma designs for onboarding flow (human issue)
- Implement user profile API endpoints
- Set up frontend routing

**Description:**
Implement the four onboarding screens in `app/(onboarding)/` using the Figma designs provided. See `docs/prd.md` → "Onboarding Flow" for the user journey and copy.

Screens:
1. `welcome.tsx` — splash with "Get Started" and "Sign In" buttons
2. `signup.tsx` — email + password form, calls `auth.signUp()`
3. `verify-email.tsx` — 6-digit OTP input, calls `auth.verifyEmail()`
4. `profile-setup.tsx` — display name + bio + optional photo upload

Navigation flow: Welcome → Sign Up → Verify Email → Profile Setup → Home

State management: use React Hook Form for all forms. On submit errors, display inline validation messages per field.

The `uploadProfilePhoto` Convex mutation is available for the photo upload in step 4.

**Acceptance criteria:**
- [ ] All four screens render correctly on mobile (375px) and tablet (768px)
- [ ] Navigation flow works end-to-end
- [ ] Form validation matches constraints from `docs/prd.md`
- [ ] Dark mode renders correctly
- [ ] Tapping "Sign In" on the Welcome screen routes to the main login screen
