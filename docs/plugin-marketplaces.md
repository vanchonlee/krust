# Krust plugin marketplaces

Krust follows the Codex desktop plugin lifecycle: marketplaces are explicit sources, installed plugins are copied into immutable version directories, and the installed-state registry chooses the active version. Krust does not contact Git or the network while loading plugins at startup.

## Plugin format

Marketplace plugins are directories containing `.codex-plugin/plugin.json`. Skills live below `skills/`, while MCP configuration uses standard `.mcp.json` input interpolation. Provider-specific URL routing belongs in the plugin artifact, not in Krust core.

Only the official marketplace source embedded by Krust may activate `nativeTools`. A marketplace cannot gain that permission by naming itself `krust-official`.

Krust owns Keychain credentials, MCP lifecycle, Kubernetes context, and approval policy. Marketplace files never contain or receive credential values. Removing a plugin does not delete its Keychain credentials.

## Official marketplace

The catalog is published at `plugins/krust-official/.agents/plugins/marketplace.json`. Krust pins this repository and subdirectory as the trusted first-party source.

Plugin versions are immutable. Publishing changed contents requires incrementing the version in `.codex-plugin/plugin.json`; clients reject different contents under an already-installed version.

## Security boundaries

- Plugin paths are relative to the marketplace root and must not escape it.
- Symlinks, oversized files, oversized plugin trees, and hard-linked files are rejected by the host installer.
- Credentials are stored by Krust in Keychain, not in marketplace JSON or plugin files.
- Marketplace refresh is an explicit operation; startup uses only the local installed-state registry.
- Git is executed directly without shell interpolation.
- `nativeTools` are accepted only when provenance matches the host-controlled official repository and subdirectory.
- Failed authentication, transport, validation, or updates remain visible; the host does not silently fall back.

## First-party plugin development

Plugin-only changes do not require rebuilding the Krust application. Increment the plugin version, validate the artifact with the bundled Krust CLI, install it from a local marketplace, and refresh the plugin catalog.
