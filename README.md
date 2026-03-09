# Data for Natural variational annealing for multimodal optimization

Data for the remote sensing inverse problem example in the article:

Le Minh, T., Arbel, J., Möllenhoff, T., Khan, M. E. and Forbes, F. (2026+). Natural variational annealing for multimodal optimization. *Information and Inference: A Journal of the IMA.*


## Problem

The objective is to find the modes of the posterior distribution

p(ψ | y_obs)

for a given observation y_obs.

In this dataset:

- The observation vector y_obs is 10-dimensional
- The parameter vector ψ is 4-dimensional

The example corresponds to the nontronite mineral.

## Data Description

### Observation

```Obs.csv```

- Contains the observed signal y_obs
- Dimension: **10**

This is the signal corresponding to the nontronite sample.

### Training Data

Two files provide simulated data generated from the **Hapke forward model**, with additive Gaussian noise.

#### Parameter samples

```Xgllim.csv```

- 100,000 rows
- Each row is a parameter vector ψ ∈ R⁴

#### Generated observations

```Ygllim.csv```

- 100,000 rows
- Each row is a vector y ∈ R¹⁰

Each pair (ψ, y) satisfies

y = Hapke(ψ)

These simulated pairs are used to learn an approximation of the inverse mapping.

## Posterior Approximation with GLLiM

Using `Xgllim.csv` and `Ygllim.csv`, we train a **Gaussian Locally Linear Mapping (GLLiM)** model [1, 2].

The trained model provides a mixture-of-Gaussians approximation of the posterior distribution

p(ψ | y)

for any observation y.

Applying this model to `Obs.csv` yields an approximation of the posterior

p(ψ | y_obs)

which is the distribution of interest.

## Mode Detection with NVA

The **NVA algorithm** is then applied to the approximated posterior distribution.

NVA identifies **modes** ψ_1, ψ_2, ... of p(ψ | y_obs), producing candidate solutions.

## Evaluation of the Solutions

For this example, the true parameter ψ is unknown.

To evaluate the candidate solutions, each estimated parameter vector is passed through the Hapke model:

y_i = Hapke(ψ_i)

This produces reconstructed signals y_i in dimension 10.

The reconstructed signals are then compared with the original observation y_obs.

Figure 5 of the paper shows this comparison and illustrates how closely the reconstructed signals match the observed one.

## Additional software

The software PlanetGLLiM, available [here](https://kernelo-mistis.gitlabpages.inria.fr/planet-gllim-front-end/index.html), provides :
- the **GLLiM algorithm**
- the **Hapke model**

An implementation for NVA can be found [here](https://github.com/tam-leminh/natural-variational-annealing).

## References

[1] A. Deleforge, F. Forbes and R. Horaud. (2015). High-Dimensional Regression with Gaussian Mixtures and Partially-Latent Response Variables. Statistics and Computing, 25(5):893--911.

[2] B. Kugler, F. Forbes and S. Doute. (2022). Fast Bayesian Inversion for high dimensional inverse problems. Statistics and Computing, 32(2):31.
