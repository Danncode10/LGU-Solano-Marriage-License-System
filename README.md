# LGU Solano Marriage License System

## Project Overview

This is a specialized internal tool developed for the **Local Civil Registrar (LCR) of Solano, Nueva Vizcaya**. The system's primary objective is to automate the intake of marriage license applications and to generate legally compliant, pre-formatted Excel documents based on the **Philippine Family Code (Arts. 14 & 15)**.

The project aims to streamline the existing manual and often error-prone process, improving efficiency for LCR employees and ensuring legal compliance in document generation.

## Key Features

### Applicant Intake (Public)
*   Applicants fill out a web-based form (Next.js) with personal, age, address, and parents' information.
*   Upon submission, a **Unique 6-Character Code** (e.g., `000-010`) is generated and shown to the couple. This code serves as an application index for tracking.
*   Applications from individuals under 18 years of age are automatically blocked as per R.A. 11596.

### Employee Processing (Internal)
*   LCR employees log into an internal system.
*   Employees can retrieve applications using the unique 6-character code.
*   Built-in webcam feature allows employees to take and upload photos of the couple directly to Supabase Storage.
*   Employees can initiate the generation of the marriage license document.

### Document Generation (Backend)
*   A FastAPI backend processes application data.
*   It dynamically evaluates the ages and addresses of both applicants to include/exclude specific sheets (e.g., `CONSENT`, `ADVICE`, `AddressBACKnotice`, `EnvelopeAddress`) in the final `.xlsx` document, ensuring legal compliance.
*   Applicant data and captured photos are injected into a pre-formatted Excel master template using `openpyxl`.
*   The final, populated Excel file is returned as a downloadable stream.

## Technical Stack

*   **Frontend:** Next.js (App Router), Tailwind CSS
*   **Backend:** FastAPI (Python), uvicorn
*   **Database & Authentication:** Supabase (PostgreSQL, Row Level Security, Supabase Auth)
*   **Storage:** Supabase Storage (for applicant photos)
*   **Excel Manipulation:** `openpyxl` (Python)
*   **Data Validation:** `Pydantic` models (Python)
*   **Webcam Integration:** `react-webcam`

## Setup and Development

### Prerequisites
*   Node.js (for Next.js frontend)
*   Python (for FastAPI backend)
*   npm or yarn
*   `pip` or `pipenv`/`poetry` (for Python dependency management)
*   A Supabase project configured with appropriate tables and storage buckets.

### Environment Variables
Create a `.env` file in the root of your project (and in relevant subdirectories if structured as a monorepo) based on `.env.example`. Do not commit sensitive `.env` files to version control.

**Example `.env.example` content:**
```
NEXT_PUBLIC_SUPABASE_URL=your_supabase_project_url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_supabase_anon_key
SUPABASE_SERVICE_ROLE_KEY=your_supabase_service_role_key
DATABASE_URL=your_database_connection_string
API_SECRET_KEY=your_api_secret_key
```

### Installation

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/Danncode10/LGU-Solano-Marriage-License-System.git
    cd LGU-Solano-Marriage-License-System
    ```

2.  **Frontend Setup (Next.js):**
    ```bash
    cd frontend_directory # if applicable, otherwise commands run from root
    npm install
    npm run dev
    ```

3.  **Backend Setup (FastAPI):**
    ```bash
    cd backend_directory # if applicable, otherwise commands run from root
    python -m venv .venv
    source .venv/bin/activate
    pip install -r requirements.txt # create requirements.txt with `pip freeze > requirements.txt`
    uvicorn main:app --reload
    ```

## Security and Roles

*   **Admin:** Full access to manage employees and view database logs.
*   **Employee:** Can search applications, capture photos, and generate/download documents. Cannot delete records.
*   **Applicant:** Can only submit their own application data. Cannot read other applicants' data (enforced by Supabase Row Level Security).

## Compliance

*   Adherence to the **Philippine Family Code (Arts. 14 & 15)** for document content.
*   Compliance with **R.A. 11596** by blocking applications from individuals under 18.

