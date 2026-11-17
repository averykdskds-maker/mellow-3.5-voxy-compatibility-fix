# Test details

## Setup used for the earlier test

- Minecraft 26.3
- Mellow 3.5
- Iris 1.11.7
- Voxy 0.2.20-beta

## Important: the patch has been corrected

The earlier test build used `CLOUD_STYLE == 3`. After checking Mellow's source, I found that the cloud-style values are:

- `0`: Vanilla
- `1`: Flat
- `2`: Volumetric
- `3`: Blocky

The source shows that Minecraft's vanilla cloud pass is disabled whenever `CLOUD_STYLE != 0`. The patch has therefore been corrected to use `CLOUD_STYLE == 0`.

**The evidence below is from the earlier build. It does not verify the corrected `CLOUD_STYLE == 0` version. That version still needs a fresh runtime test.**

## Previous test evidence

- [Log from the earlier patched shader with Voxy](https://mclo.gs/XOWix2E)
- [Screenshot from the earlier patched shader with Voxy](https://sshot.online/f/kGQ8EGt.png)

For comparison, these are from the original shader without Voxy:

- [Original shader log without Voxy](https://mclo.gs/JJNQ5U6)
- [Original shader screenshot without Voxy](https://sshot.online/f/ZhsX6hn.png)
