# Quantum Potential Dataset Generator

Dataset generator for quantum potentials and probability densities for machine learning models, especially JEPA-based sparse reconstruction.

## Description

This repository contains code and generated data for reconstructing complete quantum potentials `V(x)` and probability densities `rho(x)` from sparse potential measurements.

The dataset includes:

- Sparse potential measurements
- Measurement masks
- Normalized coordinate channel
- Interpolated potential prior
- Complete target potentials
- Complete target probability densities
- Labels identifying each potential family
- Train/validation splits

## Repository structure

```text
quantum-potential-dataset-generator/
├── README.md
└── notebooks
    ├── generate_quantum_potential_dataset.ipynb
└── dataset_export/
    ├── metadata.csv
    ├── generated_dataset_full_labeled.npz
    ├── generated_dataset_train_labeled.npz
    ├── generated_dataset_val_labeled.npz
    ├── x_train_labeled_flat.csv
    ├── v_train_labeled.csv
    ├── rho_train_labeled.csv
    └── by_family/
```

All generated files are stored inside:

```text
dataset_export/
```

The folder `by_family/` is also inside `dataset_export/` and contains separated CSV files for each potential family.

## Main files

### `generated_dataset_full_labeled.npz`

Main dataset file. It contains:

```text
x_train
v_train
rho_train
family_names
split
train_indices
val_indices
seed
```

### `metadata.csv`

Contains:

```text
sample_id
family_name
split
```

### `generated_dataset_train_labeled.npz`

Training subset.

### `generated_dataset_val_labeled.npz`

Validation subset.

### CSV files

```text
x_train_labeled_flat.csv
v_train_labeled.csv
rho_train_labeled.csv
```

These files are provided mainly for inspection and spreadsheet compatibility.

## Dataset shapes

```text
x_train:   (1800, 200, 4)
v_train:   (1800, 200)
rho_train: (1800, 200)
```

Number of samples:

```text
total: 1800
train: 1440
val:   360
```

## Potential families

The dataset contains 9 potential families:

```text
coulomb_radial_effective_potential
harmonic_oscillator_wavefunction
morse
radial_oscillator_potential
rose_morse
scar_II
solve_kratzer_potential
solve_poschl_teller_h
solve_poschl_teller_t
```

## Input features

The model input is stored in `x_train`.

Each sample has shape:

```text
x_train[i].shape = (200, 4)
```

The four channels are:

```text
x_train[:, :, 0] = sparse potential V_sparse(x)
x_train[:, :, 1] = measurement mask
x_train[:, :, 2] = normalized coordinate
x_train[:, :, 3] = interpolated potential prior
```

So the input can be interpreted as:

```text
X = [V_sparse, mask, x_coord, interpolation_prior]
```

Important:

```text
rho_train is not part of x_train.
rho_train is a supervised target.
```

## Targets

```text
v_train   = complete target potential V(x)
rho_train = complete target probability density rho(x)
```

The supervised structure is:

```text
x_train -> v_train
x_train -> rho_train
```

## Loading the dataset

```python
import numpy as np

data = np.load("dataset_export/generated_dataset_full_labeled.npz", allow_pickle=True)

x_train = data["x_train"]
v_train = data["v_train"]
rho_train = data["rho_train"]
family_names = data["family_names"]
split = data["split"]

print(x_train.shape)
print(v_train.shape)
print(rho_train.shape)
print(family_names[:10])
```

Expected output:

```text
(1800, 200, 4)
(1800, 200)
(1800, 200)
```

## PyTorch example

```python
import torch
from torch.utils.data import TensorDataset, DataLoader

x_tensor = torch.tensor(x_train, dtype=torch.float32)
v_tensor = torch.tensor(v_train, dtype=torch.float32)
rho_tensor = torch.tensor(rho_train, dtype=torch.float32)

dataset = TensorDataset(x_tensor, v_tensor, rho_tensor)

loader = DataLoader(dataset, batch_size=16, shuffle=True)

xb, yb_v, yb_rho = next(iter(loader))

print(xb.shape)
print(yb_v.shape)
print(yb_rho.shape)
```

Expected batch shapes:

```text
xb:     (16, 200, 4)
yb_v:   (16, 200)
yb_rho: (16, 200)
```

## Requirements

Install dependencies with:

```bash
pip install -r requirements.txt
```

Suggested packages:

```text
numpy
scipy
pandas
matplotlib
torch
scikit-learn
jupyter
```

## Notes

The recommended file for sharing is:

```text
dataset_export/generated_dataset_full_labeled.npz
```

It preserves the original array shapes and includes the family labels for each sample.

## Author

Rafael de Oliveira Lima
