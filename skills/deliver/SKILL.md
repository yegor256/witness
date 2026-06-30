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
Preserve supplied text word for word, its wording and length intact.
Preserve its order and language, with nothing prepended or appended.
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

Keep Claude, Anthropic, and every AI agent unnamed in message.
Keep every language model and automation tool unnamed too.
Keep value of `WITNESS_TOKEN` out of every log, print, echo, and file.
Stop after single successful `sendMessage` call.

## Example

```text
WITNESS_CHAT=-1001234567890
Supplied artifact (verbatim):
  Talk accepted! See you at JPoint 2026 in Moscow on 2026-04-09.

Body after MarkdownV2 escaping (fed via stdin):
  Talk accepted\! See you at JPoint 2026 in Moscow on 2026\-04\-09\.

POST https://api.telegram.org/bot<WITNESS_TOKEN>/sendMessage
  chat_id=-1001234567890
  parse_mode=MarkdownV2
  disable_web_page_preview=true

Response: {"ok":true,"result":{"message_id":4471}}
Stop after this single successful call.
```
