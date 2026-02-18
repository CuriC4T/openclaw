---
name: room-lights
description: Turn your room Hue lights on/off in SmartThings.
homepage: https://docs.smartthings.com
metadata: { "openclaw": { "emoji": "💡", "requires": { "bins": ["curl"] } } }
---

# Room Lights (SmartThings)

Control your room lights through SmartThings.

Required env:

- `ST_ACCESS_TOKEN`
- optional `ST_API_BASE` (default: `https://api.smartthings.com/v1`)

## Device IDs

- `BED` = `ab7841c9-6ad3-4714-804e-a351f3b69b1a` (침대)
- `DESK_BACK` = `37dff0f6-125b-4553-afa3-f0d1320934fa` (책상 뒤)
- `LEFT` = `5f0724fb-46ea-46a1-997c-1c4610fea32a` (컴 왼쪽)
- `BACK` = `881d870a-6755-4ff2-bd7f-995aa6239b00` (등 뒤)
- `RIGHT` = `c2de5edb-6f5e-439a-895a-288240a22b5d` (컴 오른쪽)

## Turn all room lights ON

```bash
export ST_API_BASE="${ST_API_BASE:-https://api.smartthings.com/v1}"
for DEVICE_ID in \
  ab7841c9-6ad3-4714-804e-a351f3b69b1a \
  37dff0f6-125b-4553-afa3-f0d1320934fa \
  5f0724fb-46ea-46a1-997c-1c4610fea32a \
  881d870a-6755-4ff2-bd7f-995aa6239b00 \
  c2de5edb-6f5e-439a-895a-288240a22b5d
do
  curl -fsSL -X POST \
    -H "Authorization: Bearer $ST_ACCESS_TOKEN" \
    -H "Content-Type: application/json" \
    "$ST_API_BASE/devices/$DEVICE_ID/commands" \
    -d '{"commands":[{"component":"main","capability":"switch","command":"on"}]}' >/dev/null
  echo "ON $DEVICE_ID"
done
```

## Turn all room lights OFF

```bash
export ST_API_BASE="${ST_API_BASE:-https://api.smartthings.com/v1}"
for DEVICE_ID in \
  ab7841c9-6ad3-4714-804e-a351f3b69b1a \
  37dff0f6-125b-4553-afa3-f0d1320934fa \
  5f0724fb-46ea-46a1-997c-1c4610fea32a \
  881d870a-6755-4ff2-bd7f-995aa6239b00 \
  c2de5edb-6f5e-439a-895a-288240a22b5d
do
  curl -fsSL -X POST \
    -H "Authorization: Bearer $ST_ACCESS_TOKEN" \
    -H "Content-Type: application/json" \
    "$ST_API_BASE/devices/$DEVICE_ID/commands" \
    -d '{"commands":[{"component":"main","capability":"switch","command":"off"}]}' >/dev/null
  echo "OFF $DEVICE_ID"
done
```

## Toggle one light (example)

```bash
DEVICE_ID="ab7841c9-6ad3-4714-804e-a351f3b69b1a"
curl -fsSL -X POST \
  -H "Authorization: Bearer $ST_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  "$ST_API_BASE/devices/$DEVICE_ID/commands" \
  -d '{"commands":[{"component":"main","capability":"switch","command":"on"}]}'
```
