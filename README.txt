
Dataset generated for the JEPA potential reconstruction model.

Files:

metadata.csv
    Contains sample_id, family_name and split.

generated_dataset_full_labeled.npz
    Contains:
    - x_train
    - v_train
    - rho_train
    - family_names
    - split
    - train_indices
    - val_indices
    - seed

x_train_labeled_flat.csv
    Flattened model input with sample_id, family_name and split.

v_train_labeled.csv
    Complete target potentials V(x), with sample_id, family_name and split.

rho_train_labeled.csv
    Complete target probability densities rho(x), with sample_id, family_name and split.

by_family/
    Contains separate CSV files for each potential family.

Shapes:
x_train: (1800, 200, 4)
v_train: (1800, 200)
rho_train: (1800, 200)

Number of samples:
total: 1800
train: 1440
val: 360

Families:
['coulomb_radial_effective_potential', 'harmonic_oscillator_wavefunction', 'morse', 'radial_oscillator_potential', 'rose_morse', 'scar_II', 'solve_kratzer_potential', 'solve_poschl_teller_h', 'solve_poschl_teller_t']
