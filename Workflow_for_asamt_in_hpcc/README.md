# aSAMt / sam2 — HPCC Workflow (MSU)

## Objective
Create structural ensembles with aSAMt (sam2) using predicted starting models (Boltz-derived PDBs), then convert the outputs into a PyMOL-friendly format.

## Inputs used
- Per-protein **PDB** starting structures (converted from Boltz CIF results).
- An HPCC **scratch** directory to keep inputs and outputs organized.

## Execution environment
- Workspace setup and job launches were handled on an HPCC **development node** using VS Code Remote-SSH.
- Ensemble generation ran on compute nodes as a **SLURM batch job**, often as an **array** (one protein per task).
- Data was stored in scratch so runs weren’t interrupted if the local machine disconnected.

## Workflow overview
1) **Log in and initialize a workspace**
   - Made a scratch directory with a predictable folder structure.

2) **Stage starting structures**
   - Verified each protein had a clean, correctly formatted PDB.
   - Kept filenames consistent so automation could reliably pair inputs with outputs.

3) **Launch aSAMt via SLURM**
   - Submitted SLURM jobs (frequently arrays) to process proteins in parallel.
   - Tracked job status in the queue until completion.

4) **Gather results**
   - aSAMt produced a topology file plus a trajectory representing the ensemble for each target.

5) **Convert trajectories to multi-model PDBs**
   - Sampled a set number of frames (e.g., 50) and wrote them to `ensemble_50.pdb` for each protein.

6) **Inspect in PyMOL**
   - Opened `ensemble_50.pdb` in PyMOL.
   - Superposed ensemble conformations and aligned them to experimental PDBs when available.

## Outputs
- For each protein: `ensemble_50.pdb` (a multi-model ensemble PDB).
- Optional: a PyMOL `.pse` session file plus a brief summary comparing ensemble spread to any experimental structure.

## Notes
- The repository includes workflow scripts and conversion helpers; large trajectory files can be left out of GitHub.
