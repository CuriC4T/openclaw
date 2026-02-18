---
name: samsung-smartthings
description: Control SmartThings devices (lights, plugs, sensors, scenes) using the SmartThings REST API.
homepage: https://playbooks.com/skills/openclaw/skills/samsung-smartthings
metadata: { "openclaw": { "emoji": "🏠", "requires": { "bins": ["curl", "jq"] } } }
---

# Samsung SmartThings

Use SmartThings REST APIs to list locations/devices, read device state, run commands, and trigger scenes.

## Required env

- `ST_ACCESS_TOKEN`: SmartThings personal access token
- `ST_LOCATION_ID`: target location id

Optional:

- `ST_API_BASE`: default `https://api.smartthings.com/v1`

## Quick checks

```bash
export ST_API_BASE="${ST_API_BASE:-https://api.smartthings.com/v1}"
curl -fsSL -H "Authorization: Bearer $ST_ACCESS_TOKEN" "$ST_API_BASE/locations" | jq .
```

```bash
curl -fsSL -H "Authorization: Bearer $ST_ACCESS_TOKEN" \
  "$ST_API_BASE/devices?locationId=$ST_LOCATION_ID" | jq .
```

## Find devices by label/name

```bash
curl -fsSL -H "Authorization: Bearer $ST_ACCESS_TOKEN" \
  "$ST_API_BASE/devices?locationId=$ST_LOCATION_ID" \
  | jq -r '.items[] | [.deviceId, .label, .name, .type] | @tsv'
```

## Read device status

```bash
DEVICE_ID="<device-id>"
curl -fsSL -H "Authorization: Bearer $ST_ACCESS_TOKEN" \
  "$ST_API_BASE/devices/$DEVICE_ID/status" | jq .
```

## Common commands

Turn switch on/off (plug, light, strip):

```bash
DEVICE_ID="<device-id>"
curl -fsSL -X POST \
  -H "Authorization: Bearer $ST_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  "$ST_API_BASE/devices/$DEVICE_ID/commands" \
  -d '{"commands":[{"component":"main","capability":"switch","command":"on"}]}'
```

```bash
DEVICE_ID="<device-id>"
curl -fsSL -X POST \
  -H "Authorization: Bearer $ST_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  "$ST_API_BASE/devices/$DEVICE_ID/commands" \
  -d '{"commands":[{"component":"main","capability":"switch","command":"off"}]}'
```

Brightness (0-100):

```bash
DEVICE_ID="<device-id>"
LEVEL=50
curl -fsSL -X POST \
  -H "Authorization: Bearer $ST_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  "$ST_API_BASE/devices/$DEVICE_ID/commands" \
  -d "{\"commands\":[{\"component\":\"main\",\"capability\":\"switchLevel\",\"command\":\"setLevel\",\"arguments\":[${LEVEL}]}]}"
```

Run scene:

```bash
curl -fsSL -H "Authorization: Bearer $ST_ACCESS_TOKEN" \
  "$ST_API_BASE/scenes" | jq -r '.items[] | [.sceneId, .sceneName] | @tsv'
```

```bash
SCENE_ID="<scene-id>"
curl -fsSL -X POST \
  -H "Authorization: Bearer $ST_ACCESS_TOKEN" \
  "$ST_API_BASE/scenes/$SCENE_ID/execute"
```

## Notes for your setup

- Motion sensors are usually read-only. Poll their status via `/devices/<id>/status`.
- Hue bulbs and LED strips can usually be controlled with `switch` and `switchLevel`.
- Tapo smart plugs connected via SmartThings should respond to `switch` commands.
- If a command fails, check which capabilities the device exposes in status payload first.
