# Prompt Design

A single skill, `prompt-design`, for designing, revising, evaluating and debugging prompt
artifacts for any model: system prompts and personas, agent and subagent definitions, skills and
their routing descriptions, rule files, tool and output contracts, few-shot banks, graders and
rubrics, and one-shot requests.

## What it does differently

Most prompt-rewriting skills take a draft and return a better draft. This one treats a prompt as an
artifact with a deployment:

- **It asks for what changes the answer.** If the target model, how the prompt is delivered (one
  chat turn, a standing system prompt, an agent in a loop), or the objective is missing, it asks
  before committing to a design, and keeps asking until those three are clear. It makes stated,
  flagged assumptions only about details that cannot change the result — and, when no person is
  there to answer, proceeds on the most defensible reading and logs every assumption.
- **It repairs from the actual failure.** Paste the prompt and the outputs that went wrong; it
  locates the instruction that produced them and patches that, instead of rewriting from scratch.
- **It keeps the artifact's own format** (YAML frontmatter stays YAML, a JSON contract stays a
  contract) and reports each behavioural change with its reason.
- **It does not invent model facts.** Claims about a model's capabilities or a harness's limits are
  verified, marked as common knowledge, or marked as assumed.

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
web fetch or subagents, Claude may use them for that, and the queries can include details of the
prompt you are working on. Without such tools it marks the claim as assumed instead.

## Evidence

Tested on 2026-10-01 against four other prompt skills and against no skill, on 31 test cases
(27 written without knowledge of which skills were under test, 4 from another skill's own eval
suite), with pre-registered decision rules. On held-out cases its checklist pass rate was 0.883
against 0.801 for the same model with no skill. Its clearest lead was on requests missing a
requirement that changes the answer — a small set (three cases in all), so read it as a tendency.

## License

MIT — see `LICENSE`.

Written by Bogdan Gheorghiu with Claude (Anthropic).
