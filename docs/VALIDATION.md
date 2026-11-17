# Validation

Version 0.2.1, checked 2026-11-16 on native Windows 11 with Python 3.15.2.
0.2.1 corrects the version that `opdisc --version` and the collector's User-Agent
report (0.2.0 still said 0.1.0); nothing else changed.
Earlier validation history is in the Git history of this file.

## What changed in 0.2

- **Student Workspace.** Workspace setup, backup and application handoffs moved
  to [Student Workspace](https://github.com/lai-derek/student-workspace), which
  installs this tool. Collector data now lives in the workspace under
  `.student-workspace/opportunity-discovery/`.
- **Speed and storage.** Duplicate lookups use an index on `canonical_url`
  (migration 0007). Description extraction is cached for unchanged text
  (0008). Cached responses are compressed (0009). Each distinct observation is
  stored once (0010).
- **`opdisc prune --apply`** no longer fails with `KeyError`, and it keeps run
  rows that history still references.
- **Routing.**
  - On Greenhouse, Lever, Ashby, SmartRecruiters and Workday boards, job type
    comes from the title and the board's own job type.
  - The student route excludes non-internship roles that require two or more
    years of experience.
  - In sectioned community lists, rows in a "Program" or "Scholarship" table
    are programs.
  - Titles naming both degree levels ("Bachelor's/Master's") aren't
    graduate-only.
  - "Summer Analyst" titles count as internships.
  - Stored classifications the current rules no longer support reset to
    `unknown`.
  - The personal feed orders leads by the user's own interest order.

## Checks

| Check | Result |
| --- | --- |
| `python -m pytest -m "not live"` | 494 passed |
| `ruff check`, `ruff format --check`, `mypy` | clean |
| `opdisc audit .` | 0 errors, 0 warnings |
| Upgrade of a copy of a real collector database (31,561 opportunities, 90,669 observations) | migrations and backfills 8.9 s; 500/500 sampled observations read back identical; file 1,060 MB -> 663 MB after `VACUUM` |
| Replay of `greenhouse-spacex` (2,905 records) from cached responses | 224.8 s before; 27.6 s first run after upgrade; 4.7 s unchanged second run |
| Extraction cache | cached and uncached extraction identical for four large boards |
| Routing against 240 hand-labeled leads | estimated student-route precision 0.98 (0.91 before); recall of labeled student opportunities 0.99 (unchanged); non-student leads waiting for research ~10,600 -> ~4,200. Two labeled misses remain, and two incoming-freshman programs listed in a Program table are new false includes. Table labels were simulated from stored text |
| Stable IDs | identity keys and merge rules unchanged |
| Student Workspace install (this tool alone, and with Academic Planning) | setup created collector storage, `Open Dashboard.cmd` and the WORKSPACE.md section; `student-workspace check` exited 0; dashboard Home and Explore served |
| Screening inside a parent Git folder | allowed when nothing private is tracked; refused when private state is tracked |
| Live `validate-sources` in a disposable workspace (2026-11-12, before the speed and routing changes) | 240 validated, 4 valid-empty, 5 failed (4 HTTP 404, 1 response over the size cap), 25 quarantined |

After upgrading, the first collection re-extracts everything once to fill the
cache. The database file shrinks only after a manual `VACUUM`.

Not verified yet: a live collection with the routing changes, the daily Task
Scheduler registration (the command is unit-tested), and GitHub CI.
