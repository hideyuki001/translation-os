# **/api/endpoints.md — Translation OS API Endpoints (v1.0)**

**Minimal, deterministic, and structure-first translation API.**

All endpoints follow:

* JSON request / JSON response

* UTF-8 text

* Stateless operation

* Model-agnostic design

* Fully aligned with the Six-Phase Pipeline

---

# **Endpoint Overview**

Translation OS exposes **five core REST endpoints**, corresponding to Phases 1–5 of the pipeline:

| Phase | Function | Endpoint |
| ----- | ----- | ----- |
| 1 | Semantic Core Extraction | `/v1/semantic-core` |
| 2 | Structural Mapping | `/v1/structure-remap` |
| 3 | Draft Synthesis (Surface Realization) | `/v1/synthesize` |
| 4 | META Evaluation (YES/NO) | `/v1/evaluate` |
| 5 | Delta-S-Based Refinement | `/v1/refine` |
| — | ΔS Measurement (Utility) | `/v1/deltaS` |

Phase 6 (Cultural Verification) is intentionally **human-in-the-loop** and has no endpoint.

---

# **1\. `/v1/semantic-core` — Extract Semantic Nucleus**

Extracts essential meaning units from the source text.

### **Purpose**

Identify the minimal semantic representation required for structure-first translation.

### **Request**

`{`  
  `"source_text": "The update will roll out next week."`  
`}`

### **Response**

`{`  
  `"nucleus": [`  
    `"future_event",`  
    `"scheduled_update",`  
    `"time_reference: next_week"`  
  `]`  
`}`

### **Notes**

* Output is **language-agnostic**.

* Required by all downstream phases.

---

# **2\. `/v1/structure-remap` — Build Cross-Lingual Structure**

Transforms the semantic nucleus into a stable structural template.

### **Purpose**

Create a structure-first scaffold before sentence generation.

### **Request**

`{`  
  `"nucleus": [`  
    `"future_event",`  
    `"scheduled_update",`  
    `"time_reference: next_week"`  
  `]`  
`}`

### **Response**

`{`  
  `"structure": {`  
    `"event": "update rollout",`  
    `"time": "next week",`  
    `"modality": "planned"`  
  `}`  
`}`

### **Notes**

* Prevents structural drift.

* Ensures consistent generation across languages.

---

# **3\. `/v1/synthesize` — Generate Draft Sentence**

Performs Phase 3 (Syntactic Optimization \+ Surface Realization).

### **Purpose**

Convert the structure into a coherent sentence in the target language.

### **Request**

`{`  
  `"structure": {`  
    `"event": "update rollout",`  
    `"time": "next week",`  
    `"modality": "planned"`  
  `},`  
  `"target_lang": "ja"`  
`}`

### **Response**

`{`  
  `"draft": "アップデートは来週から段階的に展開される予定です。"`  
`}`

### **Notes**

* Produces a **structure-faithful** draft.

* Typically fed directly into `/evaluate`.

---

# **4\. `/v1/evaluate` — META Evaluation (YES / NO Gate)**

Performs Phase 4 of the pipeline.

### **Purpose**

Verify that the candidate translation matches the structural template.

### **Request**

`{`  
  `"candidate": "アップデートは来週段階的に展開される予定です。",`  
  `"structure": {`  
    `"event": "update rollout",`  
    `"time": "next week",`  
    `"modality": "planned"`  
  `}`  
`}`

### **Response (YES)**

`{`  
  `"meta_decision": "YES",`  
  `"issues": []`  
`}`

### **Response (NO)**

`{`  
  `"meta_decision": "NO",`  
  `"issues": [`  
    `"missing: planned modality",`  
    `"weakened emphasis on schedule"`  
  `]`  
`}`

### **Notes**

* Strict binary decision.

* Issues are passed directly into `/refine`.

---

# **5\. `/v1/refine` — Delta-S-Based Recursive Refinement**

Phase 5 of the pipeline. Repairs drift and stabilizes structure.

### **Purpose**

Strengthen structure, repair drift, and lower structural entropy.

### **Request**

`{`  
  `"candidate": "アップデートは来週から展開されます。",`  
  `"issues": ["missing: planned modality"]`  
`}`

### **Response**

`{`  
  `"refined": "アップデートは来週から段階的に展開される予定です。"`  
`}`

### **Notes**

* Always produces a **stronger, more stable** output.

* Repeat until META Evaluation becomes YES.

---

# **6\. `/v1/deltaS` — Structural Entropy Measurement**

Utility endpoint (not part of the main pipeline).

### **Purpose**

Quantify structural entropy (Delta-S) for convergence.

### **Request**

`{`  
  `"candidate": "アップデートは来週から段階的に展開される予定です。"`  
`}`

### **Response**

`{`  
  `"delta_s": 0.12,`  
  `"status": "stable"`  
`}`

### **Notes**

* Lower ΔS \= better structural alignment.

* Can be used as a refinement stop condition.

---

# **Error Handling**

All endpoints may return errors in the following format:

### **Error JSON**

`{`  
  `"error": {`  
    `"type": "InvalidInput",`  
    `"message": "source_text field is missing."`  
  `}`  
`}`

### **Standard HTTP Status Codes**

| Status | Meaning |
| ----- | ----- |
| **400 Bad Request** | InvalidInput / MissingField |
| **409 Conflict** | StructureMismatch |
| **422 Unprocessable Entity** | EvaluationFailure |
| **500 Internal Server Error** | InternalError |

---

# **Evidence Chain Integration (Short Description)**

Each correction must include explicit justification.

### **Example**

`{`  
  `"issue": "meaning_drift",`  
  `"evidence": "time_reference missing compared to structure: next_week"`  
`}`

This evidence can be consumed by `/refine` and tracked for QA.

---

# **Versioning**

Endpoints follow semantic versioning:

`/v1/...`  
`/v2/...`  
`/v3/...`

---

# **License**

Recommend: **MIT License**  
 Compatible with both commercial and open-source reuse.

