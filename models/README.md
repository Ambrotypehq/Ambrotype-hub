# Models

Files the Ambrotype app downloads the first time it needs them, kept here so the app has a copy it controls. The app checks each against a sha256 it carries, so a changed file is refused.

| File | What | Licence |
|---|---|---|
| `u2netp.onnx` | U²-Net, small version, for Select Subject masks. sha256 `309c8469258dda742793dce0ebea8e6dd393174f89934733ecc8b14c76f4ddd8` | Apache-2.0 ([U-2-Net-LICENSE.txt](U-2-Net-LICENSE.txt)) |

U²-Net is by Xuebin Qin, Zichen Zhang, Chenyang Huang, Masood Dehghan, Osmar R. Zaiane and Martin Jagersand (<https://github.com/xuebinqin/U-2-Net>, "U²-Net: Going Deeper with Nested U-Structure for Salient Object Detection", Pattern Recognition 2020). The ONNX file is the one the rembg project (MIT, <https://github.com/danielgatis/rembg>) converted and publishes; it is unchanged.
