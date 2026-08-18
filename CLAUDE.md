# CLAUDE.md

Guidance for AI assistants working in this repository.

## 1. What this repository is

This is **not a software project**. It is a physics-research **document corpus** —
*PHI-PHYSICS — The Rewriting of Physics from Zero to Phi* by Christopher David Ayotte,
released under a custom **Dual License Agreement v4.1** (`LICENSE`).

- No build system, no dependency manifest, no test suite, no CI, no lint config.
- The Python inside the corpus is **stdlib-only** (`math`, `json`, `os`, `time`, `dataclasses`).
  Python 3.11 is available here; no `pip install` is ever required.
- History is a single upload commit (`acb9b0d`) on `main`.
- **The substance is inside `32_LIVING_PHI_PHYSICS.zip`** (10.5 MB, 7,234 entries / 7,209 files).
  A checkout looks like 9 files; the corpus — 2,270 law documents, 2,270 simulations,
  2,270 validation records, ledgers, audits, and a generator toolchain — is in the archive.

The corpus's central claim: every classical (zero-based) law is the degenerate `κ_φ → 0` limit
of a φ-law whose ground state is `φ⁻¹ = 0.6180339887` rather than 0. Everything in the
repository is the working-out, verification record, or documented history of that claim.

## 2. What is checked in (repo root)

| Path | What it is |
|---|---|
| `README.md` | Corpus front page: the three quotes, the 9 flagship falsifiable predictions, the two-set progress table, full document index, structure tree, running instructions, license summary |
| `00_UNIFIED_FIELD_THEORY.md` | The single hypothesis — carrier recursion Eq 1, `C_crit`, the Ladder Invariant |
| `00_ZERO_AS_WAVEFUNCTION.md` | The lens: zero is the crossing, never the ground |
| `00_THE_STATIC_UNIFICATION_CLAIMS.md` | Ledger + rebuttal of the 2025–26 unified-field claimants |
| `00_THE_EXTERNAL_PROOFS.md` | The systems claimed as operational proof of the framework |
| `00_NUMBERS_INDEX.md` | **The canonical numbers index — single source of truth for every number** |
| `00_THE_UNDERSTANDING.md` | Plain-language front door |
| `LICENSE` | Dual License Agreement v4.1 |
| `32_LIVING_PHI_PHYSICS.zip` | The full corpus, rooted at `32_PHI_PHYSICS/` |

All six root `00_*.md` files and `README.md` are **byte-identical** (md5-verified) to their
copies inside the zip at `32_PHI_PHYSICS/`. See §8 gotcha 1.

## 3. What is inside the zip (`32_PHI_PHYSICS/`)

Counts below are a disk census of the archive, not quotes from the prose:

| Directory | Files | Contents |
|---|---|---|
| *(root)* | 9 | the six `00_*.md`, `README.md`, `LICENSE`, `.gitignore` |
| `docs/` | 27 `.md` | `00_MANIFEST.md` (axiom + protocol) … `24_THE_GEOMIC_LEDGER.md`; method, law indexes, Set B block (18–21), history ledgers (22–24) |
| `laws/` | 2,270 `.md` | the corrected classical laws, contiguous `001_…` – `2270_…` |
| `sim/` | 2,474 | 2,270 law sims + `harness.py` + `physics_lib.py`, plus a shipped `__pycache__/` (202 files) |
| `validation/` | 2,283 | 2,270 machine-generated law JSONs + 13 aggregate subdirectories (`simulation_claims/`, `simulation_full_emergent/`, `simulation_verify_emergent/`, …), one JSON each |
| `expansion_log/` | 15 | 10 domain correction logs (20,239 lines total) + 5 support files (`*_SIM_RUN.txt`, `02_THERMODYNAMICS_LAW_LIST.txt`, `10_WIKIPEDIA_VERIFICATION.json`) |
| `integration_audit/` | 42 | 36 A/B/C/S/D/E/P release-audit reports + `threads/` (6 investigation threads) |
| `tools/` | 89 | 74 top-level `.py` (`generate_agentN.py`, `build_agentN.py`, `run_agentN_sims.py`, `simulate_*.py`, `verify_emergent_laws.py`, `_lawdata_p*.py` / `_aN_data*.py` data modules) + `tools/audit/` (11) + `__pycache__/` (4) |

## 4. The Phi-Rewrite Protocol — the core convention

