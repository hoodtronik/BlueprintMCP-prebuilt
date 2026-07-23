# BlueprintMCP — Prebuilt (UE 5.6, Win64)

This is the **compiled, drop-in build** of the BlueprintMCP plugin. It ships with
`Binaries/Win64/` already built, so you can use it in a **Blueprint-only** UE project
without a C++ toolchain or a compile-on-open step.

- **Engine:** Unreal Engine **5.6**
- **Platform:** **Win64** (editor)
- **Source repo:** https://github.com/hoodtronik/ue5-mcp

> Built from [`hoodtronik/ue5-mcp`](https://github.com/hoodtronik/ue5-mcp) @ commit
> [`b67783f`](https://github.com/hoodtronik/ue5-mcp/commit/b67783f) (branch `main`) —
> adds **`build_graph`**: create many nodes, wire them together, and set pin defaults in a
> single call, with one compile+save for the whole batch instead of one per node. Built on
> top of the PCG graph-authoring tools, the skills / examples / discovery systems, and the
> `set_material_scalar_default` / `reset_transaction_buffer` endpoints. For source,
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
