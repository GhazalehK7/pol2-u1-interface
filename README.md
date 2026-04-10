# Structural conservation at the direct Pol II-U1 interface in 7B0Y

## Project title
**Structural conservation and disease-variant mapping framework at the direct Pol II-U1 snRNP interface in 7B0Y: RPB2 + RPB12 ↔ U1-70K**

## Project goal
This project analyzes the **direct protein-protein interface** between RNA Polymerase II and U1 snRNP in the cryo-EM structure **7B0Y** and asks:

1. Which residues form the direct **Pol II-U1-70K** contact surface?
2. How do these residues map from **PDB numbering** to **UniProt numbering** and then to **human residue coordinates**?
3. Are the interface residues on **U1-70K / SNRNP70** evolutionarily conserved across vertebrates?
4. **Planned next step:** do disease- and cancer-associated variants cluster near this interface?

This repository is being developed as a computational structural biology portfolio project motivated by interest in **co-transcriptional splicing**, **cryo-EM**, and **mechanistic analysis of Pol II-spliceosome coupling**.

---

## Why this project matters
The Zhang Lab's research program explicitly connects cryo-EM structure determination with mechanistic analysis of co-transcriptional splicing and cancer-relevant biology. The distinctive goal of this project is therefore not only to define the **direct Pol II-U1 interface**, but also to build the foundation for asking whether **human disease or cancer variants** concentrate near that interface.

At the current stopping point, the project has completed the **structural interface definition**, **PDB→UniProt→human residue mapping**, and a **local vertebrate conservation analysis for SNRNP70**. The variant-mapping component remains a planned next stage.

---

## Biological background
The structure **7B0Y** corresponds to a **transcribing RNA polymerase II-U1 snRNP complex**. The key mechanistic interface of interest is the **direct contact between U1-70K and Pol II**, not the entire assembly.

The published structural interpretation indicates that:
- **U1-70K / SNRNP70** contacts the **protrusion domain of RPB2 / POLR2B**
- **U1-70K / SNRNP70** also contacts the **zinc-ribbon region of RPB12 / POLR2K**
- the key interface includes **U1-70K residues around 121-156**, including **Arg121, Glu124, Val125, and Asp156**

This repository rebuilds that interface definition from the structure itself and then maps it into a human biological interpretation.

---

## Important structural facts established before analysis

### PDB entry
- **7B0Y** - *Structure of a transcribing RNA polymerase II-U1 snRNP complex*

### Verified chain assignments
- **Chain A** = RPB1 / POLR2A - *Sus scrofa*
- **Chain B** = RPB2 / POLR2B - *Sus scrofa*
- **Chain L** = RPABC4 / POLR2K (RPB12) - *Sus scrofa*
- **Chain b** = SNRNP70 / U1-70K - *Homo sapiens*
- **Chain c** = SNRPA / U1-A - *Homo sapiens*
- **Chain P** = nascent transcript RNA
- **Chain a** = U1 snRNA

### Critical species note
This is a **hybrid complex**:
- Pol II chains are deposited from **pig**
- U1-70K is deposited from **human**

This matters because:
- **SIFTS maps chain B and chain L to pig UniProt accessions**
- chain **b / U1-70K maps directly to human UniProt P08621**
- any human interpretation of B/L requires an explicit **pig → human ortholog bridge**

### Direct interface definition used in this project
The project focuses specifically on:
- **Pol II side:** chain **B** (RPB2) + chain **L** (RPB12)
- **U1 side:** chain **b** (U1-70K / SNRNP70)

### Sequence caveat for chain b
The coordinate model contains only about **186 observed residues** for chain **b**, while canonical human **SNRNP70** is **437 aa** long. Therefore:
- PDB-extracted chain sequence is **not** appropriate for full-length conservation analysis
- full canonical sequences were retrieved from **UniProt**

---

## What was completed

## Step 1 - Direct interface extraction from 7B0Y

### Inputs
- `data_raw/7B0Y.cif`
- `data_raw/7b0y_sifts.xml.gz`

### Why mmCIF was used
mmCIF preserves chain identifiers more robustly than legacy PDB format, which is essential here because the structure contains both uppercase and lowercase chain IDs. Correctly distinguishing **chain `B`** from **chain `b`** is biologically critical.

### Contact extraction
A residue-residue contact table was generated using a **4.5 Å heavy-atom minimum-distance cutoff** between:
- **POL2_CHAINS = ['B', 'L']**
- **U1_CHAINS = ['b']**

Non-protein residues and waters were excluded.

### Main output
- `data_processed/pol2_u1_interface_contacts.csv`

### Summary of extracted interface
- **29 unique residue pairs**
- **12 unique Pol II chain B residues**
- **6 unique Pol II chain L residues**
- **15 unique U1-70K chain b residues**

### Key residues recovered

#### Chain B (RPB2 / POLR2B)
`92, 95, 97, 103, 105, 106, 107, 109, 145, 146, 173, 174`

#### Chain L (RPB12 / POLR2K)
`21, 36, 37, 38, 39, 40`

#### Chain b (U1-70K / SNRNP70)
`115, 118, 121, 122, 124, 125, 126, 153, 155, 156, 163, 167, 168, 170, 171`

