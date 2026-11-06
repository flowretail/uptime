# Flow Retail uptime

The status and uptime of Flow Retail's services, as the "Oppetid" page on flowretail.no shows them. Two files, both made by our monitoring (Icinga) and pushed here automatically:

```
https://raw.githubusercontent.com/flowretail/uptime/status/status.json
https://raw.githubusercontent.com/flowretail/uptime/main/uptime.json
```

- `status.json` (branch `status`): how each service is right now. Updated every 5 minutes. The branch always holds one commit, replaced each time.
- `uptime.json` (branch `main`): the last 90 days, day by day. Updated once a night (about 00:20, Europe/Oslo). The history of the file is the history of the numbers.

GitHub's raw CDN caches files for up to 5 minutes. Field names and codes are English; text meant for visitors (`method`, `explanation`) is Norwegian.

## How it is measured

Icinga checks every service over HTTPS every 5 minutes, day and night, from our own network. Only a service that does not answer counts as down: a slow answer or a warning is up, as long as the service answers its customers. Downtime counts from the first failed check, once Icinga has confirmed the failure with its retries. Time inside planned maintenance, registered in Icinga before the work, is left out.

"Nettbutikk" is every production shop we run, each shop server one unit.

Measurements start on 2026-10-20; days before that are `nodata`.

## status.json

```
generated            when the status was measured (ISO 8601, Europe/Oslo)
status               the worst of the services
services[]
  id, name
  status             operational | degraded | outage
```

A unit counts as down now when it has failed for 5 minutes or more (short blips never show) and is not in planned maintenance. One unit down makes the service `degraded`, half or more `outage`.

## uptime.json

```
generated            when the file was made
window_days          90
measured_from        the first measured day
sla                  {target: 99.9, excludes_planned_maintenance: true}
status               as of this run, all services: operational | degraded | outage
method               how we measure, Norwegian, for the page
services[]
  id, name, description (Norwegian)
  uptime_90d         percent up over the measured days in the window
  status             as of this run: operational | degraded | outage
  checks             internal: how many Icinga checks make up the service
  days[]             oldest first, the last one is yesterday
    date             YYYY-MM-DD (Europe/Oslo)
    status           ok | degraded | down | maintenance | nodata
    uptime           percent up that day: the average unit's share of the day
    down_minutes     minutes with half or more of the units down
    degraded_minutes minutes with fewer than half of the units down
    maintenance_minutes  planned maintenance
    explanation      a short Norwegian text written by us, or null
```

`uptime` and the minute fields are missing on `nodata` days.

A day is:

- `down` when half or more of the units were down at the same time for 5 minutes or more in a row,
- otherwise `degraded` when one unit or more was down for 5 minutes or more in a row,
- otherwise `maintenance` when there was planned maintenance,
- otherwise `ok`. Shorter blips leave the day `ok` but still count in `uptime`.

Made by `flow-uptime` (flowretail-openvox, profile::monitoring::uptime) on the Icinga server; explanations are set in Puppet (profile::monitoring::uptime::explanations).
