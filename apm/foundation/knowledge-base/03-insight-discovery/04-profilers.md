# Profilers (UI usage)

> **Domain 3 — Insight Discovery**
> *Setup is covered in Domain 2 — this file focuses on using the data.*

## Navigating the Profiler UI

| Tab / View          | Use it to…                                                            |
| ------------------- | --------------------------------------------------------------------- |
| **Profile Search**  | Filter profiles by service, env, version, host, custom tags.          |
| **Service profile**  | Aggregate flame graph for a service over a time range.               |
| **Profile comparison** | Diff two periods or two versions.                                  |
| **Endpoints / Methods** | Top hot methods, sortable by CPU, allocations, etc.                |
| **Trace ↔ Profile** | Jump from a slow span to the profile sample taken during it.          |

## Flame graph (default visualization)

- Each row = a stack frame; each block's **width = consumed resource** (CPU time, allocations, etc.).
- Click a frame to filter the graph to that subtree.
- Switch the metric (CPU / Wall / Allocations / Lock) using the dropdown.
- Reverse the flame graph (icicle/bottom-up) to see costliest leaves first.

## Other visualizations

| Visualization     | Best for                                                  |
| ----------------- | --------------------------------------------------------- |
| **Hotspots**      | Quick top-N most expensive functions list.                |
| **Source code**   | Hot lines highlighted inline with the source.             |
| **Comparison**    | Verify a fix actually reduced CPU/allocations.            |
| **Timeline**      | Per-thread profiling over time (Java, Node).              |
| **Top down / Bottom up** | Same flame data viewed from leaf-up or root-down.    |

## Profile types per language (typical)

| Type            | Java | Python | Node | Go | .NET | Ruby |
| --------------- | ---- | ------ | ---- | -- | ---- | ---- |
| CPU             | ✅   | ✅     | ✅   | ✅ | ✅   | ✅   |
| Wall            | ✅   | ✅     | ✅   | ✅ | ✅   | ✅   |
| Allocations     | ✅   | ✅     | ✅   | ✅ | ✅   | ✅   |
| Lock contention | ✅   | —      | —    | ✅ | ✅   | —    |
| Exceptions      | ✅   | —      | —    | —  | ✅   | —    |

## Profile ↔ trace correlation

- The tracer tags each profile sample with the active trace and span ID.
- In the **Trace view → Code Hotspots** tab, see which functions consumed time during *this specific* trace.
- In the **Profile view**, click "View traces" to jump to slow requests covering that frame.

## Embedding in dashboards

Use the **Profiling Flame Graph** widget to pin a profile view to a dashboard, scoped by tags.

## Things to remember for the exam

- The default visualization is the **flame graph**.
- Profile data is correlated to traces via the **trace/span ID** baked into each sample.
- A profile-comparison view is the canonical way to **prove a deploy improved (or regressed) performance**.
- Profiling adds roughly **~1% CPU overhead** target.
