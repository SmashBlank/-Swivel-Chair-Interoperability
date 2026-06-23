```markdown
# ADR 0001: Rejection of Google Flex Templates in Favor of Local Compilation & Java CLI Execution

## Status
Approved

## Context
Google deprecated and removed public Docker registries containing standard Dataflow orchestration nodes for health optimization mappings, leading to `data-harmonization:latest not found` errors and persistent initialization launcher VM timeouts.

## Decision
We completely abandoned pre-packaged cloud UI templates. We cloned the raw pipeline source code (`GoogleCloudPlatform/healthcare-data-harmonization-dataflow`), compiled the binary locally using Gradle 7.6 into a standalone FAT executable (`converter-0.1.0-all.jar`), and established direct Java CLI runner commands to execute our workloads.

## Consequences
We eliminated external background container version vulnerabilities, reduced pipeline boot times from 3 minutes to immediate execution, and gained absolute control over internal mapping dependency versions.