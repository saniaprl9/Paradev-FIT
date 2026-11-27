# Development

Run the dev server without env validation or auth:

```bash
SKIP_ENV_VALIDATION=1 bun run dev
```

This skips environment variable validation and the sign-in screen. Desktop chat also falls back to local-only session bootstrap in this mode, so you can test chat/streaming without the cloud API as long as you have local model credentials configured.

# Release

When building for release, make sure `node-pty` is built for the correct architecture with `bun run install:deps`, then run `bun run release`.

# Linux (AppImage) local build

From `apps/desktop`:

```bash
bun run clean:dev
bun run compile:app
bun run package -- --publish never --config electron-builder.ts
```

Expected outputs in `apps/desktop/release/`:

- `*.AppImage`
- `*-linux.yml` (Linux auto-update manifest)

# Linux auto-update verification (local)

From `apps/desktop` after packaging:

```bash
ls -la release/*.AppImage
ls -la release/*-linux.yml
```

If both files exist, packaging produced the Linux artifact + updater metadata that `electron-updater` expects.

# Windows (.exe)

The Windows NSIS `.exe` installer is produced by the `build-windows` job in the `Build Desktop App` CI workflow (`.github/workflows/build-desktop.yml`), which runs on `windows-latest`. It is downloadable from the workflow run's artifacts under the name `<artifact_prefix>-windows-x64` (for example `desktop-windows-x64`).

Local Windows builds are not supported on Linux or macOS hosts; use the CI job to produce the installer.
