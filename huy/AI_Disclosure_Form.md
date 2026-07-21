# AI Disclosure Form

- Student name: Dương Gia Huy
- Student ID: 23127052

## 1. AI Tools Used
- Gemini 3.1 Pro

## 2. Purpose of AI Usage
- Requirement analysis and outlining: Understand the assigned tasks within the project (Sections 5.1a and 5.1b of the User_Guide.md document) and establish a professional document structure.
- Technical knowledge research: Investigate and comprehend API testing concepts in Postman, including Pre-request Scripts, Post-response Scripts, Variable Chaining techniques, Advanced Assertions, and Negative Testing.
- Code generation assistance: Generate sample JavaScript code snippets for Postman scripts applied directly to the EShop SUT business workflows.
- Translation and tone adjustment: Translate technical concepts into professional English adhering to the team's documentation standards, and refine the Markdown structure for coherence.

## 3. Statement of Commitment
I confirm that Artificial Intelligence (AI) was utilized as an assistive tool during the execution of this assignment. Despite the use of AI, I actively participated, contributed, and made critical adjustments. My specific contributions are categorized as follows:

- Independent Contributions (Manually authored without AI):
    - Authored manual instructions and captured screenshots to visually guide users on verifying extracted variables in the Postman Environment.
    - Documented actual SUT deviations by adding explicit notes on non-standard API behaviors and system bugs (e.g., returning 200 instead of 201 or 400).
    - Corrected outdated AI-generated syntax by replacing deprecated Postman commands with modern APIs (e.g., `pm.execution.setNextRequest`).
    - Manually authored missing Chai.js test scripts for complex edge cases (e.g., coupon validation, illegal order cancellation) that the AI omitted.
    - Fixed broken variable chaining flows by adding missing data extractions (e.g., `product_id`) and customizing Pre-request scripts for the domain.

- AI Guidance and Content Refinement:
    - Designed the document structure, directed the writing style to transition from general theoretical concepts to practical examples (EShop SUT), and requested the inclusion of navigation steps and a Postman command cheat sheet.
    - Proactively reviewed and fulfilled requirements by cross-referencing with the project outline, identifying missing mandatory elements, and directing the AI to supplement them (e.g., Nested JSON validation, invalid data types, the complete Variable Chaining flow, and linking the content to Failure Mode 6.2).
    - Updated logic to reflect actual APIs by detecting architectural changes in the API and instructing the AI to rewrite the entire Variable Chaining flow (expanding from 4 to 6 steps, replacing the `cart_id` logic with `sum_money` calculation, and integrating `shipping_address`).
    - Refined Negative Testing scenarios by guiding the AI to update error testing scripts (e.g., 403 Account Lockout after 3 failed attempts, 401 Unauthorized) to ensure 100% alignment with the `server.js` source code logic (EShop SUT).
    - Requested the AI to distinctly separate explanations (Response Time, Data Types, Array Length) and optimize instructional clarity of Test Results (Pass/Fail) for better reader comprehension.

I hereby pledge that the declarations above are true and take full responsibility in accordance with the university's regulations.

---

### **Student Signature:** Huy - Dương Gia Huy
### **Date:** 21/07/2026