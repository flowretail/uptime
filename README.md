# Flow Retail uptime

`uptime.json` holds the uptime of Flow Retail's services for the last 90 days, day by day. It is updated once a night (Europe/Oslo) and is what the "Oppetid" page on flowretail.no shows.

Fetch it from:

```
https://raw.githubusercontent.com/flowretail/uptime/main/uptime.json
```

## How it is measured

Icinga, our monitoring, checks every service over HTTPS every minute, day and night, from our own network. A day's uptime is the share of the day the check answered OK, counted after Icinga's retries (a single failed request does not count). Time inside a planned maintenance window, registered in Icinga before the work, is left out. "Nettbutikk" is the average across every production shop we run.

Measurements start on 2026-10-12; days before that are `nodata`.

## The file

```
generated            when the file was made
window_days          90
measured_from        the first measured day
sla                  {target: 99.9, excludes_planned_maintenance: true}
status               now, all services: operational | degraded | outage
method               the text above, short
services[]
  id, name, description
  uptime_90d         percent over the measured days in the window
  status             now: operational | degraded | outage
  checks             how many Icinga checks make up the service
  days[]             oldest first, the last one is yesterday
    date             YYYY-MM-DD
    status           ok | degraded | down | maintenance | nodata
    uptime           percent for the day (not on nodata days)
    down_minutes, degraded_minutes, maintenance_minutes
```

A day is `down` when the service was down a minute or more, `degraded` for a shorter dip or a slow answer (warning), `maintenance` when only planned work took time, otherwise `ok`.

Made by `flow-uptime` (flowretail-openvox, profile::monitoring::uptime) on the Icinga server. The history of the file is the history of the numbers.
