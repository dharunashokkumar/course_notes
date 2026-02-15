# Lecture 4 — Basic Concepts of Testing (Contd.)

**Course:** NPTEL Software Testing | **Instructor:** Prof. Rajib Mall, IIT Kharagpur

---

## Topics Covered in This Lecture

1. Test Cases and What They Do
2. Test Data vs Test Cases
3. Test Case as a Triplet [I, S, O]
4. Negative Test Cases vs Positive Test Cases
5. Test Suite
6. Test Execution Example (Library Software)
7. Test Case Recording Format
8. Test Team — Human Resources
9. Why Design Test Cases? (Random vs Designed)
10. Example — Finding Maximum Bug
11. Test Plan
12. Testing Strategy
13. Usage-Based Testing
14. Past Bug Detection Data for Planning
15. Defect Clustering
16. Introduction to Unit Testing
17. Why Unit Testing?
18. Driver and Stub

---

## Concept 1: Test Cases and What They Do

📌 **Concept Name:** Test Cases

🧠 **Simple Explanation:**
A test case is something we design to check whether a particular functionality of the software is working correctly. When we run a test case, it executes the software and in doing so, it **covers** (or touches) some **program elements**. These program elements can be statements, conditionals (like if-else), jumps, etc.

We then measure how many of those elements got covered. This idea of checking whether required elements are covered is called **coverage-based testing**.

There is also another type called **fault-based testing** — here, the goal is not about coverage. Instead, we try to find out whether certain specific types of bugs exist in the software and have been removed.

🎯 **Exam Important Points:**
- Each test case executes some functionality and covers some program elements.
- Program elements = statements, conditionals, jumps, etc.
- Coverage-based testing = ensuring targeted elements are covered.
- Fault-based testing = exposing whether specific bug types exist.

⚠️ **Common Confusion:** Coverage-based testing is about "how much code did we touch." Fault-based testing is about "did we catch this specific bug type." Don't mix them up.

---

## Concept 2: Test Data vs Test Cases

📌 **Concept Name:** Test Data vs Test Cases

🧠 **Simple Explanation:**
These two sound similar but are different things.

**Test Data** = Only the inputs that we give to the software for testing. For example, typing the number "25" in an input box. That "25" is the test data.

**Test Case** = A complete package that includes three things:
1. The **test data** (input)
2. The **state of the software** at the time of giving input (for example: you are logged in, you have selected a particular menu option)
3. The **expected output** (what result you predict the software should give)

So, test data is just the input part. Test case is much bigger — it tells you the full picture: what state, what input, what output to expect.

🛠 **Small Example:**
Suppose you are testing a banking app. Test data might be "withdraw 500 rupees." But the test case would say: "User is logged in, account has 1000 rupees balance (state), user requests withdrawal of 500 (input), expected output: balance becomes 500 and success message is shown."

🎯 **Exam Important Points:**
- Test data = just the inputs used to test.
- Test case = inputs + state of software + expected output.
- A test case is more complete than just test data.

---

## Concept 3: Test Case as a Triplet [I, S, O]

📌 **Concept Name:** Test Case Triplet

🧠 **Simple Explanation:**
At the minimum, a test case can be described as a **triplet** with three parts:

- **I** = Input data (the test data we give to the software)
- **S** = State of the software at which the input is applied (for example, the user has logged in, a book has been issued, etc.)
- **O** = Expected Output (what result we expect from the software)

This is the simplest way to define any test case.

🛠 **Small Example:**
For a calculator app:
- I = "5 + 3"
- S = Calculator is open and in ready state
- O = "8"

🎯 **Exam Important Points:**
- Test case = [I, S, O] — this is a very important definition.
- I = input data, S = state, O = expected output.
- This is the **minimum** representation of a test case.

---

## Concept 4: Negative Test Cases vs Positive Test Cases

📌 **Concept Name:** Positive and Negative Test Cases

🧠 **Simple Explanation:**

**Positive Test Cases:** These check that the software works correctly when we give **valid** inputs. For example, entering a proper number in a number field. We expect the software to work fine.

