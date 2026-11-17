# Mellow 3.5 Voxy compatibility fix

This is the small fix I made for a rendering error I was getting with Mellow 3.5, Iris and Voxy on Minecraft 26.3.

## What was going wrong

Mellow was registering Minecraft's vanilla cloud texture even when the selected cloud style did not use it. Iris loads textures lazily, and with Voxy this could trigger:

`IllegalStateException: Close the current render pass first.`

## What I changed

I changed `shaders/shaders.properties` so the two vanilla cloud texture references are only registered when `CLOUD_STYLE == 3`.

Cloud Style 3 still gets the textures it needs. Other cloud styles no longer register textures they do not use. I left the SMAA texture registrations and the rest of the shader alone.

## The source change

The change is in [`patches/voxy-cloud-texture-registration.patch`](patches/voxy-cloud-texture-registration.patch).

It applies to the Mellow source at commit `0f7f5947b7790d10ee27263fb8e814ac7534910a`. From the root of that Mellow source checkout, check and apply it with:

```bash
git apply --check path/to/voxy-cloud-texture-registration.patch
git apply path/to/voxy-cloud-texture-registration.patch
```

Replace `path/to/` with wherever you saved the patch. The patch only changes `shaders/shaders.properties`.

## Testing

I tested it with Minecraft 26.3, Mellow 3.5, Iris 1.11.7 and Voxy 0.2.20-beta. The test details and logs are in [`docs/TESTING.md`](docs/TESTING.md).

## Credits and scope

This is an unofficial fix I made. It is not an official Mellow release, and I am not affiliated with the original Mellow author.

Mellow was created by TheCMK. This repository contains my small patch and its test notes, not a copy of the full shader pack. Keep the original Mellow credits and follow its licence when using or redistributing Mellow.
