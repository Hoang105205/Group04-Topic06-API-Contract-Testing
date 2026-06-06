# Tool Survey Proposal: T6 API Testing

## 1. Candidate Tools Classification

**Traditional Tool:** Postman (Industry-standard suite for functional and integration API testing) <br>
**AI-Augmented Tool:** Postbot (Postman's native AI assistant for automated test generation) <br>
**Backup Tool:** Pact (Consumer-driven contract testing framework)

## 2. Comparison Matrix

| Axis                   | Postman                                                                                      | Pact                                                                                                             | Postbot (Postman AI)                                                                                 |
| :--------------------- | :------------------------------------------------------------------------------------------- | :--------------------------------------------------------------------------------------------------------------- | :--------------------------------------------------------------------------------------------------- |
| **License Cost**       | **Free tier** (1 user). Paid: **$12-$59** per user/month for enterprise features.            | **Free/Open Source** (Framework). Hosted Pactflow scales from **$42+** per user/month.                           | **Included in Postman** via tokens. Free: 50 credits/mo. Paid: Higher quotas/add-ons.                |
| **Learning Curve**     | **Low.** Intuitive GUI; industry standard for API testing.                                   | **High.** Requires explicit coding (Java/JS/.NET) for consumer-driven contracts.                                 | **Very Low.** Uses natural language prompts; bypasses deep JavaScript/Chai.js knowledge.             |
| **Fit with eshop-sut** | **Excellent.** Ideal for E2E/Integration testing of E-commerce data flows (catalogs/orders). | **Poor fit.** `eshop-sut` is a monolith. Pact is for decoupled microservices; using it here is over-engineering. | **Excellent.** Rapidly generates validation suites for complex JSON payloads (e.g., cart checkouts). |
| **AI Capability**      | **Minimal.** Relies entirely on the Postbot add-on.                                          | **None.** Deterministic, explicit code assertions only.                                                          | **High.** Contextual test generation, JSON parsing, and auto-assertion drafting.                     |
| **Community**          | **Massive (35M+ users).** Vast ecosystem of workspaces and templates.                        | **Medium.** Niche, but the definitive standard for Contract Testing.                                             | **Growing.** Directly leverages Postman's massive user base.                                         |

## 3. Recommended Pick: Postman + Postbot

**1. Zero Learning Curve:** Eliminates complex framework syntax. The team leverages existing Postman familiarity combined with intuitive AI prompting.<br>
**2. Rapid Test Coverage:** Auto-generates assertions for deeply nested E-commerce JSON structures (carts/orders) in seconds, eliminating tedious manual validation.<br>
**3. No Tooling Fatigue:** Avoids the operational overhead of adopting new tools (e.g., Pact Broker setup). Keeps all testing within a single, unified ecosystem.
