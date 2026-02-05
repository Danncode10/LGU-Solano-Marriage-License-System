# PROJECT SPECIFICATION: LGU Solano Marriage License System

## 1. Project Overview

A specialized internal tool for the **Local Civil Registrar (LCR) of Solano, Nueva Vizcaya**. The system automates the intake of marriage license applications and generates legally compliant, pre-formatted Excel documents based on the **Philippine Family Code (Arts. 14 & 15)**.

---

## 2. Technical Stack

* **Frontend:** Next.js (App Router), Tailwind CSS.
* **Backend:** FastAPI (Python).
* **Database & Auth:** Supabase (PostgreSQL + Row Level Security).
* **Storage:** Supabase Storage (for applicant photos).
* **Excel Engine:** `openpyxl` (Python) using a pre-formatted `.xlsx` master template.

---

## 3. Core Business Logic (The Rules)

### 3.1 Age-Based Sheet Generation

The system must evaluate the ages of both the Male (M) and Female (F) applicants and include only the necessary sheets in the downloadable `.xlsx` file.

| Female Age | Male Age | Required Sheet Name |
| --- | --- | --- |
| 18–20 | 25+ | `CONSENT F` |
| 25+ | 18–20 | `CONSENT M` |
| 18–20 | 18–20 | `CONSENT M&F` |
| 21–24 | 25+ | `ADVICE F` |
| 25+ | 21–24 | `ADVICE M` |
| 21–24 | 21–24 | `ADVICE M&F` |
| 18–20 | 21–24 | `ADVICED M-CONSENT F` |
| 21–24 | 18–20 | `ADVICED F-CONSENT M` |
| 25+ | 25+ | *No additional age sheet needed* |

> **Constraint:** If an applicant is **below 18**, the system must block the application (illegal under R.A. 11596).

### 3.2 Address-Based Sheet Generation

* **Condition:** If **both** applicants are residents of **"Solano, Nueva Vizcaya"**, do **not** create extra address sheets.
* **Condition:** If **one or both** are NOT from Solano, create two additional sheets:
1. `AddressBACKnotice` (populated with the non-resident's address).
2. `EnvelopeAddress` (identical data to `AddressBACKnotice`).



---

## 4. Workflows

### Phase 1: Applicant Intake (Public)

1. Applicants fill out a Next.js form (Name, Age, Address, Parents' Info).
2. Data is saved to Supabase `applications` table.
3. A **Unique 6-Character Code** (e.g., `000-010`) is generated and shown to the couple. This code serves as an application index for admins to check applications and for employees to retrieve applications and take photos via the website on their phones.

### Phase 2: Employee Processing (Internal)

1. Employee logs in (Supabase Auth - Role: `employee`).
2. Employee inputs the **Unique Code** to retrieve the application.
3. Employee uses the built-in webcam feature to take a photo of the couple.
4. The photo is uploaded to Supabase Storage and the link is saved to the record.
5. Employee clicks **"Generate License"**.

### Phase 3: Document Generation (Backend)

1. FastAPI receives the application ID.
2. Backend loads `Master_Template.xlsx`.
3. **Sheet Pruning:** Deletes all age/address sheets that do *not* meet the conditions in Section 3.
4. **Data Injection:** Injects text data into specific cells (e.g., `Sheet["B5"] = "Juan Dela Cruz"`).
5. **Photo Injection:** The captured photo is inserted into the `Notice` sheet at a designated anchor cell.
6. The final file is returned as a downloadable `.xlsx` stream.

---

## 5. Security & Roles

* **Admin:** Full access. Can manage employees (Create/Delete) and view database logs.
* **Employee:** Can only search, take photos, and generate/download files. Cannot delete records.
* **Row Level Security (RLS):** Applicants can only write to the database; they cannot read other people's data.

---

## 6. Coding Instructions for Agent

* **Template Handling:** Always use `openpyxl` for Excel tasks. Do not use `pandas` for formatting.
* **Validation:** Use `Pydantic` models in FastAPI to validate age ranges before processing.
* **Excel Cleanup:** Ensure that the `wb.remove(sheet)` command is used to delete irrelevant sheets so the final file is "clean."
* **Frontend:** Use `react-webcam` for the photo capture component.