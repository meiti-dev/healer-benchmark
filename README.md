# Healer benchmark: real bugs from BugsInPy

[Healer](https://meiti.dev/healer) repairs code automatically: it runs the project's checks and tests, fixes what fails, and only returns a version that is repaired or unchanged, never worse. This repository shows how it does on **real bugs from real open-source projects**, taken from [BugsInPy](https://github.com/soarsmu/BugsInPy).

## Result

**6 of 11 bugs fixed** (55%). 3 more were excluded because the bug cannot be reproduced with the information BugsInPy provides (see below).

| Project | Fixed |
|---|---|
| thefuck | 6 of 8 |
| youtube-dl | 0 of 3 |

## Method

For each bug:

1. The project is checked out at the **buggy commit**, and the **tests from the fixed commit** are placed on top, so the tests describe the correct behavior.
2. Healer receives that code and the name of the failing tests (in `.healer.json`). It **never sees the developer's fix**.
3. A bug counts as **fixed** when Healer returns the code with those tests passing and its own checks green (`production_ready`).
4. Each folder in `results/` has Healer's patch (`healer.diff`) next to the developer's patch (`developer.diff`), so anyone can compare them.

The dependencies BugsInPy installs for a bug (its `setup.sh`) are passed to Healer as test requirements. Nothing is tuned per bug.

## All bugs

| Bug | Result | Healer iterations | Patches |
|---|---|---|---|
| PySnooper/1 | excluded | 2 | [results/PySnooper-1](results/PySnooper-1) |
| PySnooper/2 | excluded | 2 | [results/PySnooper-2](results/PySnooper-2) |
| PySnooper/3 | excluded | 2 | [results/PySnooper-3](results/PySnooper-3) |
| thefuck/1 | fixed | 2 | [results/thefuck-1](results/thefuck-1) |
| thefuck/2 | fixed | 2 | [results/thefuck-2](results/thefuck-2) |
| thefuck/3 | not fixed | 8 | [results/thefuck-3](results/thefuck-3) |
| thefuck/4 | fixed | 4 | [results/thefuck-4](results/thefuck-4) |
| thefuck/5 | fixed | 6 | [results/thefuck-5](results/thefuck-5) |
| thefuck/6 | fixed | 5 | [results/thefuck-6](results/thefuck-6) |
| thefuck/7 | fixed | 5 | [results/thefuck-7](results/thefuck-7) |
| thefuck/8 | not fixed | 4 | [results/thefuck-8](results/thefuck-8) |
| youtube-dl/1 | not fixed | 7 | [results/youtube-dl-1](results/youtube-dl-1) |
| youtube-dl/2 | not fixed | 8 | [results/youtube-dl-2](results/youtube-dl-2) |
| youtube-dl/3 | not fixed | 8 | [results/youtube-dl-3](results/youtube-dl-3) |

## Excluded bugs

- **PySnooper/1**: BugsInPy does not declare the test dependency `python_toolbox` for this bug (empty requirements.txt, no install in setup.sh): the tests cannot even be collected.
- **PySnooper/2**: The fixed-commit test imports a helper module (`tests/mini_toolbox`) that does not exist at the buggy commit, so the test cannot be collected.
- **PySnooper/3**: The test needs `python_toolbox`, but BugsInPy only installs `decorator` for this bug: the tests cannot even be collected.

These are problems of the benchmark environment, not of the repair, so they are not counted either way.

## Limits

- This is a small first batch (14 bugs from 3 projects). More projects, and BugsJS for JavaScript, will be added.
- "Fixed" means the bug's tests pass and Healer's checks are green. It does not mean the patch is identical to the developer's: compare the diffs.

## Credits

The bugs, tests and developer patches come from [BugsInPy](https://github.com/soarsmu/BugsInPy) and from each project, under their own licenses: [thefuck](https://github.com/nvbn/thefuck) (MIT), [youtube-dl](https://github.com/ytdl-org/youtube-dl) (Unlicense) and [PySnooper](https://github.com/cool-RR/PySnooper) (MIT). Only the patches are included here, not the projects.
