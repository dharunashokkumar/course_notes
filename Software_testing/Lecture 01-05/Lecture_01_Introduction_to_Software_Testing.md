# Lecture 01 — Introduction to Software Testing

**Course:** Software Testing (NPTEL)
**Instructor:** Prof. Rajib Mall, Dept. of CSE, IIT Kharagpur

---

## Topics Covered in This Lecture

1. Course Overview and Prerequisites
2. Errors, Faults, and Failures — Key Terminology
3. IEEE Standard 1044 (1993 vs 2010 revision)
4. Relationship: Error → Fault → Failure (with diagram)
5. Bug Statistics — How many bugs do programmers make?
6. Sources of Bugs — Specification/Design vs Code
7. Four Techniques to Reduce Bugs
8. Cost of Not Testing — Ariane 5 Rocket Example
9. Simplest Form of Testing
10. Test Report and Debugging
11. Testing Facts — Effort, Parallelism, Manpower
12. Evolution of Testing — From Monkey Testing to a Sophisticated Profession
13. When is Testing Carried Out? — Waterfall vs Iterative Models
14. Unified Process and Testing Activities
15. When to Stop Testing — Bug Seeding
16. Verification vs Validation

---

## Concept 1: Course Overview and Prerequisites

📌 **What is this course about?**

🧠 This course covers basic issues of program testing across 20 half-hour sessions. The course expects you to have some programming experience — meaning you should have written at least some programs before starting this course. That programming background will help you understand the testing concepts better.

🎯 **Exam Tip:** Remember — 20 sessions, and programming experience is a prerequisite.

---

## Concept 2: Errors Are Inevitable in Programming

📌 **Why do errors happen?**

🧠 When anybody writes a program — whether they are a beginner or a professional — mistakes are bound to happen. This is because writing software (specification, design, and code) is a **manual activity**, and manual activities are **inherently error-prone**.

When there are errors (also called faults, defects, or bugs) in the code, the program **might fail** during testing. For example, you might see a program crash or observe wrong results.

However — and this is very important — **even if there are bugs in a program, it may not fail during testing.** Why? There are several reasons:

- The test case may not have exercised that particular buggy part of the code.
- The bug might not easily cause a visible failure of the program.

🎯 **Exam Tip:** Just because a program passes testing does NOT mean it is bug-free. A program can have bugs and still not fail during testing.

---

## Concept 3: Terminology — Error, Fault, and Failure (IEEE Standard 1044)

📌 **How are Error, Fault, and Failure defined?**

🧠 These terms were defined in the **IEEE Standard Document 1044**.

**In the 1993 version** of this document, the terms errors, faults, and bugs were all treated as **synonyms** — they all meant the same thing.

**In the 2010 revision**, the IEEE standard decided that having so many terms (error, bugs, defects, faults) all meaning the same thing was not useful. So they gave **finer, distinct meanings** to these terms.

Here is what the 2010 revision says:

| Term | Synonyms | Meaning |
|---|---|---|
| **Error** (or **Mistake**) | Error = Mistake | A wrong action committed by the **programmer** during specification, design, or coding. It is a human action. |
| **Fault** (or **Defect** or **Bug**) | Fault = Defect = Bug | A problem that gets **introduced into the software product** (specification, design, or code) because of the error. |
| **Failure** | — | The **observable incorrect behavior** of the program when it runs. It is what the user sees (crash, wrong output, etc.). |

🛠 **Think of it like a chain:**

**Programmer commits an Error → Error may (or may not) cause a Fault in the product → Fault may (or may not) cause a Failure when the program runs.**

📊 **Step-by-step breakdown of the chain:**

**Step 1:** A programmer is developing software. During this process (specification, design, or coding), they may commit an **error or mistake**.

**Step 2:** But not every error actually results in a fault in the program. Sometimes the error is harmless.

**Step 3:** Even if a fault exists, not every fault causes a failure. Maybe the faulty code was never executed by the test cases, or maybe the fault still produces an acceptable result.

🛠 **Example from the transcript:**

Suppose a programmer intended to write `i = j`, but by mistake wrote `i = 2`. However, at that point in the program, the variable `j` always has the value `2` (because `j` was initialized to `2`). So the program still works correctly! The programmer committed an **error**, but it did **not** cause a **fault** in the behavior — the program runs satisfactorily.

