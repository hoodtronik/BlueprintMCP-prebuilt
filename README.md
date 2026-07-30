# BlueprintMCP — Prebuilt (UE 5.6, Win64)

This is the **compiled, drop-in build** of the BlueprintMCP plugin. It ships with
`Binaries/Win64/` already built, so you can use it in a **Blueprint-only** UE project
without a C++ toolchain or a compile-on-open step.

- **Engine:** Unreal Engine **5.6**
- **Platform:** **Win64** (editor)
- **Source repo:** https://github.com/hoodtronik/Unreal-MCP-Ultra

> Built from [`hoodtronik/Unreal-MCP-Ultra`](https://github.com/hoodtronik/Unreal-MCP-Ultra)
> (formerly `ue5-mcp`) @ commit
> [`075904f`](https://github.com/hoodtronik/Unreal-MCP-Ultra/commit/075904f) (branch `main`).
>
> **New in this build — inline-image vision tools.** `viewport_capture` returns what the editor is
> showing **inline in the tool result** as a PNG image block rather than writing a file and handing
> back a path, so an agent can look at the level viewport, a running PIE session, or a Blueprint
> node graph without spending extra tool calls on a file round-trip. `vision_mode` makes that
> automatic: while enabled, every state-changing tool call gets a fresh frame appended, with the
> capture target inferred from each tool's own arguments (graph edits show the graph, level edits
> show the level) and unchanged frames suppressed by pixel digest. `scene_digest` provides a cheap
> change-detection fingerprint. Requires a running editor — a headless commandlet has no render
> device and says so explicitly.
>
> This build was **verified live against a running editor**, not just against the headless test
> suite, which found two bugs the headless tests structurally could not: `GetLevelViewportClients()[0]`
> is not reliably a sized viewport (which had also been silently breaking the older
> `take_screenshot`), and the duplicate-frame digest cannot be an exact pixel hash, because TAA and
> temporal accumulation mean two captures of an unchanged scene are never bit-identical. Both fixed.
>
> **Also new — lighting and rendering tools.** `list_lights` reports every light in the level with its type,
> mobility, intensity, colour, temperature and type-specific settings; there was previously no way
> to enumerate a level's lighting at all. `spawn_light` creates and configures a
> directional / point / spot / rect / sky light in one call, and `set_light_property` changes one by
> label with per-type validation. `get_renderer_state` reports the GI, reflection and shadow-map
> methods plus path tracing, Lumen hardware ray tracing, MegaLights and auto-exposure, read from
> live console variables rather than the project `.ini`. `set_renderer_mode` switches between
> `lumen`, `pathtracer` and `baked` as a coherent set, and `configure_post_process` sets exposure,
> bloom and Lumen quality on a post-process volume. `spawn_sky` builds a complete outdoor set (sun,
> sky light, SkyAtmosphere, height fog, volumetric clouds) with daylight / sunset / overcast / night
> presets, and `validate_lighting` flags the mistakes that produce a plausible-looking but wrong
> scene — an atmosphere with no sun assigned, a sky light needing recapture, zero-intensity lights,
> more than four overlapping stationary lights (which silently exhausts UE's shadow channels), and
> auto-exposure left unlocked so intensity edits appear to do nothing.
>
> Two correctness points worth knowing, both handled: the engine's `Set*` light functions are
> runtime APIs that silently no-op on Static lights — and, for attenuation radius and spot cone
> angles, on Stationary lights too, which is the default mobility for a newly placed light — so
> light properties are written directly instead. And every field of `FPostProcessSettings` is inert
> unless its paired `bOverride_` flag is also set, so `configure_post_process` always sets both and
> reports the flags back.
>
> **Materials, including Substrate.** `add_material_expression` resolves expression classes
> dynamically, so any `UMaterialExpression` subclass works — including Substrate nodes
> (`SubstrateSlabBSDF`, `SubstrateHorizontalMixing`, …), which wire into the material root's
> `Front Material` pin. Confirmed end to end against a running editor. `get_material_graph` now also
> reports each node's real `expressionClass` and a short `expressionType` that feeds straight back
> into `add_material_expression`; previously every node came back as the generic `MaterialGraphNode`
> with its identity only in a free-text title. Note that adding an expression regenerates earlier
> nodes' graph GUIDs, so re-read the graph after your last add before calling
> `connect_material_pins` — the tool descriptions spell this out.
>
> **Level management.** `open_level` and `new_level` switch to or create a level; previously the
> plugin could inspect the current level and manage sublevels but not change which one was open.
> Both refuse to run while packages are unsaved, because the underlying load runs under
> `GIsRunningUnattendedScript` (which is what makes it safe to call without a modal dialog
> deadlocking the editor) and would otherwise discard that work silently.
>
> `set_actor_property` now resolves a component by class when the literal name misses, so
> `"StaticMeshComponent.StaticMesh"` works on a freshly spawned actor whose component is really
> named `StaticMeshComponent0`. And `viewport_capture` gained `settle=true`: sky, atmosphere,
> volumetric clouds, Lumen and real-time sky-light capture converge over seconds, and a tight
> capture loop starves the editor of the ticks it needs — so an immediate capture after
> `spawn_sky` returns the pre-change frame and looks like a broken capture.
>
> All six viewport tools (both captures, camera get/set, the view-mode family, and the high-res
> screenshot) now share one viewport resolver. They previously each assumed
> `GetLevelViewportClients()[0]`, which is not reliably a realized, sized viewport — so they could
> act on a viewport nobody was looking at, and on a DIFFERENT one from the capture, making
> `set_view_mode` appear to do nothing.
>
> This build also resolves upstream issues [#63](https://github.com/mirno-ehf/ue5-mcp/issues/63),
> [#67](https://github.com/mirno-ehf/ue5-mcp/issues/67),
> [#69](https://github.com/mirno-ehf/ue5-mcp/issues/69),
> [#56](https://github.com/mirno-ehf/ue5-mcp/issues/56),
> [#26](https://github.com/mirno-ehf/ue5-mcp/issues/26),
> [#65](https://github.com/mirno-ehf/ue5-mcp/issues/65),
> [#16](https://github.com/mirno-ehf/ue5-mcp/issues/16),
> [#88](https://github.com/mirno-ehf/ue5-mcp/issues/88), and
> [#66](https://github.com/mirno-ehf/ue5-mcp/issues/66): `CallDispatcher` nodeType for `add_node`;
> cross-Blueprint `VariableGet`/`VariableSet` via `className`; `describe_graph` now surfaces
> unconnected pins with non-default literal values; a `compact` mode for `get_blueprint_graph`;
> `create_data_table`/`create_curve_table`/`create_data_asset`; `screenshot_graph` (headless render
> of a Blueprint graph, not just the 3D viewport); `bind_widget_event`; and save handlers that check
> out via source control before falling back to stripping the read-only bit. Also adds a scoped
> single-frame `get_frame_timing` snapshot. `Binaries/Win64/` is a fresh build including all of the
> above, verified live against a running editor. For source, contribution, and the MCP server
> details, see that repo.

## Install

1. Copy the `BlueprintMCP` folder into your project's `Plugins/` directory:
   `YourProject/Plugins/BlueprintMCP/`
2. Enable it in your `.uproject` (or via Edit → Plugins in the editor):
   ```json
   { "Name": "BlueprintMCP", "Enabled": true }
   ```
3. Launch the editor — it loads the precompiled binary directly, no build required.

### Optional: Riot Crowd

`RiotCrowd/` is a separate, optional plugin (Mass-based crowd simulation). The core plugin works
without it; the `riot_*` MCP tools simply report `featureInstalled: false` and print these
instructions until it is present.

It must be installed as a **sibling** of `BlueprintMCP`, not left nested inside it — Unreal's
plugin scanner stops descending once it finds a `.uplugin`, so a nested plugin is never discovered:

```
YourProject/Plugins/BlueprintMCP/
YourProject/Plugins/BlueprintMCPRiotCrowd/   <-- copy or junction RiotCrowd/ here
```

Then enable both it and `MassGameplay` in your `.uproject` and restart the editor:

```json
{ "Name": "BlueprintMCPRiotCrowd", "Enabled": true },
{ "Name": "MassGameplay", "Enabled": true }
```

Its binary is prebuilt too, and carries the same `BuildId` as the core plugin — both must come from
the same release or the editor will reject them.

## Notes

- Binaries are tracked in this repo on purpose (`*.pdb` is excluded to keep it lean).
- If your engine version differs from 5.6, rebuild from the
  [source repo](https://github.com/hoodtronik/Unreal-MCP-Ultra) instead — prebuilt binaries are
  engine-version-specific.
- The MCP server bridge under `Tools/` still needs `npm install && npm run build`
  if you want to connect an MCP client; see the source repo for setup.

## License

[MIT](LICENSE) — same as the source project.
