# Homework 2 — Part 2 Submission

Student name: Riyaj Uddin

GitHub username: rizzUmahadi

## 1. Git Command Observations

| Command or workflow | What did you observe? | What was the user trying to accomplish? | What problem or risk did it address? |
|---|---|---|---|
| 1. `git status` | Listed which files were modified but not yet selected for the next save point, which were already selected, and which were untracked. Nothing about the project changed. | Get an accurate picture of how the working copy differs from the last saved version before doing anything irreversible. | Relying on memory, which can lead to forgetting a change or believing something was saved when it wasn't. |
| 2. `git diff` then `git add <file>` then `git diff --staged` | `git diff` showed the exact lines changed in unselected files. After `git add`, those lines moved from `git diff` to `git diff --staged`. | Review the actual content of a change and deliberately choose what belongs in the next saved version. | Accidentally saving debug code, unfinished work, or unrelated changes without ever reviewing them. |
| 3. `git commit -m "<message>"` | The selected changes were written into the project's history as one labeled version with an ID, author, and timestamp. | Preserve a meaningful version of the project and record why it was made. | Losing work, or having a saved version with no explanation of its purpose. |
| 4. `git log --oneline -3` | A short list of the three most recent saved versions appeared, each with an ID and message. | Reconstruct recent project history and find a specific earlier version. | Not knowing what has changed recently or which version is safe to return to. |

## 2. User Needs

### UN-GIT-01 — Awareness of uncommitted work

> A student developer needs a way to know exactly which parts of the project differ from the last saved version, because relying on memory leads to forgotten or unintended changes.

### UN-GIT-02 — Deliberate selection of work to save

> A student developer needs a way to review changes in detail and choose only the ones that belong together before saving them, because unrelated or unfinished work saved alongside real work is hard to trust or undo later.

### UN-GIT-03 — Durable, explained versions of the project

> A student developer needs a way to save a complete version of the project along with an explanation of its purpose, because unsaved work can be lost, and an unexplained version cannot be evaluated later.

## 3. User Requirements

| ID and short title | User requirement | Source user need | Rationale |
|---|---|---|---|
| UR-GIT-01 — Report differences from last version | A student developer shall be able to obtain a summary of all files added, modified, or deleted since the last saved version, without altering the project. | UN-GIT-01 | Gives the accurate picture UN-GIT-01 requires, without risk of accidentally changing anything by checking. |
| UR-GIT-02 — Inspect changes in detail | A student developer shall be able to view the specific lines changed in any modified file before that file is saved. | UN-GIT-01, UN-GIT-02 | A file-level summary isn't enough to judge correctness; the user must see actual content to decide. |
| UR-GIT-03 — Select work for the next version | A student developer shall be able to choose which current changes are included in the next saved version, and revise that choice before saving. | UN-GIT-02 | Directly provides the deliberate selection UN-GIT-02 describes. |
| UR-GIT-04 — Save a labeled version | A student developer shall be able to save the selected changes as one identified version with a description they write, provided at least one change was selected. | UN-GIT-03 | Satisfies both the durability and the explanation UN-GIT-03 calls for. |