⚠️ **Common Confusions:**

- Students often think every error leads to a failure. That is WRONG. The chain can break at any point: Not every error causes a fault, and not every fault causes a failure.
- Error/Mistake is what the **human** does. Fault/Defect/Bug is what exists in the **product**. Failure is what is **observed** during execution.

🎯 **Exam Important Points:**
- Error = Mistake (synonyms) — committed by the programmer
- Fault = Defect = Bug (synonyms) — present in the product
- Failure = observable wrong behavior during execution
- Not every error leads to a fault
- Not every fault leads to a failure
- IEEE Standard 1044, revised in 2010, made these distinctions

---

## Concept 4: Bug Statistics

📌 **How many bugs do programmers make?**

🧠 Research shows that even very experienced, fine programmers make about **50 bugs per 1000 lines of code** when they initially write the code. This is a significant number.

However, users don't usually experience all these bugs because companies **test the software extensively** before releasing it.

After extensive testing, the number of bugs that still remain is about **1 bug per 1000 lines of source code**. So testing removes most bugs, but some still slip through.

📊 **Summary:**
- Before testing: approximately **50 bugs per 1000 lines of code**
- After extensive testing: approximately **1 bug per 1000 lines of code**

🎯 **Exam Tip:** Memorize these two numbers — 50/1000 (before testing) and 1/1000 (after testing). These are commonly asked.

---

## Concept 5: Sources of Bugs

📌 **Where do the bugs come from?**

🧠 According to the transcript, on average:

- About **60% of bugs** come from **specification and design** activities.
- About **40% of bugs** come from **coding** activities.

This means the majority of bugs are NOT coding mistakes — they come from wrong or incomplete requirements and flawed design.

🎯 **Exam Tip:** Remember 60-40 split. More bugs come from specification/design than from code. This is a frequently tested fact.

---

## Concept 6: Four Techniques to Reduce Bugs

📌 **How do companies reduce bugs before releasing software?**

🧠 No company wants to release software with lots of bugs because customers will reject it. There are **4 primary techniques** that companies use to reduce bugs:

**Technique 1: Review**
Faults in the program code, specification, and design are caught during **review meetings**. People go through the documents and code together to find problems.

**Technique 2: Testing**
Testing is a very important means of reducing bugs. You run the program with various inputs and check if the output matches expectations.

**Technique 3: Formal Specification and Verification**
Using mathematical/formal methods to specify and verify the software.

**Technique 4: Using a Proper Development Process**
Systematically developing software with appropriate methodology can reduce the chances of bugs in the code.

🎯 **Exam Tip:** Remember all 4 techniques — Review, Testing, Formal Specification & Verification, and Proper Development Process.

---

## Concept 7: Cost of Not Testing — The Ariane 5 Rocket Disaster

📌 **What happens if you don't test adequately?**

🧠 The lecture gives a famous real-world example to show why testing is critical.

The **Ariane 5 rocket** was launched in the early 2000s, and it **self-destructed just 37 seconds after launch**. The mission failed completely.

**What caused this?**
There was an **undetected bug** in the code. The bug was caused by the **reuse of code from an earlier, older machine**. The older machine had a less powerful processor. When this code was used on the newer machine (which had a 64-bit processor), the numbers generated were larger than what the old code could handle, causing an **overflow**.

The bug was not caught because **just before launch, that part of the code was not tested**, and the **exception handler for this overflow was disabled**.

**Total cost of this bug:** Over **1 billion dollars**.

🎯 **Exam Tip:** Ariane 5 — self-destructed 37 seconds after launch — bug due to code reuse — 64-bit overflow — cost over $1 billion. This is a classic example asked in exams.

---

## Concept 8: Simplest Form of Testing

📌 **How does basic testing work?**

🧠 At the simplest level, testing works like this:

1. The programmer **inputs some values** (data) into the program.
2. The programmer **observes the output**.
3. The programmer **checks** whether the output matches the expected result.
4. If the output does not match expectations, the programmer concludes that the **program has failed**.
5. The programmer keeps inputting different values and repeating this process.

