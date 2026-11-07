Predicting Hydrogen Production from Steam Methane Reforming Powered by Induction Heating: An Application of a Hybrid Bayesian Neural Network

OVERVIEW

This repository contains a notebook that processes steam methane reforming (SMR) operating data and compares three neural-network approaches for predicting hydrogen production (Mass of Hydrogen – MoH):

FNN — Feedforward Neural Network baseline.
BNN— Dropout-based Bayesian Neural Network with Monte Carlo (MC) dropout inference.
H-BNN — Hybrid Bayesian Neural Network initialized from the corresponding FNN weights and trained with a heteroscedastic Gaussian likelihood.

The workflow also performs preliminary data analysis, shared architecture and hyperparameter selection, model evaluation, hydrogen output postprocessing, and model-comparison plotting.

The H-BNN framework follows the algorithm originally developed and applied to induction-heated steam methane reforming. We then adapted the same computational framework for the methane pyrolysis repository, with process-specific modifications to the dataset, input variables, target definition, and post-processing calculations.

REPOSITORY FILES

The “Steam___Methane___Reforming___H2___CODE” notebook containing data preparation, preliminary analysis, architecture and hyperparameter search, FNN/BNN/H-BNN training and evaluation, post-processing, and comparison plots. It includes Palatia typography declarations and an optional Matplotlib font loader.

The `Dataset__SMR.csv` — Input dataset containing experimental operating conditions and derived process variables for induction-heated steam methane reforming.

The `requirements.txt` — Python package dependencies.

Also, the `README.md` — Project description and run instructions.

DATASET

The notebook reads `Dataset__SMR.csv`. The dataset contains experimental operating variables and derived quantities from literature-based studies of induction-heated SMR. The principal columns include:

`F(mL/h)`, `P(bar)`, `T(C)`, `TOS(h)`, `p(W)`, `B(mT)`, `X(%)`, and `Y(%)`.

Additional calculated variables include:

`v(m3/h)`, `P(Pa)`, `T(K)`, `n_CH4_in(mol/h)`, `n_CH4_r(mol/h)`, `n_H2(mol/h)`, `m_H2(g/h)`, `MoH(g)`, `E_in(kWh)`, `SEC(kWh/kg H2)`, and `EI(kgCO2/kgH2)`.

These variables support the preliminary statistical analysis, feature preparation, hydrogen production calculations, and energy-performance assessment.

Important: The Preliminary Analysis block in the supplied notebook still contains methane-pyrolysis-specific column references, including `Energy_interval(kWh)`, `MoH_interval(kg)`, and `Y_H2_cum_measured(%)`. These columns are not present in the supplied SMR dataset and must be reconciled with the corresponding SMR variables before that section can execute correctly.

MODEL INPUTS AND TARGET

The published SMR study identifies six principal operating variables for predicting hydrogen production. The detailed feature encoding formulation in Appendix A.2 represents the operating conditions using:

`T`, `TOS_mid`, `F`, `B`, `p`, and `Delta_t`.

Here, `TOS_mid` is calculated as:

`TOS_mid = TOS - 0.5 * Delta_t(h)`.

The continuous variables are temperature and midpoint time on stream, while methane flow rate, magnetic flux, power input, and interval duration are one-hot encoded.

The resulting encoded input representation is described by:

`d = 2 + n_F + n_B + n_p + n_Delta_t`.

For the operating levels reported in the publication, this gives an input dimension of 22 features.

The published H-BNN formulation uses a transformed hydrogen-production target, with predicted hydrogen-production rates converted into interval hydrogen masses during post-processing.

The supplied notebook's current data-preparation cell specifies `T(C)`, `P(bar)`, `F(mL/h)`, `Y(%)`, `p(W)`, and `TOS(h)` as its six input columns and `MoH(g)` as its target. This implementation should be checked against the published feature encoding and target definitions before claiming exact reproduction of the original results.

RUN IN GOOGLE COLAB

1. Open `Steam___Methane___Reforming___H2___CODE` in Google Colab.
2. Upload `Dataset__SMR.csv` to `/content/`.
3. Ensure that the CSV filename and path match the loading statement in the notebook.
4. Run the notebook cells in their existing order after reconciling the identified SMR column references.
5. To use the Palatia font in plots, upload an authorized `Palatia.ttf` file to `/content/`. Otherwise, Matplotlib uses its default font.

The notebook uses NumPy, Pandas, PyTorch, scikit-learn, and Matplotlib. Install any missing packages if the runtime reports an import error.



REPRODUCIBILITY

The published SMR study uses a 70:15:15 training, validation, and test split and training-only data augmentation at scaling factors `k = 2`, `5`, and `10`.

The modelling workflow uses NumPy and PyTorch random seeds of `42`. The original methane pyrolysis implementation retains the same general reproducibility strategy, with dataset-specific preprocessing and augmentation. Exact numerical reproducibility can still depend on library versions, hardware, and runtime configuration.

ABOUT THIS CODE

The code was cleaned for presentation and navigation. Markdown title and section-heading cells were added, and the original code-cell source and saved outputs were retained.

The notebook implements the FNN, BNN, and H-BNN modelling framework associated with the published SMR study. The same algorithmic framework is also used in the subsequent methane-pyrolysis implementation.

The dataset specific preprocessing, preliminary analysis, target calculations, and post-processing must remain consistent with the SMR publication when reproducing its reported results.

