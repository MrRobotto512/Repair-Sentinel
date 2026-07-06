# Repair-Sentinel — shared electronics-repair knowledge base

Community-verified repair data from real bench diagnostics, collected by
[BenchTool](https://github.com/) stations at Device Hero (and, we hope,
your shop too). Every entry pairs machine telemetry (UART/syscon error
codes, POST codes, boot behavior, battery stats) with what a human
technician actually found and fixed.

## Layout

```
pending/   submitted, not yet reviewed        — do not consume
pool/      ✔ APPROVED, human-verified entries — this is the dataset
archive/   duplicates / rejected / needs-info — audit trail only
```

**Consume `pool/` only.** Everything in it was reviewed by a human.

## Entry format (one JSON file per repair)

```json
{
  "schema_version": 1,
  "session": {
    "session_id": "DH-20260705-193014-0042",
    "device_class": "Console",
    "device_model": "PlayStation 5",
    "tool": "ps5_uart",
    "symptoms_user_input": ["No Boot", "BLOD"],
    "telemetry": {"uart_syscon_error_code": "80830000", "temp_soc_c": 45.8},
    "ml_prediction": {"predicted_fault": "RAM Power Rail Short",
                      "source": "gemini", "confidence_score": 0.88},
    "technician_verification": {"probed_component": "2r2 Large Inductor",
                                "measured_value": 0.4,
                                "measured_unit": "ohms",
                                "actual_fault_confirmed": true},
    "outcome": {"repaired": true, "fix": "Replaced shorted cap on RAM rail"},
    "notes": ["[web] Died mid-game per customer; prior repair attempt visible."]
  },
  "origin": {"bench": "bench-1"},
  "verdict": "approve",
  "reviewed_utc": "2026-07-06T02:11:08Z"
}
```

## Contributing

Shops running BenchTool: point `sentinel_hub.conf` at this repo — your
benches will submit to `pending/` automatically when a tech chooses SHARE.
Everyone else: open a pull request adding a file to `pending/` in the format
above. A maintainer reviews every submission before it reaches `pool/`.

House rules: real, bench-verified repairs only; no customer-identifying
information anywhere (names, IMEIs, serials, phone numbers); one repair per
file.

## License

Data is shared to advance community repair. (Pick a license — CC0 or
CC-BY-4.0 recommended — before announcing the repo.)
