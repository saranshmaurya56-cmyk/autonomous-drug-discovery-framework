[![DOI](https://zenodo.org/badge/1254315757.svg)](https://doi.org/10.5281/zenodo.20459877)
# autonomous-drug-discovery-framework
An end-to-end VAE and GNN framework for autonomous transcriptomic restoration and drug repurposing.
## Robustness & Validation

To ensure the biological signal is mathematically sound and not a product of data leakage or random noise, this architecture was subjected to three rigorous stress tests:

* **The Null Vector Test:** Applying a randomized 256D latent vector resulted in a Mean Squared Error (MSE) of 2.95, whereas the model's calculated restoration vector achieved an MSE of 1.90. This proves the Variational Autoencoder learned a highly specific, calculated trajectory toward health rather than generating random transcriptomic noise.
* **The Null Gene Set Test:** The 50 genes selected by the model mapped to 171 significant biological pathways, compared to only 112 pathways found by a random selection of 50 genes, confirming the extraction of a true structural network.
* **Generalization (Holdout) Test:** When tested on a strictly quarantined 10% subset of cancer profiles unseen during training, the framework successfully reversed the destabilized profile by **88.17%**, proving true manifold learning without overfitting.

## Key Discoveries & Orthogonal Convergence

* **Pharmacological Master Switches:** The Graph Neural Network mathematically isolated ***LEP* (Leptin)** and ***CD36*** as the paramount structural nodes necessary for tumor network maintenance.
* **Pathway-Drug Convergence:** Independent GSEA analysis identified the **PPAR signaling pathway** as the primary restored biochemical network. The subsequent, completely orthogonal LINCS L1000/DSigDB screen autonomously recommended **Bezafibrate** (a known, FDA-approved PPAR agonist). This convergence demonstrates profound internal consistency within the framework.

## Reproducibility

All model weights, preprocessing scalers, and output CSVs are serialized and available in this repository.
