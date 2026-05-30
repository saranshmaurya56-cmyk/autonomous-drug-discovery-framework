# Autonomous Transcriptomic Restoration and Drug Repurposing Framework:

## A Latent-Space Approach to Disease-State Translation in Breast Cancer

**Author:** Saransh Maurya
**Date:** May 30, 2026
**DOI:** 10.5281/zenodo.20459878

---

# 1. Abstract

Current computational drug discovery and drug repurposing pipelines often depend on manually curated gene sets and expert-driven hypothesis generation, potentially limiting the discovery of novel regulatory relationships. Here, we present an end-to-end computational framework that integrates transcriptomic representation learning, latent-space state translation, network analysis, and drug-repurposing candidate identification.

Using breast cancer (TCGA-BRCA) transcriptomic profiles, a Variational Autoencoder (VAE) was trained to learn a compressed latent representation of disease and healthy biological states. A disease-to-health latent translation strategy was then applied to generate candidate restoration signatures, which were subsequently analyzed using graph-based network approaches to identify highly connected regulatory hubs. Drug candidates were prioritized through comparison with public perturbation and drug-signature resources.

In holdout experiments, latent-state translation reduced the distance between unseen disease profiles and the healthy reference manifold by 88.17% relative to baseline measurements. Additional robustness analyses, including random-vector and random-gene controls, suggested that the framework captured structure consistent with non-random biological organization. Pathway enrichment analyses consistently highlighted PPAR signaling as a major recovered pathway, while downstream drug-repurposing analyses independently identified Bezafibrate, a clinically approved PPAR agonist, as a candidate intervention.

The complete framework, source code, model weights, and computational artifacts are publicly archived through Zenodo, providing a reproducible platform for transcriptomics-guided drug repurposing research.

---

# 2. Introduction

The increasing availability of large-scale transcriptomic datasets has created new opportunities for computational approaches to disease characterization and therapeutic discovery. Traditional machine learning applications in oncology often focus on classification tasks, such as distinguishing tumor from healthy tissue or identifying molecular subtypes. However, such approaches typically provide limited insight into how diseased biological states might be computationally translated toward healthier configurations.

This work explores an alternative paradigm based on latent-space restoration. Rather than predicting disease labels, we seek to learn a low-dimensional representation of transcriptomic states and model a disease-to-health trajectory within that latent manifold. By projecting restored states back into gene-expression space, identifying affected regulatory hubs, and linking these signatures to pharmacological perturbation databases, the framework aims to generate biologically interpretable hypotheses for drug repurposing.

---

# 3. Methods

## 3.1 Data Acquisition and Preprocessing

Transcriptomic profiles for Breast Invasive Carcinoma (BRCA) were obtained from The Cancer Genome Atlas (TCGA-BRCA) HiSeqV2 dataset. The expression matrix contained 20,530 genes across 1,218 samples. Samples were filtered to include primary solid tumor tissues (n = 1,097) and normal breast tissues (n = 114).

Expression values were standardized using z-score normalization (StandardScaler, scikit-learn) to reduce scale-dependent effects and facilitate representation learning.

---

## 3.2 Transcriptomic Representation Learning via Variational Autoencoder

A Variational Autoencoder (VAE) was implemented to learn a compressed representation of transcriptomic states.

The encoder consisted of two fully connected layers:

* 1,024 neurons
* 512 neurons

with ReLU activation and Batch Normalization.

Latent representations were parameterized as Gaussian distributions and sampled using the reparameterization trick. The latent dimensionality was fixed at 256.

Training minimized a composite objective consisting of:

* Mean Squared Error (MSE) reconstruction loss
* Kullback-Leibler (KL) divergence regularization

The model was trained for 50 epochs using the Adam optimizer with a learning rate of 1 × 10⁻⁴. Gradient clipping was applied to improve optimization stability.

---

## 3.3 Disease-to-Health Latent State Translation

Latent centroids were calculated independently for healthy and tumor samples:

μhealthy and μcancer

A restoration vector was defined as:

