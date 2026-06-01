![Symphony Parallax™](docs/assets/parallax-logo.png)

# Symphony Parallax™ Releases

Public documentation and release repository for Symphony Parallax™, the cloud-scale consensus orchestration product in the K Means AI Symphony™ suite.

Parallax extends the Symphony approach **from the console to the cloud**. Symphony Maestro™ gives operators a local, terminal-driven workflow for multi-model collaboration. Parallax brings that same family of ideas into durable cloud infrastructure: authenticated operation submission, tenant-aware status tracking, persona-driven model orchestration, traceable results, and scalable cloud processing.

Use Parallax when you need multiple independent model perspectives, an integrator-driven final answer, repeatable consensus workflows, and cloud operations that can be dispatched, tracked, retried, reviewed, and scaled.

Learn more at [kmeans.ai](https://kmeans.ai).

## License Notice

Symphony Parallax™ is proprietary software.

Copyright (C) K MEANS AI LLC. All rights reserved.

Release artifacts in this repository are provided for evaluation and use according to the terms supplied by K MEANS AI LLC. No source-code license is granted.

## What Parallax Does

Parallax coordinates multi-model reasoning operations in the cloud. A client submits an operation, Parallax routes the work through cloud-native infrastructure, model participants respond according to the selected execution mode, and an integrator model produces or confirms the final answer.

The system is designed for workloads where a single model response is not enough:

- high-stakes analysis and review
- policy or strategy exploration
- product and technical decision support
- research synthesis
- comparative reasoning
- validation of generated answers
- workflows that benefit from traceable multi-model perspective

## How It Fits With Symphony™

Symphony™ has two complementary paths:

- **Maestro™**: local, console-first multi-model collaboration.
- **Parallax™**: cloud-scale consensus orchestration.

Together, they express the same idea at different scales: local operator control when you want a hands-on console workflow, and distributed cloud execution when you need durable, scalable, trackable operations.

## Execution Modes

Parallax supports three execution modes.

### Sequential Bridge

`SequentialBridge` is the default Linguistic Bridge mode.

Reasoners are called sequentially inside each round. Each model can receive the shared context produced by earlier participants before contributing its own response. This mode is best when the task benefits from deliberation, critique, refinement, and conversational continuity.

Use it for deep consensus work.

### One-Shot Synthesis

`OneShotSynthesis` sends the same prompt to all active reasoners in parallel. The integrator receives the independent responses and synthesizes them into one complete answer.

This mode is faster than Sequential Bridge and is useful when breadth, coverage, or independent drafting matters more than multi-round deliberation.

Use it for fast synthesis.

### One-Shot Confirmation

`OneShotConfirmation` sends the same prompt to all active reasoners in parallel, then asks the integrator to compare their responses against a configured consensus threshold.

If the threshold is met, Parallax returns a final answer. If the threshold is not met, Parallax reports that confirmation failed instead of presenting the result as consensus.

Use it for fast validation.

## Personas And Model Catalogs

Parallax uses personas to define operational profiles for model collaboration. A persona determines the model ensemble and orchestration behavior used for an operation.

Administrators can manage available personas and the models associated with them. This allows a deployment team to enable, disable, or adjust model participation without changing the client workflow. Demo and operator experiences can read the current persona catalog from the API so submitted operations reflect the deployed configuration rather than stale hard-coded choices.

Model-provider credentials should be handled through secure secret references and customer-controlled secret stores. Public release artifacts do not include provider keys or deployment-specific credential material.

## Provider Strategy

Parallax is designed to support both native model providers and OpenAI-compatible endpoints.

Current provider lanes include:

- **OpenAI** and **OpenAI-compatible** chat endpoints through the official OpenAI SDK.
- **Anthropic** through the official Anthropic SDK and native Messages API.
- **Google Gemini** through the official Google GenAI SDK and native Gemini API.

Internally, Parallax keeps a provider-neutral message contract so orchestration state, traces, checkpoints, and worker protocols are not coupled to a single vendor SDK. Native adapters are used where they improve correctness and provider compatibility; OpenAI-compatible endpoints remain useful for platforms that intentionally expose that interface.

## Signaling And Traceability

Parallax captures lightweight participant signals during orchestration. Signals summarize whether model participants completed their work, aligned with the emerging result, or could not confidently proceed. This gives operators a compact view of how the consensus process evolved without requiring full access to raw model responses.

Signals appear in operation status and trace metadata alongside state, confidence, timing, and result information. They are especially useful for cloud operations because they help distinguish a successful consensus, a failed confirmation, and a prompt that could not be interpreted reliably.

Publicly, the important idea is simple: Parallax does more than collect several model answers. It tracks the state of the collaboration so the final result can be reviewed, audited, and operated at scale.

## Typical Cloud Flow

1. A client creates an operation with a tenant id, persona, prompt, interaction mode, and execution mode.
2. Parallax authenticates and validates the request.
3. Parallax dispatches the operation to cloud infrastructure.
4. A processor acquires the work and runs the selected orchestration mode.
5. Reasoner models produce independent or sequentially contextual responses.
6. An integrator model synthesizes or confirms the final answer.
7. The operation status is updated with state, confidence, timing, signals, and trace metadata.
8. Authorized clients read the final status using the operation receipt.

## Interfaces

Parallax is designed around three public-facing integration surfaces:

- **API**: authenticated operation submission, status retrieval, result retrieval, and safe catalog discovery.
- **Admin Console**: operator experience for runtime visibility, operational policy, persona and model management, and administrative controls.
- **Demo Suite**: customer-facing demonstration experience for authenticated prompt submission, persona selection, progress polling, and result review.

Authorization is enforced by the API. Client-side controls are treated as user-experience aids, not security boundaries.

## Azure-Native Deployment Posture

The current Parallax deployment path is Azure-native and Functions-first. Customer deployments are intended to use managed cloud services rather than application containers as the primary runtime.

The production architecture uses services such as:

- Azure Functions for API and background processing
- Azure Service Bus for operation dispatch
- Azure Cosmos DB for operation status and trace metadata
- Azure Blob Storage for larger trace and attachment payloads
- Azure Key Vault for provider-secret resolution
- Azure Static Web Apps for the Admin Console and Demo Suite
- managed identities and least-privilege access assignments

Parallax deployments are intended to be customer-isolated. Configuration, identity, network posture, retention policies, and model-provider choices are customer-specific production decisions.

## Configuration Principles

Production configuration should follow these principles:

- Store credentials outside source-controlled files.
- Prefer managed identity and secure secret stores for cloud deployments.
- Use logical secret references for model-provider credentials.
- Configure model participants deliberately: at least two reasoners and one integrator are required for consensus workflows.
- Keep prompt templates and operational policies under controlled release management because they shape orchestration behavior.
- Treat prompts, traces, uploaded files, generated outputs, and model responses as potentially sensitive application data.
- Use non-production environments to validate each provider, persona, execution mode, and policy configuration before scaling traffic.

## Security And Data Handling

Parallax may process prompts, uploaded files, model responses, trace metadata, and final outputs. These may contain sensitive information depending on how the system is configured and used.

Recommended controls:

- Use enterprise identity and role-based access controls.
- Avoid embedding provider keys in configuration files.
- Scope operation data by tenant and operation id.
- Restrict ordinary callers to their own submitted operations unless a privileged role explicitly grants broader access.
- Apply retention policies to status and trace data.
- Restrict access to storage, queues, status records, logs, and monitoring.
- Review model-provider data policies before sending sensitive material.
- Decide whether prompt and response trace content should be stored as hashes, raw text, or a customer-approved hybrid policy.

## Public Release Artifacts

When public Parallax release artifacts are published, they may include items such as:

```text
deployment templates
configuration samples
checksums
release notes
public documentation
```

Use the GitHub Releases page for versioned downloads when artifacts are available.

## Operational Notes

Parallax is intended for cloud-scale, multi-model consensus workflows. The right execution mode depends on the task:

- Choose `SequentialBridge` when the reasoning process itself matters.
- Choose `OneShotSynthesis` when speed and coverage matter.
- Choose `OneShotConfirmation` when independent validation matters.

For production use, validate each configured model provider, orchestration persona, and execution mode in a non-production environment before scaling traffic.

## Release Status

This repository is the public release and documentation home for Symphony Parallax™. Source code, private deployment runbooks, customer-specific configuration, credentials, and internal operational details are maintained separately. Public artifacts, release notes, and checksums will appear here as they become available.