**Negative Test Cases:** These check that the software **does not crash** and **behaves gracefully** when we give **invalid or unexpected** inputs. For example, typing a letter "abc" in a field where a number is expected. The software should not crash. Instead, it should show a proper error message like "Incorrect data type, please enter a number."

🛠 **Small Example from transcript:**
If a user types a character (like "a", "b", "c") where a number (like 23) was expected, the software should not crash. It should display a proper message. This is what a negative test case checks.

🎯 **Exam Important Points:**
- Positive test case = valid input → check correct working.
- Negative test case = invalid/unexpected input → check graceful handling (no crash, proper error message).
- Both types are essential in good testing.

⚠️ **Common Confusion:** Negative testing does NOT mean testing for "bad software." It means testing with "bad/invalid inputs" to see if software handles them well.

---

## Concept 5: Test Suite

📌 **Concept Name:** Test Suite

🧠 **Simple Explanation:**
When we design test cases for a software, we don't design just one test case. We design many test cases — sometimes hundreds or thousands. The **complete set of all test cases** that we design for testing a software is called the **Test Suite**.

These test cases are designed based on some **test strategy** (like black box strategies, white box strategies, etc.).

🎯 **Exam Important Points:**
- Test Suite = the set of ALL test cases designed for testing a software.
- Test cases are designed based on test strategies.

---

## Concept 6: Test Execution Example — Library Software

📌 **Concept Name:** Test Execution Example (Return Book)

🧠 **Simple Explanation:**
The transcript gives an example of a library management software to explain how a test case is written in practice.

**Scenario:** Testing the "Return Book" (or "Renew Book") feature.

The test case [I, S, O]:
- **S (State):** A book record has been created, a member record has been created, and the book has been issued to the member. This is the state before we start.
- **I (Input):** Select the "renew book" option and request renewal for a 2-week period.
- **O (Output):** Observe whether the book has been renewed or not.

This example makes it very clear how a test case works in real life — first set up the state, then give input, then check output.

🎯 **Exam Important Points:**
- Before testing, the software must be brought to a specific state.
- The input is applied on that state.
- Output is compared with expected result.

---

## Concept 7: Test Case Recording Format

📌 **Concept Name:** Standard Test Case Documentation Format

🧠 **Simple Explanation:**
While the triplet [I, S, O] is the minimum, in professional testing, we use a more detailed format to record each test case. This format includes:

- **Test Case Number** — a unique ID
- **Test Case Author** — who designed it
- **Test Purpose** — what this test case is trying to check
- **Pre-condition** — the state of software before testing (same as S in triplet)
- **Test Inputs** — what data we give (same as I)
- **Expected Outputs** — what we expect to happen (same as O)
- **Post-condition** — what should be the software state after test completes
- **Test Execution Date** — when was it executed
- **Test Execution Person** — who ran the test
- **Test Result** — Pass or Fail
- **If Failed: Failure Information** — what went wrong
- **Fix Status** — after developer fixes it, what is the status

🎯 **Exam Important Points:**
- The full format has many more fields than just [I, S, O].
- Post-condition is the state AFTER test completes.
- Fix status is updated by the developer after fixing the bug.
- This format helps in tracking, repeatability, and documentation.

---

## Concept 8: Test Team — Human Resources

📌 **Concept Name:** Test Team Roles

🧠 **Simple Explanation:**
Different testing activities need people with different levels of experience:

- **Test Planning** → Needs **experienced** people (they decide what to test, how much to test)
- **Test Scenario and Test Case Design** → Needs **experienced and qualified** people (they write the detailed test cases)
- **Test Execution** → Can be done by **semi-experienced to inexperienced** people (they follow the written test cases, give inputs, and observe results)
- **Test Result Analysis** → Needs **experienced** people (they analyze whether a failure occurred and what it means)
- **Test Tool Support** → Needs **experienced** people (they manage and support test automation tools)
- **External People** → Sometimes **users** (for usability testing) and **industry experts** may also be involved.

🎯 **Exam Important Points:**
- Test execution can be done by less experienced people.
- Test planning, design, result analysis, and tool support need experienced people.
- Users and industry experts may also participate.
- Test scenario = high-level description; test case = actual detailed case with all fields.

