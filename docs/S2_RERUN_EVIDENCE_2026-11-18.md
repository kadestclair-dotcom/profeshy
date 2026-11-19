# S2 re-run: evidence record (2026-11-18)

**Purpose:** the custody record for the S2 re-run (`TASK_S2_BATTERY_RERUN_2026-11-18.md`, responding to review #28). Phase 2 cites this file instead of re-deriving fingerprints. **Evidence files: never edit, rename or delete.**
**Compiled by:** Claude (reviewer), 2026-11-18, from the files in this folder and the owner's Termux output.

## Harness and inputs

| File | SHA-256 | Note |
|---|---|---|
| `s2_battery.py` | `7c372413c6758c51f7e7f655c5d23a8f8bfd5b2671723970d5aa3da7c210992f` | r2, approved by the reviewer before any real run. Every log below cites this hash |
| `s2_house113.db` | `f765211ca4b7637f2734d6e6386bf5eb040f9563513a16716e8220235991c899` | The Part A database (184,320 bytes, 5,015 rows). The phone copy matched it (phone logs, P1) |
| `s2_aggregate_house113.json.gz` | `4fd530f63e45a8471e886541d46a209aa33358c8889d7060a7903db8c021caef` | Exported by the desktop run; 64,184 bytes gzip, 533,193 bytes JSON. Downloaded by the phone with a matching hash (P2b) |

## Runs, in order (all times PST, 2026-11-18)

| Time | Where | Log | SHA-256 | Outcome |
|---|---|---|---|---|
| 16:48:21 | PC, Windows 11, Python 3.12.10 | `s2b_desktop_20261118-164821.json` | `e253d8a441c24e2b4300f179494abdb28f54412f6a385320d43782b13abc2406` | COMPLETE |
| | | `s2b_desktop_20261118-164821.txt` | `6da2020078f0690f964bf033dbc5486ddc7b14cbda53e1f12ad3722eeeea668c` | |
| 16:52:47 | Phone, Termux, Python 3.14.10 | `s2b_phone_20261118-165247.json` | `a1c4c9ca6002c9394af5f82bee5a9d4d00b5af656c21f8ceff7dcbeeeb6183f4` | FAILED: download timed out (serve mode not yet started) |
| | | `s2b_phone_20261118-165247.txt` | `e67a105c934969e8c3644510a488d430be0a41211d1d7474166437f522415d3b` | |
| 16:54:50 | PC | `s2b_serve_20261118-165450.json` | `42dc0e1816e163c690d552999347b341e4d0d868ead42c29a0abcfa7a5ce6dde` | COMPLETE: one request, from 192.168.0.6 at 17:05:56, 200 |
| | | `s2b_serve_20261118-165450.txt` | `c47ac5bacb1dcdb2236c05be49984022bffd02ff3aa0ae7f7f45d168884a91cf` | |
| 16:55:09 | Phone | `s2b_phone_20261118-165509.json` | `f3b84788f5d1e88c291f83837748f25ee93014063f5cd5a5042a99b8dd501314` | FAILED: download timed out (blocked by the PC, see below) |
| | | `s2b_phone_20261118-165509.txt` | `2d680af62430efe2d3a29b3919e087e51aca471cb1372ad81ca19f8a848533d7` | |
| 17:05:56 | Phone | `s2b_phone_20261118-170556.json` | `c643f2054ba2e0899bb2555ee53f10009052b8ccfdf21e9c17e533328466b3ed` | **COMPLETE: the evidence run** |
| | | `s2b_phone_20261118-170556.txt` | `3c069efe785f092c6d04f0dd3ef6cf900513672ae3afda9149bd75e4dbd7d576` | |

**Why two phone runs failed.** Run 1: the owner started the phone run before serve mode. Run 2: serve mode was running (its log shows no request arrived), but the PC's Wi-Fi was set to Public and no inbound firewall rule allowed Python. The owner set the Wi-Fi to Private and added a temporary rule, covering only Python 3.12's `python.exe`, TCP port 8765 and the Private profile. Run 3 then completed. The rule was removed after the run. Neither failure touched any data; both logs stay as evidence. Each failed run stopped at the download step, as the harness is designed to.

**The refreshed database.** The phone's working copy `s2_refresh_work.db` (176,128 bytes) was uploaded by the owner. Its SHA-256 `148d76e69de3eb4914e4a799f91c7323df28f0d916a7b4071fb011514a062216` matches the value the 17:05:56 run recorded (P2d). It is a working file, not evidence, and was not added to this folder.

## The original Part B logs (2026-11-14), added at review #28's request (finding A4)

| File | SHA-256 |
|---|---|
| `s2_partb_log.json` | `d7d9cb95eaf8f596bb9c0d5da2420c9403bec1ade1d0dc4404e0434fc6859a09` |
| `s2_partb_log.txt` | `55fcc286391548f8587f10ca0bf271c343942a8bf6ebd364b3c1e9c24549b259` |

Their figures match `docs/S2_RESULTS_2026-11-14.md` Part B exactly: open 5.42 ms; lookup 3.3, top-50 cheapest 3.6, count 3.55, top-50 most-listed 5.17 ms; device SM-G975U, Android 12, Python 3.14.10.

**Limits of these old logs:**
- They record no script hash, and the run's start and finish carry the same timestamp (20:41:38).
- `s2_partb.py` itself is not in the evidence set.
- The text log's last line says to send the logs "back to Muse", which suggests Part B was guided by another assistant.

## Custody chain

Before transfer, the owner ran `sha256sum` in Termux and sent a screenshot. The reviewer transcribed the fingerprints and confirmed the transcription against `s2b_phone_cmd.sh`, the one file that exists on both devices (`3c1d758883304a1360e2fd1e8ed920edf5a966866e03fd7e1af047a6846a03d6`). The owner then uploaded the eight files through the Claude app. Each upload matched its phone fingerprint, and each matched again after being written to this folder. **8/8 verified.**

## Run conditions to report (Phase 2)

- **Phone battery** was about 9–20% and charging during the runs (owner's screenshots). Whether battery saver was on was not recorded. Today's lookup times (5–8 ms) are slower than Part B's 3.3 ms. Low-battery throttling is a possible cause, a **Hypothesis**, not tested.
- **The phone was 192.168.0.6 and the PC 192.168.0.5**, on the same home Wi-Fi. The download covered the LAN, not the internet.
