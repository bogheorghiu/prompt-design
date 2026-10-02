# Privacy

The prompt-design plugin collects nothing and sends nothing on its own. It contains no
scripts, hooks, MCP servers or network calls. It has no telemetry and keeps no data of
its own. What it ships is instructions that Claude reads.

Those instructions tell Claude to check load-bearing claims about a model or harness.
If your session has web search, web fetch, subagents or other models available, Claude
may use them for that check without being asked. What it sends can include details of
the prompt you are working on. Those tools, and Claude itself, run under the terms and
privacy policies of whoever provides them: Anthropic for Claude, and the provider of any
other tool or model you have connected. They are not governed by this plugin.

Without such tools, Claude marks the claim as assumed instead of checking it. If you
don't want any external lookups, say so in your request.

Questions: https://github.com/bogheorghiu/prompt-design/issues
