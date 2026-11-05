---
license: cc-by-nc-sa-4.0
task_categories:
- image-to-image
tags:
- avatar
- image-restoration
- synthetic
---
# EDGAR-SET — CAFCA-only

Training triplets for **EDGAR: Efficient Diffusion-Driven Gaussian Avatar Refinement via Synthetic Failure**,
accepted to ACM Transactions on Graphics.

Kwan Yun · Min-Hyeok Bang · Colette Suhjung Koo · Junyong Noh  
[Project page](https://kwanyun.github.io/edgar/)

Prepared for research distribution under CC BY-NC-SA 4.0. Retain NOTICE.md and LICENSE-CAFCA.txt.

## Contents

- **22,420 triplets**, **1,500 identities**.
- 67,260 individual 512×512 image panels.
- 11,685,611,520 TAR bytes (10.88 GiB), plus metadata.
- Packed size: **1536×512**, ordered **reference | ground truth | degraded rendering**.
- `shards/`: approximately 1 GiB uncompressed TAR archives of the existing PNG images.
- `index.jsonl`: relative paths, sources, identity IDs, image sizes in bytes, SHA256, shard names.
- `SHA256SUMS` and `shards.json`: archive integrity metadata.

Every triplet was checked for image dimensions, color mode, and PNG integrity.
Canonical image bytes are preserved; no further filtering or recompression was applied.
Ten byte-identical Jupyter image backups in the NeRSemble tree were excluded;
their canonical originals are included. The sorted split is applied after this cleanup.
This is the filtered EDGAR training set, not a mirror of either complete source dataset.

## Training splits

`manifest.json` preserves the recovered loader's sorted 99/1 file split:
22,195 training and 225 validation triplets.
Identities can overlap across these partitions.

`manifest_identity_disjoint.json` is a new split for new training runs: approximately
2% of identities from each source are held out using deterministic SHA256 ordering
with seed 2026. It is not the original paper split.

## Usage

From the EDGAR code repository:

```bash
python tools/unpack_dataset.py --package /path/to/CAFCA-only --output data/edgar_set
edgar-train --manifest data/edgar_set/manifest_identity_disjoint.json \
  --sapiens checkpoints/sapiens_0.3b_epoch_1600_torchscript.pt2 \
  --poster checkpoints/affectnet8-sd.pth --output outputs/train
```

Extraction verifies all shard hashes and rejects unsafe archive paths.
Sapiens, POSTER, and EDGAR model weights are distributed separately.

## Scope

The images depict recoverable rendering failures; severe structural failures are
not reliably covered. CAFCA-only training is a synthetic-subset experiment and
does not reproduce the model trained with both sources. The full upstream avatar
generation/filtering process is not included in these image archives.

## Attribution

Read [NOTICE.md](NOTICE.md) and [LICENSE-CAFCA.txt](LICENSE-CAFCA.txt).
The CAFCA component is distributed under the same CC BY-NC-SA 4.0 terms as its source.
The NeRSemble component, if included, remains subject to its own access and redistribution terms.


## Private downloads

[Download release assets](https://github.com/kwanyun/edgar-set-cafca/releases/tag/v1.0). Sign in with a GitHub account that has access.

Download `EDGAR-SET-CAFCA-only-metadata.zip` and all `.tar` assets. Extract the metadata ZIP into a `CAFCA-only/` directory and put the TAR assets in its `shards/` subdirectory.

With the [GitHub CLI](https://cli.github.com/), after `gh auth login`:

```bash
gh release download v1.0 --repo kwanyun/edgar-set-cafca --pattern "EDGAR-SET-CAFCA-only-metadata.zip" --dir packages/CAFCA-only
python -m zipfile -e packages/CAFCA-only/EDGAR-SET-CAFCA-only-metadata.zip packages/CAFCA-only
gh release download v1.0 --repo kwanyun/edgar-set-cafca --pattern "*.tar" --dir packages/CAFCA-only/shards
python tools/unpack_dataset.py --package packages/CAFCA-only --output data/CAFCA-only
```

Run the last command from the [EDGAR code repository](https://github.com/kwanyun/edgar-code). The unpacker verifies every TAR SHA256 before extracting images.
