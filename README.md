# Protein Threading by Double Dynamic Programming

This repository contains a Python implementation of the THREADER sequence-to-structure alignment algorithm (Jones, 1998). It performs protein fold recognition by threading a target amino acid sequence onto a known candidate structure, evaluating the optimal fit using DOPE statistical pseudo-energies.

## Pipeline Overview
1. **Data Parsing:** Extracts alpha carbon (CA) coordinates, constructs a distance matrix, and assigns secondary structures (Helix/Strand/Coil) from PDB files. Target sequences are parsed dynamically from FASTA files.
2. **Low-Level Alignment (Matrix L):** For candidate anchor pairs, an inner Needleman & Wunsch algorithm computes the optimal structural fit using DOPE pseudo-energies.
3. **High-Level Alignment (Matrices H & F):** Optimal low-level paths are accumulated into a high-level matrix (H). A final Needleman & Wunsch pass (Matrix F) generates the optimal global alignment and returns the final DOPE pseudo-energy.
4. **Evaluation:** The pipeline calculates the Root Mean Square Deviation (RMSD) between the predicted alignment and the true native structure using the Kabsch algorithm.

## Requirements
* Python 3.14.7
* NumPy 2.5.3

```bash
pip install -r requirement.txt
```

## Usage
The entire pipeline is orchestrated through the command-line interface in `main.py`

```bash
python main.py <structure.pdb> <sequence.fasta> <true_structure.pdb> --seq_id <PDB_ID> <CHAIN>
```

### Positional Arguments
* `structure_pdb`: Path to the candidate structural template.
* `sequence_fasta`: Path to the target sequence.
* `sequence_pdb`: Path to the true native structure of the target (used exclusively for Kabsch RMSD evaluation).

### Optional Arguments
* `--seq_id`: The PDB ID and chain to extract from the FASTA file (e.g., `--seq_id 1CPC A`).
* `--dope`: Path to the DOPE parameter file (defaults to `./data/dope.par`).
* `--stru_chain` / `--target_chain`: Specify which chains to extract from the PDB files (defaults to "A").
* `--verbose`: Prints execution progress to the terminal.
* `--outdir`: Directory to save the final alignment text files and the `threading_results_log.csv` (defaults to `./results`).

### Example Run
```bash
python main.py data/1MBD.pdb data/1MBA.fasta data/1MBA.pdb --seq_id 1MBA A --dope data/dope.par --verbose
```
