# **PHASE 2 — Software Requirements Specification (SRS)**

## 2.1 Functional Requirements

*   **Applicant Intake:**
    *   **Function:** Allow applicants to fill out a Next.js form with personal, age, address, and parents' information.
    *   **Trigger:** Applicant accesses the public-facing form.
    *   **Output:** Data saved to Supabase `applications` table; a unique 6-character code (e.g., `000-010`) is generated and displayed to the couple.

*   **Employee Processing:**
    *   **Function:** Allow employees to log in, search applications by unique code, capture applicant photos via webcam, upload photos, and generate marriage license documents.
    *   **Trigger:** Employee logs in to the internal system; employee inputs unique code; employee activates webcam; employee clicks "Generate License."
    *   **Output:** Retrieved application data; uploaded photo to Supabase Storage; generated and downloadable `.xlsx` stream.

*   **Admin Management:**
    *   **Function:** Allow administrators to manage employees (create/delete accounts) and view database logs.
    *   **Trigger:** Admin logs in to the system.
    *   **Output:** Updated employee records; displayed database logs.

*   **Document Generation Logic:**
    *   **Function:** Evaluate applicant ages and residences to include/exclude specific sheets (`CONSENT`/`ADVICE` sheets for age, `AddressBACKnotice`/`EnvelopeAddress` for non-residents) in the final Excel document.
    *   **Trigger:** Backend receives application ID from employee processing.
    *   **Output:** A legally compliant `.xlsx` file with only necessary sheets.

*   **Data and Photo Injection:**
    *   **Function:** Inject text data from the application and the captured photo into designated cells/locations within the Excel template.
    *   **Trigger:** Backend processes document generation.
    *   **Output:** An Excel file populated with applicant data and photo.

--- 

## 2.2 Non-Functional Requirements

*   **Performance:**
    *   Applicant form submission: Fast response times.
    *   Employee application retrieval: Fast response times.
    *   Document generation: Response time within a few seconds (e.g., < 5 seconds).
*   **Security:**
    *   Authentication: Secure user login for employees and admins via Supabase Auth.
    *   Authorization: Role-based access control (Admin, Employee) and Row Level Security (RLS) for applicants (read/write restrictions).
    *   Data Protection: Data encrypted at rest and in transit (handled by Supabase).
    *   Photo Uploads: Secure uploading of applicant photos to Supabase Storage.
*   **Scalability:**
    *   The system should be capable of handling an increasing number of applicants and data without significant performance degradation.
    *   Utilize scalable services (Supabase, FastAPI) to accommodate growth.
*   **Usability:**
    *   Intuitive and user-friendly interfaces for both public applicants and internal employees.
    *   Clear and concise error messages and feedback.
*   **Availability:**
    *   High availability during LCR operating hours.
*   **Reliability:**
    *   Accurate generation of legally compliant documents.
    *   Data integrity for all stored application information.

--- 

## 2.3 Technical Requirements

*   **Frontend:** Next.js (App Router), Tailwind CSS, `react-webcam` for photo capture.
*   **Backend:** FastAPI (Python), utilizing `Pydantic` for data validation.
*   **Database & Authentication:** Supabase (PostgreSQL with Row Level Security).
*   **Storage:** Supabase Storage for applicant photos.
*   **Document Generation:** `openpyxl` (Python) for Excel manipulation.
*   **Containerization (Implicit):** FastAPI backend can be containerized for deployment flexibility.

--- 

## 2.4 Constraints

*   **Legal & Compliance:**
    *   All generated documents must comply with the Philippine Family Code (Articles 14 & 15).
    *   Strict adherence to R.A. 11596, blocking applications for individuals below 18 years of age.
*   **Technology Stack:** Adherence to the specified technologies (Next.js, FastAPI, Supabase, openpyxl, Pydantic, react-webcam).
*   **Excel Manipulation:** Must use `openpyxl`; `pandas` cannot be used for formatting or direct Excel generation as per project instructions.
*   **Internal Tool:** Primarily designed as an internal tool for LGU Solano, Nueva Vizcaya.

--- 

## 2.5 Acceptance Criteria

