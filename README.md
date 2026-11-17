# Mellow 3.5 Voxy fix

I made this patch after running into a crash with Mellow 3.5, Iris and Voxy on Minecraft 26.3.

**Download:** [Mellow v3.5-voxy-cloudstyle3 (v1.0.1)](https://github.com/averykdskds-maker/mellow-3.5-voxy-compatibility-fix/releases/download/v1.0.1/Mellow.v3.5-voxy-cloudstyle3.zip)

Just to be clear, this version has built and I've checked the ZIP, but I still need to test it in-game. I don't want to say it's fixed until I've actually tried it.

## The crash

I got this when changing shader profiles with Voxy enabled:

```
IllegalStateException: Close the existing render pass before performing additional commands
```

The log points to Iris and Voxy while a texture is being loaded. I had a look through Mellow's cloud code because it registers the vanilla cloud texture for two buffers, `colortex3` and `colortex8`.

The cloud settings in Mellow are:

- `0` Vanilla
- `1` Flat
- `2` Volumetric
- `3` Blocky

The Blocky cloud code is the bit that uses those two buffers. I changed the registrations in `shaders/shaders.properties` so they are only added when Blocky clouds are selected:

```properties
#if CLOUD_STYLE == 3
texture.deferred.colortex3=minecraft:textures/environment/clouds.png
#endif

#if CLOUD_STYLE == 3
texture.gbuffers.colortex8=minecraft:textures/environment/clouds.png
#endif
```

I left the SMAA texture alone. Nothing in Iris or Voxy has been changed.

This is a small change, and I'm not certain yet that it's the full fix for the crash. An earlier build used `CLOUD_STYLE == 0`, and that one still crashed when I switched from Fancy to Fast. The current ZIP uses `3`, which matches the Blocky code, but it needs another test.

## Installing it

Download the ZIP above and put it in your instance's `shaderpacks` folder. Then select **Mellow v3.5-voxy-cloudstyle3** in Iris. Keep it zipped.

I'm testing with Minecraft 26.3, Iris 1.11.7 and Voxy 0.2.20-beta.

The main thing I need to check is switching from **Fancy** to **Fast** with Voxy on. I also need to make sure Blocky clouds still look right and that the other cloud styles haven't been affected. I've put the earlier crash and the rest of the test notes in [docs/TESTING.md](docs/TESTING.md).

## What's in here?

The repo just has the source patch, the test notes and this README. The shader ZIP is in [Releases](https://github.com/averykdskds-maker/mellow-3.5-voxy-compatibility-fix/releases), rather than being committed here.

- [Patch file](patches/voxy-cloud-texture-registration.patch)
- [Testing notes](docs/TESTING.md)

This isn't an official Mellow release. Mellow was made by TheCMK, and I'm not affiliated with the original author. I haven't included the full shader pack here, so make sure you follow the original licence if you use or share Mellow.