⚠️ **Common Confusion:** Test scenario and test case are NOT the same. A test scenario is a high-level description (like "test the login feature"). A test case is the detailed triplet with exact inputs, state, and expected output.

---

## Concept 9: Why Design Test Cases? (Random vs Designed)

📌 **Concept Name:** Why Design Test Cases?

🧠 **Simple Explanation:**
A very important question: Why can't we just give random inputs to the software — say 10,000 random values — instead of carefully designing test cases?

The answer: **Random testing is not effective.** Even if you test with 10,000 random inputs, you might not find many bugs. The reason is that many of those random inputs might be testing the **same type of bug** or covering the **same program elements** over and over again.

So, just saying "we tested with 10,000 test cases" does NOT mean the testing was thorough or effective. The **number of test cases does not indicate the effectiveness** of testing.

We need **systematic test case design** where each test case targets a **different type of fault** or covers **different program elements**.

🛠 **Example from transcript (Finding Maximum of two numbers):**

Consider this simple program:
```
if (x > y) max = x;
else max = x;   // BUG! Should be max = y
```

There is a bug: in the else part, it should be `max = y`, but the programmer wrote `max = x`.

Now look at two test suites:

**Test Suite A (Randomly chosen, all with x > y):**
(x=3, y=2), (x=4, y=3), (x=5, y=1) — Even hundreds of such test cases will **NOT detect the bug** because the else branch is never executed.

**Test Suite B (Carefully designed):**
(x=3, y=2), (x=2, y=3) — Just **2 test cases** will detect the bug because the second test case where y > x will execute the else branch and expose the bug.

🎯 **Exam Important Points:**
- Random testing is ineffective — many test cases may cover the same elements.
- Number of test cases ≠ effectiveness of testing.
- Systematic design ensures each test case targets different faults.
- A smaller, well-designed test suite can be more effective than a large random one.
- The max-of-two-numbers example is very exam-important.

---

## Concept 10: Test Plan

📌 **Concept Name:** Test Plan

🧠 **Simple Explanation:**
Before any testing starts, a document called the **Test Plan** is prepared. It is a planning document that tells everything about how testing will be conducted.

A test plan typically contains:

- **Features to be tested** — which functionalities will be tested
- **Features NOT to be tested** — some features may not be ready or not planned for this release, so they are excluded
- **Test Strategy** — which testing techniques will be used (equivalence testing, boundary value testing, condition testing, etc.)
- **Test Suspension/Stopping Criteria** — when to stop testing. For example, if core features don't work, stop testing and send it back to developers.
- **Test Effort** — estimate of how much effort (time, people) is needed
- **Test Schedule** — how many days testing is expected to take

🎯 **Exam Important Points:**
- Test plan is created BEFORE testing starts.
- It documents: features to test, features not to test, strategy, stopping criteria, effort, and schedule.
- Stopping criteria = if core functionality fails, stop and send back.

---

## Concept 11: Testing Strategy

📌 **Concept Name:** Testing Strategy

🧠 **Simple Explanation:**
A testing strategy decides:

1. **Which types of tests to use?** (equivalence partitioning, boundary value, condition testing, etc.)
2. **How much effort to give to each type?**

Not all strategies get equal time. If a strategy is more effective at finding bugs, we give it more time. If another is less effective, we give it less time.

🎯 **Exam Important Points:**
- Testing strategy = which tests + how much effort for each.
- Each strategy acts like a "bug filter" — catches specific types of bugs.
- We need multiple strategies because no single strategy catches all bugs.

---

## Concept 12: Usage-Based Testing

📌 **Concept Name:** Usage-Based Testing

🧠 **Simple Explanation:**
In **black-box testing**, one approach is **usage-based testing**. The idea is simple: test the features that customers use **most frequently** with **more effort**.

🛠 **Example from transcript:**
In a library software:
- **Book Issue** and **Book Return** are used very heavily by users every day.
- **Book Lost Report** is used very rarely.

