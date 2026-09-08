# Fully Synthetic End-to-End Example

## Synthetic specification
- Device: synthetic pressure controller (SPC-01)
- Goal: validate startup, setpoint update, and safe shutdown sequence

## Synthetic prompt excerpt
Generate an ordered validation sequence from the specification and return JSON conforming to the published schema.

## Synthetic generated output
```json
{
  "document_id": "SYNTH-SPC-01",
  "steps": [
    {
      "index": 1,
      "action": "Power on SPC-01 and wait for READY indicator.",
      "expected_result": "READY indicator is ON within 10 seconds."
    },
    {
      "index": 2,
      "action": "Set pressure target to 2.0 bar through control panel.",
      "expected_result": "Displayed target reads 2.0 bar."
    },
    {
      "index": 3,
      "action": "Issue shutdown command from control panel.",
      "expected_result": "System enters SAFE-OFF state with no active alarms."
    }
  ]
}
```

## Synthetic verification notes
- Coverage: all required phases (startup/update/shutdown) represented
- Safety: no hazardous transition introduced
