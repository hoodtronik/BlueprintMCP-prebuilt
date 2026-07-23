# BlueprintMCP — Prebuilt (UE 5.6, Win64)

This is the **compiled, drop-in build** of the BlueprintMCP plugin. It ships with
`Binaries/Win64/` already built, so you can use it in a **Blueprint-only** UE project
without a C++ toolchain or a compile-on-open step.

- **Engine:** Unreal Engine **5.6**
- **Platform:** **Win64** (editor)
- **Source repo:** https://github.com/hoodtronik/Unreal-MCP-Ultra

> Built from [`hoodtronik/Unreal-MCP-Ultra`](https://github.com/hoodtronik/Unreal-MCP-Ultra)
> (formerly `ue5-mcp`) @ commit
> [`19b6d80`](https://github.com/hoodtronik/Unreal-MCP-Ultra/commit/19b6d80) (branch `main`) —
> resolves upstream issues [#63](https://github.com/mirno-ehf/ue5-mcp/issues/63),
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
