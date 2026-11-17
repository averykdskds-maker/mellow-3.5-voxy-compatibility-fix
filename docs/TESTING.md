# Testing notes

This is where I'm keeping track of what I've actually tested. The important bit: **v1.0.1 has built successfully, but I haven't confirmed it in-game yet.**

## Current build

[**v1.0.1 — Mellow.v3.5-voxy-cloudstyle3.zip**](https://github.com/averykdskds-maker/mellow-3.5-voxy-compatibility-fix/releases/tag/v1.0.1)

I checked the ZIP and confirmed both vanilla cloud texture registrations are now inside `#if CLOUD_STYLE == 3` blocks.

The build is intended for:

- Minecraft 26.3
- Mellow 3.5
- Iris 1.11.7
- Voxy 0.2.20-beta

## What still needs testing

1. Start Minecraft with Voxy enabled and select the v1.0.1 shader ZIP.
2. Load into a world with the **Fancy** profile.
3. Switch from **Fancy** to **Fast**. This is the change that caused the earlier crash.
4. Switch back to Fancy and try it again.
5. Set the cloud style to **Blocky** and make sure the clouds still render.
6. Try the other cloud styles too.

I want to check that the game keeps rendering normally after the profile changes, not just that the shader reloads without an immediate error.

## What happened with the earlier build?

The previous build had the two texture registrations guarded by `CLOUD_STYLE == 0`. I tried switching from Fancy to Fast with that ZIP, and it still crashed with:

```text
IllegalStateException: Close the existing render pass before performing additional commands
```

[Crash log from that test](https://mclo.gs/gOuWsBg)

After that, I went back through Mellow's cloud shader code. It showed that the Blocky cloud function is the one reading from `colortex3` or `colortex8`, and that function is used for cloud style `3`. That's why the current patch uses `CLOUD_STYLE == 3` instead.

**The crash above was with the old `== 0` build. It doesn't tell us whether the current v1.0.1 build works.**

## Older comparison logs and screenshots

These are from earlier tests. I'm leaving them here for reference, but they aren't proof that v1.0.1 works:

- [Earlier patched shader log with Voxy](https://mclo.gs/XOWix2E)
- [Earlier patched shader screenshot with Voxy](https://sshot.online/f/kGQ8EGt.png)
- [Original shader log without Voxy](https://mclo.gs/JJNQ5U6)
- [Original shader screenshot without Voxy](https://sshot.online/f/ZhsX6hn.png)