This is the most basic, fundamental idea of testing — give input, observe output, check correctness.

🎯 **Exam Tip:** Understand this basic concept — testing is fundamentally about comparing actual output with expected output.

---

## Concept 9: Test Report and Debugging

📌 **What happens after a failure is found?**

🧠 When a tester finds that the program has failed, they prepare a **test report**. The test report mentions:

- Under what **conditions** the failure occurred
- What **input values** were given
- What **output/result** was observed

These test reports (for all bugs found) are then given to the **development team** for **debugging** — that is, locating the source of the error and then correcting the code (or the specification or design, as needed).

📊 **Flow:**
Testing finds failure → Tester prepares test report → Development team does debugging (locate error + fix it)

🎯 **Exam Tip:** Debugging is NOT the same as testing. Testing finds failures; debugging locates and fixes the errors that caused those failures.

---

## Concept 10: Testing Facts — Effort, Manpower, and Parallelism

📌 **How much effort does testing take?**

🧠 This section has several important facts from the transcript:

**Fact 1:** Among all software development activities, **testing takes the largest effort**. Companies spend at least **50% of their total effort on testing**. The other 50% is spent on specifying, designing, coding, etc.

**Fact 2:** Because testing requires so much effort, companies need a **large number of testers**. If you walk into any software company and randomly ask someone what they do, there is a **50% chance** they will say they are testing a program. This means testing has the **maximum job opportunities** among all software development roles.

**Fact 3:** Even though 50% of the effort goes to testing, this 50% effort is completed in only about **10% of the total development time**. The other 90% of development time is used for specification, design, coding, etc.

**How is this possible?** Because **testing has the maximum parallelism**. Many testers can work **concurrently** — different testers test different parts of the software (different units, integration aspects, system testing aspects) at the same time. In contrast, activities like specification and design are more **sequential** and fewer people can work on them concurrently.

📊 **Summary Table:**

| Aspect | Value |
|---|---|
| Testing effort share | At least 50% of total effort |
| Time share for testing | About 10% of total development time |
| Why this is possible | Maximum parallelism in testing |
| Manpower in testing | Largest among all roles |

🎯 **Exam Tip:** 50% effort, 10% time, maximum parallelism — these three facts go together. Understand why.

---

## Concept 11: Evolution of Testing — From Monkey Testing to a Sophisticated Profession

📌 **How has testing evolved over the years?**

🧠 Testing has become more complex and sophisticated over the years. The reasons for this are:

1. **Programs themselves have become large and complex.**
2. **New programming paradigms** have emerged — for example, web-based software, embedded software, software running on mobile phones, etc.
3. **Many tools** have been developed for testing.
4. **Newer testing techniques** have been introduced.

**What is Monkey Testing?**
In the early days of computing (50–60 years back), programmers would just **input random values** into the program and try to crash it or see if it produces wrong results. This is called **monkey testing**. It has very little intellectual content — just random input.

Because of this history, testing used to be considered **not very attractive** as a career. But that stigma is outdated now.

**The current reality:**
Testing is now one of the **most challenging jobs** in any company. It has taken a **centre stage** in all types of software development. Testers now need to have good knowledge of different testing techniques and also develop proficiency with automated tools.

🎯 **Exam Tip:** Know what monkey testing means (random input). Know that testing has evolved into a sophisticated, challenging profession.

---

## Concept 12: When Is Testing Carried Out? — Waterfall vs Iterative Models

📌 **At what point in the development lifecycle does testing happen?**

🧠 This depends on the development model being used:

**In the Waterfall Model:**
Testing occurs **towards the end** of the development cycle. During the coding phase, **unit testing** is done. After that, **integration testing** and **system testing** are carried out during the testing phase. In waterfall, testers are needed only towards the end, so they are mostly idle during the early parts.

**In Iterative/Modern Models (Unified Process, Agile, V-Model):**
Testing is **spread all over the development cycle**. Even the older V-model has testing spread across the entire lifecycle. This means testers are busy throughout the project, and bugs are exposed earlier.

**Unified Process Example:**
The Unified Process has **4 phases**: Inception, Elaboration, Construction, and Transition. The testing effort is present **all through** these phases. Testers continuously:

