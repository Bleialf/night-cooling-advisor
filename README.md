# Night Cooling Advisor (Home Assistant Blueprint)

An evening advisor for **free night ventilation**. Once a day at a fixed time it
checks whether opening the windows tonight is worthwhile and, if so, sends a
notification. It is **notification-only** — it never moves covers, windows, or
the heat pump.

## How the decision works

A notification is sent only when **all** of these are true:

1. **Indoor is too warm** — the indoor average is above *Indoor threshold*.
2. **Air isn't humid** — the outdoor dew point is below *Max outdoor dew point*
   (so you don't ventilate humidity into the house).
3. **It will actually cool down** — the **lowest** outdoor temperature in the
   **look-ahead window** is at least *Minimum benefit* (K) below indoor.

Because step 3 uses the **forecast minimum** instead of the current temperature,
the advisor still fires when it's a little too warm right now but cools down
soon (e.g. an hour after bedtime and all night).

If tomorrow's forecast high reaches the *Hot-day threshold*, the default message
adds an urgent "pre-cool the slab tonight" hint.

## Requirements

- Home Assistant **2024.x or newer** (uses the `weather.get_forecasts` action).
- A weather entity with an **hourly** forecast.

## Import

[![Import Blueprint](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https://raw.githubusercontent.com/bleialf/night-cooling-advisor/main/night_cooling_advisor.yaml)

Or go to **Settings → Automations & Scenes → Blueprints → Import Blueprint** and paste:

```
https://raw.githubusercontent.com/bleialf/night-cooling-advisor/main/night_cooling_advisor.yaml
```

## Inputs

| Input | What it does | Default |
|---|---|---|
| **Evening notification time** | Time of day the advisor runs and (if conditions met) notifies. Evaluated once per day. | `20:00:00` |
| **Notify service** | Notify service or group that delivers the message. Free text (groups often aren't in pickers). | required |
| **Notification title** | Push title. Supports Jinja templates. | `🌙 Free cooling available` |
| **Notification message** | Push body. Fully editable Jinja template (see variables below). | see file |
| **Indoor average sensor** | Sensor for the indoor average temperature; basis of the "too warm" decision. | required |
| **Room temperature sensors** | Optional list; only used to name the 3 warmest rooms. No effect on whether it fires. | `[]` |
| **Outdoor temperature (current)** | Current outdoor temp; shown as "outdoor now" and used as fallback if forecast is missing. | required |
| **Weather entity (hourly forecast)** | Source of the look-ahead minimum via `weather.get_forecasts`. | required |
| **Outdoor dew point** | Humidity guard; at/above the max, no notification. | required |
| **Tomorrow's high temperature** | Only used to add the "pre-cool the slab" hint. | required |
| **Look-ahead window (hours)** | How many hours ahead to scan for the lowest outdoor temp. | `4` |
| **Indoor threshold** | Min indoor temp required to notify. | `23 °C` |
| **Minimum benefit** | Required (indoor − forecast min) advantage in K. | `2 K` |
| **Max outdoor dew point** | Above this, air is too humid → no notify. | `16 °C` |
| **Hot-day threshold** | Tomorrow's high at/above this switches message to the urgent pre-cool wording (text only). | `30 °C` |
| **Block entities** | Optional; if any is on/home/true, suppress the notification (e.g. vacation mode). | `[]` |

## Variables available in the message template

`indoor`, `outdoor_now`, `fc_min`, `dewpoint`, `tomorrow_high`, `look_h`,
`warm_rooms`, `hot_thresh`, `delta_req`.

## Version

Current release: 1.1.0
