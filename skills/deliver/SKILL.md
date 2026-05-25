---
name: deliver
description: |
  Use this skill to deliver a ready-to-send text artifact
  verbatim to a Telegram chat.
---

Stop when `WITNESS_TOKEN` or `WITNESS_CHAT` is unset or empty.
Report a single-line failure naming the missing variable.
Read the bot token from `WITNESS_TOKEN` and the chat id from `WITNESS_CHAT`.
Treat `WITNESS_CHAT` as the integer chat identifier.
Match the format the Telegram Bot API expects.
Take the message body verbatim from the supplied text artifact.
The previous plugin, skill, or agent hands it in.
Never rewrite, summarize, paraphrase, or shorten the supplied text.
Never reorder, translate, prepend, append, or otherwise alter it.
Apply no transformation beyond the `MarkdownV2` escaping Telegram requires.
Stop when the supplied text artifact is missing, empty, or only whitespace.
Set `parse_mode` to `MarkdownV2`.
Escape Telegram reserved characters in the body.
Leave inline code, link labels, and link URLs untouched.
Stop when the supplied text exceeds four thousand characters.
Report the actual length and the cap.
Post the message once and feed the body via stdin.
Feeding via stdin lets backticks and reserved characters survive the shell.
Pass `disable_web_page_preview=true` on every call.
Verify the response `ok` field is `true`.
Surface the `description` field and stop the run when `ok` is `false`.
Never name Claude, Anthropic, or any AI agent in the message.
Never name a language model or any automation tool either.
Never log, print, echo, or write the value of `WITNESS_TOKEN` anywhere.
Stop after a single successful `sendMessage` call.
