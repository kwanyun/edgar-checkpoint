---
license: other
license_name: retained-nvidia-stability-terms
license_link: LICENSE.txt
pipeline_tag: image-to-image
tags:
- avatar
- image-restoration
- diffusion
---
# EDGAR — six-expert image refiner

Inference weights for **EDGAR: Efficient Diffusion-Driven Gaussian Avatar Refinement
via Synthetic Failure**, accepted to ACM Transactions on Graphics.

Authors: Kwan Yun, Min-Hyeok Bang, Colette Suhjung Koo, Junyong Noh.
[Project page](https://kwanyun.github.io/edgar/)

## Checkpoint

- File: `edgar_exp6.pt`
- Size: 2,247,645,095 bytes.
- SHA256: `9b7dc08c24190e7cb78cfebbc6d8e20787dda214ac444da9029ae90fe80ad712`
- Architecture: SD-Turbo with reference conditioning and six experts in each of
  11 MoE modules, plus VAE skip/LoRA weights and a reference projection.
- Provenance: inference tensors exported unchanged from the recovered
  `model_40001_exp6.pkl` used by the inspected main CAP4D and RGBAvatar launchers.
  Optimizer state was removed. This does not establish provenance for every
  historical paper or supplemental experiment.

The file contains the EDGAR network state. Sapiens, the base SD-Turbo files, and
POSTER are not bundled. Use the companion code and the separate upstream assets.

## Inference

```bash
edgar-infer --checkpoint edgar_exp6.pt \
  --sapiens /path/to/sapiens_0.3b_epoch_1600_torchscript.pt2 \
  --input renders/ --reference reference.png --output refined/ \
  --preprocessing application
```

Use a white-background avatar rendering and a reference image of the same identity.
The historical application convention white-pads renders before resizing to 512²
and normalizes 1024² reference images to [-1,1]. Output images are raw 512² RGB.
An additional native-avatar optimization step is required to update the 3D model.

## Training and limitations

The recovered checkpoint is associated with EDGAR-SET's CAFCA- and NeRSemble-derived
triplets. It is not a CAFCA-only checkpoint. The network targets recoverable avatar
appearance artifacts; severe geometry failures, unsupported identities/conditions,
or missing reference information may not be corrected. Single-image inference does
not itself guarantee temporal or cross-view consistency.

The export matched the original application network exactly on the checked FP32
image with identical preprocessing and seed. This is a compatibility check, not
a rerun of all published metrics. See the companion code's VALIDATION.md.

## License and attribution

The supplied NVIDIA/Stability license text is retained in [LICENSE.txt](LICENSE.txt).
See [NOTICE](NOTICE). This package grants no rights to redistribute the underlying
NeRSemble images or other external assets. Review the applicable model and source
data terms before publishing the model package.

**Powered by Stability AI.**


## Private downloads

[Download release assets](https://github.com/kwanyun/edgar-checkpoint/releases/tag/v1.0). Sign in with a GitHub account that has access.

Download `edgar_exp6.zip`, then unzip it to obtain `edgar_exp6.pt`, its model card, license, notices, and SHA256 checksum. ZIP compression preserves the checkpoint bytes. This is the combined-data six-expert model.
