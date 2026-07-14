# tap-appsflyer

This is a [Singer](https://singer.io) tap that produces JSON-formatted 
data following the [Singer spec](https://github.com/singer-io/getting-started/blob/master/SPEC.md).

This tap:
- Pulls raw data from AppFlyer's [Raw Data Reports V5 API](https://support.appsflyer.com/hc/en-us/articles/208387843-Raw-Data-Reports-V5-)
- Outputs the schema for each resource
- Incrementally pulls data based on the input state

---

## Slack no-events alert

The tap can post a Slack message when no events were received for a stream
(`installs`, `organic_installs`, `in_app_events`) for more than a configured
number of days.

After each run, if a stream returned at least one record, the tap records the
current time under `last_received` in the state file. At the end of the sync it
checks each stream and, for any stream whose last receipt is older than the
threshold (or that has never received records), posts a single combined message
naming the affected streams.

Add these optional fields to your `config.json`:

| Field | Required | Default | Description |
| --- | --- | --- | --- |
| `slack_webhook_url` | No | none | Slack [Incoming Webhook](https://api.slack.com/messaging/webhooks) URL. If omitted, the alert is logged as a warning instead of being posted. |
| `staleness_threshold_days` | No | `1` | Number of days without receiving events before a stream is reported as stale. |

Note: the check only runs when the tap runs, so set `staleness_threshold_days`
larger than your scheduled run interval.

---

Copyright &copy; 2017 Stitch, Inc.