# AI Disclosure Form

### 1. Confirmation of Draft Creation by AI

I acknowledge that the following parts of the seminar project were initially drafted, structured, or suggested by AI tools:

- **AI Tools Used:** Gemini 3.1 Pro.
- **Parts Drafted by AI:**
  - **User Guide (User_Guide.md) — Sections 5.1a & 5.1b:** Drafted initial structures, general explanations of API testing in Postman (Pre-request Scripts, Post-response Scripts, Variable Chaining techniques, Advanced Assertions, and Negative Testing).
  - **Code Generation Assistance:** Generated initial JavaScript test scripts and sample assertions applied to EShop SUT business workflows.
  - **Translation and Tone Adjustment:** Translated technical concepts into professional English and formatted Markdown layout.

---

### 2. Details of Review and Modifications

I actively reviewed, refined, and corrected the AI outputs before including them in the final deliverables:

- **Document Structure & Flow Design:** Directed the writing style to transition from general theoretical concepts to practical examples (EShop SUT), and requested the inclusion of navigation steps and a Postman command cheat sheet.
- **Outlining & Requirements Verification:** Proactively reviewed and fulfilled requirements by cross-referencing with the project outline, identifying missing mandatory elements, and directing the AI to supplement them (e.g., Nested JSON validation, invalid data types, the complete Variable Chaining flow, and linking the content to Failure Mode 6.2).
- **Code Logic Updates:** Detected architectural changes in the API and instructed the AI to rewrite the entire Variable Chaining flow (expanding from 4 to 6 steps, replacing the `cart_id` logic with `sum_money` calculation, and integrating `shipping_address`).
- **Negative Testing Scenarios Alignment:** Refined Negative Testing scenarios by guiding the AI to update error testing scripts (e.g., 403 Account Lockout after 3 failed attempts, 401 Unauthorized) to ensure 100% alignment with the `server.js` source code logic (EShop SUT).
- **Instructional Clarity:** Requested the AI to distinctly separate explanations (Response Time, Data Types, Array Length) and optimize instructional clarity of Test Results (Pass/Fail) for better reader comprehension.
- **Syntax Correction:** Corrected outdated AI-generated syntax by replacing deprecated Postman commands with modern APIs (e.g., `pm.execution.setNextRequest`).
- **Variable Chaining Fixes:** Fixed broken variable chaining flows by adding missing data extractions (e.g., `product_id`) and customizing Pre-request scripts for the domain.

---

### 3. Components Completed Entirely by Humans

The following sections were completed entirely through my own effort and judgment, without any AI assistance:

- **Manual Instruction & Visual Assets:** Authored manual instructions and captured screenshots to visually guide users on verifying extracted variables in the Postman Environment.
- **Documentation of SUT Deviations:** Documented actual SUT deviations by adding explicit notes on non-standard API behaviors and system bugs (e.g., returning 200 instead of 201 or 400).
- **Custom Edge Case Testing:** Manually authored missing Chai.js test scripts for complex edge cases (e.g., coupon validation, illegal order cancellation) that the AI omitted.

---

### 4. Commitments and Declarations

I take full responsibility before the Faculty and University Discipline Committee for the truthfulness, accuracy, and transparency of what I have declared in this form.

- **Student Name:** Dương Gia Huy
- **Student ID:** 23127052
- **Completion Date:** 21/07/2026