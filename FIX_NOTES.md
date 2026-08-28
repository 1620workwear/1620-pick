# fix/print-reliability — work notes (commit 1)

This file was added by the Copilot assistant as the first commit on branch `fix/print-reliability`.

Goal: stabilize printing and scanning reliability for the 1620 Picking app.

Planned first-phase changes (high level):

1. Diagnostics & print-state UI
   - Diagnostics panel capturing Bluetooth connect/disconnect events, chunk write attempts, CSV parsing warnings, and timestamps.
   - Local log buffer with "Download logs" button for sharing traces.
   - Clear visible printer state: Connected / Sending / Retrying / Failed / OK. Disable "mark picked" until print success.

2. Scanner normalization & input robustness
   - Normalize scanned input (trim CR/LF/control chars, strip common scanner suffixes), handle oninput and onpaste, and show raw scanned value in diagnostics.

3. BLE write tuning & retry improvements
   - Configurable CHUNK_SIZE and CHUNK_DELAY defaults (smaller chunk, longer delay), safer chunk loop, and an exponential-backoff reconnect + retry.

4. ZPL test-mode and size guard
   - Simple label toggle to omit embedded GFA/logo for smaller transfers and faster debug prints.

5. CSV preview / small robustness fixes
   - Show sample rows, validate column counts, and warn on irregular rows.

6. Per-order telemetry and reprint audit
   - Store per-order lastPrint metadata (timestamp, bytesSent, attempts) in localStorage.

How to test after this commit:
- Check this branch: https://github.com/1620workwear/1620-pick/tree/fix%2Fprint-reliability
- I will push a follow-up commit that modifies the HTML to add the diagnostics panel and logging hooks.
- After the follow-up commit, please run the following on a test device:
  - Open the app from this branch and connect to your printer.
  - Press "Print Test Label" and "Print Simple Label" (if available) and capture the logs via "Download logs".
  - If a print fails, attach the downloaded logs here and I will tune chunk size / delay / retry settings for your hardware.

Requesting device & printer details for tuning:
- Exact printer model(s): e.g., Zebra ZQ320, ZQ630, Brother RJ-4250WB
- Devices & browsers used by pickers: e.g., Zebra TC52 with Chrome, Samsung A-series + Chrome, iOS devices (iOS Safari lacks Web Bluetooth)
- Whether scanners are keyboard-wedge (act like keyboard) or camera-based / Bluetooth HID.

Next action (I will perform):
- Commit 2: modify `1620-picking-app(7).html` to add the diagnostics UI and log hooks and update Bluetooth write behavior to use configurable chunk settings and to log events. After pushing, I will ask you to run a quick print test and provide logs.

Committed by: Copilot assistant
Timestamp: PLACEHOLDER — commit made via API
