# A geometrically non-reduced regular surface with trivial canonical class in characteristic 5

This repository is a Lean 4 formalization, built on mathlib, of the existence of a
proper, integral, regular surface \(Y\) over
\(k=\mathbb F_5(s_1,s_2)\) (`FractionRing (MvPolynomial (Fin 2) (ZMod 5))`) with
\(H^0(Y,\mathcal O_Y)=k\) that is not geometrically reduced, and whose sheaf
\(\omega\), defined below from absolute differentials, is isomorphic to
\(\mathcal O_Y\). In the code this is Theorem 6.1. Projectivity is not formalized.

[`exists_theorem61_omega_rationalBase`](Red61/Theorem61.lean) uses the explicit sheaf
defined in [`Omega.lean`](Red61/Omega.lean):
\[
\omega=\mathcal Hom_{\mathcal O_Y}
\left(f^*\widetilde{\bigwedge^2\Omega_{k/\mathbb Z}},
\bigwedge^4\Omega_{Y/\mathbb Z}\right).
\]
Over the field of characteristic five, absolute differentials over the integers
identify canonically with differentials over the prime field.
The standard base volume identifies this sheaf with the fourth exterior differential
sheaf, and the invariant rational volume proves \(\omega\cong\mathcal O_Y\).
Its pullback has Euler degree zero on every proper integral curve.
The theorem also states that \(\Omega_{Y/\mathbb Z}\) is locally free of rank four
([`IsLocallyFreeOfRank`](Red61/AlgebraicGeometry/Modules.lean)): every point lies in the image of an
open immersion over which it is free on four generators.
The definition uses actual differential, exterior, pullback, and Hom sheaves;
triviality is proved separately in [`OmegaTrivialization.lean`](Red61/Canonical/OmegaTrivialization.lean).

Identification with the Serre dualizing sheaf is not formalized.

The active library contains 42 modules, exactly the import closure of
`Red61.Theorem61`, in four layers:

- `LinearAlgebra/`, `RingTheory/`, `Foliation.lean` and `AlgebraicGeometry/`: general
  mathematics, independent of the paper's base field (exterior powers, localization,
  regularity, Kähler differentials, p-closed derivations, sheaves of modules, differential
  and exterior-power sheaves, rational forms, principal atlases);
- `Surface/`: the construction of the surface and its properties;
- `Canonical/`: local differential bases, the invariant volume and the proof that
  \(\omega\cong\mathcal O_Y\);
- `Theorem61.lean`, `Omega.lean`, `CurveNumericalTriviality.lean`: the statement.

Each merged file lists the former modules it contains.

[`proper_regular_surface_rationalBase_properties`](Red61/Surface/Properties.lean)
proves, for the constructed surface `S`, properness, integrality, Noetherianity, dimension two,
regularity of every stalk, the prescribed global constants, and geometric
nonreducedness without geometric inputs.

The active proof has the following components:

| Component | Files |
|---|---|
| Base field, derivations, invariant field and constants | [Surface/Field](Red61/Surface/Field.lean), [Foliation](Red61/Foliation.lean), [Surface/SixChartModel](Red61/Surface/SixChartModel.lean) |
| Coordinate charts and quotient construction | [Surface/Charts](Red61/Surface/Charts.lean), [Surface/Quotient](Red61/Surface/Quotient.lean) |
| Properness, regularity, dimension, constants and geometric nonreducedness | [Surface/Projective](Red61/Surface/Projective.lean), [Surface/GeometricNonreduced](Red61/Surface/GeometricNonreduced.lean), [Surface/Properties](Red61/Surface/Properties.lean) |
| Absolute differential bases on affine neighborhoods and stalks | [Canonical/CINeighborhoods](Red61/Canonical/CINeighborhoods.lean), [Canonical/Volume](Red61/Canonical/Volume.lean) |
| Differential, exterior and framed Hom sheaves | [AlgebraicGeometry/Differentials/Basic](Red61/AlgebraicGeometry/Differentials/Basic.lean), [AlgebraicGeometry/ExteriorPower](Red61/AlgebraicGeometry/ExteriorPower.lean), [AlgebraicGeometry/Modules](Red61/AlgebraicGeometry/Modules.lean) |
| Explicit omega and its trivialization by the invariant volume | [Omega](Red61/Omega.lean), [Canonical/OmegaTrivialization](Red61/Canonical/OmegaTrivialization.lean), [Canonical/Volume](Red61/Canonical/Volume.lean) |
| Curve Euler degree and the main theorem | [CurveNumericalTriviality](Red61/CurveNumericalTriviality.lean), [Theorem61](Red61/Theorem61.lean) |

The environment uses Lean **4.34.1** and mathlib commit
`d13f23b723b8a846827a245b89c10fc7d3f11612`, pinned in the project files.
Run verification from this directory:

```bash
lake exe cache get   # download prebuilt mathlib; without it mathlib compiles from source
lake build
python3 verify.py
```

`verify.py` checks that every source module is imported by the default build,
rejects proof placeholders and prohibited declarations, builds the project,
and checks `#print axioms` for the main theorem and every owned project
declaration, including private auxiliaries. The allowed logical axioms are only
`Classical.choice`, `Quot.sound`, and `propext`. It also compares a fingerprint
of the main statement with the value pinned in `verify.py`. The fingerprint hashes,
by content, the elaborated types and definition bodies of all project constants the
statement depends on: proofs are ignored, and private, auxiliary and instance
constants are hashed by their own content, not their names, so moving declarations
between files does not change it. Any change to the statement or to those
definitions makes the check fail until the pinned value is updated on purpose.

The latest completed full verification is recorded in
[`verification/status.txt`](verification/status.txt), with the detailed
[`build log`](verification/build.log) and [`axiom log`](verification/axioms.log).
The audit itself is [Audit.lean](Audit.lean).

## License

Apache License 2.0; see [LICENSE](LICENSE). [`third_party/NOTICES.txt`](third_party/NOTICES.txt)
records the origin and license of adapted code.
