# Enterprise Healthcare Data Activation Pipeline (GCP)

## Project Overview
An end-to-end, real-time healthcare data harmonization and aggregation platform built on Google Cloud Platform. This architecture ingests legacy HL7v2 clinical messages, maps them to standard FHIR R4 resources using the Whistle mapping language, and provides a highly optimized transactional layer for extracting comprehensive Longitudinal Patient Records in a single network hop.

## Environment Architecture
This ecosystem enforces absolute blast-radius isolation by separating Development, Staging, and Production layers at the GCP project level.

```mermaid
graph TD
    subgraph GCP Environment Isolation [Project ID: healthcare-dev-497314]
        dev-app-sa[Identity: dev-app-sa] -->|1. Impersonates & Fetches Token| Auth[Short-Lived Access Token]
        Auth -->|2. Authorizes REST Request| API[Cloud Healthcare API Endpoint]
        
        subgraph dev-clinical-dataset [Location: us-central1]
            API -->|3. Resolves Graph Parameters| Store[(dev-fhir-core-store)]
            Store -->|Internal Index Scan| Pat[Patient Resource]
            Pat -->|_revinclude| Enc[Encounter]
            Pat -->|_revinclude| Obs[Observation]
            Pat -->|_revinclude| Cond[Condition]
            Pat -->|_revinclude| Med[MedicationRequest]
        end
        
        Store -->|4. Packages Graph Fragments| Bundle[Single FHIR Searchset Bundle]
        Bundle -->|5. Streams Payload Response| dev-app-sa
    end