# Boltz — MSU HPCC Workflow

## Goal
Run Boltz on Michigan State’s HPCC to predict 3D structures for the eight course proteins, then inspect the results (and optionally convert formats) for later work in PyMOL.

## Inputs used
- Protein sequence data (all 8 course protein sequences).
- A project workspace on HPCC scratch to keep inputs and outputs organized.

## Where tasks were performed
- We logged into an HPCC **development node** using VS Code Remote-SSH to edit files, set up inputs, and submit jobs.
- Compute-intensive runs were executed as **SLURM batch jobs** on the cluster’s compute nodes.
- Everything was stored under **`/mnt/scratch/<netid>/`** to ensure files persisted after the session ended.

## High-level workflow
1) **Connect and create a workspace**
   - Used VS Code Remote-SSH to access an HPCC dev node.
   - Made a scratch project directory with a basic structure (e.g., `inputs/`, `outputs/`, `logs/`).

2) **Format sequence files**
   - Converted the 8 sequences into the format Boltz expects (FASTA and/or YAML depending on setup).
   - Treated each protein as its own separate run/input.

3) **Submit Boltz with SLURM**
   - Launched the computation using a SLURM batch script so it ran on compute hardware.
   - Checked job progress with queue/status commands until completion.

4) **Gather predicted structures**
   - Retrieved Boltz predictions from the outputs directory.
   - Results were typically produced as **mmCIF (`.cif`)** structure files.

5) **Visualize and/or convert for PyMOL**
   - Loaded structures into PyMOL for viewing and comparison.
   - When needed for other tools, converted `.cif` files to **`.pdb`** (via PyMOL export or a conversion utility).

## Deliverables
- Predicted structure outputs for each protein (usually `.cif`, plus optional `.pdb` versions).
- Optional PyMOL session (`.pse`) containing aligned/compared structures.

## Notes
- The repository is meant for documentation and scripts; large model files, caches, or weights are excluded from version control.
