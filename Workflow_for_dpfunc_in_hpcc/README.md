# DPFunc — HPCC Workflow (MSU)

## Objective
Use DPFunc to produce **Gene Ontology (GO) predictions** for 8 course proteins, returning top-ranked terms for:
- Molecular Function (MF)
- Biological Process (BP)
- Cellular Component (CC)

## Inputs we provided
In our setup, DPFunc used **two** inputs:
1) **Protein structures (PDB files)** → transformed into structure graphs (the primary signal)
2) **Protein sequences (FASTA)** → used for residue-level alignment plus ID tracking/bookkeeping

Since this was a small class dataset, we ran a streamlined DPFunc pipeline:
- Structure graphs were generated from PDBs on the HPCC.
- We relied on pretrained DPFunc model weights.
- Some optional features (like InterPro and full ESM embeddings) were handled minimally so the pipeline would run cleanly for just 8 proteins.

## Compute environment
- We connected to an HPCC **development node** using VS Code Remote-SSH for editing and setup.
- All files were stored in scratch (`/mnt/scratch/<netid>/...`) for persistence across sessions.
- Graph generation and prediction ran on the dev node (dataset was small) within the `dpfunc` conda environment.

## Core workflow (high level)
1) **Prepare inputs**
   - Put the 8 predicted structures (**PDBs**) into DPFunc’s PDB input directory.
   - Created a **single FASTA** containing all 8 sequences, using headers that matched PDB basenames so DPFunc could consistently link sequence ↔ structure.

2) **Make the protein ID list**
   - DPFunc expects a stable set of protein IDs (“pids”), generated from the PDB filenames (without extensions).

3) **Create PDB coordinate “points”**
   - Extracted residue coordinate points from each PDB and saved them into `pdb_points.pkl`.

4) **Add required residue-feature placeholders and mapping files**
   - DPFunc’s graph builder expects residue feature files plus a mapping that assigns feature chunks to each protein. We produced the minimal required files so graph building and prediction would work for our dataset.

5) **Construct structure graphs**
   - Using DPFunc graph scripts, converted each protein into graph inputs (separately for MF/BP/CC). These graphs are what the models consume at inference time.

6) **Retrieve pretrained weights**
   - Downloaded `save_models.tar.gz` (pretrained weights) from the project link.
   - Extracted the `.pt` model files into `save_models/` so DPFunc could load them.

7) **Run inference**
   - Ran predictions for MF, BP, and CC with pretrained models and our generated graph inputs.
   - Outputs were written as `*_final.pkl` files.

8) **Export CSV deliverables**
   - Converted the final prediction objects into CSV files listing the **top 10 GO terms** per protein for each ontology.

## Final deliverables
Submission / Excel-friendly outputs:
- `results/mf_top10.csv`
- `results/bp_top10.csv`
- `results/cc_top10.csv`

Each CSV includes:
- `protein` (protein ID)
- `rank` (1–10)
- `GO` (GO identifier)
- `score` (model score used for ranking)

## Notes / caveats
- This workflow is optimized for a small classroom dataset on HPCC.
- A full DPFunc run may include richer residue embeddings and domain annotations; this version focuses on producing valid GO predictions and clean, exportable outputs for the 8 proteins.

