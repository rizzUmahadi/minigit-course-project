# Homework 3 — System Requirements and Verification

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

- UR-GIT-01 → UN-GIT-01, UN-GIT-07
- UR-GIT-02 → UN-GIT-02
- UR-GIT-03 → UN-GIT-03
- UR-GIT-04 → UN-GIT-03
- UR-GIT-05 → UN-GIT-04
- UR-GIT-06 → UN-GIT-05, UN-GIT-04
- UR-GIT-07 → UN-GIT-06
- UR-GIT-08 → UN-GIT-07
- UR-GIT-09 → UN-GIT-07

## Functional System Requirements

### SR-01 — First initialization

**Source UR:** UR-GIT-01

**Starting condition:** An uninitialized project contains existing project files.

**Action:** The student uses `init`.

**System requirement:** Given an uninitialized project containing existing project files, when `init` is used, MiniGit shall initialize tracking for the current local project folder without removing the existing project files.

**Check:** Inspect the existing project files after `init` and confirm that they are still present.

---

### SR-02 — Repeated initialization

**Source UR:** UR-GIT-08, UR-GIT-09

**Starting condition:** An already initialized project contains existing project files and may contain a previously recorded checkpoint.

**Action:** The student uses `init` again.

**System requirement:** Given an already initialized project, when `init` is used again, MiniGit shall report a useful error and leave existing project files and any already recorded checkpoint unchanged.

**Check:** Inspect the error output, existing project files, and any recorded checkpoint and confirm that they remain unchanged.

---

### SR-03 — Add one existing file

**Source UR:** UR-GIT-05

**Starting condition:** An initialized project contains `notes.txt` with content `ONE` and `plan.txt` is also present.

**Action:** The student uses `add notes.txt`.

**System requirement:** Given an initialized project with `notes.txt` containing `ONE` and `plan.txt` present, when `add notes.txt` is used, MiniGit shall select the current content of `notes.txt` for the next checkpoint without selecting `plan.txt`.

**Check:** Inspect the selected content and status and confirm that `notes.txt` containing `ONE` is selected and `plan.txt` is not selected.

---

### SR-04 — Add a missing file

**Source UR:** UR-GIT-08, UR-GIT-09

**Starting condition:** An initialized project has `notes.txt` already selected as `ONE` and no file named `missing.txt` exists.

**Action:** The student uses `add missing.txt`.

**System requirement:** Given an initialized project with `notes.txt` already selected as `ONE` and no file named `missing.txt`, when `add missing.txt` is used, MiniGit shall report a useful error indicating that `missing.txt` is unavailable and shall leave the selected `notes.txt` content as `ONE`.

**Check:** Inspect the error output and selected content and confirm that `missing.txt` caused an error and `notes.txt` remains selected as `ONE`.

---

### SR-05 — Report file status

**Source UR:** UR-GIT-02

**Starting condition:** An initialized project contains files that may be untracked, staged, changed after staging, modified, deleted, or clean.

**Action:** The student uses `status`.

**System requirement:** Given an initialized project containing files in the supported file states, when `status` is used, MiniGit shall report whether each applicable project file is untracked, staged, changed after staging, modified, deleted, or clean.

**Check:** Create examples of the supported file states and inspect the `status` output to confirm that each file is reported with the corresponding state.

---

### SR-06 — Working content versus selected content

**Source UR:** UR-GIT-03

**Starting condition:** An initialized project has `notes.txt` selected with content `ONE`, while the current working `notes.txt` contains `TWO`.

**Action:** The student uses `diff`.

**System requirement:** Given an initialized project with `notes.txt` selected as `ONE` and the current working `notes.txt` containing `TWO`, when `diff` is used, MiniGit shall show the whole-file difference between the selected content `ONE` and the current working content `TWO` without changing either content.

**Check:** Inspect the `diff` output and confirm that it represents the difference between selected `ONE` and working `TWO`, then confirm that both contents remain unchanged.

---

### SR-07 — Selected content versus latest checkpoint

**Source UR:** UR-GIT-04

**Starting condition:** An initialized project has a latest recorded checkpoint containing `notes.txt` as `ONE`, while the selected content for the next checkpoint contains `TWO`.

**Action:** The student uses `diff --staged`.

**System requirement:** Given an initialized project with the latest recorded checkpoint containing `notes.txt` as `ONE` and selected content containing `TWO`, when `diff --staged` is used, MiniGit shall show the whole-file difference between the latest recorded checkpoint content `ONE` and the selected content `TWO` without changing either content.

**Check:** Inspect the `diff --staged` output and confirm that it represents the difference between the latest checkpoint `ONE` and selected content `TWO`, then confirm that both contents remain unchanged.

---

### SR-08 — Create a full checkpoint

**Source UR:** UR-GIT-06

**Starting condition:** An initialized project has selected content and a nonempty checkpoint explanation.

**Action:** The student uses `commit -m "save notes"`.

