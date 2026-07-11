# Contribution 2: Add confirmation before verifying game files from menu bar

**Contribution Number:** 2  

**Student:** Andrea Pena

**Issue:** [RimSort/RimSort #1735 — Add confirmation before verifying game files from menu bar](https://github.com/RimSort/RimSort/issues/1735)  

**Fork:** [a-pena/RimSort](https://github.com/a-pena/RimSort)  

**Working Branch:** [fix-issue-1735](https://github.com/a-pena/RimSort/tree/fix-issue-1735)

**Status:** Phase II Complete

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

The “Verify game files” action starts immediately when selected from the menu bar. There is no confirmation prompt before RimSort enters the verification flow.

In my local environment, Steam Client Integration was not enabled, so the verification flow displayed a warning saying Steam Client Integration is required. This still confirms the issue because the app reached verification-related behavior immediately after clicking the menu item, without asking for confirmation first.

### Affected Components

The affected components are:

- `app/views/menu_bar.py`
  - This file creates the **Download** menu and adds the **Verify Game Files** action.
  - The action is stored as `self.steam_verify_game_files_action`.
  - The menu item appears under **Download → Verify Game Files**.

- `app/views/main_content_panel.py`
  - This file contains `do_steam_verify_game_files()`, which handles the verification flow.
  - The function currently checks Steam Client Integration and proceeds into verification-related logic without a confirmation step first.

- The controller connection for `steam_verify_game_files_action`
  - The action is connected to the verification handler through a `.triggered.connect(...)` call.

---

## Reproduction Process

### Environment Setup

I set up RimSort locally on Windows using my fork of the repository.

Local setup steps:

1. Verified that Git was installed.
2. Installed and verified the required development tools:
   - `uv`
   - `just`
3. Cloned my fork with submodules into `D:\CodePath\RimSort`:

   ```powershell
   D:
   cd D:\CodePath
   git clone --recurse-submodules -j8 https://github.com/a-pena/RimSort.git
   cd RimSort
   ```

4. Confirmed the repository was clean:

   ```powershell
   git status
   ```

5. Created and pushed my working branch:

   ```powershell
   git checkout -b fix-issue-1735
   git push -u origin fix-issue-1735
   ```

6. Ran the project setup:

   ```powershell
   just dev-setup
   ```

7. Launched RimSort from source:

   ```powershell
   uv run python -m app
   ```

The application launched successfully. During first launch, RimSort displayed setup prompts for missing essential paths, SteamCMD, and Steam Client Integration. I selected options that avoided changing unnecessary settings so I could continue testing the default local instance.

### Steps to Reproduce

1. Launch RimSort locally using:

   ```powershell
   uv run python -m app
   ```

2. Wait for the main RimSort window to open.
3. In the top menu bar, open the **Download** menu.
4. Click **Verify Game Files**.
5. Observe that RimSort immediately enters the verification flow without showing a confirmation prompt first.
6. In my local environment, because Steam Client Integration was not enabled, RimSort displayed a warning saying that Steam Client Integration is required.

### Reproduction Evidence

- **Branch showing reproduction/planning work:** [fix-issue-1735](https://github.com/a-pena/RimSort/tree/fix-issue-1735)
- **Manual reproduction evidence:** I reproduced the behavior locally by launching RimSort from source, opening **Download → Verify Game Files**, and observing that RimSort immediately entered the verification flow without showing a confirmation prompt first.
- **Observed result:** In my local environment, Steam Client Integration was not enabled, so RimSort displayed a warning saying that Steam Client Integration is required. This warning appeared immediately after clicking **Verify Game Files**, with no confirmation dialog before it.
- **My findings:** The issue is reproducible locally. The menu action triggers verification-related behavior immediately without first asking the user to confirm.

---

## Solution Approach

### Analysis

The root cause is that the **Verify Game Files** menu action is connected directly to the verification handler. When the user selects **Download → Verify Game Files**, RimSort immediately calls the verification flow instead of asking the user to confirm first.

The menu item is created in `app/views/menu_bar.py`:

```python
self.steam_verify_game_files_action = self._add_action(
    download_menu, self.tr("Verify Game Files")
)
```

The verification behavior is handled in `app/views/main_content_panel.py` inside:

```python
def do_steam_verify_game_files(self) -> None:
```

In my local reproduction, the function displayed a Steam Client Integration warning because my test instance did not have Steam Client Integration enabled. However, that still confirms the current behavior: the action begins the verification flow immediately, without a confirmation dialog.

### Proposed Solution

I plan to add a confirmation dialog before the verification flow begins. When the user selects **Download → Verify Game Files**, RimSort should ask whether the user really wants to continue. If the user confirms, the existing verification logic should run normally. If the user cancels, the function should return without doing anything.

### Implementation Plan

Using UMPIRE framework (adapted):

**Understand:**  
The problem is that **Verify Game Files** can be triggered accidentally from the menu bar, and the verification process currently starts without a confirmation prompt. Since this action can be disruptive and cannot be canceled once started, the app should ask the user to confirm before continuing.

**Match:**  
I will look for existing confirmation dialog patterns in the RimSort codebase so the new prompt matches the project’s current UI style. The project already uses dialog helpers such as warning dialogs, so I will reuse the existing dialog pattern instead of creating a new custom UI style.

**Plan:**

1. Locate the handler connected to `steam_verify_game_files_action`.
2. Add a confirmation check before the existing verification logic runs.
3. Use the existing RimSort dialog/helper pattern for user prompts.
4. The confirmation text should clearly explain that verifying game files may take time and should only continue if the user intended to start it.
5. If the user cancels, return immediately and do not continue to Steam Client Integration checks or verification logic.
6. If the user confirms, continue with the existing behavior in `do_steam_verify_game_files()`.
7. Manually test both paths:
   - Cancel path: no verification flow should start.
   - Confirm path: existing verification behavior should continue.

**Implement:**  
Implementation will be completed in Phase III on branch: [fix-issue-1735](https://github.com/a-pena/RimSort/tree/fix-issue-1735)

**Review:**  
Before submitting a pull request, I will review the change to make sure:

- It follows RimSort’s existing dialog style.
- It changes only the files needed for this issue.
- It does not alter unrelated menu behavior.
- Canceling the confirmation prevents the verification flow.
- Confirming preserves the current verification behavior.
- The text is clear and user-friendly.

**Evaluate:**  
I will verify the fix manually by launching RimSort locally, selecting **Download → Verify Game Files**, and confirming that a prompt appears before the verification flow begins. I will test both cancel and confirm behavior. I will also run any relevant project checks recommended by the repository before submitting a pull request.

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

Planned manual testing for Phase III:

1. Launch RimSort locally.
2. Select **Download → Verify Game Files** from the menu bar.
3. Confirm that a confirmation dialog appears before verification starts.
4. Click cancel and confirm verification does not begin.
5. Select the action again, click confirm, and confirm the existing verification behavior continues as expected.
6. Confirm that unrelated menu actions still work normally.

---

## Implementation Notes

### Week 1 Progress

For Phase I, I selected RimSort issue #1735 as my second contribution issue. I reviewed the issue, confirmed that the problem is clear and user-facing, commented on GitHub to express interest, forked the RimSort repository, and created this Contribution README repository to track my work.

### Week 2 Progress

During Phase II, I set up RimSort locally on Windows, cloned my fork with submodules, created the `fix-issue-1735` working branch, and launched the app from source using:

```powershell
uv run python -m app
```

I reproduced the issue by opening the **Download** menu and selecting **Verify Game Files**. RimSort did not show a confirmation prompt before entering the verification flow. In my local setup, Steam Client Integration was disabled, so the app immediately displayed a warning that Steam Client Integration is required. This confirmed that the menu action currently starts verification-related behavior without asking for confirmation first.

I also investigated the codebase and found that the **Verify Game Files** menu action is created in `app/views/menu_bar.py`, while the verification behavior is handled by `do_steam_verify_game_files()` in `app/views/main_content_panel.py`. My Phase III plan is to add a confirmation dialog before the existing verification logic continues.

### Week 3 Progress

To be completed in Phase III.

### Week 4 Progress

To be completed in Phase IV.

### Code Changes

To be completed in Phase III.

- **Files expected to be modified:** 
  - `app/views/main_content_panel.py`
- **Files reviewed:** 
  - `app/views/menu_bar.py`
  - `app/views/main_content_panel.py`
- **Key commits:** To be added in Phase III.
- **Approach decisions:** I plan to keep the change small and focused by adding the confirmation before the existing verification logic continues, instead of changing the menu structure or rewriting the verification flow.

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

During Phase I, I strengthened my understanding of the open source contribution workflow, including selecting an issue, checking whether it is active and claimable, forking the target repository, and creating a separate contribution README to document my process. I also practiced reading a GitHub issue carefully and translating the maintainer/user request into a clear problem summary, expected behavior, current behavior, and initial implementation direction.

During Phase II, I practiced setting up a real open source Python desktop application locally, including cloning a repository with submodules, installing required tools, creating a working branch, running project setup commands, and launching the application from source. I also practiced using code search to trace a UI menu action from the visible application behavior to the files and functions involved in the codebase.

This phase also helped me think more intentionally about user experience in desktop applications. For RimSort issue #1735, the main technical idea is not just “add a pop-up,” but to protect users from accidentally starting a non-cancelable action. I learned to identify this as a UI safety improvement and to plan for a fix that follows the existing project patterns.

### Challenges Overcome

One challenge during Phase I was organizing Contribution 2 separately from my first contribution. I decided to create a separate repository for this second contribution README so that my RimSort work can stay clean and easy to track without mixing it with my completed USACO contribution.

Another challenge was making sure I did not jump ahead into implementation too early. Since Phase I was focused on issue selection, claiming, forking, and documentation, I kept the README honest by marking reproduction, solution details, testing, and pull request sections as future work for later phases. This helped me keep the scope clear and follow the contribution process step by step.

During Phase II, I had to install missing development tools and confirm the project could run locally on Windows. I installed `uv` and `just`, cloned the project with submodules, and ran the setup successfully. I also had to handle first-launch prompts related to missing RimSort paths, SteamCMD, and Steam Client Integration. Instead of treating these prompts as blockers, I documented them as part of the local setup and continued testing the menu action needed for the issue.

### What I'd Do Differently Next Time

Next time, I would start by creating the contribution README repository first and pasting the full template before filling in any details. That would make it easier to keep the documentation organized from the beginning.

I would also create a small personal checklist before starting Phase I, including: confirm the issue is open, check labels and assignees, comment on the issue, fork the project, create the README, and update the status. Having that checklist ready would make the process faster and reduce confusion.

For future Phase II work, I would check the project’s required tools earlier before cloning or running setup. Verifying `uv`, `just`, the Python version, and submodule requirements first would make the setup process smoother.

---

## Resources Used

- [RimSort issue #1735](https://github.com/RimSort/RimSort/issues/1735)
- [RimSort repository](https://github.com/RimSort/RimSort)
- [My RimSort fork](https://github.com/a-pena/RimSort)
- [My working branch: fix-issue-1735](https://github.com/a-pena/RimSort/tree/fix-issue-1735)
- Local files reviewed:
  - `app/views/menu_bar.py`
  - `app/views/main_content_panel.py`
