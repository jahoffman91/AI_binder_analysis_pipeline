# External Tool Installation Guide

The pipeline integrates several external structural biology tools. Not all steps
require all tools -- you can skip steps in `config.yaml` by setting `enabled: false`
for any step that depends on a tool you don't have installed.

## Required for Core Pipeline

### FoldSeek (structural clustering)

Needed for: Processing step 13, Analysis steps 6-7

```bash
# conda (recommended)
conda install -c conda-forge -c bioconda foldseek

# or download binary from:
# https://github.com/steineggerlab/foldseek
```

**Verify:** `foldseek --help`

---

## Required for Binding Affinity Predictions

### PyMOL (structural alignment)

Needed for: Processing steps 4-7

```bash
# conda (open-source build)
conda install -c conda-forge pymol-open-source

# or macOS app: https://pymol.org/
# or Linux: sudo apt install pymol
```

Set the path in `config.yaml` under `tools.pymol_path`.

**Verify:** `pymol -c -d "print('ok')" -d "quit"`

### PRODIGY (binding affinity from structure)

Needed for: Processing step 10

```bash
pip install prodigy
# or: https://github.com/haddocking/prodigy
```

**Verify:** `prodigy --help`

### PyRosetta (interface energy calculations)

Needed for: Processing step 8

PyRosetta requires an academic license. See:
https://www.pyrosetta.org/downloads

```bash
# After obtaining credentials:
pip install pyrosetta-installer
python -c "import pyrosetta_installer; pyrosetta_installer.install_pyrosetta()"
```

**Verify:** `python -c "import pyrosetta; pyrosetta.init()"`

### PPI-Graphormer (ML-based binding affinity)

Needed for: Processing step 9

1. Clone the repository:
   ```bash
   git clone https://github.com/microsoft/PPI-Graphormer.git ~/Documents/PPI-Graphormer
   ```
2. Create the conda environment:
   ```bash
   cd ~/Documents/PPI-Graphormer
   conda env create -f environment.yml -n ppi-graphomer
   ```
3. Update `config.yaml`:
   ```yaml
   tools:
     ppi_graphormer:
       conda_env: "ppi-graphomer"
       repo_path: "~/Documents/PPI-Graphormer"
   ```

### FreeSASA (solvent-accessible surface area) -- optional

Used by: PyRosetta step (optional enhancement)

```bash
pip install freesasa
```

---

## Checking Your Environment

Run the pipeline with `--dry-run` to verify all paths resolve without actually
executing any steps:

```bash
python -m pipeline run --phase all --dry-run
```
