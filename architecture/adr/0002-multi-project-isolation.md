# ADR 0002: Multi-Project Cryptographic Environment Isolation

## Context
We require absolute security isolation between development, staging, and production environments to comply with HIPAA and ONC §170.315(g)(10). 

## Decision
We reject single-project multi-tenancy. We implement a 4-project topology: a dedicated identity/edge gateway project, and three completely separate data projects (Dev, Stage, Prod). 

## Consequences
All infrastructure deployment must be driven by service-account-led Terraform pipelines. Developers are barred from direct access to Staging and Prod project scopes.