**System requirement:** Given an initialized project with selected content and a nonempty explanation, when `commit -m "save notes"` is used, MiniGit shall record a new full-project checkpoint with the selected content, the exact explanation `save notes`, a numbered identifier, and a single parent link when a previous checkpoint exists.

**Check:** Inspect the recorded checkpoint and confirm its numbered identifier, exact explanation, complete project snapshot, and parent relationship.

---

### SR-09 — Preserve later working edits after commit

**Source UR:** UR-GIT-06

**Starting condition:** An initialized project has selected content `ONE` for `notes.txt`, while the current working `notes.txt` has later content `TWO`.

**Action:** The student uses `commit -m "save selected notes"`.

**System requirement:** Given an initialized project with selected `notes.txt` content `ONE` and later working content `TWO`, when `commit -m "save selected notes"` is used, MiniGit shall record `ONE` in the new checkpoint while leaving the later working content `TWO` unchanged.

**Check:** Inspect the new checkpoint and the working `notes.txt` after the commit and confirm that the checkpoint contains `ONE` while the working file still contains `TWO`.

---

### SR-10 — Reject a path outside the allowed project files

**Source UR:** UR-GIT-08, UR-GIT-09

**Starting condition:** An initialized project contains its allowed project files, and the requested path is outside the allowed project files.

**Action:** The student uses `add` with the outside path.

**System requirement:** Given an initialized project, when `add` is used with a path outside the allowed project files, MiniGit shall report a useful error identifying the invalid path and shall leave ordinary project files and any already recorded checkpoint unchanged.

**Check:** Inspect the error output and confirm that the ordinary project files and any already recorded checkpoint remain unchanged.

---

### SR-11 — Reject an empty commit explanation

**Source UR:** UR-GIT-08, UR-GIT-09

**Starting condition:** An initialized project has selected content and an existing latest checkpoint.

**Action:** The student uses `commit -m ""`.

**System requirement:** Given an initialized project with selected content and an existing latest checkpoint, when `commit -m ""` is used, MiniGit shall report a useful error for the empty explanation and shall leave the selected content and latest recorded checkpoint unchanged.

**Check:** Inspect the error output, selected content, and latest checkpoint and confirm that they remain unchanged.

---

### SR-12 — View checkpoint history

**Source UR:** UR-GIT-07

**Starting condition:** An initialized project contains zero or more recorded checkpoints.

**Action:** The student uses `log`.

**System requirement:** Given an initialized project with zero or more recorded checkpoints, when `log` is used, MiniGit shall report the recorded checkpoints from newest to oldest, including each checkpoint's identifier and explanation, or report that no checkpoint history is available when none exists.

**Check:** Run `log` in a project with no checkpoints and in a project with multiple checkpoints. Confirm the empty-history behavior and the newest-to-oldest order, identifiers, and explanations.

---

# Acceptance Tests

## AT-01 — Initialize an existing project

**Given:** A local project contains existing files and has not been initialized.

**When:** The student runs `init`.

**Then:** MiniGit initializes tracking and all existing project files remain present.

**Verifies:** SR-01

---

## AT-02 — Reject repeated initialization

**Given:** The project is already initialized.

**When:** The student runs `init` again.

**Then:** MiniGit reports an error and existing project files and any recorded checkpoint remain unchanged.

**Verifies:** SR-02

---

## AT-03 — Select one existing file

**Given:** `notes.txt` contains `ONE` and `plan.txt` exists.

**When:** The student runs `add notes.txt`.

**Then:** `notes.txt` containing `ONE` is selected and `plan.txt` is not selected.

**Verifies:** SR-03

---

## AT-04 — Reject a missing file

**Given:** `notes.txt` is selected as `ONE` and `missing.txt` does not exist.

**When:** The student runs `add missing.txt`.

**Then:** MiniGit reports an error and the selected `notes.txt` content remains `ONE`.

**Verifies:** SR-04

---

## AT-05 — Report file states

**Given:** The project contains examples of the supported file states.

**When:** The student runs `status`.

**Then:** MiniGit reports the applicable state of each file as untracked, staged, changed after staging, modified, deleted, or clean.

**Verifies:** SR-05

---

## AT-06 — Compare working and selected content

**Given:** Selected `notes.txt` contains `ONE` and working `notes.txt` contains `TWO`.

**When:** The student runs `diff`.

**Then:** MiniGit shows the whole-file difference between `ONE` and `TWO` without changing either content.

**Verifies:** SR-06

---

## AT-07 — Compare selected content and latest checkpoint

**Given:** The latest checkpoint contains `ONE` and selected content contains `TWO`.

**When:** The student runs `diff --staged`.

**Then:** MiniGit shows the whole-file difference between `ONE` and `TWO` without changing either content.

**Verifies:** SR-07

---

## AT-08 — Create a checkpoint

**Given:** Selected content exists and the explanation is `save notes`.

**When:** The student runs `commit -m "save notes"`.

**Then:** MiniGit records a new full-project checkpoint with the exact explanation, numbered identifier, and appropriate parent link.

