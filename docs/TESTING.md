# Testing notes

## Current build

**Release:** [v1.0.1 — Mellow.v3.5-voxy-cloudstyle3.zip](https://github.com/averykdskds-maker/mellow-3.5-voxy-compatibility-fix/releases/tag/v1.0.1)

The release ZIP was built successfully. I checked its `shaders/shaders.properties` file and confirmed that both vanilla cloud texture registrations are guarded by `#if CLOUD_STYLE == 3`.

**Runtime status: not confirmed yet.** The current build still needs to be tested in Minecraft.

## Target setup

- Minecraft 26.3
- Mellow 3.5
- Iris 1.11.7
- Voxy 0.2.20-beta

## What needs testing

1. Launch the game with Voxy enabled and select the v1.0.1 shader ZIP.
2. Load a world using the **Fancy** profile.
3. Switch from **Fancy** to **Fast**. This profile change triggered the earlier crash.
4. Switch back to Fancy and repeat the change to check that it is reliable.
5. Select **Blocky** clouds and check that they still render correctly.
6. Check the other cloud styles and make sure they still work.

A successful shader reload is not enough on its own; the world should render normally after each profile change.

## Earlier failed test — previous condition

An earlier build used `CLOUD_STYLE == 0` for the two registrations. With that build selected, switching from Fancy to Fast still caused:

```text
IllegalStateException: Close the existing render pass before performing additional commands
```

[Crash log from that test](https://mclo.gs/gOuWsBg)

That result is why the guard was checked against Mellow's actual cloud-shader code and changed to `CLOUD_STYLE == 3`. **The failure above does not test the current v1.0.1 build.**

## Older comparison evidence

These links relate to earlier builds and are kept for reference. They are not proof that v1.0.1 works:

- [Earlier patched shader log with Voxy](https://mclo.gs/XOWix2E)
- [Earlier patched shader screenshot with Voxy](https://sshot.online/f/kGQ8EGt.png)
- [Original shader log without Voxy](https://mclo.gs/JJNQ5U6)
- [Original shader screenshot without Voxy](https://sshot.online/f/ZhsX6hn.png)