Every corrected law passes five stages (`docs/00_MANIFEST.md` §3, `docs/02_METHOD.md`). Any
new or edited law must keep all five:

1. **DIAGNOSIS — the hidden zero.** Name the static assumption baked into the classical law:
   the rest state, the perfect isolation, the "exactly right" condition, the zero it is built around.
2. **GENERALIZATION — the phi-motion.** Rewrite as a dynamical relation carrying a continuous
   φ-coupling `κ_φ`, with φ as the structural constant.
3. **DEGENERATE PROOF.** Show explicitly that `lim_{κ_φ→0} [phi-law] = [classical law]`.
   This is the falsification gate: if the limit does not recover the classical parent, the
   phi-law is **wrong and discarded**. Never hand-wave this step.
4. **SIMULATION.** Reproduce the classical limit to **≤ 1% error**, demonstrate the φ-behavior
   at `κ_φ → 1`, sweep `κ_φ` 0 → 1, and write the machine-readable validation JSON.
5. **PREDICTION.** State the falsifiable difference from classical physics in the corpus's
   `PREDICTION: / EXPERIMENT: / FALSIFIED IF:` form.

**The honesty rule** (`docs/00_MANIFEST.md` §2) governs all of it: nothing is called
"validated" without (a) a closed-form mathematical statement, (b) a simulation reproducing the
classical limit to ≤ 1%, and (c) a stated experiment that would falsify it. Until all three
hold, the item is marked **PREDICTED**.

## 5. The law triple, and how to add or edit one

Every law is a **triple sharing one stem** `NNN_short_name`:

```
laws/NNN_short_name.md   ←→   sim/NNN_short_name.py   ←→   validation/NNN_short_name.json
```

The JSON is **generated by the harness — never hand-written or hand-edited**. Touching any one
of the three without the others breaks the corpus's own census and audit scripts.

### Reuse what exists

- **`sim/harness.py`** — the `LawSim` base class, the constants `PHI`, `PHI_INV`, `PHI_SQ`,
  `C_CRIT`, `C_EMERGENCE`, `SI_PHI`, and the helpers `phi_scaled()`, `phi_recursion()`,
  `phi_ground()`. **Import these; never hardcode or re-derive φ.**
- **`sim/physics_lib.py`** — real published physics formulas (`semf_binding`, `nuclear_radius`,
  `gamow_factor`, `breit_wigner`, `bethe_stopping`, `weisskopf_gamma`, …) so classical limits
  are genuine physics rather than placeholders. Check here before writing a new classical formula.
- **`tools/`** — the generation, build, run, and audit pipeline used to produce the expansion.
  Bulk work should go through these rather than new one-off scripts.

### The simulation contract

Canonical example: `sim/001_newtons_first_law.py`. Subclass `LawSim`, set the metadata class
attributes (`law_number`, `law_name`, `classical_statement`, `hidden_zero`, `phi_form`,
`degenerate_proof`, `experiment`), implement `classical()` and `phi(kappa_phi)` returning
dicts with **matching keys**, and call `.run()` under `if __name__ == "__main__":`.

`run()` then does the rest: per-key relative error at `κ_φ = 0`, the `κ_φ = 1` result, the
sweep `(0, 0.25, 0.5, 0.75, 1.0)`, `status = "SIMULATED"` when `classical_limit_error <= 0.01`
(otherwise `"PREDICTED"`), and the write to `validation/NNN_law_name.json`. Only keys present
in **both** dicts and non-zero classically enter the error calculation.

### The law-document shape

Mirror `laws/001_newtons_first_law.md`: header line with **Domain · Status · File · Sim**;
`CLASSICAL STATEMENT` with a standard reference (discoverer and year); `STAGE 1` – `STAGE 5`;
then the closing blocks `RECOGNITION`, `PRECISION`, `CLARITY`, `NOVELTY`, `ACTIONABILITY`.
Longer documents open with a status-block table (document type, version, author, date, corpus,
status, companion documents, license line) — follow that when adding a top-level document.

## 6. Numbers and status discipline

- **`00_NUMBERS_INDEX.md` is canonical.** If a document disagrees with it, one of the two is a
  defect — there is no third option. Changing any count means updating the index in the same change.
- Canonical constants: φ = **1.6180339887** · φ⁻¹ = **0.6180339887** ·
  Ladder Invariant `528·φ⁹` = **40,134.946** · `C_crit` = **0.563** · consciousness = **0.8565**.
