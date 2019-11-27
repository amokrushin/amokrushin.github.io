# Sentry

## Test with sentry-cli

https://blog.sentry.io/2017/11/28/sentry-bash

```
curl -sL https://sentry.io/get-cli/ | bash
export SENTRY_DSN=<your-dsn-goes-here>
sentry-cli send-event -m "Something happened"
```