So, if there are even small bugs in Book Issue or Book Return, users will notice them immediately. But small bugs in Book Lost Report may go unnoticed for a long time.

Therefore, it makes sense to spend **more testing effort on heavily used features** and less on rarely used ones.

🎯 **Exam Important Points:**
- Usage-based testing = test effort proportional to how frequently a feature is used.
- Heavily used features → more testing effort.
- Rarely used features → less testing effort.
- This is a black-box testing consideration.

---

## Concept 13: White-Box Testing Guided by Black-Box Results

📌 **Concept Name:** White-Box Guided by Black-Box Results

🧠 **Simple Explanation:**
White-box testing is typically done after black-box testing. An important idea from the transcript is that **white-box testing can be guided by black-box testing results**.

During black-box testing, if we find that certain modules or features have more bugs, then during white-box testing, we should spend **more time on those buggy modules**.

This works because of a principle called **defect clustering** — bugs tend to occur in clusters. If a module has bugs, it is likely to have even more bugs.

🎯 **Exam Important Points:**
- White-box testing done after black-box testing.
- Black-box results guide where to focus white-box effort.
- Buggy modules deserve more white-box testing time.

---

## Concept 14: Past Bug Detection Data for Test Planning

📌 **Concept Name:** Using Past Data for Test Planning

🧠 **Simple Explanation:**
Companies that have shipped many software products collect data on how many bugs each testing technique catches. This data is then used to plan testing effort for future projects.

🛠 **Example 1 from transcript (Hypothetical):**
- Reviews detected 10% of bugs
- Unit testing detected 40% of bugs
- Integration testing detected 25% of bugs
- System testing detected 15% of bugs
- Customer-reported: 10% of bugs

Since unit testing catches the most bugs (40%), the company should spend **more time and effort on unit testing**.

🛠 **Example 2 from transcript:**
- Test Technique 1 detected 50% of bugs
- Test Technique 2 detected 30% of bugs
- Test Technique 3 detected 10% of bugs

So, Technique 1 should get the most time, then Technique 2, and least time for Technique 3.

🎯 **Exam Important Points:**
- Past data helps in planning test effort allocation.
- More effective techniques → allocate more time.
- This is part of test planning activity.

---

## Concept 15: Defect Clustering

📌 **Concept Name:** Defect Clustering

🧠 **Simple Explanation:**
**Defect clustering** means that a few modules usually contain **most of the defects** in a software system. Bugs don't spread evenly across all modules. Instead, they tend to concentrate in certain modules.

**Why does this happen?**
- That module might be very **complex** (harder logic, more lines of code).
- That module might have been written by a **less experienced programmer** who didn't fully understand the language or the algorithm.
- Other modules might be simpler or written by more experienced programmers, so they have fewer bugs.

🛠 **Example from transcript:**
During black-box testing of Release 1, it was found that Module 6 had the highest number of bugs, Module 1 had many, but Module 3 had fewer. So for white-box testing (and for Release 2 planning), Module 6 should be tested more thoroughly.

**Key takeaway:** We should NOT spend equal time testing all modules. Spend more time on modules that are known to be buggy.

🎯 **Exam Important Points:**
- Defect clustering = a few modules contain most defects.
- Bugs live in "communities" — where you find some, you will find more.
- Reasons: module complexity, programmer experience.
- Testing effort should be proportional to bug density, not uniform.
- This is a very important principle — can be directly asked in exam.

---

## Concept 16: Introduction to Unit Testing

📌 **Concept Name:** Unit Testing

🧠 **Simple Explanation:**
**Unit testing** means testing individual units of a software **in isolation** — meaning one at a time, separately from the rest of the software.

A **unit** can be:
- A **function**
- A **module**
- A **class** (in object-oriented programming)
- A **component** (in component-based development)

Unit testing is started **as soon as the code for that unit is complete and compiled successfully** (meaning all syntax errors are removed).

🎯 **Exam Important Points:**
- Unit = function / module / class / component.
- Unit testing is done AFTER coding is complete and code compiles successfully.
- It is done in isolation (not with other parts of the software).
- It is the first level of testing after code is written.

---

