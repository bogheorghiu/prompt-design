# Prompt Design

A single skill, `prompt-design`, for designing, revising, evaluating and debugging prompt
artifacts for any model: system prompts and personas, agent and subagent definitions, skills and
their routing descriptions, rule files, tool and output contracts, few-shot banks, graders and
rubrics, and one-shot requests.

## What it is built to do differently

Most prompt-rewriting skills take a draft and return a better draft. This one treats a prompt as an
artifact with a deployment. The points below describe its instructions; what testing actually
measured is under Evidence.

- **It asks for what changes the answer.** If the target model, how the prompt is delivered (one
  chat turn, a standing system prompt, an agent in a loop), or the objective is missing, it is told
  to ask before committing to a design. In testing it sometimes asked and sometimes stated its
  assumptions openly and delivered anyway. When no person is there to answer, it proceeds on the
  most defensible reading and logs every assumption.
- **It repairs from the actual failure.** Paste the prompt and the outputs that went wrong; it
  locates the instruction that produced them and patches that, instead of rewriting from scratch.
- **It keeps the artifact's own format** (YAML frontmatter stays YAML, a JSON contract stays a
  contract) and reports each behavioural change with its reason.
- **It is told not to invent model facts.** Claims about a model's capabilities or a harness's
  limits are to be verified, marked as common knowledge, or marked as assumed.

Invoke it as `/prompt-design:prompt-design <your request>`, or let Claude pick it up when you
ask for help with a prompt.

## Install

In Claude Code, this repository is also a plugin marketplace:

```
/plugin marketplace add bogheorghiu/prompt-design
/plugin install prompt-design@prompt-design
```

For a quick polish of a one-off chat prompt, a plain rewriting skill is faster (about a third of the
time in testing); this one is built for prompts that run many times, run unattended, or are
consumed by code.

## Example requests

1. "This classifier prompt for gpt-5-mini keeps returning extra keys and breaks our JSON parser.
   Here is the prompt and three outputs that failed: …"
2. "Write the system prompt for a support agent that runs on Claude Sonnet with read access to our
   docs; it must never promise refunds."
3. "Our skill's description isn't being picked for requests like these five, but is for these two
   it shouldn't catch. Rewrite the description."

## What it runs and sends

It ships no scripts, hooks or MCP servers. It is instructions only — but those instructions tell
Claude to verify load-bearing claims about a model or harness, so when your session has web search,
web fetch, subagents or other models available, Claude may use them for that without being asked,
and what it sends can include details of the prompt you are working on. Without such tools it marks
the claim as assumed instead. If you don't want that, say so in your request.

## Evidence

Tested on 2026-10-01 under its earlier name, `prompt-engineering` (the instructions are unchanged),
against four other prompt skills in Anthropic's Directory and against no skill: 31 test cases (27
written without knowledge of which skills were under test, 4 from another skill's own eval suite),
Claude Opus 5 running every arm, decision rules fixed before the runs.

- On held-out cases its checklist pass rate was 0.883 against 0.801 for the same model with no
  skill (0.855 if one run that only asked questions and delivered nothing is scored as failing).
- None of the other prompt skills was ahead of it by the pre-set margin of 0.05, and it was not
  clearly ahead of them either: prompt-brain scored 0.883 to its 0.857 in the first round, and
  0.834 to its 0.883 on held-out cases.
- Its clearest lead was on requests missing a requirement that changes the answer — three cases in
  all, so read it as a tendency.
- In the first round it did worse than no skill on writing skills and their descriptions (0.78
  against 0.96) and was the lowest of all arms on graders (0.75). It took about 1.5 times as long
  as no skill.
- Replies were scored against per-case checklists by two judge models: Claude and GPT in the first
  round, two Claude models with a Gemini spot check on held-out cases.

The test records are not public yet.

## Support

Questions and bug reports: open an issue on this repository.

## License

MIT — see `LICENSE`.

Written by Bogdan Gheorghiu with Claude (Anthropic). The skill adapts ideas from
`intrinsic-prompt-design` (github.com/bogheorghiu/ex-cog-dev, MIT, same author); the adapted ideas
are listed in the CREDITS section of `skills/prompt-design/SKILL.md`.
