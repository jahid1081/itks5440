# Semantic CV - ITKS5440

This repository contains a **machine‑readable Curriculum Vitae** for **M Jahidul Islam**, built for the course **ITKS5440: Semantic Web and Linked Data**.  
It includes an HTML5 page with **RDFa** annotations, an **OWL (RDF/XML)** ontology, and a **Turtle** version of the same ontology, plus supporting assets and publications.

> The ontology base IRI is: `https://jahid1081.github.io/itks5440/cv-ontology.owl#`

---

## 🔗 Live Links

- **CV (HTML + RDFa):** https://jahid1081.github.io/itks5440/cv.html  
- **Ontology (RDF/XML, OWL):** https://jahid1081.github.io/itks5440/cv-ontology.owl  
- **Ontology (Turtle):** https://jahid1081.github.io/itks5440/cv-ontology.ttl  

Publications (PDFs) are under `/pubs/` and linked from the CV page.

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

## 🧠 Vocabularies & Modeling

- **schema.org** - core person, roles, education, software/services, dates
- **FOAF** - `foaf:depiction`, `foaf:img`, `foaf:homepage`, `foaf:page`
- **PROV‑O** - activities linked to overlapping organizational roles
- **CEFR** - modeled via `schema:DefinedTermSet` and a simple `LanguageProficiency` node per language (English C1; Finnish A1; Bengali native)

Key sections represented in the HTML and OWL/TTL:

- **Person** (`cv:#jahid`) with `schema:description` and `foaf:bio`
- **Experience**: organizational roles (`schema:EmployeeRole`) with **month‑level** `startDate`/`endDate` (`xsd:gYearMonth`)
- **Functional roles** (Access Control Analyst; Business Analyst), also as `schema:EmployeeRole`
- **Banking Platforms, Rails & Tools**: `schema:SoftwareApplication` and `schema:GovernmentService` (e.g., FLEXCUBE, Velocity AML, BEFTN, BD‑RTGS; operator: Bangladesh Bank)
- **Activities**: `prov:Activity` associated with overlapping roles
- **Education**: `schema:EducationalOccupationalCredential` with `dateCreated` (`xsd:date`)
- **Publications**: `schema:ScholarlyArticle` linked to local PDFs
- **Linking**: `schema:sameAs` to **DBpedia** where confirmed; Wikipedia/official links where appropriate

> **Privacy choices:** email is **obfuscated** (no `mailto:`); **phone/street** are not published on the HTML page; **date of birth** is shown by choice but could be hidden in HTML while kept in OWL.

---

## ▶️ Preview / Run

### GitHub Pages
This repo is already configured for GitHub Pages under the default branch. Open the live CV link above.

### Local Preview
Use any static server:
```bash
# Python 3
python -m http.server 8080
# then open http://localhost:8080/cv.html
```

---

## ✅ Validation & Consistency

- **RDFa extraction:** verified (e.g., on rdfa.info).  
- **Cross‑file checks:** the HTML subjects (`cv:#…`) exist in the OWL/TTL; `typeof ↔ rdf:type` agrees; all dates match; FOAF additions are present in all files; avatar `<img>` includes `property="image foaf:depiction foaf:img"`.

If you change any data, keep **HTML, OWL, and TTL** in sync (see next section).

---

## ✍️ How to Update

### Update the photo
1. Replace images in `assets/` (keep same names or update URLs in all three files).
2. In `cv.html`, the avatar is the `<img>` inside `<figure class="avatar">`. It carries the RDFa properties:
   ```html
   <img property="image foaf:depiction foaf:img" src="assets/profile.webp" ... >
   ```
3. `cv-ontology.owl` and `cv-ontology.ttl` contain the image as `schema:image`, `foaf:depiction`, and `foaf:img` for `cv:#jahid`.

### Add a publication
1. Place the PDF in `/pubs/`.
2. Add a `schema:ScholarlyArticle` item in **both**:  
   - `cv.html` (RDFa block under *Publications*), and  
   - `cv-ontology.owl` / `cv-ontology.ttl` (new subject with matching properties).
3. Link the PDF with `schema:encoding` or `schema:url` (HTML side) and an appropriate property in OWL/TTL.

### Add a DBpedia link
- Only add `schema:sameAs` **after confirming** the exact DBpedia page exists.
- Update both HTML (RDFa `property="sameAs"`) and ontology files.

### Dates & Formats
- **Roles/Activities:** use `YYYY‑MM` in `datetime`/literals; **visible** format in HTML is `From MM‑YYYY To MM‑YYYY`.
- **Birthdate/Credential:** use `YYYY‑MM‑DD` in machine values; **visible** format can be `DD‑MM‑YYYY` in HTML.

---

## 🧾 Submission Checklist

- [x] CV webpage live (`cv.html`)
- [x] Ontology published (`cv-ontology.owl`, `cv-ontology.ttl`)
- [x] RDFa extraction works
- [x] DBpedia/official links added where confirmed
- [x] Publications linked under `/pubs/`
- [x] Photo present and semantically linked (schema + FOAF)
- [x] **DOCX report** prepared and emailed (includes Conclusion)


---

## 📜 Notes

- Keep the **base IRI** unchanged unless you intentionally migrate the ontology URL.
- Do **not** publish private data unintentionally. The current page keeps email obfuscated and omits phone/street.
- If you add more FOAF terms (e.g., `foaf:knows`), do so deliberately and consistently across all files.

---

**Author:** M Jahidul Islam  
**Course:** ITKS5440 - Semantic Web and Linked Data
