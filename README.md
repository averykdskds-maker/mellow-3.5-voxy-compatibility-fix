What caused the bug?

Mellow was registering the vanilla Minecraft cloud texture, even if it wasn't required based on the cloud style selected. Iris will load the textures lazily, but this combined with Voxy can cause an error in Minecraft 26.3. Error: IllegalStateException: Close the current render pass first.

What I changed

I edited the shaders/shaders.properties file and changed to only register the two vanilla cloud texture references when CLOUD_STYLE == 3.

```properties
#if CLOUD_STYLE == 3
texture.deferred.colortex3=minecraft:textures/environment/clouds.png
texture.gbuffers.colortex8=minecraft:textures/environment/clouds.png
#endif
```

This means Cloud Style 3 will still get its required cloud textures, whilst other cloud styles won't register textures they don't need. No cloud effects or shader features have been removed.

## Source code change

The source-level change is now provided as a Git patch, rather than only as a compiled shader archive:

- [View the Voxy cloud texture registration patch](patches/voxy-cloud-texture-registration.patch)

The patch modifies only `shaders/shaders.properties`. It adds conditional registration for the two vanilla cloud texture references and leaves the SMAA texture registrations unchanged. It is based on Mellow source commit `0f7f5947b7790d10ee27263fb8e814ac7534910a`.

Testing

Tested on:

Minecraft 26.3

Mellow 3.5

Iris 1.11.7

Voxy 0.2.20-beta

The modified shader worked in my test. Not all mod combinations / cloud styles were tested. If there are any issues found, please let me know.

## Important

This is a small unofficial fix I created on my own. This isn't an official Mellow release and isn't related to the original Mellow author.

Mellow was originally created by TheCMK. Please keep the original licensing and credits when redistributing. See included licence files for more info.

Test log: https://mclo.gs/XOWix2E

Screenshot: https://sshot.online/f/kGQ8EGt.png

### Without Voxy and using the original shader

Test log: https://mclo.gs/JJNQ5U6

Screenshot: https://sshot.online/f/ZhsX6hn.png
