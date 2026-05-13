<div align="center">

# ProtoCodeBase Skills
Agent skills for the ProtoCodeBase ecosystem

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

<p align="center">
  <img src="./assets/github-header-banner.png" alt="ProtoCodeBase" width="900">
</p>

English · [简体中文](./README.md)

</div>

# ProtoCodeBase

Do not make your Agent reinvent the wheel.

Let it read less, guess less, and wander less inside a vanishing context window.  
Let it stand on the bones of mature software, follow proven industrial patterns,
and turn your need into code that can actually ship.

ProtoCodeBase is not about making Agents write more code. It is about helping
Agents find the small piece of code worth referencing, and the small piece of
code worth changing for your need.

## Needs Before Code

Code is not the goal. Your need is.

What deserves to survive in a software project is not only the files, but also
how a need was understood, decomposed, planned, and executed.

Your `Intent` is the need itself. It comes from you. The protocol does not need
to disguise it as another kernel object.

ProtoCodeBase persists the engineering chain produced after the program
normalizes that need:

```text
Request  ->  Plan  ->  Change
```

The program turns your need into an executable `Request`.  
The Agent expands that request into a bounded `Plan`.  
The codebase receives a traceable `Change`.

A software change no longer has to live only as fragile memory scattered across
a chat window. It becomes project context that the next Agent can pick up.

## Save Tokens, Not Understanding

The most expensive Agent waste is not writing a few extra lines of code. It is
walking into the same forest again and again, unable to find the tree you need.

If a mature codebase is a complete Gundam, do not ask your Agent to disassemble
every part before every repair. Give it a manual: where the arm is, where the
drive shaft connects, what can be modified, what must not move, and what each
change may affect.

ProtoCodeBase leaves that manual through a three-layer project structure:

```text
Capabilities  ->  Project Map  ->  Route
```

- `Capabilities` describe what the app actually does and where each ability begins and ends.
- `Project Map` describes how the project is organized and which modules own which responsibilities.
- `Route` helps the Agent move from a need directly to the target code, component, or service.

The Agent does not need to pour the entire repository into context. It can
free old chat state with confidence, keep the normalized work record, and use
the project manual to return to the exact target next time.

## Build From Mature Industrial Templates

Do not imagine a whole system from an empty file.

File management, permissions, collaboration, editors, workflows, history,
deployment, error handling, and user interfaces have already appeared again
and again in mature software. ProtoCodeBase aims to preserve those proven
engineering structures so your Agent can reference, cut, merge, and adapt them
instead of inventing them again.

Inherit before creating.  
Modify along industrial practice before blindly writing.  
Let the code tell the Agent which path the need should take, instead of making
the Agent burn tokens searching for hidden details.

## Install

Install the ProtoCodeBase skill:

```bash
npx skills add https://github.com/OhMyYuwan/ProtoCodeBase.Skill.git --skill acp-v1-0-0
```

For concrete skills, protocol versions, and project manifest examples, see
[skills/README.md](./skills/README.md).

## License

The skill wrappers, registry, scripts, and repository documentation in this
repository are licensed under the MIT License. See [LICENSE](./LICENSE).

Note: bundled protocol text, compiled protocol output, and reference packages
inside skills may carry their own LICENSE files. They are not relicensed by
the root MIT License. See [NOTICE](./NOTICE).
