# CMB Power Spectrum Analysis

A Google Colab notebook that computes the CMB temperature power spectrum from
ΛCDM parameters using **CAMB**, simulates a random full-sky map with
**healpy**, and shows how cosmic variance limits what one sky can tell us.

**What's inside**
- Theory spectrum with annotated acoustic peaks, Sachs–Wolfe plateau and Silk damping tail
- Simulated sky map and spectrum recovery, with cosmic variance measured over 500 realizations
- Parameter experiments varying Ωb h², Ωc h² and H₀
- Conceptual explanations alongside every plot, plus references (Planck 2018, CAMB, HEALPix)

**Run it:** open the notebook in Google Colab and choose *Runtime → Run all*.
**Requires:** `camb`, `healpy`, `numpy`, `matplotlib`, `scipy`