- Define and conduct **unit testing**
- Define and conduct **integration testing**
- Define and conduct **usability testing**
- Define and conduct **user acceptance testing**

**Key Insight about Iterative Development:**
In any iterative development process, every iteration is like a **small waterfall** — specification, design, coding, and testing all happen in each iteration. So testing is present throughout.

🎯 **Exam Tip:**
- Waterfall → testing only at the end
- Iterative (Unified Process, Agile, V-Model) → testing throughout the lifecycle
- Unified Process has 4 phases: Inception, Elaboration, Construction, Transition

---

## Concept 13: When to Stop Testing — Bug Seeding

📌 **How do you know when to stop testing?**

🧠 As you test more and more, you find more bugs. But over time, the rate of finding new bugs **decreases**. So when do you decide to stop?

**Approach 1:** If no new bugs are found in a day or two of testing, it may be time to stop (though this depends on the specific application).

**Approach 2 — Bug Seeding:**
The program manager (or leader) **intentionally inserts (seeds) bugs** into the program code **without the testers knowing**. Then the testers test the software normally and report failures. After testing, the program manager checks:

- If **all (or most) of the seeded bugs** have been detected by the testers, then it is reasonable to conclude that the **real bugs** that were originally in the code have also **most likely been detected**.
- This means it is now time to stop testing.

So, seeding bugs and checking how many are found gives an **indication of how thoroughly the software has been tested**.

🛠 **Simple analogy:** Imagine you drop 10 marked coins into a swimming pool and ask divers to find coins. If they find 9 out of 10 marked coins, you can be fairly confident that they also found most of the other coins that were already in the pool.

🎯 **Exam Tip:** Bug seeding is used to determine **when to stop testing**. The manager seeds known bugs and checks what fraction is found to estimate testing thoroughness.

---

## Concept 14: Verification vs Validation

📌 **What is the difference between Verification and Validation?**

🧠 This is a very important and frequently tested concept.

**Verification:**
Verification is checking whether the **output of one development phase** conforms to (matches) the **output of the previous phase**. In simple words — are we **developing the product correctly** at each step?

Verification is concerned with **phase containment of errors** — detecting and eliminating errors as the development proceeds from one phase to the next.

**Validation:**
Validation is the process of checking whether the **fully developed, working software** conforms to the **requirement document**. In simple words — have we **developed the correct product**? Is the final product what the customer actually wanted?

📊 **The famous one-liner distinction:**

| | Question it answers |
|---|---|
| **Verification** | "Are we developing it **right**?" (Is the process correct?) |
| **Validation** | "Have we developed the **right** product?" (Is the final product correct?) |

**Verification Techniques:**
Review, Simulation, Unit Testing, Integration Testing

**Validation Technique:**
System Testing

⚠️ **Important Clarification from the Transcript:**
It may seem surprising that **Unit Testing and Integration Testing are Verification techniques** (not Validation). The reason is:

- **Unit Testing** checks whether a unit (function or module) conforms to **its design** — so it is checking one phase output against the previous phase. That is verification.
- **Integration Testing** similarly checks how integrated units work together against the design.
- **System Testing** checks the entire system against the **requirements** — that is validation.

🎯 **Exam Tip (Very Important):**
- Unit Testing = Verification technique
- Integration Testing = Verification technique
- System Testing = Validation technique
- Verification = "developing it right" (process-oriented)
- Validation = "developed the right product" (product-oriented)

---

## Summary of Key Terms from Lecture 1

| Term | Meaning |
|---|---|
| Error / Mistake | Wrong action by programmer (human activity) |
| Fault / Defect / Bug | Problem in the software product due to error |
| Failure | Observable wrong behavior during execution |
| Monkey Testing | Inputting random values to find crashes or wrong results |
| Bug Seeding | Manager plants known bugs to measure testing thoroughness |
| Verification | Checking phase output against previous phase ("developing it right") |
| Validation | Checking final product against requirements ("right product") |
| Test Report | Document recording failure conditions, inputs, and observed results |
| Debugging | Locating and fixing the source of errors after failures are found |

---

## 10 MCQs — Strictly from Lecture 1

---

