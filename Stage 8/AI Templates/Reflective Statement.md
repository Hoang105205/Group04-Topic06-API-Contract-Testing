# Reflective Statement

**Topic T06 — API & Contract Testing with Postman + Postbot | Group 04**

We used AI throughout this project, and the experience was mixed in useful ways.

AI helped us plan. It broke the seminar requirements into a task list, split the work across five people, and drafted the timeline. When Phương started with zero knowledge of Postbot, AI walked through the setup step by step, which shortened the learning curve from days to an afternoon. It drafted first versions of the User Guide sections, structured the slide outline, formatted the contribution tables, and turned our rough notes into clean markdown. For repetitive writing, like the troubleshooting section or the references list, it saved real time.

But AI could not do the parts that mattered most. It could not check anything against the running system. We worked with two APIs, the lecturer's EShop SUT and our own demo app, and AI freely mixed them up: it used a login body field that belonged to one system while we were testing the other, and quoted credentials that did not match the actual data file. We only caught these by reading the source code and sending real requests in Postman. AI also could not find the bugs in the SUT. Thuận's contract tests and Phương's manual discount formula found those, because someone had to know what the correct behavior should be before testing. And AI could not replace hands-on practice. Each of us still had to open Postman, run the requests, and watch the tests pass or fail ourselves.

We also learned to distrust the output. AI once duplicated an entire section while merging files, and twice we had to delete content it produced because it sounded right but was wrong.

The pattern was consistent: AI is a fast drafter and a patient tutor, but it has no contact with reality. Every fact it gave us needed checking against the actual system. The thinking, the testing, and the judgment stayed with us.
