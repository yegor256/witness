🍒 Boss, I [walked](https://claude.ai/epitaxy/session_01K9dXyR5MnphYZo6QcZ9trB)
  the call graph of `Payment::charge` across the
  `@foobar/billing` repo and found three callers that
  bypass the retry wrapper in `lib/retry.rb` —
  `app/jobs/refund_job.rb`, `app/services/cron/nightly.rb`,
  and `scripts/backfill_2024.rb`; I opened
  [issue #312](https://github.com/foobar/billing/issues/312)
  describing the gap and tagged `@yegor` for triage.
