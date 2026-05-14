# witness

[![License](https://img.shields.io/badge/license-MIT-green.svg)](https://github.com/yegor256/witness/blob/master/LICENSES/MIT.txt)

`witness` is a small Claude Code plugin that reports the
  outcome of work done by other plugins, skills, and agents
  to a Telegram chat.

The bundle ships two skills, each focused on a single
  Telegram delivery task:

* [`summarize`](skills/summarize/SKILL.md)
  — read the work just performed by earlier plugins,
    skills, and agents in the current conversation,
    compose a one-paragraph Markdown report addressed to
    the supervisor, and post it once via the Telegram Bot
    API.
* [`deliver`](skills/deliver/SKILL.md)
  — take a ready-to-send text artifact produced by
    another plugin, skill, or agent and post it verbatim
    to the same Telegram chat, with no rewriting or
    summarizing.

Both skills read the same two environment variables:

* `WITNESS_TOKEN` — the Telegram Bot API token of the
  bot that will send the message.
* `WITNESS_CHAT` — the integer chat id of the
  destination chat.

Either variable being unset or empty stops the skill
  immediately with a single-line failure naming the
  missing variable; the skills never log the token value
  to the conversation, a file, or a shell trace.

Suppose you write with [Claude Code].
You do not need to clone this repository — install the
  bundle as a plugin straight from GitHub.
Inside a Claude Code session, run:

```text
/plugin marketplace add yegor256/plugins
/plugin install witness@yegor256
```

The first command registers the [yegor256/plugins]
  marketplace, which lists every plugin maintained under
  the `yegor256` account; the second installs the
  `witness` plugin from it, which exposes every skill
  under `skills/` to your sessions automatically.

Then export the two environment variables before you
  start the session, for example:

```bash
export WITNESS_TOKEN=123456:ABC-DEF...your-bot-token
export WITNESS_CHAT=-1001234567890
```

Inside a session, ask Claude to use the
  `witness:summarize` skill at the end of a run to post
  a one-paragraph status update covering what the
  previous agents did:

```text
Use the witness:summarize skill to report the work above
to the Telegram chat.
```

Or, when an upstream plugin or agent has already
  produced the exact message body you want sent, ask
  Claude to use the `witness:deliver` skill to pass that
  text through unchanged:

```text
Use the witness:deliver skill to send the text artifact above
to the Telegram chat.
```

To update later, run `/plugin marketplace update yegor256`;
  to remove, run `/plugin uninstall witness@yegor256`.

[yegor256/plugins]: https://github.com/yegor256/plugins
[Claude Code]: https://code.claude.com/docs/en/skills