## Concept 17: Why Unit Testing?

📌 **Concept Name:** Why Do Unit Testing?

🧠 **Simple Explanation:**
Someone might ask: "Why not skip unit testing and just do a very thorough system testing?"

The answer: **Unit testing reduces debugging effort substantially.**

If we skip unit testing and do only system testing, when a failure is found, the bug could be **anywhere** in the entire codebase — which might be 50,000 lines of code. Finding the bug in such a large codebase is extremely difficult and expensive.

But if we do unit testing, when a failure is found, we only need to look at the code of **that particular small unit** — maybe just 100 or 200 lines. Finding the bug is much easier and cheaper.

So, system testing might catch the same bug eventually, but **the cost of debugging and fixing it will be much higher** if we didn't do unit testing first.

🎯 **Exam Important Points:**
- Unit testing greatly reduces debugging effort.
- Without unit testing → bug could be anywhere in large codebase → very hard to find.
- With unit testing → bug is in small unit → easy to find and fix.
- Same bug may be caught in system testing, but fixing cost is much higher.

---

## Concept 18: Driver and Stub

📌 **Concept Name:** Driver and Stub in Unit Testing

🧠 **Simple Explanation:**
Since unit testing tests a unit **in isolation**, there is a problem: the unit might depend on other units. Specifically:

- Some other function might need to **call** our unit and give it data → but that function may not be ready yet.
- Our unit might need to **call some other function** to get results → but that function may not be written yet.

To solve this, we create two helper programs:

**Driver:**
- A driver **simulates** the function that would normally **call** our unit.
- It supplies test data to the unit being tested.
- Think of it as a "fake caller."

**Stub:**
- A stub **simulates** a function that our unit **needs to call** but that has not been written yet.
- It returns some predefined dummy result.
- Think of it as a "fake helper function."

Before we can do unit testing, we may need to write **drivers and stubs** for the unit.

🛠 **Small Example:**
Suppose you are testing function B. Function A calls B, and B calls function C.
- Function A is not ready → Write a **Driver** that acts like A and calls B with test data.
- Function C is not ready → Write a **Stub** that acts like C and returns a dummy result when B calls it.

🎯 **Exam Important Points:**
- Driver = simulates the CALLER of the unit (provides data to the unit).
- Stub = simulates the CALLED function (function that unit needs but is not ready).
- Both are needed for testing units in isolation.
- Driver calls the unit; stub is called BY the unit.

⚠️ **Common Confusion:** Students often mix up Driver and Stub.
- **Driver** = calls YOUR unit (comes from above).
- **Stub** = YOUR unit calls it (goes below).

---

## Summary Table of Key Concepts

| Concept | Key Point |
|---|---|
| Test Case | Checks functionality; covers program elements |
| Test Data | Only the inputs |
| Test Case Triplet | [I, S, O] = Input, State, Expected Output |
| Positive Test Case | Valid input → correct output? |
| Negative Test Case | Invalid input → graceful handling? |
| Test Suite | Complete set of all test cases |
| Test Plan | Document before testing: features, strategy, schedule |
| Testing Strategy | Which tests + how much effort |
| Usage-Based Testing | More effort on frequently used features |
| Defect Clustering | Few modules contain most bugs |
| Unit Testing | Testing individual units in isolation |
| Driver | Simulates the caller of the unit |
| Stub | Simulates the function called by the unit |
| Coverage-Based Testing | Ensure targeted program elements are covered |
| Fault-Based Testing | Expose specific types of bugs |

---

## 10 MCQs — Strictly From Lecture 4

---

**Q1.** A test case is at minimum represented as:

(A) [I, O] — Input and Output only
(B) [I, S, O] — Input, State, Expected Output
(C) [S, O] — State and Output only
(D) [I, S] — Input and State only

**Answer: (B)**
**Explanation:** The transcript clearly states that a test case at minimum is a triplet [I, S, O] where I = input data, S = state of software, O = expected output.

---

**Q2.** What is the difference between test data and a test case?

(A) They are the same thing
(B) Test data includes state and output, test case does not
(C) Test data is only the inputs; test case includes inputs, state, and expected output
(D) Test case is only the input data

