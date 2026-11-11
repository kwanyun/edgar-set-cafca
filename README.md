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

Training triplets for **EDGAR: Efficient Diffusion-Driven Gaussian Avatar Refinement via Synthetic Failure**, accepted to ACM Transactions on Graphics.

Kwan Yun · Min-Hyeok Bang · Colette Suhjung Koo · Junyong Noh  
[Project page](https://kwanyun.github.io/edgar/) · [Download v1.0](https://github.com/kwanyun/edgar-set-cafca/releases/tag/v1.0)

This public dataset contains **22,420 triplets from 1,500 synthetic CAFCA identities**.
It contains no NeRSemble images. The data are an adaptation of
[CAFCA](https://github.com/syntec-research/Cafca/blob/main/Dataset.md), distributed
under [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/).
Retain [NOTICE.md](NOTICE.md) and [LICENSE-CAFCA.txt](LICENSE-CAFCA.txt).

## Contents

- Each PNG is **1536 × 512**, ordered **reference | ground truth | degraded rendering**.
- 67,260 individual 512 × 512 panels, packed into 11 TAR shards.
- Approximately **11.7 GB** to download; allow another 11.7 GB for extracted images.
- Metadata includes sample paths, source and identity IDs, SHA256 checksums, and training/validation manifests.

Every triplet was checked for image dimensions, color mode, and PNG integrity.
Canonical image bytes are preserved. This is the filtered CAFCA-derived EDGAR
training subset, not the complete upstream CAFCA dataset.

## Download

No GitHub account is required. Open the [v1.0 release](https://github.com/kwanyun/edgar-set-cafca/releases/tag/v1.0), expand **Assets**, and download:

1. [EDGAR-SET-CAFCA-only-metadata.zip](https://github.com/kwanyun/edgar-set-cafca/releases/download/v1.0/EDGAR-SET-CAFCA-only-metadata.zip)
2. [EDGAR-SET-CAFCA-only-metadata.zip.sha256](https://github.com/kwanyun/edgar-set-cafca/releases/download/v1.0/EDGAR-SET-CAFCA-only-metadata.zip.sha256)
3. All 11 TAR files, `cafca-00000.tar` through `cafca-00010.tar`.

GitHub's **Source code (zip)** and **Source code (tar.gz)** contain documentation,
not the images. Use the named release assets above.

### Verify and extract

Create a new `CAFCA-only` folder. Place the metadata ZIP and its checksum there,
and place all TAR files inside `CAFCA-only/shards/`:

```text
CAFCA-only/
├── EDGAR-SET-CAFCA-only-metadata.zip
├── EDGAR-SET-CAFCA-only-metadata.zip.sha256
└── shards/
    ├── cafca-00000.tar
    ├── ...
    └── cafca-00010.tar
```

On Linux or WSL, run the commands below in order. Continue only if both checksum
checks succeed for every file:

```bash
cd CAFCA-only
sha256sum -c EDGAR-SET-CAFCA-only-metadata.zip.sha256
python -m zipfile -e EDGAR-SET-CAFCA-only-metadata.zip .
sha256sum -c SHA256SUMS
for shard in shards/cafca-*.tar; do
    tar -xf "$shard"
done
```

The extracted images are under `triplets/CAFCA/`. Paths in `manifest.json`,
`manifest_identity_disjoint.json`, and `index.jsonl` are relative to this package
folder. Training and refinement code are available in
[edgar-code](https://github.com/kwanyun/edgar-code). The combined-data
[exp6 checkpoint](https://github.com/kwanyun/edgar-checkpoint/releases/tag/v1.0) is also public.

## Training splits

`manifest.json` uses the recovered loader's sorted 99/1 file split:
**22,195 training and 225 validation triplets**. Identities can overlap between
these partitions.

`manifest_identity_disjoint.json` provides a separate split for new training runs:
approximately 2% of identities are held out using deterministic SHA256 ordering
with seed 2026. It is not the original paper split.

## Scope and attribution

The triplets depict recoverable rendering failures; severe structural failures
are not reliably covered. Training with CAFCA-only does not reproduce EDGAR's
combined-data training experiment. The upstream avatar-generation and filtering
process is not included in these archives.

EDGAR constructs degraded Gaussian-avatar renderings, pairs them with ground-truth
and reference images, filters samples, and assembles three-panel training images.
The release adds sample hashes, split manifests, and TAR packaging without changing
the recovered image bytes. Full source attribution and the change notice are in
[NOTICE.md](NOTICE.md).

The license requires attribution, noncommercial use, and distribution of adaptations
under the same license. CAFCA is not an official Google product and is not supported
by Google. See the retained license and notices for the complete terms.