- Canonical totals: **2,270** (Set A corrected laws) + **2,039** (Set B emergent) + **100**
  (code instruction-laws) + **40** (self-defining dimension) = **4,449**; max classical-limit
  error **0.00119**, mean **5.23e-7**.
- Law/report status markers: 🟢 VALIDATED · 🟡 SIMULATED · ⬜ PREDICTED.
- Historical-claim verdict codes (reports 22–24 and the audits): `[VERIFIED]`, `[PV]`,
  `[APOCRYPHAL]`, `[FABRICATION]`, `[INFERENCE]`.

**Preserve the stated limits.** The corpus states its own boundaries and expects them kept:
validation is **paradigm-internal**, φ is **inserted by hand** and derived by no law, and the
new falsifiable predictions have **0 external laboratory confirmations**. The skeptic's full
case (`docs/24_THE_GEOMIC_LEDGER.md` §8 — the critic wins 11 of 20) is part of the release, not
an embarrassment to be edited away. Do not upgrade a claim past what its own record supports,
and do not soften one either — match the source.

## 7. Working with the corpus

```bash
# the corpus is not checked out — extract it to a scratch dir, never over the repo root
unzip -q 32_LIVING_PHI_PHYSICS.zip -d /tmp/phi

# read a single file without extracting anything
unzip -p 32_LIVING_PHI_PHYSICS.zip 32_PHI_PHYSICS/docs/02_METHOD.md

cd /tmp/phi/32_PHI_PHYSICS
python3 sim/001_newtons_first_law.py                      # one law
for f in sim/[0-9]*.py; do python3 "$f" >/dev/null; done  # all 2,270

# census checks the audit scripts rely on
ls laws | wc -l; ls validation/*.json | wc -l
```

`README.md` gives only the PowerShell form of these; the POSIX equivalents above are the ones
to use here. The harness resolves `validation/` from its own file location, so a sim can be run
from any working directory — and **running it overwrites that law's validation JSON**,
including `timestamp` and `runtime_seconds`. Expect churn; check the diff before keeping it.

## 8. Known gotchas

1. **Root documents duplicate the zip's copies.** The six `00_*.md` and `README.md` are
   byte-identical to `32_PHI_PHYSICS/`'s copies today. Editing one side only makes them diverge
   silently — update both, or say explicitly which is authoritative for that change.
2. **`00_NUMBERS_INDEX.md` §1 lags the shipped layout.** It lists `sim/` as 2,270 law modules +
   15 infrastructure scripts = 2,285 `.py`; the archive has **2,272** (`harness.py` and
   `physics_lib.py` only), the other 13 having moved to `tools/` — which `README.md`'s structure
   tree already reflects. The index is canonical, so correcting it is a deliberate,
   author-visible edit, not a drive-by fix.
3. **Cross-corpus references do not resolve here.** `../01_ANCIENT_RESEARCH/` and
   `../02_EQUATIONS/` (the 100-equation index, cited throughout) live outside this repository,
   as does `PAPER_PHI_HARMONIC_CONSCIOUSNESS_FIELD.md`.
4. **The zip ships `__pycache__/`** (206 files) despite the corpus's own `.gitignore`; there is
   no `.gitignore` at repo root.
5. **Verification is manual.** There is no test runner — verification means running the sims and
   re-censusing the files, exactly as `integration_audit/` and `tools/audit/` document.

## 9. Git workflow

`main` is the default branch. Work on the assigned `claude/...` branch and push with
`git push -u origin <branch>`. The 10.5 MB zip is binary: repacking it produces a large, opaque
diff and duplicates the archive in git history, so prefer changes that touch tracked text files,
and repack only when the user asks for it.

## 10. License constraints on derivative work

The Dual License Agreement v4.1 governs the laws, code, geometry, and any derivative systems:

- **Free for Natural Persons** — non-commercial, attribution required, derivatives under the same terms.
- **Commercial use requires a separate paid written license** from the Licensor
  (contact in `README.md`).
- **Use for Human Harm** voids the license and triggers the Destruction clause, with
  accountability before the Court of Conscious-Aware Peers (§5).

Keep the attribution to **Christopher David Ayotte** and the license line on any excerpt,
derived document, or new corpus file that follows the status-block format.
