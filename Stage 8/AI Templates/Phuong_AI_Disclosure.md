# AI Disclosure Form

### 1. Confirmation of Draft Creation by AI

I confirm that the following components of the seminar project were initially drafted, structured, or suggested by Artificial Intelligence (AI) tools:

- **AI Tools Used:** Claude Sonnet 5.
- **Parts Drafted by AI:**
  - Initial draft text for User Guide Section 5.3 (Postbot) and Section 6 (Failure Modes), including the structure for describing each Failure Mode (description – example – consequence – prevention)
  - The comparison table template (Manual vs. Postbot) populated with results I provided
  - The script for the introduction video on Postbot
  - Step-by-step task guidance and daily planning based on the team's existing task plan
  - Wording/phrasing suggestions for summarizing test findings into report-ready language

---

### 2. Details of Review and Modifications

- All API endpoints, request payloads, and coupon data used in testing were verified against the actual EShop SUT repository specification, not assumed by the AI.
- Every claim in Section 5.3 and Section 6 about what Postbot "does well" or "misses" is based on test results I personally produced in Postman — the AI did not invent or estimate any test outcome; it only helped phrase and organize results I reported to it.
- I reviewed and corrected instances where the AI's initial guidance was based on generic assumptions rather than the real repository, and requested revisions once discrepancies were found (e.g., endpoint paths, sample payloads).
- The video script was adjusted based on the actual outcome of my "Fix Test" demo rather than left as AI-generated assumption.
- I edited the AI-drafted report sections for accuracy, removed placeholder content once real data was available.
---

### 3. Components Completed Entirely by Humans

I guarantee that the following parts of the project were completed 100% through my own intellectual efforts and execution, without any AI intervention:
- All hands-on testing in Postman: sending requests, configuring environments, logging in, applying coupons, and triggering error cases (invalid/expired/insufficient coupons, missing tokens, malformed payloads)
- Discovery of both real bugs in the system: the incorrect percentage discount calculation and the missing coupon usage-limit check
- All screenshots and evidence capture (response bodies, Test Results, Console logs)
- Manual test case writing (the "Manual" column in all comparison tables)
- Cross-review of teammates' sections (Huy's Test Scripts/Negative Testing, Quang's sections) and coordination on the Activity Worksheet
- Final judgment on which findings were significant enough to include in the report, and how to interpret their implications for API testing practice
---

### 4. Commitments and Declarations

I assume full responsibility before the Faculty and University Discipline Committee regarding the truthfulness, accuracy, and transparency of the information declared in this form.

- **Student Name:** Nguyen Binh Minh Phuong
- **Student ID:** 23127104
- **Completion Date:** 20/07/2026
