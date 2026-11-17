# Opportunity Discovery

Collect public internship, research, program and event listings, screen them
against your interests, and track applications in a local dashboard. It's meant
for undergraduate students. Your own AI agent (Claude Code or Codex) builds your
profile, looks into promising leads and helps with drafts. The app itself
doesn't call an AI service or submit anything.

## Install

Opportunity Discovery installs with [Student Workspace](https://github.com/lai-derek/student-workspace).
You need Windows, Python 3.14 or newer (setup can install it), and an AI agent
with file, command and web access.

1. Download [Student-Tools-Setup.zip](https://github.com/lai-derek/student-workspace/releases/latest/download/Student-Tools-Setup.zip), extract it and run `Install.cmd`.
2. Choose Opportunity Discovery in the wizard. You can also turn on daily
   collection at 7:30 a.m.; it's off unless you check it.
3. Follow [set up your profile and first board](docs/AI_SETUP.md), then
   double-click **Open Dashboard.cmd** in your workspace. The dashboard is
   empty until the first collection, which takes about 10 minutes.

To update, download the latest setup ZIP and run it on the same workspace.

## More

- [Collection on Windows](docs/OPERATIONS_WINDOWS.md): where data lives, manual and daily runs.
- [Troubleshooting](docs/TROUBLESHOOTING.md).
- [Review commands](docs/PRIVATE_WORKSPACE_OPERATIONS.md) and [integration contract](docs/INTEGRATION_CONTRACT.md), mostly for your AI agent.
- [Architecture](docs/ARCHITECTURE.md), [data model](docs/DATA_MODEL.md), [adapters](docs/ADAPTERS.md).
- Contributors: read `AGENTS.md`; current checks are in [docs/VALIDATION.md](docs/VALIDATION.md).

MIT license; see `LICENSE`.