### Sanity check
This interface is biologically plausible because:
- chain **L** contacts cluster in the expected **RPB12 zinc-ribbon** region
- chain **b** includes the literature-supported hotspot around **Arg121 / Glu124 / Val125 / Asp156**
- only chains `B`, `L`, and `b` were included, excluding RNA-mediated contamination

---

## Step 2 - Residue mapping with SIFTS and pig-to-human bridging

### Why SIFTS
Residue numbering in PDB coordinates does not automatically equal UniProt numbering. For this project, **SIFTS** was used as the primary residue-level mapping source.

### Important chain-ID issue discovered and solved
The author chain IDs in mmCIF and the internal chain/entity IDs in SIFTS are not always the same.

The critical mapping was:
- author chain **B** → internal entity **B**
- author chain **L** → internal entity **L**
- author chain **b** → internal entity **Q**

### SIFTS UniProt mappings recovered
- **Entity B** → `A0A0B8RVL1` (pig POLR2B)
- **Entity L** → `A0A4X1TRS6` (pig POLR2K / RPABC4)
- **Entity Q** → `P08621` (human SNRNP70)

### Ortholog bridge to human
Canonical UniProt sequences were retrieved for:
- pig **POLR2B**
- pig **POLR2K**
- human **POLR2B** (`P30876`)
- human **POLR2K** (`P53803`)
- human **SNRNP70** (`P08621`)

Pairwise comparison showed:

#### POLR2B
- pig length = **1174 aa**
- human length = **1174 aa**
- **99.91% identity**
- **0 gaps**

#### POLR2K
- pig length = **58 aa**
- human length = **58 aa**
- **100% identity**
- **0 gaps**

### Interpretation
Because the pig→human alignments contained **no gaps**, the numbering bridge is effectively **1:1** for interface residues:
- pig residue N ↔ human residue N

### Main output
- `data_processed/interface_residue_human_mapping.csv`

### Final counts
- **POLR2B**: 12 interface residues
- **POLR2K**: 6 interface residues
- **SNRNP70**: 15 interface residues

---

## Step 3 - Local conservation analysis for SNRNP70

### Why a local pipeline was used
The original plan was to use the **ConSurf** server, but the server was unavailable during this work. To maintain progress and reproducibility, a local MSA-based conservation workflow was used instead.

### Important methodological note
This is **not identical to ConSurf**. It is a **local vertebrate multiple-sequence-alignment conservation analysis**, using:
- UniProt homolog retrieval
- MAFFT alignment
- column-wise occupancy checks
- Shannon entropy as a conservation metric

### Cleaning strategy
Sequences were filtered to retain likely full-length vertebrate SNRNP70 orthologs by:
- keeping U1-70K / SNRNP70-like names
- excluding:
  - fragment
  - partial
  - like
  - uncharacterized
  - lin-7
- keeping sequences roughly in the **380-500 aa** range
- removing exact duplicate sequences

### Final homolog set
- **416** filtered sequences
- **354** unique amino-acid sequences after exact deduplication

### Alignment outputs
- `results/SNRNP70_vertebrate_clean.fasta`
- `results/SNRNP70_vertebrate_clean_dedup.fasta`
- `results/SNRNP70_vertebrate_clean_dedup_aligned.fasta`
- `results/SNRNP70_RRM_92_202_aligned.fasta`

### Critical validation step
Instead of trusting the full alignment blindly, the analysis checked the **actual interface-bearing columns** corresponding to human residues:
`115, 118, 121, 122, 124, 125, 126, 153, 155, 156, 163, 167, 168, 170, 171`

These interface columns were **very well occupied**, with:
- mean nongap fraction ≈ **0.986**
- minimum nongap fraction ≈ **0.975**
- maximum nongap fraction ≈ **0.997**

### Residue-level SNRNP70 interface conservation

| Residue | AA | Entropy | Interpretation |
|---|---:|---:|---|
| 115 | T | 0.157 | moderately conserved |
| 118 | K | 0.057 | conserved |
| 121 | R | 0.057 | conserved |
| 122 | E | 0.029 | highly conserved |
| 124 | E | 0.029 | highly conserved |
| 125 | V | 0.029 | highly conserved |
| 126 | Y | 0.000 | highly conserved / invariant |
| 153 | H | 0.028 | highly conserved |
| 155 | R | 0.028 | highly conserved |
| 156 | D | 0.079 | conserved |
| 163 | H | 0.084 | conserved |
| 167 | K | 0.028 | highly conserved |
| 168 | K | 0.078 | conserved |
| 170 | D | 0.112 | conserved |
| 171 | G | 0.084 | conserved |

### Summary
- **7 highly conserved residues**
- **7 conserved residues**
- **1 moderately conserved residue**

### Biological interpretation
The SNRNP70 interface contains a strongly constrained evolutionary core, including:
- **R121**
- **E122**
- **E124**
- **V125**
- **Y126**

This supports the idea that the direct Pol II-binding surface on U1-70K is under strong vertebrate evolutionary constraint.


## Repository structure

```text
pol2-u1-interface/
├── README.md
├── data_raw/
├── data_processed/
├── results/
├── scripts/
└── docs/
