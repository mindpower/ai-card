# Common AI Catalog and Registry Standard

[![Specification](https://img.shields.io/badge/GitHub%20Pages-AI%20Catalog-222222?logo=githubpages&logoColor=white)](https://agent-card.github.io/ai-catalog/) [![Build](https://github.com/Agent-Card/ai-catalog/actions/workflows/publish-spec.yml/badge.svg?branch=main)](https://github.com/Agent-Card/ai-catalog/actions/workflows/publish-spec.yml)

## tl;dr

Members from various AI protocols (MCP, A2A, and others) are collaborating on a common AI Catalog standard for discovering heterogeneous AI artifacts across the ecosystem.

Contact us via GitHub Discussions, [Issues](https://github.com/Agent-Card/ai-catalog/issues), and [Pull Requests](https://github.com/Agent-Card/ai-catalog/pulls).

## Specification Site

The AI Catalog specification is built from `specification/ai-catalog.md` and published to GitHub Pages by the workflow in `.github/workflows/publish-spec.yml`.

Published site: [agent-card.github.io/ai-catalog](https://agent-card.github.io/ai-catalog/)

Pushes to the `main` branch update the canonical published site. Same-repo pull requests publish rendered preview pages under `https://agent-card.github.io/ai-catalog/pr/<number>/`, including a rendered diff preview against the PR base branch. The workflow also keeps a pull request comment updated with the live preview URL while the PR remains open.

GitHub Pages for the repository should be configured to serve from the `gh-pages` branch at the repository root.

To build the published HTML locally:

```bash
uv run --locked python tools/build_spec.py specification/ai-catalog.md dist/index.html --config specification/respec-config.json
```

This uses `uv` to resolve the Markdown dependency and writes the generated page to `dist/index.html`.

The build dependencies live in `pyproject.toml` and are pinned in `uv.lock`.

If you want to omit the explicit paths, the builder defaults to the spec source and `dist/index.html` output:

```bash
uv run --locked python tools/build_spec.py
```

If you change the spec build dependencies, refresh the lockfile with:

```bash
uv lock
```

The generated site entry point is `dist/index.html`.

Contribution guide: [CONTRIBUTING.md](CONTRIBUTING.md)

## **What problem are we solving?**

There are multiple evolving standards for communication protocols between AI clients and servers. While these protocols differ in capabilities and technical architecture, there are common ecosystem needs for producers and consumers of services built on top of them. This includes discovery, verifiable metadata, and trusted identity standards.

Without a common discovery standard, we see duplicative and incompatible efforts in the ecosystem, such as registry, marketplace, identity providers, UI and payments extensions, and others. This increases complexity, risk of vulnerabilities, and harms interop within the developer community.

## **What are we doing?**

In this repo, we are defining the **AI Catalog**: a typed, nestable JSON container for discovering heterogeneous AI artifacts. Each catalog entry identifies an artifact by media type and may either reference the native artifact metadata by URL or inline it directly. The specification also defines an optional Trust Manifest extension for identity, attestations, and provenance metadata.

*Important Note:* The **AI Catalog** standard does not replace or redefine protocol-specific artifact formats. It provides a common discovery and trust layer around them.

## **Terminology & Scope**

#### Core AI Catalog Features

The project defines a schema for a secure, extensible AI Catalog. This may include items such as:

* **Common catalog metadata**, such as publisher, documentation links, descriptions, icons, and versioning.
* **Verifiable metadata**, such as attestations, certifications, provenance, and trust signals.
* **Identity metadata**, such as Decentralized Identifiers (DID) or Secure Production Identity Framework For Everyone (SPIFFE).
* **Custom metadata** defined by individual producers or consumers for their own needs.

1. **AICard (Identity & Capabilities):** The metadata for a **single AI Service**. It binds a verifiable identity (DID) to its capabilities, security proofs, and protocol interfaces.  
   * *Analogy:* A **Digital Passport**. It belongs to one specific entity and travels with them.  
2. **AICatalog (Discovery):** A lightweight inventory of all AI Services available on a specific **Host**.  
   * *Analogy:* A **Building Directory**. It lists who is in the building today.

## **Guiding Requirements**

### **Core AI Card Features**

The project will define a schema for a secure, tamper-resistant AICard. This schema flattens identity for simplicity while reserving space for high-trust verification. Key components include:

* **Identity:** A globally unique, verifiable identifier (e.g., DID) for the AI Service.  
* **Publisher:** Identity information for the organization or individual that owns the service.  
* **Trust** & **Compliance:** A dedicated section for verifiable credentials (e.g., SOC 2 reports, HIPAA attestations, JWTs) and legal policies (Privacy, Terms of Service).  
* **Interfaces (Protocol Support):** A map of supported protocols (e.g., a2a, mcp). Each entry contains the protocol-specific payload required to connect.  
* **Lifecycle:** Metadata for versioning and maturity status (preview, stable, deprecated).

### **AI Protocol Services (The "Wrapper" Model)**

AI Catalog entries can reference or inline a wide range of AI artifacts, including MCP servers, A2A agent cards, Claude Code plugins, datasets, model cards, and nested catalogs.

Protocol-specific metadata remains defined by the underlying artifact specifications. The catalog does not duplicate or constrain those schemas; it uses media types to identify what each entry contains.

Nested AI Catalog entries allow publishers to organize large catalogs, delegate sub-catalogs, or package related artifacts together.

This architecture allows a single Identity to be interoperable across multiple ecosystems without modifying the core AI Card specification.

As part of the specification, this project defines a standard way to publish a catalog at a well-known URL for a given domain, for example at `./well-known/ai-catalog.json`.

The standard supports both decentralized and centralized discovery patterns:

Defining those protocol-specific dynamic behaviors is NOT in scope for the AI Catalog specification.

### **Common Registry Standard**

The AI Catalog project should also define a common **AI Catalog Registry** standard that provides a universal interface for clients to interact with catalog collections.

## **Proposed Mechanics**

### **Governance**

This is a temporary working repo maintained by the Linux Foundation. Proposals and changes will be made in public in this repo.

This project will be moved to a permanent location a later date with a permanent governance model.

### **Adoption**

When the specification is finalized, A2A and MCP steering committees will vote on adoption of the AI Catalog standard, potentially replacing existing protocol-specific standards or proposals.

Protocol-specific artifacts would remain in their native formats and become discoverable through common AI Catalog entries.

### **Ecosystem**

If adopted, MCP and A2A steering committees will recommend duplicative card-adjacent efforts be consolidated, such as Registry, Agent Identity, and others.