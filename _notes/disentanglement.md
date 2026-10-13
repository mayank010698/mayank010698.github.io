---
layout: page
title: Disentanglement of Learned Representations
date: 2025-10-16
tags: [Controlled-Generation, debugging]
mathjax: true
---

### What does it mean to learn a disentangled representation, and why might Gromov--Wasserstein geometry help?

Suppose the observed data are generated from underlying factors

$$
s = (s_1,\ldots,s_k),
$$

through some unknown generative map

$$
x = g(s).
$$

Informally, a representation

$$
z = E_\phi(x)
$$

is **disentangled** when individual coordinates of $z$ correspond to individual underlying factors of variation. Ideally, up to permutation and coordinate-wise transformations,

$$
z_i \approx h_i(s_{\pi(i)}).
$$

This is stronger than simply requiring the coordinates of $z$ to be statistically independent. Independence is a useful inductive bias, and is commonly encouraged by matching the latent distribution to a factorized prior, but an independent representation need not necessarily correspond to the true generative factors.

For example, a $\beta$-VAE uses the objective

$$
\mathcal{L}_{\beta\text{-VAE}}
=
\mathbb{E}_{x\sim p_{\mathrm{data}}}
\left[
\mathbb{E}_{z\sim q_\phi(z\mid x)}
[-\log p_\theta(x\mid z)]
+
\beta
D_{\mathrm{KL}}
\left(
q_\phi(z\mid x)
\Vert
p(z)
\right)
\right],
$$

where the prior is typically factorized,

$$
p(z) = \mathcal{N}(0,I).
$$

The prior therefore introduces an **independence bias**, while reconstruction forces the representation to retain information about the data.

The question I want to investigate is whether an additional **geometric bias** can improve disentanglement.

---

## Geometric view of the representation

Consider an encoder

$$
T = E_\phi : \mathcal{X} \rightarrow \mathcal{Z}.
$$

For two datapoints $x,x'$, their representations are

$$
z = T(x),
\qquad
z' = T(x').
$$

A perfectly distance-preserving representation would satisfy

