# BlueprintMCP — Prebuilt (UE 5.6, Win64)

This is the **compiled, drop-in build** of the BlueprintMCP plugin. It ships with
`Binaries/Win64/` already built, so you can use it in a **Blueprint-only** UE project
without a C++ toolchain or a compile-on-open step.

- **Engine:** Unreal Engine **5.6**
- **Platform:** **Win64** (editor)
- **Source repo:** https://github.com/hoodtronik/ue5-mcp

> `Binaries/Win64/` is built from [`hoodtronik/ue5-mcp`](https://github.com/hoodtronik/ue5-mcp) @
> commit [`a9c48f7`](https://github.com/hoodtronik/ue5-mcp/commit/a9c48f7) (branch `main`) —
> adds **`build_graph`**: create many nodes, wire them together, and set pin defaults in a
> single call, with one compile+save for the whole batch instead of one per node. Built on
> top of the PCG graph-authoring tools, the skills / examples / discovery systems, and the
> `set_material_scalar_default` / `reset_transaction_buffer` endpoints.
>
> `Source/`, `Config/`, and `Tools/` are refreshed to
> [`d27359a`](https://github.com/hoodtronik/ue5-mcp/commit/d27359a) — TypeScript-only changes
> across several commits: 4 new MCP tools (`rebuild_groom_bindings`, `list_mirror_table_rows`,
> `set_mirror_table_rows`, `remove_mirror_table_rows`) with a TS/C++ route-parity test;
> `discover_python_class` / `discover_python_search` (structured Python API introspection over
> the `run_python` bridge); three new skill packs (Niagara, Groom, Mirror Data Tables); a fix for
> 4 tools whose `batch` mode was silently unusable (schema validation rejected batch-only calls
> before the handler ever ran) plus a standing invariant test; and `refresh_agent_config`, which
> writes a managed capability-reference block into a consuming project's
> CLAUDE.md/AGENTS.md/GEMINI.md. None of this touched C++, so the prebuilt binary above is still
> the `a9c48f7` build. For source, contribution, and the MCP server details, see that repo.

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