**Q1.** According to the IEEE Standard 1044 (2010 revision), which of the following are synonyms?

(A) Error and Fault
(B) Error and Mistake
(C) Fault and Failure
(D) Error and Failure

**Answer: (B)**
**Explanation:** The 2010 revision of IEEE 1044 distinguishes between Error/Mistake (what the programmer does), Fault/Defect/Bug (what exists in the product), and Failure (what is observed). Error and Mistake are synonyms. Error and Fault are NOT synonyms in the revised standard.

---

**Q2.** Approximately what percentage of bugs originate from specification and design (as opposed to coding)?

(A) 40%
(B) 50%
(C) 60%
(D) 80%

**Answer: (C)**
**Explanation:** The transcript states that about 60% of bugs come from specification and design, and about 40% come from coding.

---

**Q3.** What was the primary cause of the Ariane 5 rocket failure?

(A) Hardware malfunction
(B) Reuse of code from an older machine causing an overflow
(C) Incorrect fuel calculations
(D) Network communication failure

**Answer: (B)**
**Explanation:** The Ariane 5 failure was caused by reuse of code from an older machine with a less powerful processor. When run on the newer 64-bit processor, it caused an overflow. The exception handler was disabled, and the code was not tested before launch.

---

**Q4.** After extensive testing, approximately how many bugs per 1000 lines of code remain?

(A) 50
(B) 10
(C) 5
(D) 1

**Answer: (D)**
**Explanation:** The transcript states that initially there are about 50 bugs per 1000 lines of code. After extensive testing, about 1 bug per 1000 lines of source code still remains.

---

**Q5.** Which of the following is a VALIDATION technique?

(A) Unit Testing
(B) Integration Testing
(C) System Testing
(D) Review

**Answer: (C)**
**Explanation:** The transcript clearly states that system testing is a validation technique because it checks whether the final working software conforms to the requirements. Unit testing and integration testing are verification techniques, and review is also a verification technique.

---

**Q6.** What is "monkey testing"?

(A) Testing performed by untrained users
(B) Testing by inputting random values and seeing if the software crashes or gives wrong results
(C) Testing using automated tools that simulate monkey-like behavior
(D) A formal testing technique used in agile development

**Answer: (B)**
**Explanation:** The transcript describes monkey testing as the old practice where programmers would just input random values into the program and try to crash it or see if it produces wrong results. It has very little intellectual content.

---

**Q7.** In the Waterfall model, when is testing primarily carried out?

(A) At the beginning of the development cycle
(B) Throughout the development cycle
(C) Towards the end of the development cycle
(D) Only during the design phase

**Answer: (C)**
**Explanation:** In the waterfall model, testing occurs towards the end of the development cycle — unit testing during the coding phase, and integration and system testing during the testing phase. In contrast, iterative models like the Unified Process have testing spread throughout.

---

**Q8.** What is the purpose of "bug seeding"?

(A) To introduce new features into the software
(B) To train testers on finding bugs
(C) To determine when to stop testing by checking how many seeded bugs are found
(D) To increase the number of bugs for testing practice

**Answer: (C)**
**Explanation:** The program manager seeds (inserts) known bugs into the code without the testers' knowledge. After testing, if most seeded bugs are found, it indicates that the real bugs have also likely been found, and it is time to stop testing.

---

**Q9.** What percentage of total development effort is typically spent on testing?

(A) About 10%
(B) About 25%
(C) At least 50%
(D) About 90%

**Answer: (C)**
**Explanation:** The transcript states that all companies spend at least 50% of their effort on testing and the other 50% on specifying, designing, coding, etc. This 50% effort is completed in about 10% of the development time due to maximum parallelism.

---

**Q10.** Which of the following statements is TRUE about Verification?

(A) It checks whether the final product meets user requirements
(B) It checks whether the output of one phase conforms to its previous phase
(C) It is the same as Validation
(D) System testing is a verification technique

**Answer: (B)**
**Explanation:** Verification checks whether the work product of one phase conforms to the work product of the previous phase — it is about "developing it right." Validation checks the final product against requirements ("right product"). System testing is a validation technique, not verification. Unit testing and integration testing are verification techniques.

---

*End of Lecture 1 — Complete Notes*
