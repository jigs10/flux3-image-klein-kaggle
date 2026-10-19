# FLUX 3 Image Klein on Kaggle (Wan2GP)

Kaggle notebook that runs **FLUX 3 Image Klein** (5.6B, by Black Forest Labs) in [Wan2GP](https://github.com/deepbeepmeep/Wan2GP) on a free single T4 GPU.

## What it does

- Installs Wan2GP and its requirements
- Downloads the INT8 ConvRot weights from [`DeepBeepMeep/Flux3`](https://huggingface.co/DeepBeepMeep/Flux3) into `/tmp`
- Launches the Wan2GP web UI with a public share link

## Files

| File | Purpose |
|---|---|
| `flux3_image_klein_kaggle.ipynb` | The notebook |

## Quick start

1. Open the notebook on Kaggle (File > Import Notebook, or upload it).
2. Set the accelerator to **GPU T4** and turn **Internet** on.
3. Run the cells in order:
   1. Install Wan2GP
   2. Install requirements
   3. Download the transformer and text encoder
   4. Launch Wan2GP
4. Open the `gradio.live` link printed in the output.
5. Pick **Flux 3 Image Klein** in the model dropdown. If it isn't listed, add the `/tmp` files as a finetune (see below).

## Models

| Part | File | Size |
|---|---|---|
| Transformer (step-distilled, INT8) | `flux3_image_klein_sd_int8_convrot.safetensors` | 5.83 GB |
| Text encoder (Qwen3-VL-4B, INT8) | `Qwen3-VL-4B-Instruct/qwen3vl_4b_int8_convrot.safetensors` | 4.86 GB |
| VAE | `flux3/flux2-vae.safetensors` | 336 MB, auto-downloaded by Wan2GP |

All files go to `/tmp/flux3_klein`, so they don't count against Kaggle's working directory limit.

INT8 ConvRot is used because the T4 supports neither BF16 nor NVFP4 well.

To use the full (non-distilled) checkpoint instead, change `TRANSFORMER` in the download cell to `flux3_image_klein_int8_convrot.safetensors`. It needs CFG and about 50 steps.

## Adding the files as a finetune

If the model isn't in the dropdown, or you want to use the `/tmp` files, open Finetune Creator / Editor and set:

```
Main checkpoint:
/tmp/flux3_klein/flux3_image_klein_sd_int8_convrot.safetensors

Text encoder checkpoint:
/tmp/flux3_klein/Qwen3-VL-4B-Instruct/qwen3vl_4b_int8_convrot.safetensors
```

Keep the default VAE.

## Recommended settings

| Setting | Value |
|---|---|
| Resolution | `1024x1024` |
| Steps | `8` (fast) or `16` (best quality) |
| Guidance (CFG) | `1` |
| Acceleration LoRA | none |
| KV Cache | Off |
| Batch | `1` |

Negative prompts have no effect at CFG 1.

For image editing, add reference images (up to 10) and refer to them as `image 1`, `image 2`, and so on in the prompt.

## Test prompt

```
A cute orange cat sitting on a wooden table beside a small cup of coffee, warm morning sunlight coming through a window, realistic photography.
```

## Notes

- Kaggle sessions are temporary. Models in `/tmp` have to be downloaded again each session.
- Generation on a T4 is slow. Stay at 1024x1024 until you know your settings fit in memory.
- Wan2GP changes often. If a step breaks, pull the latest Wan2GP and check its docs for the current model names.

## License

FLUX 3 Image Klein is released under the **FLUX Kommunity License**:

- Research and non-commercial use is free.
- Commercial use of outputs is free for individuals and businesses with gross annual revenue (including affiliates) **under $5M**.

Read the full license in the [model repo](https://huggingface.co/DeepBeepMeep/Flux3/blob/main/LICENSE.md) before using outputs commercially.

## Credits

- [Black Forest Labs](https://bfl.ai) for FLUX 3 Image Klein
- [DeepBeepMeep](https://huggingface.co/DeepBeepMeep) for Wan2GP and the quantized checkpoints
