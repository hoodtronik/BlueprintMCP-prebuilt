# BlueprintMCP — Prebuilt (UE 5.6, Win64)

This is the **compiled, drop-in build** of the BlueprintMCP plugin. It ships with
`Binaries/Win64/` already built, so you can use it in a **Blueprint-only** UE project
without a C++ toolchain or a compile-on-open step.

- **Engine:** Unreal Engine **5.6**
- **Platform:** **Win64** (editor)
- **Source repo:** https://github.com/hoodtronik/ue5-mcp

> Built from [`hoodtronik/ue5-mcp`](https://github.com/hoodtronik/ue5-mcp) @ commit
> [`27b1525`](https://github.com/hoodtronik/ue5-mcp/commit/27b1525) (branch `main`) — includes a
> fix for [mirno-ehf/ue5-mcp#70](https://github.com/mirno-ehf/ue5-mcp/issues/70): `add_node` on an
> Animation/Actor Blueprint override event (e.g. `BlueprintUpdateAnimation`) could silently lose
> its wiring later, because UE's auto-placed "ghost" stub event was only weakly promoted when a
> connection was made to it. `add_node` now permanently promotes the node the same way a real
> "implement this override" editor action does. `Binaries/Win64/` is a fresh build including this
> fix. Also carries everything TypeScript-only from the prior refresh: 4 new MCP tools
> (`rebuild_groom_bindings`, `list_mirror_table_rows`, `set_mirror_table_rows`,
> `remove_mirror_table_rows`) with a TS/C++ route-parity test; `discover_python_class` /
> `discover_python_search`; three new skill packs (Niagara, Groom, Mirror Data Tables); a batch-mode
> schema fix across 4 tools with a standing invariant test; and `refresh_agent_config`. For source,
> contribution, and the MCP server details, see that repo.

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
  [source repo](https://github.com/hoodtronik/ue5-mcp) instead — prebuilt binaries are
  engine-version-specific.
- The MCP server bridge under `Tools/` still needs `npm install && npm run build`
  if you want to connect an MCP client; see the source repo for setup.

## License

[MIT](LICENSE) — same as the source project.
