# Semantic CV - ITKS5440

This repository contains a **machine‑readable Curriculum Vitae** for **M Jahidul Islam**, built for **ITKS5440: Semantic Web and Linked Data**.  
It ships an HTML5 CV with **RDFa**, plus an ontology in **OWL (RDF/XML)** and **Turtle** - kept fully consistent.

> Base IRI: `https://jahid1081.github.io/itks5440/cv-ontology.owl#`

---

## 🔗 Live Links

- **CV (HTML + RDFa):** https://jahid1081.github.io/itks5440/cv.html  
- **Ontology (RDF/XML, OWL):** https://jahid1081.github.io/itks5440/cv-ontology.owl  
- **Ontology (Turtle):** https://jahid1081.github.io/itks5440/cv-ontology.ttl  

Publications (PDFs) live under `/pubs/` and are linked from the CV page.

---

## 📁 Repository Layout

```
itks5440/
├─ assets/
│  ├─ profile.jpg
│  ├─ profile.webp
│  └─ profile@2x.webp
├─ pubs/
│  ├─ mahmud2014.pdf
│  ├─ Low_Resolution_Camera_for_Human_Detectio.pdf
│  └─ au-poon.pdf
├─ cv.html                # HTML5 CV with RDFa annotations
├─ cv-ontology.owl        # OWL (RDF/XML) ontology
└─ cv-ontology.ttl        # Turtle serialization of the same ontology
```

---

## 🧠 Modeling Summary

- **schema.org** for Person, EmployeeRole, credentials, software/services, dates.  
- **FOAF** for profile image/page (`foaf:depiction`, `foaf:img`, `foaf:homepage`, `foaf:page`).  
- **PROV‑O** for activities linked to overlapping organizational roles.  
- **CEFR** modeled as a `schema:DefinedTermSet` and per‑language nodes linked via custom properties.

**Custom terms (namespace `cv:`)**  
- `cv:LanguageProficiency` (class)  
- `cv:hasLanguageProficiency` (Person → LanguageProficiency)  
- `cv:languageName` (xsd:string)  
- `cv:cefrLevel` (LanguageProficiency → `schema:DefinedTerm` in {{A1,A2,B1,B2,C1,C2}})

**New (logic-only) improvements**  
- **Inverse properties**  
  - `cv:hasLanguageProficiency  owl:inverseOf  cv:isLanguageProficiencyOf`  
  - `cv:cefrLevel              owl:inverseOf  cv:isLevelFor`

- **SWRL rules (inference)**  
  1) _Role ⇒ worksFor_: If `Person schema:hasOccupation Role` and `Role schema:memberOf Org`, infer `Person schema:worksFor Org`.  
  2) _Activity ⇒ worksFor_: If `Activity prov:wasAssociatedWith Role`, `Role schema:memberOf Org`, and `Person schema:hasOccupation Role`, infer `Person schema:worksFor Org`.

- **SHACL shapes (validation)**  
  - `cv:EmployeeRoleShape` - require `schema:startDate` & `schema:endDate` (`xsd:gYearMonth`)  
  - `cv:ActivityShape` - same date requirements **and** at least one `prov:wasAssociatedWith`  
  - `cv:LanguageProficiencyShape` - require `cv:languageName` (`xsd:string`) and `cv:cefrLevel ∈ {{A1,A2,B1,B2,C1,C2}}`

- **Datatype polish**  
  - `schema:datePublished` typed as **`xsd:gYear`** for scholarly articles (e.g., 2014; 2013; 2013).

---

## ▶️ Preview / Run

**GitHub Pages**: open the live CV link above.  
**Local**: any static server will do.
```bash
python -m http.server 8080
# open http://localhost:8080/cv.html
```

---

## ✅ Validation & Consistency

- **RDFa extraction** verified (e.g., rdfa.info).  
- **Cross‑file parity**: subjects in HTML exist in OWL/TTL; `typeof` ↔ `rdf:type` match; dates align.  
- **Rules/Shapes present in both serializations**: SWRL & SHACL are embedded in **OWL** and **TTL**.  
- **FOAF** links (image/page/homepage) appear consistently in **HTML**, **OWL**, and **TTL**.  
- Publications have `schema:datePublished` as **`xsd:gYear`**.

To lint with **pySHACL** locally (optional):
```bash
pip install pyshacl rdflib
pyshacl -s cv-ontology.ttl -d cv-ontology.ttl -f human
# or validate the RDF/XML:
pyshacl -s cv-ontology.ttl -d cv-ontology.owl -f human
```

---

## 🧾 Submission Checklist

- [x] CV webpage live (`cv.html`)  
- [x] Ontology published (`cv-ontology.owl`, `cv-ontology.ttl`)  
- [x] RDFa extraction works  
- [x] **Inverse properties** declared  
- [x] **SWRL rules** present (inference)  
- [x] **SHACL shapes** present (validation)  
- [x] Publications `schema:datePublished` → **`xsd:gYear`**  
- [x] DBpedia/official links where confirmed  
- [x] Photo present & semantically linked (schema + FOAF)

---

**Author:** M Jahidul Islam  
**Course:** ITKS5440 - Semantic Web and Linked Data  
**Last updated:** 2025-11-12