**Answer: (C)**
**Explanation:** Test data is just the inputs used to test. A test case includes test data PLUS the state of the software PLUS the expected output.

---

**Q3.** A negative test case is designed to check:

(A) Whether the software works with valid inputs
(B) Whether the software crashes when given invalid or unexpected inputs
(C) Whether the software handles invalid inputs gracefully without crashing
(D) Whether the software produces wrong output

**Answer: (C)**
**Explanation:** Negative test cases check that the application gracefully handles invalid and unexpected inputs and does not crash. The purpose is graceful handling, not just checking for crashes.

---

**Q4.** The complete set of all test cases designed for testing a software is called:

(A) Test Data
(B) Test Plan
(C) Test Suite
(D) Test Strategy

**Answer: (C)**
**Explanation:** As per the transcript, the set of all test cases is called the test suite.

---

**Q5.** Consider the buggy program:
```
if (x > y) max = x;
else max = x;   // Bug: should be max = y
```
Which test suite will detect the bug?

(A) {(x=3, y=2), (x=4, y=3), (x=5, y=1)}
(B) {(x=3, y=2), (x=2, y=3)}
(C) {(x=10, y=5), (x=20, y=15)}
(D) {(x=100, y=50)}

**Answer: (B)**
**Explanation:** The bug is in the else branch. To detect it, we need a test case where y > x, so the else branch executes. Only option B has (x=2, y=3) which triggers the else branch and exposes the bug. All other options only have x > y, so the else branch is never executed.

---

**Q6.** In a test team, test execution can be performed by:

(A) Only very experienced testers
(B) Only industry experts
(C) Semi-experienced to inexperienced people
(D) Only the test case author

**Answer: (C)**
**Explanation:** The transcript states that test execution can be done by semi-experienced to inexperienced people because they simply follow the documented test cases — give inputs and observe outputs. Test planning and design need experienced people.

---

**Q7.** Defect clustering means:

(A) All modules have equal number of bugs
(B) Bugs are uniformly distributed across the software
(C) A few modules usually contain most of the defects
(D) Only system testing can find bugs

**Answer: (C)**
**Explanation:** The transcript explains that defect clustering means a few modules usually contain most defects. This happens because of module complexity or less experienced programmers writing certain modules.

---

**Q8.** In unit testing, a Driver is used to:

(A) Simulate a function that the unit being tested needs to call
(B) Simulate the behavior of a function that calls the unit being tested and supplies data to it
(C) Replace the unit being tested
(D) Perform integration testing

**Answer: (B)**
**Explanation:** A driver simulates the function that calls the unit under test and possibly supplies some data to it. A stub (not driver) simulates functions that the unit needs to call.

---

**Q9.** Why is random testing considered ineffective?

(A) Random inputs always crash the software
(B) Random inputs are too small
(C) Many random test cases may cover the same program elements and detect the same errors, so they don't improve effectiveness
(D) Random testing takes less time

**Answer: (C)**
**Explanation:** The transcript explains that with random testing, many test cases would only detect errors already detected by other test cases. They might cover the same elements repeatedly. Therefore, the number of random test cases does not indicate testing effectiveness.

---

**Q10.** A test plan is developed:

(A) After testing is complete
(B) During coding
(C) Before testing activities start
(D) After bugs are found

**Answer: (C)**
**Explanation:** The transcript clearly states that before testing activities start, a test plan is developed. It contains features to test, features not to test, test strategy, stopping criteria, effort, and schedule.

---

## What Else Is Covered — Lecture 4

All topics from the Lecture 4 transcript have been covered above. The lecture continues into Unit Testing topics (Driver, Stub) which are further elaborated in Lecture 5 and beyond.

**Key Topics to Revisit from Lecture 4 for Exam:**
- Test case triplet [I, S, O] — definition question very likely
- Positive vs Negative test cases
- Random vs Designed testing — the max-of-two-numbers example
- Defect clustering — definition and reason
- Driver vs Stub — which does what
- Test Plan contents
- Usage-based testing
- Test team roles (who does what)
