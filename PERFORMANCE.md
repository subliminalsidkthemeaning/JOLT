# JOLT 0.1.1 performance

Measurements use Windows x64 optimized builds, Performance Mode enabled, minimized muted online playback, and 30 process-tree observations over approximately 32 seconds. CPU is process CPU-time delta divided by elapsed time and 12 logical processors. Working set is resident memory; private bytes is private committed memory. Neither is interchangeable with a single Task Manager figure. Other applications remain running, so these observations are not a controlled hardware-independent guarantee.

| Release / scenario | Interval | Mean working set | Mean private bytes | Machine CPU | JOLT process count |
|---|---:|---:|---:|---:|---:|
| Existing 0.1.0, Discord OFF | 32.370 s | 70.64 MiB | 21.58 MiB | 0.129% | 1 |
| Final 0.1.1, Discord OFF | 32.243 s | 71.07 MiB | 23.53 MiB | 0.117% | 1 |
| Final 0.1.1, Discord ON and connected | 32.211 s | 60.83 MiB | 22.95 MiB | 0.182% | 1 |

Each interval contained 30 observations after startup preparation. All use minimized muted network playback of the same Get Lucky track with Performance Mode enabled. The paired 0.1.1 runs reuse one isolated profile. The Discord-compatible client available for the ON test was Vesktop; the separate client process is not included in JOLT's process-tree totals. Official Discord and a second viewer still need manual checks.

Tree peaks: 0.1.0 OFF 73.55 MiB working set / 25.37 MiB private bytes; 0.1.1 OFF 73.28 / 25.99 MiB; 0.1.1 ON 63.93 / 26.65 MiB. No additional JOLT child process appeared in these intervals. Startup helper/version checks are a separate transient workload and are excluded by the initial wait.

The lower working set in the ON sample does **not** establish a memory-saving effect. Cache content, Windows residency and other applications vary between runs. The observed ON/OFF CPU difference is 0.065 percentage points of this 12-thread machine; these single intervals do not isolate an exact causal overhead. The ON process was connected and Discord acknowledged the real track activity; normal steady playback did not send activity messages every second. Runtime module inspection confirmed the packaged Microsoft runtime DLLs were loaded from the portable distribution.

The Discord implementation reuses the existing Tokio runtime and one presence worker thread only while enabled. It creates no browser, bot, service or permanent server. IO completion events detect disconnects. Track changes/seeks are coalesced; stable playback does not send a Discord update every second. Reconnect backoff and a pending update/pause-clear deadline are the only presence scheduling deadlines.