**Verifies:** SR-08

---

## AT-09 — Preserve later working edits

**Given:** Selected `notes.txt` contains `ONE` and working `notes.txt` contains later content `TWO`.

**When:** The student commits the selected content.

**Then:** The checkpoint contains `ONE` and the working file remains `TWO`.

**Verifies:** SR-09

---

## AT-10 — Reject an outside path

**Given:** The requested path is outside the allowed project files.

**When:** The student runs `add` with that path.

**Then:** MiniGit reports an error and does not change ordinary project files or an existing checkpoint.

**Verifies:** SR-10

---

## AT-11 — Reject an empty explanation

**Given:** Selected content and an existing checkpoint are present.

**When:** The student runs `commit -m ""`.

**Then:** MiniGit reports an error and leaves the selected content and existing checkpoint unchanged.

**Verifies:** SR-11

---

## AT-12 — View checkpoint history

**Given:** The project has either no checkpoints or multiple checkpoints.

**When:** The student runs `log`.

**Then:** MiniGit reports that no history is available when there are no checkpoints, or lists checkpoints from newest to oldest with their identifiers and explanations.

**Verifies:** SR-12

---

# Verification Methods

| SR | Verification method |
| --- | --- |
| SR-01 | Inspection of existing project files before and after `init`. |
| SR-02 | Command-output inspection plus comparison of project files and checkpoint state before and after the rejected command. |
| SR-03 | Status/selected-content inspection after `add notes.txt`. |
| SR-04 | Error-output inspection plus selected-content comparison before and after the failed `add`. |
| SR-05 | Controlled status-state tests covering untracked, staged, changed after staging, modified, deleted, and clean files. |
| SR-06 | Compare the `diff` output with known selected and working file contents. |
| SR-07 | Compare the `diff --staged` output with known checkpoint and selected contents. |
| SR-08 | Inspect the new checkpoint identifier, explanation, full snapshot, and parent relationship. |
| SR-09 | Compare the checkpoint content with the working file after commit. |
| SR-10 | Inspect the error output and compare ordinary project files and checkpoint state before and after the rejected command. |
| SR-11 | Inspect the error output and compare selected content and checkpoint state before and after the rejected command. |
| SR-12 | Run `log` with no checkpoints and with multiple checkpoints and inspect the resulting history. |

---

# Traceability Matrix

| System Requirement | Source UR | Acceptance Test | Verification Method |
| --- | --- | --- | --- |
| SR-01 | UR-GIT-01 | AT-01 | Inspection |
| SR-02 | UR-GIT-08, UR-GIT-09 | AT-02 | Error/state inspection |
| SR-03 | UR-GIT-05 | AT-03 | Selected-content/status inspection |
| SR-04 | UR-GIT-08, UR-GIT-09 | AT-04 | Error/state inspection |
| SR-05 | UR-GIT-02 | AT-05 | Controlled status tests |
| SR-06 | UR-GIT-03 | AT-06 | Difference comparison |
| SR-07 | UR-GIT-04 | AT-07 | Difference comparison |
| SR-08 | UR-GIT-06 | AT-08 | Checkpoint inspection |
| SR-09 | UR-GIT-06 | AT-09 | Snapshot/working-file comparison |
| SR-10 | UR-GIT-08, UR-GIT-09 | AT-10 | Error/state inspection |
| SR-11 | UR-GIT-08, UR-GIT-09 | AT-11 | Error/state inspection |
| SR-12 | UR-GIT-07 | AT-12 | History inspection |

## UR Coverage

| User Requirement | Supporting System Requirements |
| --- | --- |
| UR-GIT-01 | SR-01 |
| UR-GIT-02 | SR-05 |
| UR-GIT-03 | SR-06 |
| UR-GIT-04 | SR-07 |
| UR-GIT-05 | SR-03 |
| UR-GIT-06 | SR-08, SR-09 |
| UR-GIT-07 | SR-12 |
| UR-GIT-08 | SR-02, SR-04, SR-10, SR-11 |
| UR-GIT-09 | SR-02, SR-04, SR-10, SR-11 |

## Command Coverage

| MiniGit command | Supporting System Requirements |
| --- | --- |
| `init` | SR-01, SR-02 |
| `status` | SR-05 |
| `diff` | SR-06 |
| `diff --staged` | SR-07 |
| `add <file>` | SR-03, SR-04, SR-10 |
| `commit -m <message>` | SR-08, SR-09, SR-11 |
| `log` | SR-12 |

## HW3 Completion Check

- [x] Approved UN baseline included
- [x] Approved UR baseline included
- [x] UR-to-UN mapping included
- [x] 12 system requirements included
- [x] All 9 URs covered
- [x] All 7 required MiniGit command forms covered
- [x] Each SR has a source UR
- [x] Each SR has a starting condition
- [x] Each SR has an action
- [x] Each SR has an observable system requirement
- [x] Each SR has a Check line
- [x] Acceptance tests included
- [x] Verification methods included
- [x] Traceability matrix included