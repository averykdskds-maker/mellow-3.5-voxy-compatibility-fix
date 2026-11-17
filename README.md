# Mellow 3.5 — Voxy compatibility patch

A small, unofficial source patch for a rendering crash seen with **Mellow 3.5, Iris and Voxy on Minecraft 26.3**.

**Current build:** [Download Mellow.v3.5-voxy-cloudstyle3.zip (v1.0.1)](https://github.com/averykdskds-maker/mellow-3.5-voxy-compatibility-fix/releases/download/v1.0.1/Mellow.v3.5-voxy-cloudstyle3.zip)

> This build is available for testing. The source change and ZIP build have been checked, but the updated build still needs an in-game test before I can say the crash is fixed.

## What's going on?

The crash report shows Minecraft trying to upload a texture while a render pass is still active:

```text
IllegalStateException: Close the existing render pass before performing additional commands
```

The stack trace goes through Iris' Voxy render pipeline while it is creating and binding textures. Mellow registers Minecraft's vanilla cloud texture for two shader buffers, `colortex3` and `colortex8`. Those registrations should only be needed by Mellow's Blocky cloud implementation.

## What I changed

I added a `CLOUD_STYLE == 3` guard around both vanilla cloud texture registrations in `shaders/shaders.properties`:

```properties
#if CLOUD_STYLE == 3
texture.deferred.colortex3=minecraft:textures/environment/clouds.png
#endif

#if CLOUD_STYLE == 3
texture.gbuffers.colortex8=minecraft:textures/environment/clouds.png
#endif
```

I checked the Mellow 3.5 shader source. Its cloud-style values are:

| Value | Cloud style |
|---:|---|
| `0` | Vanilla |
| `1` | Flat |
| `2` | Volumetric |
| `3` | Blocky |

The `get_clouds_blocky_volumetric()` function reads from `colortex3` or `colortex8`, and the shader calls that function for style `3`. The SMAA texture registration for `colortex3` is left alone.

This is a shader-side change. It does not modify Iris or Voxy.

## Install the test build

1. Download the ZIP from the link at the top of this page.
2. Put the ZIP in your Minecraft instance's `shaderpacks` folder.
3. Select **Mellow v3.5-voxy-cloudstyle3** in Iris.
4. Keep it zipped; do not extract it.

Use **v1.0.1** for testing. The older **v1.0.0** release is superseded.

## Compatibility and test status

The build is intended for this setup:

- Minecraft 26.3
- Mellow 3.5
- Iris 1.11.7
- Voxy 0.2.20-beta

The release script completed successfully and the ZIP contains both `CLOUD_STYLE == 3` guards. The remaining check is to load a world with Voxy enabled, then switch from **Fancy** to **Fast** (the profile change that previously triggered the crash). Blocky clouds should also be checked to make sure they still render.

The earlier build using `CLOUD_STYLE == 0` still crashed during the Fancy-to-Fast switch. That log is recorded in [the testing notes](docs/TESTING.md). It is not a test of the current `CLOUD_STYLE == 3` build.

## Source patch

The source-only patch is in [`patches/voxy-cloud-texture-registration.patch`](patches/voxy-cloud-texture-registration.patch).

From the root of your Mellow 3.5 source checkout, run these commands (replace the patch path with where you saved the file):

```bash
git apply --check path/to/voxy-cloud-texture-registration.patch
git apply path/to/voxy-cloud-texture-registration.patch
```

The patch changes only `shaders/shaders.properties`.

## Credits

This is an unofficial fix I made. It is not an official Mellow release, and I am not affiliated with the original Mellow author.

Mellow was created by TheCMK. This repository contains my small patch and notes, not a copy of the full shader pack. Keep the original Mellow credits and follow its licence when using or redistributing Mellow.
