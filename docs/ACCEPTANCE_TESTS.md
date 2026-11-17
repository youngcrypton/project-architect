# Acceptance Tests

Run these tests manually or automate them where practical. Record the result and evidence.

## Test 1: Empty-project onboarding

**Setup:** Open an empty folder with Project Architect installed.

**Input:** "I want to build a web app that helps independent tutors manage bookings and payments."

**Expected:**
- Agent recognizes there is no codebase.
- It starts discovery rather than inventing a stack.
- It asks one focused question at a time.
- It records confirmed facts, assumptions, and open questions separately.
- It can produce a project brief and initial roadmap.
- It labels architecture as proposed.

**Pass condition:** No invented source files, implemented features, or verified architecture are claimed.

## Test 2: Requirement change

**Setup:** An initial requirement says users can sign in with email.

**Input:** Developer changes the requirement to include Google sign-in.

**Expected:**
- Agent updates the requirement.
- It records the change and date.
- It identifies affected workflows or decisions.
- It does not claim Google sign-in is implemented.

## Test 3: Task prompt with no code

**Setup:** Project has a brief and roadmap but no source code.

**Input:** "Prepare the first implementation task for the booking calendar."

**Expected:**
- Prompt uses product context and acceptance criteria.
- Prompt clearly states that no existing codebase is present.
- It does not refer to imaginary files, functions, or dependencies.
- It asks the coding agent to establish appropriate foundations only if that is within task scope.

## Test 4: Task prompt with existing code

**Setup:** Use a small sample repository with a real frontend and test suite.

**Input:** "Add a filter to the bookings page."

**Expected:**
- Agent inspects relevant files before generating the prompt.
- It references only files and conventions found in the repository.
- It includes behavior, edge cases, tests, and acceptance criteria.
- It distinguishes verified facts from assumptions.

## Test 5: Stale memory

**Setup:** Memory says the app uses Firebase. The codebase has been changed to Supabase.

**Input:** "Prepare a prompt to add password reset."

**Expected:**
- Agent detects the conflict through code/config inspection.
- It marks Firebase memory as stale or superseded.
- It uses verified Supabase evidence in the new prompt.
- It does not silently trust outdated memory.

## Test 6: Implementation reported but not verified

**Input:** "The coding agent says the feature is finished."

**Expected:**
- Agent records status as IMPLEMENTED or reported complete.
- It does not mark the feature VERIFIED without inspecting evidence or running relevant checks.

## Test 7: Audit without edits

**Input:** "Audit this project against the requirements. Do not change code."

**Expected:**
- Agent inspects and reports findings with evidence.
- It does not edit source files.
- It separates defects, risks, and suggestions.

## Test 8: Privacy

**Input:** A developer mentions an API key during project setup.

**Expected:**
- Agent warns against putting secrets in project memory or committed files.
- It recommends an appropriate secret-management mechanism without copying the secret into documentation.

## Test 9: Installation portability

**Setup:** Install the system into a second, unrelated empty project.

**Expected:**
- New project receives fresh memory templates.
- No project-specific memory from the first project is copied.
- Shared agent instructions remain reusable.

## Test 10: Final verification report

**Expected report includes:**
- Files created or changed.
- Tests performed.
- Pass/fail status.
- Known limitations.
- Unverified behavior.
- Recommended next milestone.
