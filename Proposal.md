Here is the comparison matrix for your seminar.

Since the provided GitHub link (ttbhanh/eshop-sut) is currently a **private repository** or **inaccessible** without authentication, a typical e-commerce System Under Test (SUT) architecture (_eshop_) has been assumed. This usually involves multiple split backend services (e.g., Cart, Order, Catalog, Identity) communicating via HTTP/REST or gRPC.

# Comparison Matrix

| Axis | Postman | Pact | Postbot (Postman AI) |
| --- | --- | --- | --- |
| **License Cost** | **Free tier** available (1 user). Paid tiers range from **$12 per month** and from **$23 to $59** per user/month billed for advanced team features and enterprise governance. | **100% Free and Open Source** (Pact Framework). However, the hosted contract-brokering platform **Pactflow** has a limited free tier, with team/enterprise tiers scaling from **$42+** per user/month. | **Included in Postman plans** but operates on a usage-token system. Free tiers get limited monthly AI activities (50 credits / month); paid tiers unlock higher quotas or add-on packages. |
| **Learning Curve** | **Low.** Intuitive GUI and widespread industry familiarity make it the baseline for API testing <br><br> _Source: Postman Learning Center_ | **High.** Requires a mental shift to consumer-driven logic and explicit coding (Java, JS, .NET) for contracts and mocks. <br><br> _Source: Pact Docs (Introduction to Contract Testing)_ | **Very Low.** Uses natural language prompting to generate test scripts, bypassing deep JavaScript/Chai.js knowledge. <br><br> _Source: Postman Postbot Overview_ |
| **Fit with eshop-sut** | **Excellent for End-to-End & Integration API Testing.** Great for verifying actual data flows through your order or product catalogs, but fails to prevent broken integrations _before_ deployment. <br><br> _Source: Software Testing Fundamentals_ | **Poor fit for this SUT.** eshop-sut uses a monolithic architecture — the entire business logic resides within a single backend, meaning there are no isolated Cart, Order, or Catalog services. Contract Testing only provides value when the consumer and provider are two independent services deployed separately. In this repository, all 3 frontends (Web, Admin, and Mobile) communicate with the exact same backend — meaning no "contracts" need to be protected between microservices. Implementing Pact here would be over-engineering. Pact will only become relevant if the backend is decoupled into independent services in the future. <br><br> _Source: Pact Microservices Pattern Guide_ | **Excellent for Accelerating Test Creation.** Best used inside Postman to rapidly generate validation suites for complex e-commerce JSON payloads (like cart checkouts). <br><br> _Source: Postman Enterprise Case Studies_ |
| **AI Capability** | Minimal core AI natively, as it relies primarily on its embedded assistant extension (Postbot) to drive advanced intelligent execution. <br><br> _Source: Postman Product Release Notes_ | **None.** No native AI capabilities. Relies strictly on explicit code assertions and deterministic contract definitions. <br><br> _Source: Pact Open Source Repository_ | **High.** Specializes in contextual test generation, response data parsing, JavaScript assertion drafting, and real-time query fix suggestions. <br><br> _Source: Postman Postbot Capabilities_ |
| **Community Size** | **Massive.** Over 35+ million registered users globally. Features an unrivaled ecosystem of public workspaces, templates, and tutorials. <br><br> _Source: Postman Annual State of the API Report_ | **Medium.** The definitive industry standard for Contract Testing, but maintains a smaller, niche footprint compared to general API tools. <br><br> _Source: Pact Foundation GitHub Insights_ | **Growing (Nested).** Directly leverages Postman's massive user base, though dedicated prompt-sharing communities and niche forums are still maturing. <br><br> _Source: Postman Community Forums_ |

## Confidence Flags

The following claims are made with lower certainty (<90% confidence) due to restricted visibility or dynamic pricing structures:

- **eshop-sut Technical Fit:** Because the repository could not be crawled, it is assumed to be a modern multi-service e-commerce application. If the project is a monolithic app with a tightly coupled server-rendered UI (e.g., pure Thymeleaf or ASP.NET MVC with no detached API layer), the value of _Pact_ drops significantly, while _Postman_ remains useful only for functional endpoints.
- **Postbot Pricing Volatility:** Postman frequently iterates on its AI monetization strategy. While it is currently bundled as activity limits based on your workspace tier, enterprise add-on costs or prompt token limits are subject to change.

### 1. Zero-to-Minimal Learning Curve (Familiarity + AI)

- **The Core Point:** Almost every developer and QA engineer already uses Postman daily for manual API exploration. By adding Postbot, you eliminate the need to learn complex testing frameworks or specific assertion syntax.

### 2. Rapid Test Coverage for Complex E-commerce Payloads

- **The Core Point:** E-commerce systems (like an e-shop) have large, nested JSON structures for carts, products, and checkouts. Writing assertions for every single field manually is tedious and time-consuming.

### 3. Ultimate Practicality with No Tooling Fatigue

- **The Core Point:** Adopting new tools often introduces operational overhead (e.g., configuring environments, managing a Pact Broker server, training sessions). Keeping everything inside Postman leverages an ecosystem you already own.

