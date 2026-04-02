# Molecular Dynamics Simulations

[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)
[![GROMACS](https://img.shields.io/badge/Engine-GROMACS-green)](https://www.gromacs.org/)
[![Status: Active](https://img.shields.io/badge/Status-Active-brightgreen)]()

> Simulations and analysis of molecular interactions and systems using **GROMACS** and related computational tools.

---

## Overview

This repository houses a collection of molecular dynamics (MD) simulation workflows built around the GROMACS engine. It covers the full MD pipeline — from system preparation and energy minimization to production runs and post-simulation analysis — making it useful as both a research reference and a learning resource for computational biophysics and chemistry.

The simulations are designed to study molecular interactions, conformational dynamics, thermodynamic properties, and structural behavior of biomolecular or chemical systems over time.

---

## Key Features

- Ready-to-use GROMACS parameter (`.mdp`) files for common MD protocols
- Analysis scripts for trajectory processing and property extraction
- Organized directory structure for reproducible simulation workflows
- Result datasets for comparison and validation
- Documentation and notes covering methodology and interpretation

---

## Directory Structure

```
Molecular_Dynamics/
│
├── data/                  # Input structure files and raw simulation trajectories
│   ├── *.gro              # GROMACS coordinate files
│   ├── *.top              # Topology files
│   └── *.tpr              # Portable binary run input files
│
├── scripts/               # Python/Bash/GROMACS analysis scripts
│   ├── rmsd.py            # RMSD calculation and plotting
│   ├── rmsf.py            # RMSF per-residue fluctuation analysis
│   ├── rg.py              # Radius of gyration
│   └── ...                # Additional analysis utilities
│
├── mdp_files/             # GROMACS run parameter files
│   ├── em.mdp             # Energy minimization parameters
│   ├── nvt.mdp            # NVT equilibration (constant volume & temperature)
│   ├── npt.mdp            # NPT equilibration (constant pressure & temperature)
│   └── md.mdp             # Production MD run parameters
│
├── results/               # Processed output data and plots
│   ├── plots/             # Figures (RMSD, RMSF, Rg, energy, etc.)
│   └── data/              # Extracted numerical results (CSV, XVG)
│
├── docs/                  # Additional documentation and notes
│   └── methodology.md     # Simulation setup and protocol descriptions
│
├── LICENSE                # GPL-3.0 License
└── README.md              # This file
```

---

## Simulation Workflow

A typical GROMACS MD simulation in this repository follows the standard protocol:

```
1. System Preparation
   └── Structure → Solvation → Ionization

2. Energy Minimization
   └── Steepest Descent / L-BFGS  (em.mdp)

3. Equilibration
   ├── NVT — Thermostat equilibration  (nvt.mdp)
   └── NPT — Barostat equilibration   (npt.mdp)

4. Production MD Run
   └── Unrestrained simulation        (md.mdp)

5. Analysis
   └── RMSD · RMSF · Rg · H-bonds · Energy · Clustering
```

---

## Prerequisites

Ensure the following tools are installed before running simulations:

| Tool | Purpose | Recommended Version |
|------|---------|-------------------|
| [GROMACS](https://www.gromacs.org/) | MD engine | 2021+ |
| [Python 3](https://www.python.org/) | Analysis scripting | 3.8+ |
| [NumPy](https://numpy.org/) | Numerical computation | Latest |
| [Matplotlib](https://matplotlib.org/) | Plotting | Latest |
| [MDAnalysis](https://www.mdanalysis.org/) *(optional)* | Trajectory analysis | Latest |
| [VMD](https://www.ks.uiuc.edu/Research/vmd/) *(optional)* | Visualization | Latest |

---

## 🚀 Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/Om-Physics/Molecular_Dynamics.git
cd Molecular_Dynamics
```

### 2. Install Python Dependencies

```bash
pip install numpy matplotlib MDAnalysis
```

### 3. Run Energy Minimization

```bash
gmx grompp -f mdp_files/em.mdp -c data/system.gro -p data/topol.top -o em.tpr
gmx mdrun -v -deffnm em
```

### 4. NVT Equilibration

```bash
gmx grompp -f mdp_files/nvt.mdp -c em.gro -r em.gro -p data/topol.top -o nvt.tpr
gmx mdrun -deffnm nvt
```

### 5. NPT Equilibration

```bash
gmx grompp -f mdp_files/npt.mdp -c nvt.gro -r nvt.gro -t nvt.cpt -p data/topol.top -o npt.tpr
gmx mdrun -deffnm npt
```

### 6. Production MD Run

```bash
gmx grompp -f mdp_files/md.mdp -c npt.gro -t npt.cpt -p data/topol.top -o md.tpr
gmx mdrun -deffnm md
```

---

## Analysis

After the production run, use the provided scripts to extract key observables:

```bash
# RMSD (Root Mean Square Deviation)
gmx rms -s md.tpr -f md.xtc -o results/data/rmsd.xvg

# RMSF (Root Mean Square Fluctuation)
gmx rmsf -s md.tpr -f md.xtc -o results/data/rmsf.xvg -res

# Radius of Gyration
gmx gyrate -s md.tpr -f md.xtc -o results/data/gyrate.xvg

# Run Python plotting scripts
python scripts/rmsd.py
python scripts/rmsf.py
```

Results (plots and processed data) are saved to the `results/` directory.

---

## MDP Parameter Files

| File | Stage | Key Settings |
|------|-------|-------------|
| `em.mdp` | Energy Minimization | `integrator = steep`, `emtol = 1000` |
| `nvt.mdp` | NVT Equilibration | `tcoupl = V-rescale`, `ref-t = 300` |
| `npt.mdp` | NPT Equilibration | `pcoupl = Parrinello-Rahman`, `ref-p = 1.0` |
| `md.mdp` | Production Run | Long-timescale, unrestrained dynamics |

---

## License

This project is licensed under the **GNU General Public License v3.0**. See the [LICENSE](LICENSE) file for details.

---

## Contributing

Contributions, bug reports, and improvements are welcome!

1. Fork the repository
2. Create a new feature branch (`git checkout -b feature/your-feature`)
3. Commit your changes (`git commit -m 'Add new analysis script'`)
4. Push to your branch (`git push origin feature/your-feature`)
5. Open a Pull Request

---

## Contact

**Om-Physics**
GitHub: [@Om-Physics](https://github.com/Om-Physics)

---

## 📚 References & Resources

- [GROMACS Manual](https://manual.gromacs.org/)
- [GROMACS Tutorials by Justin Lemkul](http://www.mdtutorials.com/gmx/)
- [MDAnalysis Documentation](https://docs.mdanalysis.org/)
- Frenkel, D. & Smit, B. *Understanding Molecular Simulation* (2002)
- Allen, M.P. & Tildesley, D.J. *Computer Simulation of Liquids* (2017)
