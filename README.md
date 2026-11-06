# DFT optical spectra of 2D Mo–W–S–Se–Te transition-metal dichalcogenide alloys

This repository contains the first-principles dielectric functions and the supercell structures used in

> V. Chowdhury, T. A. Aditto, Md. Samrat, H. Imtiaz, and A. Zubair, *Machine learning based prediction of optical properties in two-dimensional Mo–W–S–Se–Te transition-metal dichalcogenide alloys through physics-informed sampling*.

For every alloy structure, the real part (ε₁) and the imaginary part (ε₂) of the frequency-dependent dielectric function are given for light polarized along the in-plane (**x**) and the out-of-plane (**z**) directions.

## Repository layout

```
Binary/       x/  z/     6 structures
Ternary/      x/  z/    45 structures
Quaternary/   x/  z/    36 structures
Quinary/      x/  z/    12 structures
structures/              Binary/ Ternary/ Quaternary/ Quinary/  one CIF file per structure (99)
compositions.csv         index of all structures (atom counts, supercell, file paths)
quinary_predictions.csv  model predictions for the quinary alloys
WMoSe2.zip, WMoTe2.zip   earlier upload: structures, band-structure files and spectra of the WMoSe2 and WMoTe2 series
```

Each alloy family folder holds one file per structure and direction, for example `Quaternary/x/Mo6_W10_S13_Te19_x.csv` and `Quaternary/z/Mo6_W10_S13_Te19_z.csv`. The structure of the same alloy is `structures/Quaternary/Mo6_W10_S13_Te19.cif`.

## Alloy families

| Family | General formula | Compositions | Structures |
|---|---|---|---|
| Binary | $\mathrm{MoQ_{2}}$, $\mathrm{WQ_{2}}$ | Q = S, Se, Te | 6 |
| Ternary | $\mathrm{W_{1-x}Mo_{x}Q_{2}}$ | x = n/16, n = 1, …, 15; Q = S, Se, Te | 45 |
| Quaternary | $\mathrm{W_{1-x}Mo_{x}Q_{2y}P_{2(1-y)}}$ | x = 3/16, 6/16, 10/16; y = 6/32, 13/32, 19/32, 26/32; (Q, P) = (S, Se), (S, Te), (Se, Te) | 36 |
| Quinary | $\mathrm{W_{1-x}Mo_{x}S_{2y}Se_{2z}Te_{0.25}}$ | x = 3/16, 6/16, 10/16; (N_S, N_Se) = (4, 24), (11, 17), (17, 11), (24, 4) | 12 |

`compositions.csv` lists every structure with its atom counts and the paths of its spectra and structure files.

## File names

File names give the number of atoms of each element in the simulated supercell, in the order Mo, W, S, Se, Te. Elements that are absent are omitted. For example, `Mo6_W10_S13_Te19` is the 48-atom cell $\mathrm{W_{10}Mo_6S_{13}Te_{19}}$, i.e. $\mathrm{W_{0.625}Mo_{0.375}S_{0.8125}Te_{1.1875}}$.

Most structures are 4×4×1 supercells with 16 metal sites and 32 chalcogen sites (48 atoms). The binary compounds and the ternary alloys with x = 0.25, 0.5 and 0.75 (`Mo1_W3_*`, `Mo2_W2_*`, `Mo3_W1_*`) are 2×2×1 supercells with 4 metal sites and 8 chalcogen sites (12 atoms). The Mo and W atoms were placed at random on the metal sublattice. In the quaternary and quinary structures, the top and bottom chalcogen layers have different compositions; the occupancy of each layer can be read from the CIF file.

## File format

### Structures

Each file in `structures/` is a CIF file (space group P1) of the supercell, with about 20 Å of vacuum along z. Atomic positions are given in fractional coordinates.

### Spectra

Every spectrum is a comma-separated file with a one-line header and 2000 rows:

| Column | Description |
|---|---|
| `energy_eV` | Photon energy (eV) |
| `epsilon1` | Real part of the dielectric function, ε₁(ω) |
| `epsilon2` | Imaginary part of the dielectric function, ε₂(ω) |

The energy grid starts at 0.01 eV and ends between 30 and 36 eV, depending on the structure. The spectra of `Ternary/*/Mo3_W13_Te32` extend to 93 eV. The upper limit of each file is listed in the `E_max_eV` column of `compositions.csv`.

```python
import pandas as pd

eps_x = pd.read_csv("Quaternary/x/Mo6_W10_S13_Te19_x.csv")
eps_z = pd.read_csv("Quaternary/z/Mo6_W10_S13_Te19_z.csv")
index = pd.read_csv("compositions.csv")

# structure (requires ASE)
from ase.io import read
atoms = read("structures/Quaternary/Mo6_W10_S13_Te19.cif")
```

## Calculation details

The spectra were calculated with density functional theory in CASTEP, using the PBE generalized-gradient approximation, norm-conserving pseudopotentials, Gaussian smearing of 0.05 eV, and a non-spin-polarized scheme. ε₂ was obtained from interband transitions, and ε₁ from ε₂ through the Kramers–Kronig relation. The complete settings are given in the paper.

## Contact

Hafiz Imtiaz (hafizimtiaz@eee.buet.ac.bd) and Ahmed Zubair (ahmedzubair@eee.buet.ac.bd), Department of Electrical and Electronic Engineering, Bangladesh University of Engineering and Technology, Dhaka 1205, Bangladesh.
