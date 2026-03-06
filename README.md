# Robustness of a Bayesian First Order AutoRegressive Model under Prior Perturbations


## Overview

This paper investigates whether posterior distributions obtained from a Bayesian AR(1) stochastic volatility model — estimated via the No-U-Turn Sampler (NUTS) — reliably capture latent volatility dynamics or reflect artefacts of modelling assumptions and prior specification.

Using a perturbation-based validation framework grounded in [Weiss (1996)](https://projecteuclid.org/euclid.lnms/1215452202), I systematically stress-test baseline posteriors through prior perturbation, case deletion, and observation-value shifts. Robustness is assessed via Kullback–Leibler divergence and Expected Log Pointwise Predictive Density (ELPD) across ASX equity (CBA, LYC, 4DMedical) and G10 FX (EURUSD, GBPJPY, USDJPY) datasets.

The central finding is that **latent volatility paths are structurally robust** across all perturbation classes, with posterior sensitivity concentrated in persistence (φ) and tail (ν) parameters rather than in the inferred volatility dynamics themselves. Regime separation and occupancy probabilities remain stable, supporting the conclusion that the model captures genuine stylised features — clustering, mean reversion, and persistence — rather than sampling artefacts.

## Key Results

- **KL Divergence:** Bounded distributional shifts under all perturbations. Largest deviations arise from global θ-scale and tail (ν) prior perturbations — consistent with known identification challenges in persistence parameters near unity.
- **ELPD:** Predictive adequacy remains stable under case deletion and observation shifts. Sensitivity is concentrated in tail specification, highlighting the importance of heavy-tailed innovation structure for predictive performance.
- **Regime Stability:** No regime crossover observed under any perturbation class. Regime-specific mean log-variance shifts remain within narrow bounds, and occupancy probabilities show minimal drift.

## Technical Challenges

**Posterior geometry.** The initial design called for a jointly estimated AR-HMM specification with discrete regime indicators embedded within the stochastic volatility model. This produced severe divergences and highly curved posterior geometries that standard NUTS could not reliably explore — motivating the modular approach (AR(1) volatility estimation followed by post-hoc HMM regime classification) used in the final implementation.

**Computational cost.** Full NUTS estimation with high-dimensional latent states across multiple perturbation configurations required significant compute. This led me to adopt Google Cloud Platform for parallelised posterior sampling — an unplanned but valuable addition to my technical workflow.

**Scope vs. depth.** Starting from a limited Bayesian background, this project required building fluency in MCMC theory, Hamiltonian dynamics, perturbation-based sensitivity analysis, and hidden Markov models simultaneously. The complexity was substantial, but the process of understanding *why* posterior distributions behave as they do — not just *how* to compute them — was the most intellectually rewarding aspect of the work.
