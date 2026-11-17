# Mellow 3.5 — Voxy compatibility fix

This is a small fix I made for a rendering error with Mellow 3.5, Iris and Voxy on Minecraft 26.3.

## What caused the bug?

Mellow was registering Minecraft's vanilla cloud texture even when the selected cloud style did not use it.

Iris loads textures lazily. When this happened alongside Voxy, it could trigger:

```text
IllegalStateException: Close the current render pass first.
```

## What I changed

I changed `shaders/shaders.properties` so the two vanilla cloud texture references are only registered when `CLOUD_STYLE == 0`.

```properties
#if CLOUD_STYLE == 0
texture.deferred.colortex3=minecraft:textures/environment/clouds.png
#endif

#if CLOUD_STYLE == 0
texture.gbuffers.colortex8=minecraft:textures/environment/clouds.png
#endif
```

I checked the Mellow 3.5 source and confirmed the cloud-style values:

| Value | Cloud style |
|---:|---|
| `0` | Vanilla |
| `1` | Flat |
| `2` | Volumetric |
| `3` | Blocky |

Mellow disables Minecraft's vanilla cloud pass whenever the selected style is not Vanilla. The vanilla cloud texture registrations should therefore be kept for style `0`, not style `3`.

The SMAA texture registrations are left unchanged. No cloud effects or shader features are removed.

## The source change

The change is in [`patches/voxy-cloud-texture-registration.patch`](patches/voxy-cloud-texture-registration.patch).

It applies to the Mellow source at commit `0f7f5947b7790d10ee27263fb8e814ac7534910a`. From the root of that Mellow source checkout, check and apply it with:

```bash
git apply --check path/to/voxy-cloud-texture-registration.patch
git apply path/to/voxy-cloud-texture-registration.patch
```

Replace `path/to/` with wherever you saved the patch. It only changes `shaders/shaders.properties`.

## Testing

The earlier test build used the original `CLOUD_STYLE == 3` condition. The corrected `CLOUD_STYLE == 0` version still needs to be rebuilt and tested in Minecraft before I can say this revised fix is confirmed.

The test setup and previous evidence are listed in [`docs/TESTING.md`](docs/TESTING.md).

## Credits and scope

This is an unofficial fix I made. It is not an official Mellow release, and I am not affiliated with the original Mellow author.

Mellow was created by TheCMK. This repository contains my small patch and its test notes, not a copy of the full shader pack. Keep the original Mellow credits and follow its licence when using or redistributing Mellow.
