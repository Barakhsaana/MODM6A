# Accurate m6A Modification Force Field for Conformation Sampling of RNA

## Abstract  
N⁶-methyladenosine (m⁶A) is a prevalent RNA modification that regulates RNA structure, metabolism, and molecular recognition. Atomistic interpretation of these effects requires force fields that describe the context-dependent balance between paired and unpaired m⁶A. Here, we developed MODM6A, a localized m⁶A–U interaction correction derived from rigid interaction-energy scans in which both the quantum-mechanical (QM) and molecular-mechanical (MM) reference energies included implicit aqueous solvation. The implicit-water QM profile had a minimum of −9.58 kcal mol−1 at 0.190 nm, whereas the reference AMBER model gave −4.29 kcal mol−1 at 0.195 nm; MODM6A reproduced the target with a minimum of −9.84 kcal mol−1 at 0.185 nm. In 3-µs explicit-solvent simulations, MODM6A increased the mean U10–m⁶A22 occupancy in MALAT1 from 50.5 ± 12.0% to 81.8 ± 2.5% and the m⁶A6–U17 occupancy in a DRACH-containing hairpin from 71.3 ± 37.3% to 94.4 ± 2.4% (mean ± SD across three replicates). A matched analysis of the first 1 µs of each trajectory gave target-pair occupancies of 58.4 ± 9.2%, 77.1 ± 8.1%, and 84.4 ± 8.4% in MALAT1 and 82.2 ± 18.4%, 94.1 ± 1.9%, and 94.5 ± 7.2% in the DRACH hairpin for the reference AMBER model, MODM6A, and the Piomponi fit_A parameters, respectively. MODM6A thus achieved pairing behavior comparable to fit_A through a more localized modification that leaves the modrna08 charges and torsional terms unchanged. Global RMSD, radius of gyration, clustering, and tICA nevertheless retained substantial heterogeneity, so the improvement is interpreted as better agreement with experimentally supported local structural preferences rather than conformational locking.

---

## Instructions for Using MODM6A  

### Prerequisites  
- **Software**: AMBER 
- **Input Files**: Processed PDB file with atom numbering, MD input files (e.g., `md.in`)  

---

### Step 1: Prepare the DISANG Restraint File  
Create a `ghbfix` file (e.g., `ghbfix-m6a`) to define energy restraints. The format follows AMBER's `DISANG` syntax:  

```fortran
&rst  
  iat = atom_1, atom_2,  
  iresid = 0, nstep1 = 0, nstep2 = 0,  
  irstyp = 0, ifvari = 0, ninc = 0, imult = 0, ir6 = 0, ifntyp = 0,  
  r1 = [value], r2 = [value], r3 = [value], r4 = [value],  
  rk2 = [force_constant], rk3 = [force_constant],  
/  
&rst  
  iat = atom_3, atom_4,  
  (additional parameters as above)  
/  
```  
**Key Notes**:  
- `atom_1`, `atom_2`: Atom indices from your PDB file.  
- `[]` values: Replace with parameters from `E1`/`E2` (adjust units if needed; see [ghbfix-m6a](https://github.com/Barakhsaana/MODM6A/blob/main/ghbfix-m6a)).  

---

### Step 2: Integrate with Force Field  
In your MD input file (e.g., `md.in`), add the `DISANG` directive:  
```bash
DISANG = ghbfix-name 
```  

---

### Step 3: Run Molecular Dynamics  
Execute simulations as usual. 
**Validation Tips**:  
- First it can be validated by cheking `.out` file.
In `.out` file, for example, `name.md.out`, you can see following texts:
```bash
 RESTRAINTS:
 Requested file redirections:
  DISANG    = ghbfix-name
 Restraints will be read from file: ghbfix-name
Here are comments from the DISANG input file:

                       Number of restraints read =     2

                  Done reading weight changes/NMR restraints


- Monitor RMSD and hydrogen bonding for m6A-U pairs.  
```
- It can also be validated by MD results. 


