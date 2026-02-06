# **PHASE 1 — Project Planning & Problem Definition**

## 1.1 Problem Statement

Users (LCR employees and applicants) struggle with the manual and often error-prone process of marriage license application intake and document generation because it is time-consuming and prone to legal non-compliance. This causes inefficiencies for the LCR and potential delays/issues for applicants, resulting in a suboptimal and outdated system. This problem is exacerbated by applicants who may lack access to email or digital literacy, requiring alternative intake methods.

---

## 1.2 Target Users

Primary users are LCR employees with moderate technical skill, mostly using desktop/laptop devices. Secondary users are applicants, who may interact directly with the system via various devices or be assisted by an LCR employee. Admins are also identified as users.

---

## 1.3 Project Goals

The project succeeds if LCR employees can efficiently automate marriage license application intake (both self-service and employee-assisted) and consistently generate legally compliant Excel documents without manual data entry errors or non-compliance issues.

---

## 1.4 Competition & Market Scan

Currently, there seem to be no direct digital competitors for this specific localized process. The primary 'competitor' is the existing manual, paper-based system, which is inefficient and prone to errors. This project focuses on automating and digitizing this process for the LGU Solano, improving efficiency and compliance.

---

## 1.5 Feature Definition & Scope Control

MUST:
*   Flexible Applicant intake form (Next.js) - allows both self-service public submission and employee-assisted submission within the internal system.
*   Data saving to Supabase `applications` table.
*   Unique 6-Character Code generation (e.g., `000-010`), serving as an application index for admins to check applications and for employees to retrieve applications and take photos via the website on their phones.
*   Employee login (Supabase Auth - Role: `employee`).
*   Retrieve application by Unique Code.
*   Webcam photo capture and upload to Supabase Storage.
*   "Generate License" functionality.
*   FastAPI backend for document generation.
*   `openpyxl` for Excel generation.
*   Age-based and address-based sheet generation logic.
*   Data and photo injection into Excel.
*   Admin role with full access (manage employees, view logs).
*   Employee role (search, intake assistance, take photos, generate/download files).
*   Row Level Security (RLS) for applicants.
*   Pydantic models for validation.
*   Excel cleanup (removing irrelevant sheets).

NICE:
*   Advanced reporting features.
*   More comprehensive audit trails.
*   User interface enhancements beyond core functionality.

OUT:
*   Features related to other government services.
*   Complex integrations with external systems beyond Supabase.
*   Non-core document types.

---

## 1.6 Platform & Project Type

The system is a web application accessible via a browser. It is built as an internal tool for the LCR, with a public-facing intake form that can also be used by employees to assist applicants. There is no explicit mention of expansion to mobile, AI, or robotics, nor does it require real-time processing beyond standard web application responsiveness. It integrates with webcam hardware for photo capture.

---

## 1.7 User Requirements

*   **Applicants:**
    *   Fill out a Next.js form with personal and parental information (self-service).
    *   Can provide information to an LCR employee to have their application submitted on their behalf.
    *   Receive a Unique 6-Character Code upon submission.
    *   Restricted from reading other applicants' data.
*   **Employees:**
    *   Log in to the system.
    *   Ability to submit applications on behalf of applicants.
    *   Search and retrieve applications using the Unique 6-Character Code (which can be input from a phone via the website).
    *   Use a built-in webcam or phone camera to take applicant photos.
    *   Upload photos to Supabase Storage.
    *   Generate and download legally compliant marriage license documents.
    *   Restricted from deleting records.
*   **Admins:**
    *   Manage employees (Create/Delete).
    *   View database logs and check applications using the Unique 6-Character Code as an index.
    *   Full access to system functionalities.

*   **User-Friendly Errors:**
    *   System must block applications if an applicant is below 18 (R.A. 11596).
    *   Validation errors for age ranges must be handled gracefully (Pydantic models).
    *   Clear feedback for successful application submission and document generation.

*   **Accessibility Needs:** Not explicitly mentioned in the project spec, implying standard web accessibility practices should be followed for both the public-facing and internal employee interfaces.

---

## 1.8 System Requirements

*   **Expected number of users:**
    *   LCR Employees/Admins: Likely a small number (e.g., 5-10 concurrent users).
    *   Applicants: Potentially higher, but not concurrent for document generation (intake is asynchronous). (e.g., 50-100 unique applicants per day).
*   **Response time expectations:**
    *   Applicant form submission (self-service or employee-assisted): Fast, near real-time.
    *   Employee application retrieval: Fast, near real-time.
    *   Document generation: Reasonable, depending on complexity, but should not exceed a few seconds (e.g., < 5 seconds).
*   **Availability requirements:** High availability during LCR operating hours.
*   **Storage needs:**
    *   Database (Supabase PostgreSQL): To store application data.
    *   File Storage (Supabase Storage): For applicant photos. The capacity should scale with the number of applications/photos over time.

---

## 1.9 Software Evolution

*   The system's core functionality includes flexible applicant intake (self-service and employee-assisted), employee processing (search, photo, generate), and legally compliant document generation based on age and address logic. The focus is on automating the current manual process.

---

## 1.10 Monetization Strategy

The project is designed as an internal tool for LGU Solano, Nueva Vizcaya. Therefore, it is free for the LGU and its employees and serves as a public service for applicants. There are no premium features, ads, or direct monetization strategies as it's a government-commissioned internal system.