---
name: deliver
description: |
  Use this skill when the user wants to deliver a ready-to-send text
  artifact verbatim to a Telegram chat.
---

## Environment

Stop when `WITNESS_TOKEN` or `WITNESS_CHAT` is unset or empty.
Report single-line failure naming missing variable.
Read bot token from `WITNESS_TOKEN` and chat id from `WITNESS_CHAT`.
Treat `WITNESS_CHAT` as integer chat identifier.
Match format Telegram Bot API expects.

## Body

Take message body verbatim from supplied text artifact.
Previous plugin, skill, or agent hands it in.
Never rewrite, summarize, paraphrase, or shorten supplied text.
Never reorder, translate, prepend, append, or otherwise alter it.
Apply no transformation beyond `MarkdownV2` escaping Telegram requires.
Stop when supplied text artifact is missing, empty, or only whitespace.

## Escaping

Set `parse_mode` to `MarkdownV2`.
Escape Telegram reserved characters in body.
Leave inline code, link labels, and link URLs untouched.

## Length

Stop when supplied text exceeds four thousand characters.
Report actual length and cap.

## Send

Post message once and feed body via stdin.
Feeding via stdin lets backticks and reserved characters survive shell.
Pass `disable_web_page_preview=true` on every call.
Verify response `ok` field is `true`.
Surface `description` field and stop run when `ok` is `false`.

## Secrecy

Never name Claude, Anthropic, or any AI agent in message.
Never name language model or any automation tool either.
Never log, print, echo, or write value of `WITNESS_TOKEN` anywhere.
Stop after single successful `sendMessage` call.
