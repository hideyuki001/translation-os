# **Translation OS — Six-Phase Translation Pipeline**

A deterministic, structure-first translation pipeline ensuring **reproducible**, **drift-free**, and **culturally aligned** output across all languages.  
 This pipeline forms the operational backbone of Translation OS and connects directly to the API endpoints in `/api/endpoints.md`.

---

## **1\. Overview**

Translation OS defines a six-phase workflow:

1. **Semantic Core Extraction**

2. **Structural Mapping**

3. **Syntactic Optimization**

4. **META Evaluation (YES/NO Gate)**

5. **ΔS-Based Recursive Refinement**

6. **Naturalness & Cultural Verification (Human-in-the-loop)**

The OS guarantees structural correctness (1–5),  
 while cultural nuance is intentionally delegated to **Omakase Logic** (Phase 6).

---

## **2\. Pipeline Diagram**

flowchart TD  
 A\[1. Semantic Core Extraction\] \--\> B\[2. Structural Mapping\]  
 B \--\> C\[3. Syntactic Optimization\]  
 C \--\> D{4. META Evaluation YES/NO}

`D -- YES --> F[6. Naturalness & Cultural Verification]`  
`D -- NO --> E[5. Recursive Refinement (ΔS Control)]`

`E --> D`  
`F --> G[Final Output]`

**Flow in words:**  
 1→2→3 generate structure-first output;  
 4 is deterministic evaluation;  
 5 handles structural drift;  
 6 applies human cultural calibration.

---

## **3\. Phase Descriptions**

### **Phase 1 — Semantic Core Extraction**

**Goal:** Identify the meaning nucleus.  
 **Output:** Intent, roles, relationships (JSON-structured).  
 **Example:**  
 Input: “We will roll out the update gradually next week.”  
 Nucleus: `update rollout → gradual → next week → plan`

---

### **Phase 2 — Structural Mapping**

**Goal:** Convert nucleus into target-language structure.  
 **Example (JP):**  
 来週 → 段階的に → アップデートを展開する予定

---

### **Phase 3 — Syntactic Optimization**

**Goal:** Produce a clean sentence without altering the mapped structure.  
 **Example:**  
 アップデートは来週から段階的に展開される予定です。

---

### **Phase 4 — META Evaluation (YES/NO Gate)**

Checks:

* Semantic fidelity

* Structural consistency

* No drift

* No invented context

* Lexical precision

YES → Phase 6  
 NO → Phase 5 (Refinement)

---

### **Phase 5 — Recursive Refinement (ΔS Control)**

**Goal:** Reduce entropy; repair drift; converge structure.  
 **Example:**  
 ❌ 段階的にアップデートが始まる予定です。（“next week” missing）  
 → 修正：アップデートは来週から段階的に展開される予定です。

---

### **Phase 6 — Naturalness & Cultural Verification (Human)**

Checks:

* Politeness/register

* Local conventions

* Cultural fit

* Domain norms

Example:  
 展開される予定です → **順次展開してまいります**（Business JP）

---

## **4\. Drift Detection (Minimal Example)**

Source:  
 “We will gradually roll out the update next week.”

Bad Output:  
 アップデートは順次展開されます。  
 ❌ Temporal anchor missing

Corrected:  
 アップデートは来週から順次展開される予定です。

---

## **5\. Evidence Chain (Simple Example)**

Issue: Missing time  
 Evidence: Source contains explicit time → target lacks it  
 Fix: Restore temporal node  
 ΔS: High → Low

---

## **6\. Summary**

The Six-Phase Pipeline ensures:

* Structural fidelity

* Deterministic evaluation

* Reproducible refinement

* Human-guided cultural alignment

* Low ΔS at convergence

This pipeline is directly compatible with the API architecture described in `/api/endpoints.md`.

