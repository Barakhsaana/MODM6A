# Accurate m6A-U Force Field for RNA Conformational Sampling

## Abstract  
N6-methyladenosine is a widely studied RNA modification that is ubiquitously present in eukaryotic organisms. Abnormal levels of m6A modifications are closely associated with the regulation of RNA metabolism, heat shock responses, as well as the initiation and progression of cancers. However, due to inaccuracies in molecular mechanics and a lack of relevant experimental data, the force field accuracy for RNA systems containing m6A modifications remains insufficient. As a result, studying the atomic-level details of m6A-modified RNA via molecular dynamics simulations continues to be a significant challenge. Through testing representative systems containing m6A using both quantum mechanics and molecular mechanics, we identified clear deficiencies in the existing force fields, particularly in the pairing of m6A with uracil. To address this issue, we developed a new set of force field parameters named MODM6A, using a deep learning approach that incorporates the energy differences between quantum mechanics and molecular mechanics calculations. These parameters were integrated into the Amber force field and validated through simulations, showing that the revised parameters significantly enhance the stability of the m6A–uracil pairing and align better with experimental results.

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