v = μhealthy − μcancer

For a given disease profile zdisease, a translated latent state was generated as:

zrestored = zdisease + v

This operation represents a disease-to-health trajectory within the learned latent manifold.

The translated latent representation was decoded back into gene-expression space to generate a restoration signature. Genes were subsequently ranked according to the magnitude of restoration-associated expression change.

---

## 3.4 Regulatory Network Construction and Graph Representation Learning

The 50 genes exhibiting the largest restoration-associated expression shifts were selected for network analysis.

Protein-protein interaction data were obtained from the STRING database using a confidence threshold greater than 0.4. The resulting interaction network was represented as a graph in which nodes corresponded to genes and edges represented known or predicted interactions.

Graph representation learning was performed using a Graph Autoencoder (GAE) implemented with PyTorch Geometric. The encoder consisted of two Graph Convolutional Network (GCN) layers that generated low-dimensional node embeddings.

A composite importance score was defined as:

Score = ||Embedding||₂ × |Expression Shift|

This score was used to prioritize candidate regulatory hubs exhibiting both strong network influence and substantial restoration-associated expression change.

---

## 3.5 Pathway Enrichment and Drug Repurposing

Prioritized genes were subjected to functional enrichment analysis using publicly available biological pathway databases.

Significantly enriched pathways were identified using adjusted enrichment statistics. Candidate compounds were subsequently prioritized through comparison with publicly available perturbation resources, including LINCS L1000 and drug-signature databases.

Drug candidates were ranked according to their association with the recovered transcriptional signature and the identified regulatory hubs.

---

## 3.6 Robustness and Validation

To evaluate the stability and specificity of the framework, three independent validation procedures were performed.

### Null Vector Control

Random latent perturbation vectors were applied to disease profiles and compared against the learned restoration vector.

### Null Gene Set Control

Randomly sampled gene sets were processed through the enrichment workflow and compared against the VAE-derived restoration signature.

### Holdout Generalization Test

A 90/10 train-test split was performed. The VAE was trained exclusively on the training subset and evaluated on previously unseen patient samples.

Performance was quantified by measuring the reduction in latent-space distance between disease profiles and the healthy reference centroid. In the holdout cohort, latent-state translation reduced this distance by 88.17% relative to baseline measurements.

---

# 4. Results

The learned latent manifold successfully separated healthy and tumor transcriptomic states. Disease-to-health latent translation generated coherent restoration signatures characterized by coordinated shifts in biologically relevant pathways.

Pathway enrichment analyses consistently identified PPAR signaling among the most strongly recovered biological processes. Independent drug-repurposing analyses highlighted Bezafibrate, a clinically approved PPAR agonist, as a candidate intervention, providing convergence between pathway-level and pharmacological analyses.

Robustness testing demonstrated that the learned restoration vector substantially outperformed random latent perturbations, while restoration-derived gene signatures produced stronger biological enrichment signals than randomly selected gene sets.

---

# 5. Limitations and Future Work

This study represents a computational proof-of-concept and does not establish causal therapeutic efficacy.

The restoration framework assumes a global disease-to-health trajectory and does not explicitly account for molecular subtype heterogeneity. Additionally, candidate compounds were identified through transcriptomic signature matching rather than experimental validation.

Future work will focus on:

* External dataset validation
* Subtype-specific restoration vectors
* Multi-omics integration
* Statistical benchmarking against alternative methods
* Experimental evaluation of candidate compounds

---

# 6. Related Work

This framework builds upon prior work in transcriptomic representation learning, variational autoencoders for cancer genomics, and computational drug repurposing.

Unlike approaches that focus exclusively on representation learning or classification, the present workflow integrates latent-state translation, graph-based regulatory analysis, and pharmacological candidate prioritization within a unified computational pipeline.

---

# 7. Data and Code Availability

All source code, model weights, preprocessing artifacts, and computational outputs are publicly available through Zenodo:

DOI: 10.5281/zenodo.20459878

The corresponding GitHub repository contains the complete implementation and documentation required to reproduce the reported analyses.
