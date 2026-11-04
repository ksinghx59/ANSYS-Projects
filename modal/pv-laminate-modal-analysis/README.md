# Modal Analysis of a 3-Layer Photovoltaic Laminate

![Mode 1 – first bending mode, 21.5 Hz](Images/02-intro.png)

## Objective
Determine the natural frequencies and mode shapes of a glass/POE/glass photovoltaic
laminate (without silicon cells) using ANSYS Mechanical modal analysis, and compare
two support conditions: free-free and one edge fixed (cantilever).

## Tools
ANSYS Mechanical 2026 R1 (Student)

## Model
- Geometry: 415 x 298 mm laminate, 8 mm total thickness (STEP file included)
- Layers: bottom glass 3.6 mm / POE interlayer 0.8 mm / top glass 3.6 mm
- Glass: E = 70 GPa, v = 0.23, density = 2500 kg/m³
- POE: E = 11 MPa, v = 0.45, density = 857 kg/m³
- Contacts: bonded between all layers
- Mesh: 16,420 elements, 116,864 nodes (same mesh used for both cases)
- Modes extracted: 12

![Mesh](Images/02-fixed-mesh.png)

## Case 1: Free-free (no supports)
Modes 1–6 are rigid-body modes (about 0 Hz), which is expected for an unsupported body.
The first elastic mode is mode 7.

| Mode | Frequency (Hz) |
|---|---|
| 7 | 150.31 |
| 8 | 183.34 |
| 9 | 301.34 |
| 10 | 316.20 |
| 11 | 376.00 |
| 12 | 402.07 |

### Mode shapes (free-free)
| | | |
|---|---|---|
| ![](Images/02-free-mode7.png)<br>Mode 7 – 150.31 Hz | ![](Images/02-free-mode8.png)<br>Mode 8 – 183.34 Hz | ![](Images/02-free-mode9.png)<br>Mode 9 – 301.34 Hz |
| ![](Images/02-free-mode10.png)<br>Mode 10 – 316.20 Hz | ![](Images/02-free-mode11.png)<br>Mode 11 – 376.00 Hz | ![](Images/02-free-mode12.png)<br>Mode 12 – 402.07 Hz |

## Case 2: One edge fixed (cantilever)
One 298 mm edge is fixed, so the plate extends 415 mm from the support.
There are no rigid-body modes, so mode 1 is the lowest natural frequency.

![Fixed support setup](Images/01-fixed-support.png)

| Mode | Frequency (Hz) |
|---|---|
| 1 | 21.50 |
| 2 | 81.00 |
| 3 | 152.16 |
| 4 | 246.74 |
| 5 | 334.39 |
| 6 | 368.49 |
| 7 | 460.51 |
| 8 | 480.14 |
| 9 | 667.37 |
| 10 | 715.53 |
| 11 | 716.45 |
| 12 | 764.22 |

### Mode shapes (one edge fixed)
| | | |
|---|---|---|
| ![](Images/02-fixed-mode1.png)<br>Mode 1 – 21.50 Hz | ![](Images/02-fixed-mode2.png)<br>Mode 2 – 81.00 Hz | ![](Images/02-fixed-mode3.png)<br>Mode 3 – 152.16 Hz |
| ![](Images/02-fixed-mode4.png)<br>Mode 4 – 246.74 Hz | ![](Images/02-fixed-mode5.png)<br>Mode 5 – 334.39 Hz | ![](Images/02-fixed-mode6.png)<br>Mode 6 – 368.49 Hz |
| ![](Images/02-fixed-mode7.png)<br>Mode 7 – 460.51 Hz | ![](Images/02-fixed-mode8.png)<br>Mode 8 – 480.14 Hz | ![](Images/02-fixed-mode9.png)<br>Mode 9 – 667.37 Hz |
| ![](Images/02-fixed-mode10.png)<br>Mode 10 – 715.53 Hz | ![](Images/02-fixed-mode11.png)<br>Mode 11 – 716.45 Hz | ![](Images/02-fixed-mode12.png)<br>Mode 12 – 764.22 Hz |

Mode shapes are normalised, so displacement values show relative motion only,
not real displacement.

## Validation (MATLAB hand calculation)

The first bending mode of the fixed-edge case was checked with an Euler–Bernoulli
cantilever model (script: `laminate_cantilever_check.m`, free length 415 mm).

| Estimate | Assumption | f1 (Hz) |
|---|---|---|
| A | Glass layers slide freely (no shear transfer through POE) | 17.5 |
| B | Layers fully bonded (full composite action) | 41.1 |
| C | Partial interaction using the POE shear modulus (approximate) | 34.7 |
| ANSYS | Bonded contacts, POE E = 11 MPa, v = 0.45 | 21.5 |

The ANSYS result lies between the two limiting cases (A and B) but is about 38%
below the partial-interaction estimate (C). Estimate C is approximate (beam theory
with a shear-transfer factor), so the difference is not conclusive. Possible causes
are being investigated: the contact formulation and mismatched mesh sizes between
the glass and POE layers.

![Frequency vs POE stiffness](Images/validation-plot.png)

## Conclusion
With one edge fixed, the first natural frequency is 21.5 Hz (first bending mode),
compared with 150.3 Hz for the first elastic mode of the free-free case. The
support condition therefore has a very strong effect on the modal behaviour of
the laminate. Both cases use the same mesh, so the difference comes from the
boundary conditions alone.

## Files
- /Images: setup, mesh and mode shape plots (free-free and fixed cases)
- PV_Laminate_3Layer.STEP: laminate geometry
