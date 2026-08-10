---
'posthog-js': patch
'@posthog/browser-common': patch
---

fix(browser): stop the client rate-limit warning re-entering capture

When the client limiter dropped a capture, the SDK logged the drop through
`logger.critical`, which is a raw `console.error`. With console error autocapture
on, that log re-entered `capture`, got rate limited again, and looped -- exactly
when the client was already over budget -- and put PostHog's own diagnostic string
into the customer's error tracking. The drop now logs through `logger.warn`
(`console.warn`), which autocapture does not wrap, and `wrapConsoleError` skips any
output that starts with the `[PostHog.js]` logger prefix.