*   **Applicant Intake:**
    *   An applicant successfully completes and submits the online form.
    *   A unique 6-character code is generated and displayed to the applicant upon submission.
    *   All submitted data is accurately stored in the Supabase `applications` table.
    *   Applications from individuals under 18 years old are rejected with an appropriate error message.
*   **Employee Processing:**
    *   An employee can successfully log in to the system with their credentials.
    *   An employee can search and retrieve an application using its unique 6-character code.
    *   An employee can successfully capture a photo of the couple using a webcam/phone camera and upload it to Supabase Storage.
    *   An employee can successfully generate and download the marriage license `.xlsx` document.
*   **Admin Management:**
    *   An admin can successfully create and delete employee accounts.
    *   An admin can view database logs.
*   **Document Generation:**
    *   The generated `.xlsx` document accurately reflects the age-based sheet inclusion/exclusion logic (`CONSENT`/`ADVICE` sheets).
    *   The generated `.xlsx` document accurately reflects the address-based sheet inclusion/exclusion logic (`AddressBACKnotice`/`EnvelopeAddress`).
    *   All relevant text data (names, dates, etc.) from the application is correctly injected into the designated cells of the Excel document.
    *   The captured applicant photo is correctly inserted into the `Notice` sheet at the specified anchor cell.
*   **Security:**
    *   Employees are restricted from deleting application records.
    *   Applicants can only write their own data and cannot read other applicants' data.

--- 

## 2.6 Tech Stack Selection

*   **Frontend:** Next.js (App Router)
*   **Backend as a Service (BaaS):** Supabase (for Database, Authentication, and Storage)
*   **Custom Backend:** FastAPI (Python)
*   **Database:** PostgreSQL (managed by Supabase)
*   **Hosting:** Not explicitly defined in project specs, but typically Vercel for Next.js frontend and Render/Fly.io/AWS for FastAPI backend, all integrated with Supabase.

--- 

## 2.7 Security Design (Service-Oriented)

*   **Authentication Method:** Supabase Auth for all user roles (Applicants, Employees, Admins).
*   **Role-Based Access Control (RBAC):**
    *   **Admin:** Full access to manage employees and view system logs.
    *   **Employee:** Access to search applications, capture photos, and generate/download documents; restricted from deleting records.
    *   **Applicant:** Restricted to writing their own application data; no read access to other applicants' data (enforced via Row Level Security).
*   **Encryption Strategy:** Data at rest and data in transit are managed and secured by Supabase's underlying infrastructure.
*   **Secrets Management:** Environment variables for non-sensitive configurations. Sensitive information (e.g., Supabase keys, API keys) will be managed through secure environment variables provided by the hosting platform (e.g., Vercel, Render) and not committed to version control. Use `.env.example` for templates.
*   **Backup and Recovery Plan:** Supabase provides automated backups and point-in-time recovery for the database. Storage for files also has redundancy and backup mechanisms provided by Supabase Storage.

## 2.8 Environment Variables (.env) Questionnaire

*   **Required Environment Variables:**
    *   `NEXT_PUBLIC_SUPABASE_URL`: Supabase project URL (Frontend).
    *   `NEXT_PUBLIC_SUPABASE_ANON_KEY`: Supabase anon key (Frontend).
    *   `SUPABASE_SERVICE_ROLE_KEY`: Supabase service role key (Backend for privileged operations).
    *   `DATABASE_URL`: Connection string for PostgreSQL if direct access is needed (Backend).
    *   `API_SECRET_KEY`: Custom API secret for FastAPI, if any.
*   **Sensitive Information:** `SUPABASE_SERVICE_ROLE_KEY`, `DATABASE_URL`, `API_SECRET_KEY` contain sensitive information.
*   **Management for Different Environments:**
    *   **Development:** Local `.env` file.
    *   **Staging/Production:** Managed through the hosting provider's environment variable/secret management system (e.g., Vercel's environment variables, equivalent in Render/AWS).
*   **Sharing Templates:** Use an `.env.example` file in the repository (without sensitive values) to document required variables and their purpose. Provide clear instructions in the README on setting up environment variables locally and in deployment environments.