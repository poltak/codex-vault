# Blender MCP

Removed on 2026-06-17 to restore a vanilla demo state.

- Removed Codex global MCP entry `blender`.
- Removed Blender user extension `mcp`.
- Removed `/Users/lamhucminh/.codex/mcp/blender-lab-1.0.0`.
- Removed installer artifacts from the Codex thread workspace.
- Set Blender `use_online_access` back to `False`.

Historical install details below are kept only as reinstall notes.

Installed on 2026-06-15.

- Blender app: `/Applications/Blender.app`, verified `Blender 5.1.2`.
- Official Blender Lab add-on installed from `mcp-1.0.0.zip` into Blender user extension repo `user_default`.
- Blender preference `use_online_access` is enabled; the add-on needs this even for localhost.
- Official MCPB package unpacked at `/Users/lamhucminh/.codex/mcp/blender-lab-1.0.0`.
- Codex global MCP entry `blender` runs:
  `/Users/lamhucminh/.local/bin/uv run --project /Users/lamhucminh/.codex/mcp/blender-lab-1.0.0 blender-mcp`
- Codex global MCP entry includes `BLENDER_PATH=/Applications/Blender.app/Contents/MacOS/Blender`; the `_for_cli` tools need this because `blender` is not on PATH.

Smoke test passed by starting Blender headless with `--command blender_mcp`, invoking the MCP `execute_blender_code` tool, creating `Codex_MCP_Smoke_Cube` and `Codex_MCP_Test_Area_Light`, then saving `/Users/lamhucminh/Documents/Codex/2026-06-15/so-i-just-installed-blender-on/outputs/blender-mcp-smoke.blend`.

Do not use PyPI `uvx blender-mcp` for this setup. That resolved to the community package and mismatched Blender Lab's null-byte socket protocol, causing `Incomplete JSON response received`.
