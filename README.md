# claude-mods

A [Claude Code plugin marketplace](https://code.claude.com/docs/en/plugins) of mods that change Claude Code's own interface. One repo, one marketplace file, one folder per mod under `plugins/`.

| Mod | What it does |
| --- | --- |
| [`hivewatch`](plugins/hivewatch) | Live [Beadhive](https://github.com/beadhive/beadhive) status in a pane: in-flight beads, per-agent liveness, molecule progress. |

## Install

```
/plugin marketplace add SimplicityGuy/claude-mods
/plugin install hivewatch@claude-mods
```

## hivewatch

Run `/hivewatch` to open the pane (`/hivewatch refresh` to poll now, `/hivewatch off` to stop polling). The status line also shows `hive: 3 live · 1 stale`.

```
hivewatch · 12 in flight (8 live · 4 stale) · updated 3s ago
dispatched ████████████░░░░░░░░░░░░ 17/21 steps · 2 molecules
  phaze-oprdu    ████████████░░░░ 12/19 (2 running)  Remote tracklist providers
agents
● dev/mixesdb-provider   phaze-ttw7n    4h  dirty  MixesDB provider: REST search…
○ dev/rank               phaze-e5yyq    8d  none   Measure similar-set ranking…
```

- **Liveness is the bead's lease**, not the filesystem. `●` means the agent's lease on the bead is still valid, `○` means the bead is `in_progress` but the lease has expired (the agent stopped heartbeating), `·` means no lease was ever granted.
- **Dispatch progress** comes from `bh work readiness --json` for each in-progress epic (done steps over total steps).
- **Worktree state** (`dirty`, `none`) comes from `bh worktree status --json`.

### Settings

| Setting | Default | |
| --- | --- | --- |
| `hives` | empty | Comma-separated hive ids, for example `github/SimplicityGuy/phaze`. Empty watches the hive of the session's working directory. |
| `refreshSeconds` | `20` | Poll interval; each poll runs a few `bh` commands per hive, so keep it at 10 or more. |

### Not yet

- Test-run progress bars. Nothing in Beadhive reports test progress per bead today.
- A durable agent identity on each bead. `bd` records an assignee label such as `dev/rank`, not the Claude Code session or agent name, so the pane cannot yet join a bead to the live agent session.

## Developing

```
claude plugin validate plugins/hivewatch
claude plugin test plugins/hivewatch
claude --plugin-dir plugins/hivewatch      # try it in a session
```

MIT licensed.
