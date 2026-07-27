# BlueprintMCP — Prebuilt (UE 5.6, Win64)

This is the **compiled, drop-in build** of the BlueprintMCP plugin. It ships with
`Binaries/Win64/` already built, so you can use it in a **Blueprint-only** UE project
without a C++ toolchain or a compile-on-open step.

- **Engine:** Unreal Engine **5.6**
- **Platform:** **Win64** (editor)
- **Source repo:** https://github.com/hoodtronik/Unreal-MCP-Ultra

> Built from [`hoodtronik/Unreal-MCP-Ultra`](https://github.com/hoodtronik/Unreal-MCP-Ultra)
> (formerly `ue5-mcp`) @ commit
> [`a897274`](https://github.com/hoodtronik/Unreal-MCP-Ultra/commit/a897274) (branch `main`).
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
> **Also new — lighting tools.** `list_lights` reports every light in the level with its type,
> mobility, intensity, colour, temperature and type-specific settings; there was previously no way
> to enumerate a level's lighting at all. `spawn_light` creates and configures a
> directional / point / spot / rect / sky light in one call, and `set_light_property` changes one by
> label with per-type validation. `get_renderer_state` reports the GI, reflection and shadow-map
> methods plus path tracing, Lumen hardware ray tracing, MegaLights and auto-exposure, read from
> live console variables rather than the project `.ini`. These write light properties directly
> rather than through the engine's `Set*` light functions, which are runtime APIs that silently
> no-op on Static lights — and, for attenuation radius and spot cone angles, on Stationary lights
> too, which is the default mobility for a newly placed light.
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

## Notes

- Binaries are tracked in this repo on purpose (`*.pdb` is excluded to keep it lean).
- If your engine version differs from 5.6, rebuild from the
  [source repo](https://github.com/hoodtronik/Unreal-MCP-Ultra) instead — prebuilt binaries are
  engine-version-specific.
- The MCP server bridge under `Tools/` still needs `npm install && npm run build`
  if you want to connect an MCP client; see the source repo for setup.

## License

[MIT](LICENSE) — same as the source project.
