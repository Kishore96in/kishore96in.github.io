+++
title = "A note on various implementations of anisotropic forcing in Pencil"
date = "2025-03-23T20:11:14+05:30"
url = "blog/anisotropy-pencil-code"
tags = [
	"Pencil code",
	]

[params]
	macros = [
		["\\uvec", "\\hat{#1}"],
		["\\defn", "\\equiv"],
		]
	bib = "global_refs.json"
+++

In the context of the Pencil code, there are three distinct notions of anisotropic forcing:

<!--more-->

1.
	Changing the values of `ex`, `ey`, and `ez` in `samples/helical-MHDturb/python/generate_kvectors.py` makes the wavenumbers of the forcing different in different directions.
	This seems useful if one wants the flow to have different spatial scales in different directions.

1.
	In `forcing_run_pars`, one can set `force_strength` to a nonzero value and additionally set `force_direction` (a 3-vector).
	This is the forcing described by {{< cite t kapyla2019Lambda >}} and {{< cite t Kap19 >}}, where an anisotropic component is added to the usual isotropic random curl-eigenfunction forcing.
	I have checked that the velocity field remains nonhelical on average when `relhel=0`.
	Note that the modified forcing is not solenoidal.
	To keep the amplitude of the forcing the same while changing the degree of anisotropy, one should keep $f_0 \left[ 1 + f_1/ \left( 9f_0 \right)\right]$ constant.
	This condition results from requiring the trace of the angular average of the $f_{ij}$ tensor[^note_defn_fij] to be unchanged.

1.
	Setting `laniso_forcing_old=F` in `forcing_run_pars` (in addition to the other options mentioned in the previous point) scales the amplitude of the forcing depending on the *direction* of $\vec{k}$.
	Here, the forcing is still in terms of eigenfunctions of the curl operator.

[^note_defn_fij]: Previous studies ({{< cite alp Kap19 >}}, eq. 9; {{< cite alp kapyla2019Lambda >}}, eq. 22) seem to have a typo in their stated definition of $f_{ij}$; the correct definition is $f_{ij} \defn f_0 \delta_{ij} + f_1 \delta_{i3} \delta_{j3} ( \uvec{k}\cdot\uvec{z} )^2$.

Note that none of the described options seems to completely capture the sense in which convection is anisotropic.
