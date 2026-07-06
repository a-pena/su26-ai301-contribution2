# Contribution 2: Add confirmation before verifying game files from menu bar

**Contribution Number:** 2  
**Student:** Andrea Pena
**Issue:** [RimSort/RimSort #1735 — Add confirmation before verifying game files from menu bar](https://github.com/RimSort/RimSort/issues/1735)  
**Fork:** [a-pena/RimSort](https://github.com/a-pena/RimSort)  
**Status:** Phase I Complete

---

## Why I Chose This Issue

I chose this issue because it is a clear, user-facing improvement with a focused scope. The current behavior allows users to start the “Verify game files” action from the menu bar without a confirmation step, even though the process cannot be canceled once it starts. Adding a confirmation prompt would make the application safer and more user-friendly by preventing accidental actions.

This issue also matches my current learning goals because it gives me a chance to work with an existing open source desktop application and understand how UI actions are connected to application behavior. Since the issue is labeled as a good first issue, it seems like a realistic contribution for this phase of the program while still helping me practice reading an unfamiliar codebase, following project patterns, and making a meaningful improvement.

---

## Understanding the Issue

### Problem Description

RimSort currently lets users trigger the “Verify game files” action from the menu bar without asking for confirmation first. If a user clicks this option by mistake, the verification process begins immediately and cannot be canceled. This creates a frustrating user experience because the user does not have a chance to stop the action before it starts.

### Expected Behavior

When the user selects “Verify game files” from the menu bar, RimSort should show a confirmation dialog before starting the verification process. The dialog should clearly ask whether the user wants to continue. If the user confirms, verification should begin. If the user cancels, nothing should happen.

### Current Behavior

The “Verify game files” action starts immediately when selected from the menu bar. There is no confirmation prompt, and once the process starts, the user cannot cancel it.

### Affected Components

To be completed in Phase II after reviewing the codebase.

Likely affected areas may include:

- The menu bar action that triggers “Verify game files”
- The function or handler connected to the verification action
- Existing confirmation dialog or message box patterns in the RimSort UI

---

## Reproduction Process

### Environment Setup

To be completed in Phase II.

I will set up the RimSort development environment locally using the project documentation, then document any setup steps, issues, and solutions here.

### Steps to Reproduce

To be completed in Phase II.

Planned reproduction steps:

1. Run RimSort locally.
2. Open the menu bar option related to verifying game files.
3. Select “Verify game files.”
4. Observe whether the action starts immediately without a confirmation prompt.

### Reproduction Evidence

- **Commit showing reproduction:** To be completed in Phase II.
- **Screenshots/logs:** To be completed in Phase II, if applicable.
- **My findings:** To be completed in Phase II.

---

## Solution Approach

### Analysis

To be completed in Phase II after reproducing the issue and reviewing the relevant code.

### Proposed Solution

The likely solution is to add a confirmation dialog before the existing “Verify game files” process is triggered from the menu bar. If the user confirms, the app should continue with the existing verification behavior. If the user cancels, the app should return without starting verification.

### Implementation Plan

Using UMPIRE framework (adapted):

**Understand:**  
The problem is that a non-cancelable verification action can be started accidentally from the menu bar without user confirmation.

**Match:**  
To be completed in Phase II. I will look for similar confirmation dialogs or message box patterns already used in the RimSort codebase.

**Plan:**  
To be completed in Phase II after identifying the exact files and functions involved.

Initial possible plan:

1. Find the menu action that triggers “Verify game files.”
2. Identify the function or handler that starts the verification process.
3. Look for existing confirmation dialog patterns in the project.
4. Add a confirmation prompt before the verification process starts.
5. Ensure canceling the prompt prevents the verification process from running.
6. Manually test both confirm and cancel paths.

**Implement:**  
To be completed in Phase III.

**Review:**  
To be completed in Phase III.

Self-review checklist will include:

- The change follows existing RimSort UI patterns.
- The confirmation text is clear and user-friendly.
- The cancel path does not trigger verification.
- The confirm path preserves the original behavior.
- No unrelated files or formatting are changed.

**Evaluate:**  
To be completed in Phase III.

I will verify the change by manually testing that the confirmation dialog appears and that both confirm and cancel behave correctly.

---

## Testing Strategy

### Unit Tests

To be completed in Phase III if the project has relevant test patterns for this UI behavior.

- [ ] Test case 1: Confirmation accepted starts verification.
- [ ] Test case 2: Confirmation canceled does not start verification.
- [ ] Test case 3: Existing verification behavior remains unchanged after confirmation.

### Integration Tests

To be completed in Phase III if applicable.

- [ ] Integration scenario 1: Menu bar action opens confirmation dialog.
- [ ] Integration scenario 2: Canceling the dialog prevents the verification process.

### Manual Testing

To be completed in Phase III.

Planned manual testing:

1. Launch RimSort locally.
2. Select “Verify game files” from the menu bar.
3. Confirm that a dialog appears before verification starts.
4. Click cancel and confirm verification does not begin.
5. Select the action again, click confirm, and confirm verification begins as expected.

---

## Implementation Notes

### Week 1 Progress

For Phase I, I selected RimSort issue #1735 as my second contribution issue. I reviewed the issue, confirmed that the problem is clear and user-facing, commented on GitHub to express interest, claimed the issue in Slack, forked the RimSort repository, and created this Contribution README repository to track my work.

### Week 2 Progress

To be completed in Phase II.

### Code Changes

To be completed in Phase III.

- **Files modified:** To be determined.
- **Key commits:** To be added.
- **Approach decisions:** To be documented as implementation progresses.

---

## Pull Request

**PR Link:** To be added in Phase IV.

**PR Description:** To be drafted in Phase IV.

**Maintainer Feedback:**

- To be added when feedback is received.

**Status:** Not submitted yet.

---

## Learnings & Reflections

### Technical Skills Gained

To be completed as the contribution progresses.

### Challenges Overcome

To be completed as the contribution progresses.

### What I'd Do Differently Next Time

To be completed after the contribution is complete.

---

## Resources Used

- [RimSort issue #1735](https://github.com/RimSort/RimSort/issues/1735)
- [RimSort repository](https://github.com/RimSort/RimSort)
- [My RimSort fork](https://github.com/a-pena/RimSort)
