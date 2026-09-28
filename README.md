# Random-field Ising model on scale-free networks: an interactive demo

This is an in-browser companion to

> S. H. Lee, H. Jeong, and J. D. Noh, "Random field Ising model on networks with inhomogeneous connections," *Phys. Rev. E* **74**, 031118 (2006). [doi:10.1103/PhysRevE.74.031118](https://doi.org/10.1103/PhysRevE.74.031118)

The demo computes exact zero-temperature ground states of the random-field Ising model (RFIM) on scale-free networks. It reproduces the paper's main results on small networks:

- For 2 < γ < 3 the system is always magnetized.
- For 3 < γ < 5 the type of transition depends on the sign of a constant D.
- For γ > 5 the type of transition depends on the sign of p₀″(0).

*Created by Claude Opus 5.5 (Anthropic).*

## Running it

The demo is a single self-contained file, `index.html`, with no build step and no dependencies. You can run it in two ways:

- **Locally:** open `index.html` in any modern browser.
- **GitHub Pages:** push this folder to a repository and enable Pages from the repository settings. `index.html` is served at the site root.

All computation runs in the browser. The only external request is to Google Fonts, and the page falls back to system fonts if that request fails.

## The model

The Hamiltonian is

```
H = −J Σ_<ij> s_i s_j − Σ_i h_i s_i,   s_i = ±1,  J = 1
```

The quenched random fields are i.i.d. with p(h) = p₀(h/Δ)/Δ. The order parameter is the degree-weighted magnetization m = |Σ k_i s_i| / Σ k_i.

The demo offers three field shapes, each supported on −1 ≤ x ≤ 1:

| Name | p₀(x) | Mean-field behaviour |
|------|-------|----------------------|
| p₊ | (3/2) x² | D > 0 and p₀″(0) > 0, so the transition is first-order |
| p₋ | (π/4) cos(πx/2) | D < 0 and p₀″(0) < 0, so the transition is continuous |
| p₃/₄ | 0.45 + 0.15 x² (the paper's example with a = 3/4) | p₀″(0) > 0, yet D < 0 at γ = 4, so the transition is continuous |

## What the page shows

### 1. Ground state on one network

A static-model network with 100 to 800 nodes is drawn with a force-directed layout.
- Node colour shows the spin and node size shows the degree.
- Broken bonds and spins that point against their own field can be highlighted.
- The ground state is recomputed every time Δ, γ, the field shape or k̄ changes.
- A small plot shows m(Δ) for this one realization next to the mean-field curve.

### 2. Mean-field theory (paper §II)

- The self-consistency map f(m), plotted against the line m (paper Fig. 1).
- The mean-field order parameter m(Δ).
- The predicted transition type, with D, p₀″(0), Δc and β for the current settings.
- β(γ) plotted with the paper's simulation estimates (paper Fig. 7).

### 3. Finite-size simulation (paper §III)

Many independent network and field samples are solved exactly across a grid of Δ, for several network sizes. The results are shown as:
- ⟨m⟩ versus Δ, with an optional log–log view and a reference slope for γ < 3 (paper Figs. 2 and 4).
- The Binder parameter U = 1 − ⟨m⁴⟩ / 3⟨m²⟩², with an estimate of where the curves cross (paper Fig. 5).
- Stacked histograms H(m) near the threshold, where two peaks indicate a first-order transition (paper Fig. 3).
- A finite-size scaling collapse with adjustable Δc, β and ν′ (paper Fig. 6).

## Methods

**Networks.** Networks are generated with the static model of Goh, Kahng and Kim [2]:
1. Node i is given weight i^(−1/(γ−1)).
2. N·k̄/2 links are placed between pairs of nodes, each chosen with probability proportional to its weight.
3. Self-loops and duplicate links are rejected.

**Exact ground states.** The ground state is found by mapping the problem onto a minimum s–t cut, and the cut is computed with Dinic's max-flow algorithm. The flow network is built as follows:
- Each positive field h_i becomes an edge source → i with capacity 2h_i.
- Each negative field becomes an edge i → sink with capacity 2|h_i|.
- Each link becomes capacity 2J in both directions.

After max-flow, spins still reachable from the source in the residual graph point up. The solver was checked against brute-force enumeration on 300 random 10-node instances, and all of them matched.

**Mean field.** The continuum theory uses P(k) = c k^(−γ) for k ≥ k₀, with k₀ = k̄(γ−2)/(γ−1). The integral for f(m) is evaluated in log k with Simpson's rule, and the part where G(x) = 1 is added analytically. The stable solution is the largest root of m = f(m).

**Reproducibility.** All random numbers come from a seeded PRNG (mulberry32), so every run gives the same results.

## Caveats

- **Mean degree.** The paper does not state its mean degree. k̄ ≈ 16 gives thresholds close to those in its figures. The demo defaults to k̄ = 10 so the network drawing stays readable.
- **Network size.** The networks here have up to 4,000 nodes, compared with the paper's 64,000. Transitions therefore look rounder and fitted exponents drift. For γ < 3 the largest hubs are limited by N, so small networks still lose their magnetization at large Δ.
- **Run time.** The default sweep takes about 10 seconds. Larger sizes and more samples take proportionally longer.

## References

1. S. H. Lee, H. Jeong, and J. D. Noh, "Random field Ising model on networks with inhomogeneous connections," *Phys. Rev. E* **74**, 031118 (2006). [doi:10.1103/PhysRevE.74.031118](https://doi.org/10.1103/PhysRevE.74.031118)
2. K.-I. Goh, B. Kahng, and D. Kim, "Universal behavior of load distribution in scale-free networks," *Phys. Rev. Lett.* **87**, 278701 (2001). [doi:10.1103/PhysRevLett.87.278701](https://doi.org/10.1103/PhysRevLett.87.278701)
