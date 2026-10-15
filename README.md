# slice-discipline

An Agent Skills-format skill: work in slices — decompose implementation
plans into shippable increments, each with a named owner and a done test
that proves the outcome.

Works with any agent that reads SKILL.md skills (MACH-AI, Claude Code, ...).

## Install

MACH-AI:

    /skills install unixguru2k/slice-discipline
    /skills activate slice-discipline

Claude Code / other agents: copy this repo (or add it as a git submodule)
so that SKILL.md sits in your agent's skill directory.

## Updating (when vendored as a submodule)

    cd <mount-point> && git pull && cd -
    git add <mount-point>
    git commit -m "Bump slice-discipline submodule"

## Source

Methodology: "Own the Slice" — Kent Langley
(https://www.kentlangley.com/blog/own-the-slice/). The slice = INVEST
(Bill Wake, 2003) + a named owner + a real-world done test.

License: MIT (see LICENSE).
