# Improvement run log

Append-only record for the [improvement plan](IMPROVEMENT-PLAN.md). Add the
newest entry at the bottom. Never edit an old entry; correct it with a new one.

## Entry template

```text
### YYYY-MM-DD task <id>: <title>
Result: done | reverted | skipped | rejected | blocked
Pins: SunPad <sha>, ModernGekko <sha>, RecompCore <sha>, DolRecomp <sha>
Checkpoints: C1 pass/fail (detail); C2 <baseline> -> <candidate>, noise <x>%;
  C3 <device, route, median/worst speedRatio before -> after>; C4 <sizes, times>;
  C5 <who, scenes, verdict>
Evidence: <log or artifact paths outside Git, PR links>
Notes: <what was learned; why kept, reverted or stopped>
Next: <the next task or the question for a human>
```

## Measurements

Record each pin set's noise floor and the pass 2 ranking table here when they
exist.

## Ideas

Unmeasured ideas. Promote one to a task card only through the plan's
[candidate intake](IMPROVEMENT-PLAN.md#adding-new-work).

- Metal binary archives for known pipelines (from the performance research).
- Hot-region LLVM objects for measured chunks (from the performance research).
- Per-stage pipeline warmup during loads, from a recorded list of each stage's
  pipelines (research pass 1; sms-pc-port compiles 849 programs over 41 stages
  this way). Needs a first-moments hitch measurement on iPhone first.
- Audio after slow frames: check whether SunPad's audio path replays or drops
  blocks during sustained slowdowns, and whether a queue that grows after an
  underrun helps (research pass 1; see AUDIO_ISSUE.md).
- Heat-wave bypass that does not change code bytes, so it costs no chunk and
  can become its own setting (research pass 1).
- Button prompts that match the connected controller, using custom textures
  with original artwork (research pass 1; needs task 7.1).
- In-app "What's new" when releases resume (research pass 1).

## Entries

### 2026-10-07 plan: created
Result: done
Pins: SunPad c77dd61, ModernGekko 8f49c55, RecompCore da96175, DolRecomp fa0cf61
Checkpoints: documentation only
Evidence: [sms-pc-port review](SMS-PC-PORT-REVIEW.md)
Notes: Symbol map hash matches the extracted main.dol. The pinned DolRecomp
  already inlines the FP-enabled check and has an opt-in direct-call path that
  is off. ModernGekko supports code mods, but SunPad does not set
  mod_directories. Dolphin's GCI folder writes saves in place.
Next: task 1.1 and task 2.1 (independent).

### 2026-10-07 plan: agent entry points
Result: done
Pins: unchanged
Checkpoints: documentation only
Evidence: AGENTS.md, CLAUDE.md, plan "Start here" section
Notes: AGENTS.md now points agents to the plan; CLAUDE.md imports AGENTS.md so
  Claude-based agents get the same instructions. The status table marks what
  each task needs (source only, disc image, or a human). Releases stay paused.
Next: task 1.1 and task 2.1 (independent).

### 2026-10-11 research pass 1: SMS Launcher and sms-pc-port
Result: done
Pins: SunPad eeaa38d; read SMS Launcher b460678 (v0.1.57), sms-pc-port
  a8669e0, decompilation 967436b
Checkpoints: documentation only
Evidence: [research loop](RESEARCH-LOOP.md), with the inventory and source rules
Notes: SMS Launcher is MIT; sms-pc-port still has no license and still needs
  Rosetta 2 on Apple silicon, so the review's conclusions stand. Players get
  more there on PCs: 60/120 FPS, HD textures, graphics options, save import and
  backups, rumble, camera options. SunPad already has widescreen, render
  scale, an FPS counter, button and touch remapping, and a shareable diagnostic
  report. Saves sit in Library/Application Support, out of reach of Files.
  Promoted 10.1 save import and export, 10.2 iPhone and iPad rumble, 10.3
  invert camera up/down. Added findings to 7.1 and 8.1 and five ideas.
Next: tasks 10.1-10.3 can start now alongside 1.1 and 2.1.

### 2026-10-11 task 10.1: save import and export
Result: in progress (code landed; C5 pending)
Pins: SunPad 48f420a, ModernGekko 8f49c55, RecompCore da96175, DolRecomp fa0cf61
Checkpoints: C1 pass (new save transfer regression with synthetic saves;
  changed iOS sources type-check against the iOS Simulator SDK;
  check-repository.sh); C5 pending
Evidence: this PR
Notes: Rules come from RecompCore's GCMemcardDirectory and GCIFile: Dolphin
  loads every .gci with the current game code, refuses a second save with the
  same maker, game code and internal filename, and requires 64 bytes plus
  8 KiB per block. SunPad's user folder makes the card
  Application Support/SunPad/GC/USA/Card A. Imports are staged and applied in
  startGameIfProvisioned before the core starts, after a full card backup.
  Backups go to Documents/Save Backups (Files-visible), a change from the task
  card, so players can restore them. Apple TV is unchanged.
Next: human C5 on the iPad, following the device session protocol: export the
  current save and compare its hash with the card file; import a Dolphin GMSE01
  .gci, reopen, confirm it loads in game, then save and export again and load
  the export in Dolphin; confirm the backup folder holds the previous save.
