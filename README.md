# CRLX: X.509 CRL Acceleration Framework

[![Colab](https://img.shields.io/badge/Open%20in-Colab-orange)](https://colab.research.google.com/)

---

## 1. Project Overview

**CRLX** is an open-source framework designed to accelerate **X.509 Certificate Revocation List (CRL)** processing. It addresses the key bottlenecks of traditional RFC 5280 CRLs:  

- **Computational latency:** Expensive ASN.1 decoding and linear lookup.  
- **Security trade-offs:** Bloom filter or compressed CRL methods introduce false positives.  

CRLX implements a **non-probabilistic, client-side optimization layer**:
- ASN.1 decoding is done **once** upon CRL download.  
- Revoked serial numbers are stored in a **hash-based index** (O(1) lookup).  
- Works with both **synthetic** and **real CRLs** with **100% fidelity**.  

---

## 2. Project Objectives

| Phase | Objective | Metric |
|-------|-----------|--------|
| Profiling | Quantify latency in legacy CRL lookups | `L_decode`, `L_fetch` measurements |
| Acceleration | Achieve O(1) lookup speed | Speedup factor, storage reduction |
| Visualization | Validate correctness and high fidelity | Interactive dashboard with metrics |

---

## 3. Dataset

- **Synthetic CRLs:** Generated in Colab (3 certificates revoked)  
- **Real CRLs:** Example CRL with 36 revoked certificates  
  - Sample revoked serials:  
    [343221542082308252812471151435050411029562276936,
     652150439487329841487760055844176057896109436344, ...]

> Note: CRLX can scale to large CRLs (thousands+ revocations) due to hash-based indexing.

---

## 4. How to Run (Google Colab)

1. Open the notebook 
2. Run all cells to:
   - Generate synthetic CRLs  
   - Build CRLX index  
   - Upload real CRL (optional)  
   - View performance metrics in **interactive dashboard**

3. Optional: install dependencies locally:
```bash
pip install -r requirements.txt
````

---

## 5. Implementation Steps

1. **Directory setup:** `/crlx`, `/data`, `/Colab`, `/docs`, `/screenshots`
2. **Generate synthetic CRLs** and store them in `/data`
3. **Parse CRL files** using `crlx/crl_parser.py`
4. **Build accelerated index** using `crlx/crlx_index.py`
5. **Query lookup** and compare with legacy linear search
6. **Interactive visualization**: latency, speedup, cache size, staleness

---

## 6. Results

| Metric              | Synthetic CRL | Real CRL (36 revoked) |
| ------------------- | ------------- | --------------------- |
| Total CRLs          | 3             | 1                     |
| Total revoked certs | 3             | 36                    |
| Cache size (bytes)  | 1,301         | 12,450                |
| Avg lookup speedup  | 11.57x        | 1.02x                 |

> **Dashboard visualizations:** Latency decomposition, gauge indicators, staleness indicator, summary table.

*Screenshots / graphs can be added in `/screenshots`.*

---

## 7. Architecture Flow

```
CRL Download → ASN.1 Decode (once) → Build Hash Index → O(1) Lookup → Dashboard Metrics
```

* Works **client-side only**, requires **no changes to global PKI**.

---

## 8. References / Research

* RFC 5280 – Internet X.509 PKI CRL Profile
* TinyCR: Efficient CRL lookups in constrained environments
* C2RL: Compressed CRLs using Bloom filters
* AWS CRL scaling research (latency & caching analysis)
* Micali, Hash-tree based revocation schemes

---

## 9. License

MIT License – see `LICENSE`

