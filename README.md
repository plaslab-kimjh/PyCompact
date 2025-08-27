# PyCompact

**A fully open‑source workflow for DEM–MPFEM powder compaction.**  
PyCompact integrates **FreeCAD**, **LIGGGHTS**, **ParaView**, **LS‑PrePost**, and **OpenRadioss**, bridged by two Jupyter notebooks: `notebooks/InputGen.ipynb` (DEM input generator) and `notebooks/MeshMatGen.ipynb` (DEM→MPFEM mesh/material generator). The workflow reproduces the whole cycle — geometry, DEM packing, MPFEM compaction, and visualization — and was validated against experiments with ≈**2.5%** density deviation.

> Paper: *PyCompact: A Fully Open‑source Workflow for Discrete Element Method–Multi‑Particle Finite Element Method for Powder Compaction Simulation* (SoftwareX submission).

---

## Highlights
- End‑to‑end **open toolchain**: FreeCAD → LIGGGHTS → ParaView → LS‑PrePost → OpenRadioss.
- Two **Jupyter notebooks** as the automation backbone.
- Validated on Fe–Si–Al–P powders (Fine & Coarse PSDs); reproduces density–pressure trends within ≈2.5%.
- MIT licensed; citable via `CITATION.cff` (and optional Zenodo DOI badge once minted).

## Architecture
1. **DEM packing** (`notebooks/InputGen.ipynb`): reads an Excel PSD file (radii + fraction/density) and a LIGGGHTS input template, then writes a ready‑to‑run LIGGGHTS script. DEM export: VTK.
2. **DEM→MPFEM conversion** (`notebooks/MeshMatGen.ipynb`): reads CSV (x, y, z, r) exported from ParaView, creates an LS‑PrePost command file (`cfile`) to generate meshed spheres and a `material.k` block for material assignment. Outputs are assembled to run in **OpenRadioss**. Post‑processing in ParaView.

## System requirements
- **OS**: Windows
- **Python**: 3.10+
- **External tools** (install separately): OpenRadioss r2024, LIGGGHTS‑PUBLIC 3.8, ParaView 5.13.2, FreeCAD 1.0.2, LS‑PrePost 4.9
- See `environment.yml` or `requirements.txt` for Python dependencies only.

## Installation (Python env)
Using conda (recommended):
```bash
conda env create -f environment.yml
conda activate pycompact
```
or pip:
```bash
python -m venv .venv
. .venv/Scripts/activate    # Windows
pip install -r requirements.txt
```

## Quick start
1. **Prepare PSD**: place your Excel file under `data/` (e.g., `data/psd.xlsx`) with columns of radii and either volume fractions or probability density.
2. **Generate DEM input**: open `notebooks/InputGen.ipynb`, set atom type/density and paths, run to create a LIGGGHTS input file from the PSD and template.
3. **Run DEM** in LIGGGHTS to pack particles and export **VTK**.
4. **Convert to CSV** in ParaView with columns `(x, y, z, r)`.
5. **Build MPFEM model**: open `notebooks/MeshMatGen.ipynb`, set meshing parameters and particle count; run to create the **LS‑PrePost** `cfile` and `material.k`.
6. In **LS‑PrePost**, finish tool geometry, contacts, and boundary conditions. Export keyword (`.k`) and replace the placeholder material block with `material.k`.
7. **Solve** in **OpenRadioss** (loading→unloading). Visualize in **ParaView**.

> Tip: start from the example assets in `templates/` and `examples/` and adapt mesh density/edge size to your particle radii.

## Reproduce the paper figures
- Fine powder: RVE ≈ 210 μm, ~333 particles; Coarse powder: RVE ≈ 1000 μm, ~403 particles.
- Postprocess force–displacement and relative density (after unloading) with ParaView/Python utilities.

## Repository layout
```
PyCompact/
  notebooks/
    InputGen.ipynb
    MeshMatGen.ipynb
  data/                 # example PSDs (user-provided)
  templates/            # LIGGGHTS input template, LS‑PrePost snippets
  outputs/              # generated files (ignored by git)
  examples/             # minimal demo configs
  CITATION.cff
  environment.yml
  requirements.txt
  CHANGELOG.md
  LICENSE
  .gitignore
```

## Citing
If you use PyCompact, please cite the SoftwareX article (when available) and/or this repository:
- See `CITATION.cff` for BibTeX/APA via GitHub’s **Cite this repository** button.
- Once a Zenodo DOI is minted, add its badge here and in the paper’s Code Metadata table.

## License
MIT © Pusan National University authors. See `LICENSE`.

## Acknowledgments
This work was supported by the National Research Foundation of Korea and partners (see manuscript).

---
*Maintainers:* Majid Mohammadhosseinzadeh, Hossein Ghorbani‑Menghari, Ji Hoon Kim
