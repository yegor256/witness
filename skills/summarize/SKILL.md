---
name: summarize
description: |
  Use this skill to summarize the work just performed by
  earlier plugins, skills, and agents in the current
  conversation into a single short Markdown paragraph and
  post it once to a Telegram chat: open with one emoji
  that visualizes the outcome, format with plain Markdown
  (links and inline fixed-width text only), read the bot
  token from the `WITNESS_TOKEN` environment variable and
  the chat id from the `WITNESS_CHAT` environment
  variable, and post via the Telegram Bot API. One report
  per run — then stop.
---

Stop the run immediately when either `WITNESS_TOKEN` or
  `WITNESS_CHAT` is unset or empty: do not call the
  Telegram Bot API, do not compose a message, do not
  retry, and report a single-line failure naming the
  missing variable.

Read the bot token from the `WITNESS_TOKEN` environment
  variable and the chat id from the `WITNESS_CHAT`
  environment variable, treat `WITNESS_CHAT` as the
  integer chat identifier expected by the Telegram Bot
  API, and use those values for every Telegram Bot API
  call in this run.

Build the report only from the work the earlier
  plugins, skills, and agents actually performed in the
  current conversation — the skill that ran, the
  artifact created or edited, the URL returned by the
  delegated tool, the verdict the sub-skill reported —
  and do not invent steps, files, outcomes, or links the
  conversation does not record.

Skip every detail about how the plugin, the
  marketplace, the skills, or the routine were
  installed, loaded, authenticated, or invoked; the
  report describes the outcome of the work, not the
  setup that made the work possible.

Exclude every scaffolding step from the report —
  installing a plugin, fetching a marketplace entry,
  locating the right skill, reading a configuration
  file, listing directories, probing for a tool,
  authenticating against an API, warming a cache, or
  any other preparation that precedes the real task —
  because these steps are means, not outcomes, and
  the reader cares only about the result.

Focus the paragraph on the key objective of the
  task: the change delivered, the question answered,
  the artifact produced, the verdict reached, or the
  blocker hit; drop every clause that describes
  navigation, discovery, or readiness, even when
  those steps consumed most of the run.

Compose the message as a single paragraph of plain
  prose that names the outcome first, the artifact
  second, and the next step third when one applies; no
  headings, no bullet lists, no tables, no quoted
  blocks, no horizontal rules — one paragraph is one
  paragraph.

Write the report as a personal status update from the
  agent to a single human supervisor: open the paragraph
  with the salutation `Boss,`, phrase every clause in
  the first person with `I`, `me`, `my`, and report what
  I did, what I found, and what I left for the next
  step, so the message reads as a one-to-one account
  from a subordinate to a boss rather than an
  impersonal system log.

Embed the URL of the current agent run itself — the
  `claude.ai` session address, the hosted-IDE workspace
  URL, or whatever public link points back to the
  conversation where this execution is happening — as
  the hyperlink target on the opening verb of the
  paragraph (the first `I [did](…)` clause), so the
  supervisor can click through from the report to the
  live trace of the work just summarized; read the URL
  from the environment variable that the host injects
  for this purpose and omit the link only when no such
  variable is set.

Open the message with exactly one emoji picked from the
  set 👍🏻, ❤️, 🍒, 😊, 🍓, ⁉️, 🧨, 👌🏻, 🌶 to match
  the agent's own reading of the outcome just obtained —
  success, failure, partial result, surprise, pride,
  frustration, whatever the work felt like — and follow
  it with a single space and then the paragraph itself.

Use Markdown only for two purposes: write hyperlinks as
  `[label](https://...)` so the artifact URL is
  clickable, and wrap file paths, identifiers, branch
  names, commit hashes, label names, and shell commands
  in backticks so they render as fixed-width; do not
  bold, do not italicize, do not add headings, lists,
  tables, or quoted blocks.

Set `parse_mode` to `MarkdownV2` on the API call and
  escape the Telegram reserved characters `_`, `*`, `[`,
  `]`, `(`, `)`, `~`, `` ` ``, `>`, `#`, `+`, `-`, `=`,
  `|`, `{`, `}`, `.`, `!` with a leading backslash
  everywhere outside of inline code spans, link labels,
  and link URLs, because an unescaped reserved character
  makes Telegram reject the message with HTTP 400.

Keep the paragraph under one thousand characters and
  the whole message under four thousand characters,
  because Telegram caps a single `sendMessage` payload
  at four thousand and ninety-six characters and a
  longer text is rejected outright rather than
  truncated.

Post the message once with
  `curl -sS -X POST
  "https://api.telegram.org/bot$WITNESS_TOKEN/sendMessage"
  -d chat_id="$WITNESS_CHAT"
  -d parse_mode=MarkdownV2
  -d disable_web_page_preview=true
  --data-urlencode text@-`
  and feed the message body to that command through
  standard input, so backticks, parentheses, newlines,
  and reserved characters survive the shell intact and
  do not need to be re-escaped on the command line.

Pass `disable_web_page_preview=true` on every call so
  Telegram does not expand the artifact URL into a
  link-preview card under the message; the report is a
  one-paragraph summary and the preview card would add
  noise the reader did not ask for.

Read the JSON response on the first POST and verify
  that the top-level `ok` field is `true`; when it is
  `false`, surface the `description` field to the user
  and stop the run, because a blind retry on an unparsed
  error would mask the cause and could send the wrong
  message twice.

Never name Claude, Claude Code, Anthropic, an AI agent,
  a language model, or any other automation tool in the
  message text, and never add a `generated by` line, a
  marketing footer, or any AI-disclosure trailer; the
  report describes a contribution, not the tool that
  produced it.

Never log, print, echo, or write the value of
  `WITNESS_TOKEN` to the conversation, to a file, to a
  shell trace, or to a debug line; the token is a
  credential and a leaked credential lets a third party
  impersonate the bot in every chat it has joined.

Stop after the single `sendMessage` call returns
  `ok: true`: do not send a follow-up message, do not
  edit the message, do not pin it, do not start a
  second run, and re-run this skill from the top when
  the next batch of work needs its own report.

Read the four sample reports in the `examples/`
  sub-directory of this skill before composing the
  paragraph — `examples/inbox-reply.md`,
  `examples/pull-request-opened.md`,
  `examples/blocked-on-credentials.md`, and
  `examples/issue-filed.md` — and match their depth,
  link density, and tone rather than copying their
  wording verbatim.
