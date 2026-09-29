## Approved UN/UR Baseline

# Approved MiniGit user needs and user requirements

## Project boundary

MiniGit is a local educational version-control tool. It supports `init`, `status`, `diff`, `diff --staged`, `add <file>`, `commit -m <message>`, and `log`. It does not implement branches, merging, network operations, GitHub, or Pull Requests. Students use real Git/GitHub for class collaboration.

## User needs

| ID | Stakeholder need |
| --- | --- |
| UN-GIT-01 | A student developer needs a way to start tracking a local project because it has no recorded history. |
| UN-GIT-02 | A student developer needs to know which project files have changed because they may forget what they edited before recording a checkpoint. |
| UN-GIT-03 | A student developer needs to inspect changed content before recording it because a file may contain unintended edits. |
| UN-GIT-04 | A student developer needs to choose the file content to include in the next checkpoint because later edits may still be unfinished. |
| UN-GIT-05 | A student developer needs to record a meaningful checkpoint because they want to preserve a known project state and explain its purpose. |
| UN-GIT-06 | A student developer needs to review earlier checkpoints because they want to understand how the project reached its current state. |
| UN-GIT-07 | A student developer needs invalid commands to explain why they failed while preserving existing project files and recorded checkpoints. |

## User requirements

| ID | User-visible capability | Need |
| --- | --- | --- |
| UR-GIT-01 | A student developer shall be able to initialize tracking in the current local project folder without removing existing project files. | UN-GIT-01, UN-GIT-07 |
| UR-GIT-02 | A student developer shall be able to see whether project files are untracked, staged, changed after staging, modified, deleted, or clean. | UN-GIT-02 |
| UR-GIT-03 | A student developer shall be able to view differences between current working file content and the content selected for the next checkpoint. | UN-GIT-03 |
| UR-GIT-04 | A student developer shall be able to view differences between content selected for the next checkpoint and the latest recorded checkpoint. | UN-GIT-03 |
| UR-GIT-05 | A student developer shall be able to select the current content of one existing project file for the next checkpoint without selecting unrelated files. | UN-GIT-04 |
| UR-GIT-06 | A student developer shall be able to create a checkpoint of selected content with a nonempty explanation while leaving later unselected edits in the working files. | UN-GIT-05, UN-GIT-04 |
| UR-GIT-07 | A student developer shall be able to view recorded checkpoints from newest to oldest, including their identifier and explanation. | UN-GIT-06 |
| UR-GIT-08 | A student developer shall receive a useful error when a command is invalid, a requested file is unavailable, or a path is outside the allowed project files. | UN-GIT-07 |
| UR-GIT-09 | A student developer shall be able to retry an operation after a failure without losing ordinary project files or an already recorded checkpoint. | UN-GIT-07 |

## UR-to-UN Mapping

* UR-GIT-01 → UN-GIT-01, UN-GIT-07
* UR-GIT-02 → UN-GIT-02
* UR-GIT-03 → UN-GIT-03
* UR-GIT-04 → UN-GIT-03
* UR-GIT-05 → UN-GIT-04
* UR-GIT-06 → UN-GIT-05, UN-GIT-04
* UR-GIT-07 → UN-GIT-06
* UR-GIT-08 → UN-GIT-07
* UR-GIT-09 → UN-GIT-07

### Explanation of UR-GIT-06 (from Lab 4 E3.3)

* **Connection to UN-GIT-05:** UR-GIT-06 addresses UN-GIT-05 by allowing the student to create a checkpoint of selected content with a nonempty explanation, preserving a known project state and explaining its purpose.
* **Connection to UN-GIT-04:** UR-GIT-06 addresses UN-GIT-04 by allowing the student to create a checkpoint from selected content while leaving later unselected edits in the working files.
## Functional System Requirements

### SR-01 — First init

**Source UR:** UR-GIT-01

**Starting condition:** An uninitialized project containing existing project files.

**Action:** The student uses `init`.

**System requirement:** Given an uninitialized project containing existing project files, when `init` is used, MiniGit shall initialize tracking for the current local project folder without removing the existing project files.

**Check:** Inspect the existing project files after `init` and confirm that they are still present.

### SR-02 — Repeated init

**Source UR:** UR-GIT-08, UR-GIT-09

**Starting condition:** An already initialized project with existing project files and any previously recorded checkpoint.

**Action:** The student uses `init` again.

**System requirement:** Given an already initialized project, when `init` is used again, MiniGit shall report a useful error and leave existing project files and any already recorded checkpoint unchanged.

**Check:** Inspect the error output, existing project files, and any recorded checkpoint to confirm they remain unchanged.

### SR-03 — Add one existing file

**Source UR:** UR-GIT-05

**Starting condition:** An initialized project with `notes.txt` containing `ONE` and `plan.txt` present.

**Action:** The student uses `add notes.txt`.

**System requirement:** Given an initialized project with `notes.txt` containing `ONE` and `plan.txt` present, when `add notes.txt` is used, MiniGit shall stage a copy of `notes.txt` containing `ONE` without staging `plan.txt`.

**Check:** Inspect the stage and status to confirm that `notes.txt` is staged with `ONE` and `plan.txt` is not staged.

### SR-04 — Add a missing file

**Source UR:** UR-GIT-08, UR-GIT-09

**Starting condition:** An initialized project with `notes.txt` already staged as `ONE` and no file named `missing.txt`.

**Action:** The student uses `add missing.txt`.

**System requirement:** Given an initialized project with `notes.txt` already staged as `ONE` and no file named `missing.txt`, when `add missing.txt` is used, MiniGit shall report a useful error indicating that `missing.txt` is unavailable and shall leave the staged `notes.txt` content as `ONE`.

**Check:** Inspect the error output and staged content to confirm that `missing.txt` caused an error and `notes.txt` remains staged as `ONE`.

### SR-05 — Status for one staged file

**Source UR:** UR-GIT-02

**Starting condition:** An initialized project with `notes.txt` staged.

**Action:** The student uses `status`.

**System requirement:** Given an initialized project with `notes.txt` staged, when `status` is used, MiniGit shall report `notes.txt` as staged.

**Check:** Inspect the `status` output and confirm that `notes.txt` is reported as staged.