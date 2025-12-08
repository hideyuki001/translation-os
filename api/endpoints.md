# **📄 /api/endpoints.md — Translation OS API Endpoints**

*(Version 1.0 — Minimal & Elegant Specification)*

Translation OS exposes **five minimal REST endpoints**, each representing a core component of the structure-first translation pipeline.

All endpoints follow:

* JSON request / JSON response

* UTF-8 text

* Stateless operation

* Model-agnostic design

---

# **\#\# 1\. /v1/semantic-core**

Extracts the **Semantic Nucleus** from the input text.

### **Purpose**

Identify the essential meaning units required for structure-first translation.

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

* Output is language-agnostic.

* Used by all later pipeline stages.

---

# **\#\# 2\. /v1/structure-remap**

Maps meaning units into a stable cross-lingual structure.

### **Purpose**

Transform the semantic nucleus into a culturally neutral structural template.

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

* Ensures structural consistency before generation.

* Prevents drift in MT/LLM systems.

---

# **\#\# 3\. /v1/evaluate**

Runs the **META Evaluation Gate** (YES / NO).

### **Purpose**

Determine whether a candidate translation passes the structural requirements.

### **Request**

`{`  
  `"candidate": "アップデートは来週段階的に展開される予定です。",`  
  `"structure": {`  
    `"event": "update rollout",`  
    `"time": "next week",`  
    `"modality": "planned"`  
  `}`  
`}`

### **Response**

`{`  
  `"meta_decision": "YES",`  
  `"issues": []`  
`}`

### **If NO**

`{`  
  `"meta_decision": "NO",`  
  `"issues": [`  
    `"missing: planned modality",`  
    `"weakened emphasis on schedule"`  
  `]`  
`}`

### **Notes**

* Binary decision only（YES/NO）

* Issues → fed directly into /refine

---

# **\#\# 4\. /v1/refine**

Repairs meaning drift and reduces ΔS (structural entropy).

### **Purpose**

Produce a stronger, shorter, more stable version of the candidate translation.

### **Request**

`{`  
  `"candidate": "アップデートは来週から展開されます。",`  
  `"issues": [`  
    `"missing: planned modality"`  
  `]`  
`}`

### **Response**

`{`  
  `"refined": "アップデートは来週から段階的に展開される予定です。"`  
`}`

### **Notes**

* Always returns a “stronger” structure

* Should make META more likely to pass

* Used repeatedly until stabilization

---

# **\#\# 5\. /v1/deltaS**

Computes structural entropy (ΔS) of the candidate.

### **Purpose**

Quantify structural stability to ensure convergence.

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

* Lower ΔS \= better structural alignment

* Used as a refinement stop condition

---

# **\# Error Handling**

All endpoints return:

### **Error Format**

`{`  
  `"error": {`  
    `"type": "InvalidInput",`  
    `"message": "source_text field is missing."`  
  `}`  
`}`

Typical error types:

* `InvalidInput`

* `MissingField`

* `StructureMismatch`

* `EvaluationFailure`

* `InternalError`

---

# **\# Evidence Chain (Short Description)**

The **Counter-Evidence System** ensures that every correction includes explicit justification.

Example:

`{`  
  `"issue": "meaning_drift",`  
  `"evidence": "time_reference missing compared to structure: next week"`  
`}`

---

# **\# Versioning**

All endpoints are under:

`/v1/`

Future versions will follow semantic versioning:

`/v2/`  
`/v3/`

---

# **\# License**

Recommend: **MIT License**  
 (compatible with commercial and OSS reuse)

