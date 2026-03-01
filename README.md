# MinHash and LSH Assignment

This repository contains an end-to-end implementation of **Jaccard similarity**, **MinHash**, and **Locality Sensitive Hashing (LSH)** for near-duplicate detection and similarity search.

The work is implemented in the notebook [minhash.ipynb](minhash.ipynb) and includes:

- A toy document experiment on 4 text files (`D1.txt` to `D4.txt`)
- Exact vs approximate similarity comparison using MinHash
- LSH probability modeling using the S-curve
- A larger MovieLens experiment with false positive / false negative analysis

---

## Assignment Objective

The assignment demonstrates:

1. How to construct character and word k-grams
2. How to compute exact Jaccard similarity
3. How to estimate similarity using MinHash signatures
4. How LSH reduces pairwise comparisons using banding
5. How MinHash/LSH behave on a real-world dataset (MovieLens)

---

## Project Structure

```
.
├── minhash.ipynb           # Main notebook with all implementations and experiments
├── README.md               # This file
├── minhash/
│   ├── D1.txt
│   ├── D2.txt
│   ├── D3.txt
│   └── D4.txt
└── ml-100k/                # MovieLens 100K data files
```

---

## Concepts and Formulae

### Jaccard Similarity

For sets $A$ and $B$:

$$
J(A,B)=\frac{|A \cap B|}{|A \cup B|}
$$

### MinHash Estimation

If two MinHash signatures of length $t$ match in $m$ positions, then:

$$
\hat{J}(A,B)=\frac{m}{t}
$$

### LSH Candidate Probability (Banding)

With $r$ rows per band and $b$ bands, probability that two signatures with similarity $s$ become a candidate pair:

$$
P(\text{candidate}) = 1-(1-s^r)^b
$$

---

## Implementation Overview

All code is inside [minhash.ipynb](minhash.ipynb).

### A. K-gram construction

- Character 2-grams
- Character 3-grams
- Word 2-grams

### B. Exact similarity

- Computes pairwise exact Jaccard similarity for all document pairs from `D1` to `D4`.

### C. MinHash on toy documents

- Hash family used:

$$
h(x)=(ax+b)\bmod m
$$

- Signature sizes tested: `t = [20, 60, 150, 300, 600]`
- Compares approximate similarity with exact similarity (primarily on char 3-grams).

### D. LSH on toy documents

- Example configuration in notebook: `t = 160`, `r = 8`, `b = 20`
- Computes candidate probabilities and compares document pairs.

### E. MovieLens experiments

- Builds user-to-movie sets
- Computes exact similar user pairs above threshold
- Runs MinHash experiments with multiple hash sizes (50/100/200)
- Evaluates average false positives and false negatives over multiple runs
- Runs LSH with different `(r, b, threshold)` combinations
- Plots S-curves for several parameter settings

---

## How to Run

### Prerequisites

- Python 3.8+
- Jupyter Notebook / VS Code Notebook support

Install required packages:

```bash
pip install numpy matplotlib
```

### Run Steps

1. Open [minhash.ipynb](minhash.ipynb)
2. Ensure working directory is the repository root
3. Run cells from top to bottom
4. Observe printed outputs and S-curve plots

---

## Notes on Dataset Path

In the notebook, MovieLens loading function currently points to:

```python
load_movielens(path="ml-100k/u.data")
```

If `u.data` is not present in `ml-100k` folder, update this path to one of the available split files (for example `ua.base`, `ub.base`, `u1.base`, etc.), keeping tab-separated parsing unchanged.

---

## Summary of Results

### MinHash (Similarity ≥ 0.5)

| Hashes | Avg False Positives | Avg False Negatives |
|--------|--------------------|--------------------|
| 50     | 157.8              | 3.2                |
| 100    | 29.0               | 1.6                |
| 200    | 11.6               | 2.0                |


### LSH (Similarity ≥ 0.6)

| Hashes | (r,b)   | Avg FP | Avg FN |
|--------|--------|--------|--------|
| 50     | (5,10) | 2.2    | 0.8    |
| 100    | (5,20) | 0.2    | 0.4    |
| 200    | (5,40) | 0.0    | 0.2    |
| 200    | (10,20)| 0.0    | 1.6    |

---

## Key Learnings

- MinHash provides a scalable approximation of Jaccard similarity.
- Increasing signature length generally improves approximation quality.
- LSH banding creates a tunable trade-off between recall and precision.
- S-curve analysis helps choose meaningful `(r, b)` parameters for a target similarity region.

---

## References

- Mining of Massive Datasets (Leskovec, Rajaraman, Ullman) — MinHash and LSH chapters
- MovieLens 100K dataset documentation


---

## Author
Sahil Narkhede (B23CS1060)