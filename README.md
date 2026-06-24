# Night Cooling Advisor (Home Assistant Blueprint)

Night Cooling Advisor sends an evening notification when free night ventilation is worthwhile. It uses hourly weather forecast data, indoor average temperature, dew point, and tomorrow's high to decide whether opening windows is beneficial. The blueprint only notifies and does not control actuators.

## Import

Use this raw URL in Home Assistant Blueprint Import:

`https://raw.githubusercontent.com/bleialf/night-cooling-advisor/main/night_cooling_advisor.yaml`

You can also open the source file directly:

`https://github.com/bleialf/night-cooling-advisor/blob/main/night_cooling_advisor.yaml`

## Inputs

| Input | Default |
|---|---|
| Evening notification time (`notify_time`) | `20:00:00` |
| Notify service (`notify_service`) | `notify.ha_push_bewohner` |
| Indoor average sensor (`indoor_avg_sensor`) | `sensor.raum_ist_durchschnitt` |
| Room temperature sensors (`room_temp_sensors`) | `[]` |
| Outdoor temperature current (`outdoor_sensor`) | `sensor.hoval_cangw_af1_aussenfuehler_1` |
| Weather entity hourly forecast (`weather_entity`) | `weather.pirateweather` |
| Outdoor dew point (`dewpoint_sensor`) | `sensor.pirateweather_dew_point` |
| Tomorrow high temperature (`tomorrow_high_sensor`) | `sensor.pirateweather_daytime_high_temperature_1d` |
| Look-ahead window hours (`lookahead_hours`) | `4` |
| Indoor threshold (`indoor_threshold`) | `23` |
| Minimum benefit delta (`min_delta`) | `2` |
| Max dew point (`dewpoint_max`) | `16` |
| Hot-day threshold (`hotday_threshold`) | `30` |
| Optional block entities (`block_if_on`) | `[]` |

## Compatibility

Requires Home Assistant 2024.x or newer because it uses `weather.get_forecasts`.

## Version

Current release: 1.0.0
