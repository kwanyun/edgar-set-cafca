# Dataset attribution and changes

EDGAR-SET is prepared by Kwan Yun, Min-Hyeok Bang, Colette Suhjung Koo, and Junyong Noh.

The CAFCA component is adapted from:
Marcel C. Buehler, Gengyan Li, Erroll Wood, Leonhard Helminger, Xu Chen, Tanmay Shah,
Daoye Wang, Stephan Garbin, Sergio Orts-Escolano, Otmar Hilliges, Dmitry Lagun,
Jérémy Riviere, Paulo Gotardo, Thabo Beeler, Abhimitra Meka, and Kripasindhu Sarkar.
Cafca: High-quality Novel View Synthesis of Expressive Faces from Casual Few-shot
Captures. ACM SIGGRAPH Asia 2024. https://doi.org/10.1145/3680528.3687580

Source: https://github.com/syntec-research/Cafca/blob/main/Dataset.md
License: Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International.
https://creativecommons.org/licenses/by-nc-sa/4.0/

Changes: EDGAR constructs degraded Gaussian-avatar renderings, pairs them with
ground-truth and reference images, filters the pairs, and assembles three-panel
training images. This release repackages the recovered filtered triplets without
changing their image bytes. It adds sample hashes, source/identity metadata,
explicit split manifests, and TAR shards. The new identity-disjoint split is a
release split; it is not labeled as the original paper split.

CAFCA is not an official Google product and is not supported by Google.

The NeRSemble component, when present, derives from:
Tobias Kirschstein, Shenhan Qian, Simon Giebenhain, Tim Walter, and Matthias Nießner.
NeRSemble: Multi-View Radiance Field Reconstruction of Human Heads.
ACM Transactions on Graphics, 2023. https://doi.org/10.1145/3592455
Access: https://github.com/tobias-kirschstein/nersemble-data
No public redistribution grant for the NeRSemble-derived images is supplied here.
The CAFCA license does not apply to that component.
