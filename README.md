# iare-bus-enroute keep-alive

Public, secret-free helper repo. A GitHub Actions cron pings the IARE Bus
EnRoute health endpoint every 5 minutes between 04:00 and 24:00 IST so the
Render Free instance never cold-starts during college hours.

It contains no application code and no credentials. The only value in here is
the public health URL.

Why a separate public repo: GitHub Actions minutes are unlimited for public
repositories. The application repo is private, where a 5-minute cron would burn
~8,600 of the 2,000 free minutes per month.

If you stop using the app, delete this repository or disable the workflow.
