---
name: smart-plugs
description: Turn your SmartThings plugs and Tapo power strip on/off.
homepage: https://docs.smartthings.com
metadata: { "openclaw": { "emoji": "🔌", "requires": { "bins": ["curl"] } } }
---

# Smart Plugs (SmartThings)

Control your SmartThings plugs and Tapo power strip.

Required env:

- `ST_ACCESS_TOKEN`
- optional `ST_API_BASE` (default: `https://api.smartthings.com/v1`)

## Device IDs

- `PLUG3` = `5404d8ec-6265-471d-9ff6-e0aea8590689` (Matter OnOff Plug 3)
- `PLUG4` = `8e401318-c00b-4b5c-985a-858457f43f93` (Matter OnOff Plug 4)
- `PLUG2` = `756b7838-e915-4c78-9c96-336a92ea2a5c` (Matter OnOff Plug 2)
- `TAPO_STRIP` = `d3dd4ee4-579d-41e3-ba1b-3b46947ae08f` (Tapo Smart Power Strip)

## Turn all plugs ON

```bash
export ST_API_BASE="${ST_API_BASE:-https://api.smartthings.com/v1}"
for DEVICE_ID in \
  5404d8ec-6265-471d-9ff6-e0aea8590689 \
  8e401318-c00b-4b5c-985a-858457f43f93 \
  756b7838-e915-4c78-9c96-336a92ea2a5c \
  d3dd4ee4-579d-41e3-ba1b-3b46947ae08f
do
  curl -fsSL -X POST \
    -H "Authorization: Bearer $ST_ACCESS_TOKEN" \
    -H "Content-Type: application/json" \
    "$ST_API_BASE/devices/$DEVICE_ID/commands" \
    -d '{"commands":[{"component":"main","capability":"switch","command":"on"}]}' >/dev/null
  echo "ON $DEVICE_ID"
done
```

## Turn all plugs OFF

```bash
export ST_API_BASE="${ST_API_BASE:-https://api.smartthings.com/v1}"
for DEVICE_ID in \
  5404d8ec-6265-471d-9ff6-e0aea8590689 \
  8e401318-c00b-4b5c-985a-858457f43f93 \
  756b7838-e915-4c78-9c96-336a92ea2a5c \
  d3dd4ee4-579d-41e3-ba1b-3b46947ae08f
do
  curl -fsSL -X POST \
    -H "Authorization: Bearer $ST_ACCESS_TOKEN" \
    -H "Content-Type: application/json" \
    "$ST_API_BASE/devices/$DEVICE_ID/commands" \
    -d '{"commands":[{"component":"main","capability":"switch","command":"off"}]}' >/dev/null
  echo "OFF $DEVICE_ID"
done
```

## Toggle one plug (example)

```bash
DEVICE_ID="d3dd4ee4-579d-41e3-ba1b-3b46947ae08f"
curl -fsSL -X POST \
  -H "Authorization: Bearer $ST_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  "$ST_API_BASE/devices/$DEVICE_ID/commands" \
  -d '{"commands":[{"component":"main","capability":"switch","command":"off"}]}'
```
