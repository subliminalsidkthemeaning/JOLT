# JOLT 0.1.2 performance

This adjustment removes song-link construction and serialization. The existing audio engine, Performance Mode, caches, IPC worker, IO completion waits, update coalescing and reconnect deadlines are unchanged. No additional process, thread, timer, service or dependency was introduced. The payload receipt is written at most once per connection after a successful acknowledged Download JOLT activity.

The queue timeline uses the existing virtualized scrolling list. It exposes at most 100 past rows, retains current/upcoming ordering and reuses already loaded artwork. Restoring history does not eagerly fetch old covers. Growing radio history and old autoplay additions are trimmed, including their shuffle/continuation bookkeeping; finite source playlists remain intact for Repeat All. Cursor changes trigger scrolling once, with no per-frame or periodic scroll work.

For reference, the previous 0.1.1 Discord ON release measured 60.83 MiB mean working set, 22.95 MiB mean private bytes and 0.182% machine CPU over 32.211 seconds on a 12-logical-processor Windows PC, with one JOLT process in every observation. These are historical measurements, not a new 0.1.2 result. Windows residency, artwork/cache state and other applications can vary; they do not establish an exact causal overhead.

Final Windows x64 optimized portable 0.1.2 measurements use Performance Mode, the same isolated profile and Get Lucky track, muted/minimized online playback, a 15-second startup wait, then 30 process-tree observations. CPU is process CPU-time delta divided by elapsed time and 12 logical processors. The ON run preceded OFF. The user's installed copy and other applications remained running outside the measured process tree.

| Release / scenario | Interval | Mean working set | Mean private bytes | Machine CPU | Process count |
|---|---:|---:|---:|---:|---:|
| Historical 0.1.0, Discord OFF | 32.370 s | 70.64 MiB | 21.58 MiB | 0.129% | 1 |
| Historical 0.1.1, Discord ON | 32.211 s | 60.83 MiB | 22.95 MiB | 0.182% | 1 |
| Final 0.1.2, Discord OFF | 32.307 s | 62.94 MiB | 23.04 MiB | 0.117% | 1 |
| Final 0.1.2, Discord ON and acknowledged | 32.360 s | 72.62 MiB | 23.19 MiB | 0.121% | 1 |

Within the paired 0.1.2 observations, ON measured 0.145 MiB more private memory, 9.679 MiB more working set, and 0.0038 percentage points more machine CPU. Working set includes resident shared/mapped pages and varies with Windows residency and cache conditions; these single intervals do not isolate exact causal Discord overhead or establish a guarantee. They do not reproduce the user's earlier single Task Manager observation, which used different conditions and potentially a different memory metric.

Tree peaks: OFF 65.75 MiB working set / 26.97 MiB private bytes; ON 74.54 / 26.00 MiB. Every observation had exactly one process. Startup helper/version checks are excluded by the initial wait. The packaged Microsoft runtime modules loaded from the portable folder. Online audio playback reached Playing; the Local API remained disabled. The actual local Discord client acknowledged the ON activity with remote album artwork, timestamps, JOLT branding, no song click-through fields, and exactly the verified Download JOLT button. Both isolated verification processes were stopped afterward; the user's installed player was left running.

The 100-entry history bound and preservation of finite source lists, shuffle and replay are covered by queue/player tests. These steady playback samples are not a separate long-history UI memory benchmark. Fresh-machine installer/upgrade/uninstall and a second-viewer button click remain manual checks.
