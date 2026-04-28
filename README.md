# IPTV Services Tested — 2026 Lab Cycle

> *Methodology, instruments, raw findings. Stream Lab HQ test cycle Q2 2026.*

This is a methodology document with results, not a review. The intent is that
another lab could rerun the same protocol and get within ±5% of these numbers.

## Test bench

- **Bench A**: Fire TV Stick 4K Max, firmware 7.6.x, factory reset before
  cycle, only the IPTV-under-test app installed plus a screen-recorder
- **Bench B**: Apple TV 4K (3rd gen), tvOS 18.x, same isolation
- **Bench C**: Samsung Tizen 7.0 65" TV, native HLS playback where supported

All three benches connected to a managed 1 Gbps fibre line, no other
household traffic during test windows.

## Protocol per service

```
T+0       cold install, sign in, log credentials issuance time
T+0..30m  channel inventory walk: load each EPG channel, log first-frame time
T+30m..2h continuous play of a sports channel, capture every 5min via ffprobe
T+2h..3h  controlled throttle — 50/25/10/5 Mbps caps — log adaptation
T+3h..4h  cold-start playback five times; average startup latency
T+24h     repeat 4-hour window
```

We run this protocol on each bench once per service, weekly for 4 weeks.

## What we report (per service)

1. **Inventory honesty** — channels claimed vs. channels that load
2. **Delivered bitrate distribution** — median, p10, p90
3. **Buffer rate** — events per hour at full bandwidth and at 5 Mbps cap
4. **Failover behaviour** — does the player drop quality cleanly or stall?
5. **EPG drift** — % of slots where the playing programme didn't match the
   published guide

We do *not* report "value", "vibes", or any composite score. Buyers can
weight the numbers themselves.

## Findings — Q2 2026 cycle (summary)

The full per-service breakdown is in `findings/` (publishing on a rolling
basis as cycles complete). High-level patterns this cycle:

- Three of twelve services dropped >40% of their advertised channel count
  on first walk — these were not the cheapest services
- Median delivered bitrate at unrestricted bandwidth: **18 Mbps** for the top
  quartile, **8 Mbps** for the bottom quartile
- Two services failed the throttle test by stalling instead of stepping
  down quality — both lost their stream entirely below 10 Mbps
- Buffer-rate at unrestricted bandwidth was a poor predictor of buffer-rate
  at 5 Mbps; tested both, they don't correlate

## Related work

- Methodology document (this README) — supersedes 2025 cycle protocol
- Test scripts: `scripts/` (planned, releasing as redacted variants)
- Reference customer-facing summary: [streamreviewhq.com](https://streamreviewhq.com)
