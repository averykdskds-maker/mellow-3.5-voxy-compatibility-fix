# Test details

## Setup I tested

- Minecraft 26.3
- Mellow 3.5
- Iris 1.11.7
- Voxy 0.2.20-beta

## Result

The patched shader worked in this setup. The change stops the two vanilla cloud texture references from being registered unless Cloud Style 3 is selected.

I have not tested every possible cloud style, mod combination or version, so I cannot claim that every setup is covered.

## Test evidence

- [Log from the patched shader with Voxy](https://mclo.gs/XOWix2E)
- [Screenshot from the patched shader with Voxy](https://sshot.online/f/kGQ8EGt.png)

For comparison, these are from the original shader without Voxy:

- [Original shader log without Voxy](https://mclo.gs/JJNQ5U6)
- [Original shader screenshot without Voxy](https://sshot.online/f/ZhsX6hn.png)
