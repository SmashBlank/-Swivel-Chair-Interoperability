# ADR 0004: Apigee Edge Interception and Stateless Token Validation

## Context
We must expose the `dev-fhir-core-store` safely to public internet-facing applications without exposing the database to direct scanning, brute-force attacks, or unauthorized token execution.

## Decision
We deploy Apigee X as a reverse proxy in front of the private Cloud Healthcare API. We mandate the use of native `VerifyJWT` policies backed by an offline-cached JWKS configuration to decouple signature verification from database query routines.

## Consequences
All incoming client requests must route through Apigee. Direct access paths to the Healthcare API are completely severed at the VPC level via ingress firewalls and private service attachments.
