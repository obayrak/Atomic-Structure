# Atomic Structure

**Self-consistent atomic structure, spectra and periodic trends for every element from H to Og — computed from first principles in your browser.**

A single HTML file that solves the electronic structure of any atom or ion (density-functional theory, Hartree, or Hartree–Fock; optionally spin-polarised and scalar-relativistic) and builds on it: orbital energies and shapes, ionisation energies, emission and absorption spectra with line strengths and lifetimes, Zeeman and Stark patterns, photoionisation, hyperfine structure, X-ray lines and Moseley's law, and trends across the whole periodic table. Nothing is fitted to the measurements it is compared with.

No installation, no build step, no server: open the file and calculate.

---

An internet connection is used to load two libraries from CDNs:

| Library | Purpose | Without it |
|---|---|---|
| [Plotly.js 2.27](https://plotly.com/javascript/) | all plots | plots are replaced by a notice |
| [MathJax 3.2](https://www.mathjax.org/) | typeset equations in the Theory tab | equations fall back to plain HTML |

To run fully offline, download both libraries and change the two `<script src>` URLs in the `<head>` to local paths.

You can also host it as-is with GitHub Pages: rename it to `index.html` (or link to it) and enable Pages for the repository.

---

## Features

### Atom tab
- **Periodic table** — click any element; colour it by block, measured or calculated ionisation energy, ΔSCF error, size, orbital energy or total energy relative to Thomas–Fermi.
- **Methods** — LDA (Slater exchange + Perdew–Zunger correlation), Xα, exchange-only, Hartree, and **Hartree–Fock** (configuration average); spin-polarised LSDA and the scalar-relativistic (Koelling–Harmon) equation.
- **Comparison mode** — run a second method on the same atom and see both side by side (level schemes, radial densities, Z<sub>eff</sub>, energy tables).
- **Configuration editor** — set any occupation (e.g. Cr 3d⁵4s¹ vs 3d⁴4s²) and rank the configurations by total energy.
- **Ions** — positive and negative charge states; ΔSCF ionisation energy, Koopmans' energy, ground term from Hund's rules.
- **Plots** — orbital level scheme, radial probabilities, effective nuclear charge with Slater's rules, orbital shapes (2D slices and 3D isosurfaces), successive ionisation energies, isoelectronic series.

### Spectrum tab
- **Hydrogen-like ions** — exact Dirac levels with reduced mass, Lamb shift (Bethe logarithms), hyperfine structure (21 cm line) and isotopes ¹H, ²H, ³H.
- **Any selected atom** — one-electron excitations in the self-consistent potential (Latter tail, core polarisation), or **frozen-core Hartree–Fock**; spin–orbit splitting.
- **Grotrian diagram, emission/absorption spectra** — line strengths, oscillator strengths, Einstein A coefficients, radiative lifetimes and branching ratios; equal or Boltzmann populations; vacuum or standard-air wavelengths.
- **Line profiles** — fine structure + Zeeman + Stark diagonalised together (anomalous Zeeman, Paschen–Back, linear and quadratic Stark), Voigt broadening from Doppler and natural widths.
- **Photoionisation cross-sections** from energy-normalised continuum functions, incl. the Cooper minimum.
- **Saha–Boltzmann ionisation balance** and the temperature of peak line strength (why A stars show the strongest Balmer lines).
- **Quantum defects** and Rydberg–Ritz fits; **alkali hyperfine** splittings from the contact density.

### All elements tab
- Every element H–Og with any method, run in **parallel Web Workers** (one per logical core by default, adjustable up to 64).
- Orbital energies across the table, first ionisation energies vs measurement, error statistics, atomic radii vs Slater's empirical radii, total energy vs the Thomas–Fermi law with Scott and Schwinger corrections.
- **X-ray emission lines and Moseley's law** — Kα₁, Kα₂, Kβ₁, Lα₁ and K edges from core-hole ΔSCF for Z = 3–100, compared with the X-ray Data Booklet.

### Theory tab
A complete, equation-level description of every method, numerical scheme, parameter and data source, with a validation table and the known limitations.

---

## Accuracy (selected checks)

| Quantity | Calculated | Reference |
|---|---|---|
| Hartree–Fock total energies He, Ne, Ar, Kr | −2.861680, −128.547098, −526.817515, −2752.054996 E<sub>h</sub> | Clementi–Roetti, agree to ≤ 2 × 10⁻⁵ |
| Ne total energy, LDA | −128.2269 E<sub>h</sub> | −128.2335 (NIST) |
| First ionisation energies, mean abs. error (103 elements) | 0.58 eV (LDA), 0.45 eV (spin-polarised) | NIST |
| Au ionisation energy, non-rel. / scalar-rel. | 7.56 / 9.72 eV | 9.23 eV |
| H 2p, Na 3p lifetimes | 1.60, 15.9 ns | 1.596, 16.2 ns |
| H n = 2 fine structure / Lamb shift | 10 969.1 / 1050.5 MHz | 10 969.0 / 1057.8 MHz |
| H ground-state hyperfine | 1420.49 MHz | 1420.41 MHz |
| H 1s photoionisation at threshold | 6.31 Mb | 6.30 Mb (exact) |
| X-ray Kα₁, mean abs. error, scalar-rel. | 0.49 % | X-ray Data Booklet |
| Alkali hyperfine (Rb, Cs), scalar-rel. | 1.05, 1.04 × measured | measured |

The full table is in the Theory tab.

---

## How it works (in brief)

- Spherical, configuration-averaged atom; radial equation on a logarithmic grid with the **Numerov** method, node-counting bisection and outward/inward matching.
- Self-consistency with **Anderson mixing**; ΔSCF energy differences on matched grids.
- **Scalar-relativistic** equation rewritten without first-derivative term so Numerov still applies.
- **Hartree–Fock**: exchange as an inhomogeneous term, solved with the Green's function of the radial equation; orbital energies from the normalisation condition; off-diagonal Lagrange multipliers for open shells.
- Fields: full diagonalisation in |nℓm<sub>ℓ</sub>m<sub>s</sub>⟩ with Racah-formula 3j/6j symbols.
- All in plain JavaScript, HTML and CSS in one ~2700-line file, with no dependencies beyond Plotly and MathJax.

---

## Browser notes

- Heavy calculations (all elements, X-ray lines, successive ionisation energies) run in Web Workers created from Blob URLs. If a browser or embedding context blocks them, the code falls back to the main thread (slower, but works).
- More workers than physical cores give no extra speed. Some browsers under-report cores for privacy; set the worker count manually in the All elements tab.
- Night/Day theme is stored in `localStorage` (`siteTheme`); the worker setting as `atomWorkers`.
- The file references an optional `../../theme.js` from the site it was written for; if absent the browser logs a 404 and nothing else changes. You can delete that `<script>` line.

---

## Limitations

- Spherical, configuration-averaged atoms: multiplet (term) splittings are not calculated.
- LDA self-interaction error: −ε<sub>HOMO</sub> too small, most anions unbound (Hartree–Fock fixes both but lacks correlation).
- Relativity is scalar; spin–orbit coupling is first order; point nucleus.
- Hartree–Fock is non-relativistic and spin-averaged.
- Excited states are one-electron excitations with a frozen core — accurate for one-valence-electron atoms.

---

## Data sources

- First ionisation energies: NIST Atomic Spectra Database.
- X-ray emission energies: A. C. Thompson et al., *X-Ray Data Booklet*, LBNL, Table 1-2.
- Empirical atomic radii: J. C. Slater, J. Chem. Phys. 41, 3199 (1964).
- Reference Hartree–Fock energies: E. Clementi and C. Roetti, At. Data Nucl. Data Tables 14, 177 (1974).
- LDA reference energies: NIST Atomic Reference Data for Electronic Structure Calculations.
- Bethe logarithms: G. W. F. Drake and R. A. Swainson, Phys. Rev. A 41, 1243 (1990).

Full references are listed in the Theory tab.

---

## Contributing

Issues and pull requests are welcome. Useful additions include multiplet terms, polarisabilities and C₆ coefficients, form factors, and caching results in browser storage. Please keep the project a single self-contained HTML file and add a validation check for any new physics.

---

## License

[MIT](LICENSE) © 2026 [Orhan Bayrak]