$$
\|x-x'\|
\approx
\|T(x)-T(x')\|.
$$

More generally, rather than directly comparing points across spaces of different dimensions, we compare their **intra-space geometry**:

$$
c_{\mathcal X}(x,x')
\approx
c_{\mathcal Z}(T(x),T(x')).
$$

Possible choices include squared Euclidean distances,

$$
c_{\mathcal X}(x,x')
=
\|x-x'\|_2^2,
\qquad
c_{\mathcal Z}(z,z')
=
\alpha\|z-z'\|_2^2,
$$

or cosine similarities,

$$
c_{\mathcal X}(x,x')
=
\operatorname{cos}(x,x'),
\qquad
c_{\mathcal Z}(z,z')
=
\operatorname{cos}(z,z').
$$

This is the basic geometric motivation used by recent work on Gromov--Wasserstein-based disentanglement. In particular, the Gromov--Monge Gap paper argues for combining prior matching with preservation of geometric structure. Importantly, however, it does **not** require exact isometry: in general, a map that simultaneously matches a prescribed latent distribution and perfectly preserves all pairwise distances may not exist. The objective should therefore encourage the *least geometrically distorted map among maps satisfying the desired marginal constraint*. :contentReference[oaicite:0]{index=0}

---

## Hypothesis

My working hypothesis is:

> **Prior matching provides an independence bias, while Gromov--Wasserstein-style geometric regularization selects, among the representations compatible with that prior, representations that better preserve the intrinsic structure of the data. Under appropriate assumptions on the generative factors, this additional geometric constraint may improve identifiability and therefore disentanglement.**

This is deliberately weaker than the claim

> "If the encoder learns a GW map, then the representation is disentangled."

The latter is generally not true without additional assumptions.

GW geometry by itself does not specify which latent axes should correspond to which factors. For example, rotations of an isotropic Gaussian latent space preserve Euclidean distances but can mix all coordinates. Therefore,

$$
\text{geometry preservation}
\not\Rightarrow
\text{disentanglement}
$$

in general.

The expected mechanism is instead something like

$$
\boxed{
\text{factorized prior}
+
\text{information preservation}
+
\text{geometric constraint}
\Longrightarrow
\text{better inductive bias toward disentanglement}.
}
$$

This distinction is also visible in the GWAE paper. Its disentanglement experiment does not obtain disentanglement from the GW objective alone; it explicitly introduces a **factorized neural prior** as the disentanglement meta-prior and then uses the GW objective to align the geometry of the data and latent spaces. :contentReference[oaicite:1]{index=1}

The theoretical motivation from geometry-based disentanglement results is also conditional: under assumptions such as sufficiently non-Gaussian factors and local isometry, geometric preservation can make recovery of the underlying factors identifiable. The geometry is therefore an additional structural assumption, rather than an unconditional theorem that a GW map must be disentangled. :contentReference[oaicite:2]{index=2}

---

# Proposed approach

I want to start from a pretrained $\beta$-VAE and investigate whether its latent representation can be moved toward one with better geometric alignment.

Let

$$
z_i = E_\phi(x_i)
$$

for a minibatch

$$
X = \{x_1,\ldots,x_n\},
\qquad
Z = \{z_1,\ldots,z_n\}.
$$

Define their pairwise cost matrices

$$
C_X(i,j)
=
c_{\mathcal X}(x_i,x_j),
$$

and

$$
C_Z(i,j)
=
c_{\mathcal Z}(z_i,z_j).
$$

I then construct a geometry-improved latent batch

$$
\widetilde Z
=
\{\widetilde z_1,\ldots,\widetilde z_n\}
$$

by running an inner optimization or flow that attempts to improve the correspondence between the geometry of $X$ and the geometry of $\widetilde Z$.

Abstractly,

$$
\widetilde Z
=
\mathcal{F}(Z; C_X),
$$

where $\mathcal{F}$ denotes the inner MMD/GW-inspired flow.

For example, the inner problem can be thought of as approximately solving

$$
\widetilde Z
\approx
\arg\min_{Z'}
\mathcal{R}_{\mathrm{geom}}(X,Z')
+
\gamma
\|Z'-Z\|^2,
$$

where

$$
\mathcal{R}_{\mathrm{geom}}(X,Z')
$$

measures the discrepancy between the pairwise statistics of

$$
C_X
$$

and

$$
C_{Z'}.
$$

The proximal term

$$
\gamma\|Z'-Z\|^2
$$

is important because otherwise the inner problem is free to construct an entirely new representation unrelated to the encoder output.

After the inner optimization, I use

$$
\operatorname{stopgrad}(\widetilde Z)
$$

as a target for updating the autoencoder.

---

## A possible alternating procedure

### Stage 1: Pretrain a standard $\beta$-VAE

Train

$$
E_\phi,
\qquad
D_\theta
$$

using

$$
\mathcal{L}_{\mathrm{VAE}}
=
\mathcal{L}_{\mathrm{recon}}
+
\beta\mathcal{L}_{\mathrm{KL}}.
$$

This gives an initial representation with a factorized-prior bias.

---

### Stage 2: Compute geometry-improved latent targets

For a minibatch $X$:

$$
Z = E_\phi(X).
$$

Keeping $Z$ as initialization, solve an inner optimization problem

$$
\widetilde Z
=
\mathcal{F}(Z;X).
$$

For example,

$$
\widetilde Z
\approx
\arg\min_{Z'}
\left[
\operatorname{MMD}
\left(
\operatorname{vec}(C_X),
\operatorname{vec}(C_{Z'})
\right)
+
\gamma
\|Z'-Z\|_F^2
\right].
$$

Then detach the result:

$$
\widetilde Z_{\mathrm{target}}
=
\operatorname{stopgrad}(\widetilde Z).
$$

---

### Stage 3: Distill the geometric target back into the encoder

This is the step that is currently missing from my original formulation.

If I only decode $\widetilde Z$ and optimize

$$
\|D_\theta(\widetilde Z)-X\|^2,
$$

then I am primarily training the **decoder**. There is no reason for the encoder itself to eventually output $\widetilde Z$.

To actually learn an encoder corresponding to the geometry-improved representation, I need an explicit encoder update such as

$$
\mathcal{L}_{\mathrm{target}}
=
\left\|
E_\phi(X)
-
\operatorname{stopgrad}(\widetilde Z)
\right\|_F^2.
$$

The encoder update could therefore use

$$
\mathcal{L}_{E}
=
\mathcal{L}_{\mathrm{target}}
+
\beta
\mathcal{L}_{\mathrm{KL}},
$$

or, more conservatively,

$$
\mathcal{L}_{E}
=
\mathcal{L}_{\mathrm{recon}}
+
\beta
\mathcal{L}_{\mathrm{KL}}
+
\lambda
\mathcal{L}_{\mathrm{target}}.
$$

The decoder can simultaneously be trained using

$$
\mathcal{L}_{D}
=
\mathcal{L}_{\mathrm{recon}}
\left(
X,
D_\theta(\widetilde Z)
\right).
$$

The resulting procedure is therefore approximately

$$
Z
=
E_\phi(X)
$$

$$
\Downarrow
$$

$$
\widetilde Z
=
\operatorname{GeometryFlow}(Z,X)
$$

$$
\Downarrow
$$

$$
E_\phi(X)
\leftarrow
\widetilde Z
$$

while requiring

$$
D_\theta(\widetilde Z)
\approx X.
$$

This resembles an alternating **projection and distillation** procedure: the inner optimization produces a desirable latent configuration, and the encoder is subsequently trained to amortize this optimization.

---

# Does this actually learn a Gromov--Wasserstein map?

Not necessarily.

My current MMD-distance-statistics objective does not directly solve the Gromov--Wasserstein problem. Matching distributions of pairwise distances,

$$
\operatorname{MMD}
\left(
\{d_X(x_i,x_j)\}_{i,j},
\{d_Z(z_i,z_j)\}_{i,j}
\right),
$$

only says that the two spaces have similar **distance statistics**.

This is weaker than requiring corresponding pairs to preserve their distances:

$$
d_X(x_i,x_j)
\approx
d_Z(z_i,z_j).
$$

It is also weaker than GW, which optimizes over a coupling and asks whether the complete relational structures can be aligned.

For example, two very different point clouds can have approximately the same histogram of pairwise distances.

Therefore, at the moment, it would be more accurate to say:

> I am learning a representation whose **distribution of relational statistics** resembles that of the data.

rather than

> I am learning the Gromov--Wasserstein map.

The first experimental question should therefore be whether this weaker criterion is sufficient to improve disentanglement.

---

# A more direct formulation

A cleaner version of the hypothesis would avoid the inner flow entirely and directly regularize the encoder:

$$
\min_{\phi,\theta}
\mathcal{L}_{\mathrm{recon}}
+
\beta\mathcal{L}_{\mathrm{KL}}
+
\lambda
\mathcal{R}_{\mathrm{geom}}(E_\phi).
$$

For example, the simple distortion loss is

$$
\mathcal{R}_{\mathrm{DST}}
=
\frac{1}{n^2}
\sum_{i,j}
\left(
c_{\mathcal X}(x_i,x_j)
-
c_{\mathcal Z}
\left(
E_\phi(x_i),
E_\phi(x_j)
\right)
\right)^2.
$$

However, exact distortion minimization can conflict with prior matching. This is precisely the motivation for the Gromov--Monge Gap: rather than demanding zero distortion, it asks whether the learned map achieves the **smallest possible distortion given its transported distribution**,

$$
\operatorname{GMG}_r(T)
=
\operatorname{DST}_r(T)
-
\operatorname{GW}
\left(
r,T_\#r
\right).
$$

The GMG paper therefore proposes the much cleaner joint objective

$$
\mathcal{L}
=
\mathcal{L}_{\beta\text{-VAE}}
+
\lambda
\operatorname{GMG}(E_\phi),
$$

or alternatively regularizes the decoder. Their experiments actually find decoder regularization, particularly with cosine geometry, to work better than encoder regularization. :contentReference[oaicite:3]{index=3}

---

# My current interpretation of the experiment

The goal of the MMD-flow experiment should therefore initially be modest.

I am **not yet testing the theorem**

$$
\text{GW map}
\Rightarrow
\text{disentanglement}.
$$

Instead, I am testing the empirical hypothesis

$$
\boxed{
\text{factorized-prior VAE}
+
\text{better preservation of relational geometry}
\quad
\text{improves disentanglement}.
}
$$

The sequence of questions should be:

1. Does the regularizer actually change the geometry of the latent representation in the intended direction?

2. Does it preserve enough information to maintain reconstruction quality?

3. Does the resulting representation continue to approximately satisfy the factorized-prior constraint?

4. Does improved geometry correlate with improved disentanglement metrics such as MIG and DCI-D?

5. Is matching only the **distribution of distances** sufficient, or do we need a stronger pairwise/GW/GMG objective?

6. Does regularizing the encoder work better or worse than regularizing the decoder?

The last question is particularly important because the existing GMG results suggest that decoder regularization may provide a stronger learning signal than directly constraining the encoder. :contentReference[oaicite:4]{index=4}

---

# Current working claim

A claim that I think is defensible at this stage is:

> Standard disentanglement methods impose distributional structure on the latent representation, typically through matching to a factorized prior, but place relatively weak constraints on the geometry of individual representations within this distribution. We investigate whether additionally selecting representations that preserve the relational geometry of the data improves disentanglement. We first approximately optimize latent samples toward geometry-preserving configurations and distill these configurations back into the encoder. This provides a computational approach for studying whether geometry preservation itself is responsible for the disentanglement gains observed in Gromov--Wasserstein-based representation learning methods.

The strongest conceptual issue in the current method is therefore **not** the use of the MMD flow. It is that the original training procedure did not explicitly make the encoder imitate the flowed points. Without

$$
E_\phi(x)
\approx
\operatorname{stopgrad}(\widetilde z),
$$

the final encoder has no reason to become the map found by the inner optimization.

The second issue is theoretical: **GW or isometry alone does not imply disentanglement**. The more plausible story is that geometry preservation complements a factorized prior and other VAE inductive biases.