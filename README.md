# claude-md-pro

A 200-line **CLAUDE.md** for Claude Code that fuses Andrej Karpathy's code-level rules with field-tested agent-discipline rules from real long-session builds.

Drop it in your project root. Watch your agent stop drifting.

## What is this

Karpathy's CLAUDE.md (compiled in [multica-ai/andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills), 138K stars on GitHub) covers code-level discipline: think before coding, simplicity first, surgical changes, goal-driven execution.

It does not cover what happens over long agent sessions:

- Context Rot. The model decays as the window fills.
- Context Hog. Long sub-agent outputs eat your tokens forever.
- The Drift Lich. Invisible pull away from what you actually asked for.
- The Promise Echo. Insights spoken but never saved.
- Trust By Mechanism. The audit log your agent cannot lie to.

These 5 patterns cost real money in wasted token spend and real time in rework. This file adds 5 rules that fix them, fused with Karpathy's 4. Nine rules total. Under 200 lines.

## Who this is for

- Anyone building serious work in Claude Code (or Cursor, or any LLM agent harness).
- Anyone who has had their agent silently invent features they did not ask for.
- Anyone running long sessions where the model "feels dumber" two hours in.
- Anyone whose token bill is climbing without explanation.

## How to install

1. Copy [CLAUDE.md](CLAUDE.md) into the root of your project.
2. That is it.

Claude Code automatically loads CLAUDE.md from the project root into every session. If you already have a CLAUDE.md, merge the rules from this file into yours. The 9 rules play well with project-specific instructions.

## What is inside

- **Part A: Code-Level Discipline** (rules 1-4, adapted from Karpathy's observations).
- **Part B: Session and Orchestration Discipline** (rules 5-9, original from real builds).

Each rule has a body, a bullet list of how to apply it, and a one-line "Rule" you can tape to the wall.

## Why fuse them

Karpathy's rules tell the agent how to write better code, line by line.

The new rules tell the agent how to stay honest across long sessions, sub-agent dispatches, and context windows that fill up.

Stacked, they cover both layers: what the agent writes AND how the agent thinks across time.

## Credits

- **Part A** based on Andrej Karpathy's public observations about LLM coding mistakes. The community compilation that popularized them lives at [multica-ai/andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills).
- **Part B** original work by Varuna Jain, founder of GrowthShot.ai (global) and GrowthSetu.ai (India). Drawn from real long-session Claude Code builds running voice, WhatsApp, and website AI agents for small and medium businesses.

## License

Free to use, fork, adapt, or extend. Credit appreciated but not required.
