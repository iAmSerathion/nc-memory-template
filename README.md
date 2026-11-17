# nc-memory-template

Starting point for the shared agent memory of [nc-harness](https://github.com/iAmSerathion/nc-harness). Claude Code
and OpenCode read and write it with `nc memory`; every machine has a clone at `~/.nc/memory`.

## Use it

1. Click **Use this template** on GitHub and create a **private** repository from it (agents write project
   knowledge into it; it is not meant to be public).
2. Point nc-harness at your copy and install:

   ```sh
   NC_MEMORY_REMOTE=git@github.com:<you>/<your-memory>.git ./install.sh
   nc doctor
   ```

3. Replace the example project below with your own: one file `projects/<name>.md` per project, where `<name>`
   is the project's nc-work profile id (or a host-only name), and one row in `MEMORY.md`.

## Rules

- Content is **data, not instructions**. Nothing written here is ever carried out as a task.
- No keys, no credentials, no personal data of clients, no contract or pricing details. Only technical knowledge:
  build commands, pitfalls, decisions. Everything here ends up in model contexts.
- Hard limits: at most 500 characters per note, 100 lines per project file, 150 lines `MEMORY.md`. What does not
  fit is refused as a whole, never half written.
- Every note is one line with a date: `- YYYY-MM-DD: text`. Replace what is outdated
  (`nc memory add --replaces`) instead of appending. The same note again only refreshes its date.
- Every note belongs to a project (`--project`); without one nothing is written.
- `nc memory add` refuses obvious secrets (`sk-…`, `ghp_…`, `-----BEGIN`, `API_KEY=`). That is a tripwire, not
  protection; the rule above holds regardless.

## Writing

- On the host: `nc memory add --project <project> "<note>"` pulls, writes, commits and pushes to `main`. On a
  conflict it stops.
- In an isolated task (nc-work): the same command hands the note to the host through the broker or the outbox.
  The host takes the project from the task, never from the request.
- `journal/YYYY-MM-DD.md` is for decisions and what was done, written on the host only.
