# Mellow 3.5 — Voxy compatibility fix

This is a small fix I made for a rendering error with Mellow 3.5, Iris and Voxy on Minecraft 26.3.

## What caused the bug?

Mellow was registering Minecraft's vanilla cloud texture even when the selected cloud style did not need it.

Iris loads textures lazily. When this happened alongside Voxy, it could trigger this error:

```text
IllegalStateException: Close the current render pass first.
```

## What I changed

I edited `shaders/shaders.properties` so the two vanilla cloud texture references are only registered when `CLOUD_STYLE == 3`.

```properties
#if CLOUD_STYLE == 3
texture.deferred.colortex3=minecraft:textures/environment/clouds.png
#endif

#if CLOUD_STYLE == 3
texture.gbuffers.colortex8=minecraft:textures/environment/clouds.png
#endif
```

## What this changes

Cloud Style 3 still gets the cloud textures it needs. Other cloud styles no longer register textures they do not use.

I left the SMAA texture registrations alone, and I did not remove any cloud effects or shader features.

## Testing

I tested the modified shader with:

- **Minecraft:** 26.3
- **Mellow:** 3.5
- **Iris:** 1.11.7
- **Voxy:** 0.2.20-beta

The modified shader worked in my test. I have not tested every possible mod combination or cloud style, so I cannot claim that every setup is covered.

## Credits and disclaimer

This is an unofficial fix I made myself. It is not an official Mellow release, and I am not affiliated with the original Mellow author.

Mellow was originally created by **TheCMK**. Please keep the original credits and follow the included licence when using or redistributing Mellow.

## Test evidence

- [Test log — patched shader with Voxy](https://mclo.gs/XOWix2E)
- [Screenshot — patched shader with Voxy](https://sshot.online/f/kGQ8EGt.png)

### Comparison: original shader without Voxy

- [Test log — original shader without Voxy](https://mclo.gs/JJNQ5U6)
- [Screenshot — original shader without Voxy](https://sshot.online/f/ZhsX6hn.png)
