# BioEmu — HPCC Workflow (MSU)

## Objective
Run BioEmu to produce **protein ensembles** (sets of realistic alternative conformations) for the eight course proteins, then convert the results into a format that’s easy to load, view, and superimpose in PyMOL.

## Inputs we started from
- Protein sequences (the same set of 8 proteins used in the course).
- An HPCC scratch directory for executing jobs and storing ensemble outputs.

## Computing environment
- Setup and job submission were done on an HPCC **development node** using VS Code Remote-SSH.
- Ensemble generation was executed as **SLURM batch jobs** on compute nodes (GPU or CPU depending on the run settings).
- Outputs were kept in scratch so they persisted across logouts/disconnects.

## Workflow overview
1) **Log in and create a project workspace**
   - Made a scratch project folder and organized it into inputs, outputs, and logs.

2) **Format sequence files**
   - Saved the 8 sequences in the layout BioEmu expects (either a combined FASTA list or separate FASTA files per protein).

3) **Launch BioEmu via SLURM**
   - Submitted SLURM jobs (often as an array) to run each protein independently.
   - Checked job status until all runs finished.

4) **Gather ensemble results**
   - BioEmu outputs an ensemble/trajectory-style result (commonly a trajectory file along with a topology/structure file).

5) **Export a PyMOL-ready multi-model PDB**
   - Extracted a chosen number of frames (for example, 50) and merged them into one multi-state PDB (e.g., `ensemble_50.pdb`) for PyMOL animation and alignment.

6) **Inspect in PyMOL**
   - Opened the ensemble PDBs in PyMOL.
   - Aligned ensemble members and, when available, compared them to experimental reference structures.

## Outputs
- For each protein: a multi-model ensemble PDB (e.g., `ensemble_50.pdb`).
- Optionally: a saved PyMOL session (`.pse`) and a brief note describing the structural variability observed.


## Notes
- This repository includes the workflow and conversion scripts. Large raw trajectory files can be left out of version control if desired.
