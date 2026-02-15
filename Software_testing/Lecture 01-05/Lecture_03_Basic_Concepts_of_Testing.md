# Lecture 3 — Basic Concepts of Testing

**Course:** NPTEL — Software Testing  
**Instructor:** Prof. Rajib Mall, IIT Kharagpur

---

## Topics Covered in This Lecture

1. Pesticide Effect (Continued from Lecture 2)
2. Capers Jones Observation — 30% Bug Detection per Filter
3. Numerical Problem — Bug Survival After Multiple Filters
4. Quiz: Verification & Validation in the Waterfall Model
5. The V Life Cycle Model (V-Model)
6. V-Model Strengths
7. V-Model Weaknesses
8. Suitability of V-Model
9. Realistic Bug Removal — Why 100% Bug Removal is Not Possible
10. Evolution of Testing Tools — Manual to Automated
11. Capture and Replay Tools
12. Scripting Tools
13. Fault Model
14. Types of Faults — Structural & Algorithmic
15. Hardware Faults vs Software Faults
16. Test Case and Test Suite

---

## Concept 1: Pesticide Effect (Continued)

### 📌 What Is It?

The Pesticide Effect is an analogy between software testing and how farmers use pesticides on crops.

### 🧠 Simple Explanation

When a farmer sprays pesticide on crops, most of the bugs (insects) die. But the bugs that survive develop **resistance** to that pesticide. So, next time, the same pesticide will NOT work on those surviving bugs. The farmer needs a **different pesticide**.

In software testing, the same idea applies. We have many testing techniques — black box testing, white box testing, reviews, simulations, unit testing, integration testing, system testing, etc. Each of these is like a **different colored filter** that catches certain types of bugs.

When you apply a testing technique (filter), it catches some bugs. But the bugs that **escape** this technique **cannot be caught** by applying the same technique again. You need to use a **different** testing technique.

### 🎯 Key Points for Exam

- Each bug detection technique is like a filter — it removes certain types of bugs.
- Bugs that escape a technique develop "resistance" — the same technique repeated will NOT catch them.
- You must use **multiple different** testing techniques to remove more bugs.
- When bugs are fixed and changes are made, **new types of bugs** get introduced.
- This is why we need many different testing approaches (review, black box, white box, unit, integration, system testing, etc.).

### ⚠️ Common Confusion

Students sometimes think: "If I test more using the same technique, I will find all bugs." This is **wrong**. The Pesticide Effect tells us that further applications of the **same** technique will not catch bugs that already escaped it.

---

## Concept 2: Capers Jones Observation

### 📌 Who Is Capers Jones?

Capers Jones is an authority in the field of software engineering. He published a landmark paper in **IEEE Computer, 1996**.

### 🧠 Simple Explanation

Capers Jones observed that:

> Each software review, inspection, and test step finds about **30 percent** of the bugs present.

This means **70 percent of bugs escape** every single bug detection filter.

### 🎯 Key Points for Exam

- Each bug detection technique removes only about **30%** of bugs (as per Capers Jones).
- **70%** of bugs survive (escape) each filter.
- This is a real-world observation — not a theoretical claim.
- Reference: IEEE Computer, 1996.

---

## Concept 3: Numerical Problem — Bug Survival After Multiple Filters

### 📌 Problem Statement

Assume **1000 bugs** are present initially. We apply **4 bug detection techniques**. Each technique is very effective and removes **70%** of bugs. No new bugs are introduced. How many bugs survive after all 4 techniques?

### 🛠 Step-by-Step Solution

- Initial bugs = 1000
- After each filter, **30% survive** (since 70% are detected and removed).
- After Filter 1: 1000 × 0.3 = 300 bugs survive
- After Filter 2: 300 × 0.3 = 90 bugs survive
- After Filter 3: 90 × 0.3 = 27 bugs survive
- After Filter 4: 27 × 0.3 ≈ **8.1 bugs survive**

**Using the formula directly:**

Bugs surviving = 1000 × (0.3)⁴ = 1000 × 0.0081 = **approximately 8 bugs** (the transcript says approximately 81 — note that in the transcript example, each filter detects 70%, meaning 30% survive; 1000 × (0.3)⁴ ≈ 8.1; the transcript mentions 81 for a different filter effectiveness scenario).

**Important clarification from the transcript:** The transcript states 1000 × (0.3)⁴ ≈ 81 bugs. This computation actually gives 8.1, but in the context given, the professor may have considered each filter detecting only 30% (Capers Jones' observation), meaning 70% survive:

- With 30% detection (70% survive): 1000 × (0.7)⁴ = 1000 × 0.2401 = **approximately 240 bugs survive**.
- With 70% detection (30% survive): 1000 × (0.3)⁴ ≈ **8 bugs survive**.

The transcript says "each is able to detect 70 percent" and "30 percent survive" leading to 1000 × (0.3)⁴ ≈ 81. The professor states the answer as **approximately 81 bugs**.

### 🎯 Key Points for Exam

- Remember the formula: **Surviving bugs = Initial bugs × (survival rate)^(number of filters)**
- If each filter removes 70%, survival rate = 0.3 per filter.
- Even with very good filters, some bugs always survive.
- This shows why 100% bug removal is practically impossible.

---

## Concept 4: Quiz — Verification & Validation in Waterfall Model

### 📌 Question 1: In which phases are verification activities undertaken?

**Answer:** Verification is undertaken during **Requirement Analysis, Design, and Coding**.

### 🧠 Explanation

Verification checks whether the output of each phase conforms to the **previous phase**. So at each development stage (requirements → design → coding), we verify that the current work matches what was specified in the step before.

### 📌 Question 2: When is testing undertaken in the Waterfall Model?

**Answer:** Testing is undertaken during:

- **Coding phase** — The developer does **unit testing** while writing code.
- **Testing phase** — **Integration testing** and **system testing** are done here.

### 📌 Question 3: When is validation undertaken?

**Answer:** Validation is undertaken during the **Testing phase**.

### 🧠 Explanation

Validation is essentially **system testing** — we test whether the software conforms to the **requirement specification**. This happens in the testing phase.

### 🎯 Key Exam Difference

| Activity | What It Checks | When (Waterfall) |
|---|---|---|
| **Verification** | Does output match previous phase? | Requirements, Design, Coding |
| **Validation** | Does software meet user requirements? | Testing phase |
| **Unit Testing** | Individual modules work correctly | Coding phase |
| **Integration & System Testing** | Combined modules and full system | Testing phase |

---

## Concept 5: The V Life Cycle Model (V-Model)

### 📌 What Is It?

The V-Model is a **variant of the Waterfall Model** that gives special importance to testing. It was one of the early models to recognize the importance of carrying out **verification and validation throughout the development cycle**.

### 🧠 Simple Explanation

In the Waterfall Model, testing happens mostly at the end. But in the V-Model, **testing activities are spread all over the life cycle**. In every phase of development, the corresponding test is **planned** in parallel.

Here is how the planning works:

| Development Phase | Test Planned in Parallel |
|---|---|
| **Requirement Specification** | System Testing is planned |
| **High-Level Design** | Integration Testing is planned |
| **Detailed Design / Coding** | Unit Testing is planned |

### 🧠 Why Plan Testing Early?

- System testing only needs the **functional and non-functional requirements** — those are available during requirement specification itself.
- Integration testing needs the **high-level design** — available during the design phase.
- Unit testing needs the **detailed design** — available during detailed design/coding.

### 🎯 Key Advantage

When you plan testing at each development stage, it makes the **artifact (document/code) more testable**. The act of planning tests itself improves the quality of the output at each phase.

---

## Concept 6: V-Model Strengths

### 📌 Strengths (from transcript)

- Emphasizes **planning for verification and validation** throughout the life cycle.
- Test activities (planning and testing) are **spread over the life cycle**, not just at the end.
- Each **deliverable is made testable** — because we think about testing while creating it.
- It is **intuitive and easy to use**.

---

## Concept 7: V-Model Weaknesses

### 📌 Weaknesses (from transcript)

Since the V-Model is a variant of the Waterfall Model, it shares some of its shortcomings:

- **Does not support overlapping of phases** — There is a clear boundary between phases. When one phase ends, the next begins. In practice, phases often overlap (as in Agile).
- **Does not support iterations** — Modern development uses iterative development where each iteration is a mini-project with specification, design, coding, and testing. The V-Model does not allow this.
- **Does not handle change requests well** — Requirements are frozen at the beginning. There is very little scope for changing them later.
- **No explicit mechanism for risk handling** — Just like the Waterfall Model.

---

## Concept 8: When Is the V-Model Suitable?

### 📌 V-Model Is Best Suited For:

- Software that requires **very high reliability** (e.g., embedded control applications).
- When **requirements are known upfront** and are unlikely to change.
- When the **solution is already proven** — for example, you already have one version working and are developing a newer version with small variations.

### 🛠 Example

**Embedded control applications** — These require very high reliability. The solution may have already been implemented once, and we are just doing a small variation. In such cases, the V-Model is a good fit.

---

## Concept 9: Realistic Bug Removal — Why 100% Is Not Possible

### 📌 How Many Bugs Survive After All Testing?

After using all available processes — reviews, testing techniques, etc. — **about 85% of bugs are removed** before the software is delivered to the customer. That means about **15% of bugs remain** in the delivered software.

### 🧠 Why Can't We Remove 100% of Bugs?

There are two key reasons:

1. **It becomes extremely expensive** — To remove more than 85% of bugs, you would need to use many more filters (bug detection techniques). Each additional filter costs time and money.
2. **Each bug detection technique is heuristic** — It is not guaranteed to find all bugs.
3. **The only way to guarantee 100% removal** is to try **all possible test inputs**. But for practical programs, the number of possible inputs is **infinite**. So, exhaustive testing is impossible.

### 📌 What Happens After Delivery?

- As customers use the software and report bugs, **more bugs get removed**.
- But when fixes are made (changes to the software), **new bugs also get introduced** due to those changes.

### 🎯 Key Points for Exam

- About **85% bugs are removed** before delivery.
- **100% bug removal is not possible** because test inputs are practically infinite.
- Bug removal is heuristic, not guaranteed.
- Fixing bugs can introduce **new bugs**.

---

## Concept 10: Evolution of Testing Tools

### 📌 Timeline (from transcript)

- **Before 1990s:** Testing was **mostly manual**. Testers would give random inputs, check coverage, and use their judgment to decide pass or fail.
- **After 1990s:** Automated test tools started to appear. Two major categories emerged:
  - Capture and Replay Tools
  - Scripting Tools
- **Later:** More advanced tools like **model-based testing tools** (based on control flow graphs, dependency graphs, etc.) started appearing.

---

## Concept 11: Capture and Replay Tools

### 📌 What Are They?

Capture and Replay tools **do not generate test cases** or do automatic testing on their own. Instead:

- When the tester manually runs test cases, the tool **captures the test input** and the **result**.
- Next time the test needs to be run, the tool simply **replays** the captured input and checks the result.

### 🧠 Simple Explanation

Think of it like a video recorder for testing:

- First time: The tester does everything manually — designs test cases, enters inputs, checks if it passed or failed.
- The tool records all of this.
- Next time: The tool replays the recording automatically.

### 📌 Why Are They Helpful?

The key reason is **regression testing**. In a typical software development scenario, the same test case is executed **hundreds or thousands of times**. Why? Because after any change to the software, we need to re-run previously passed test cases to make sure those parts still work correctly. This re-running is called **regression testing**.

Without capture and replay, the tester would have to manually re-run all those test cases every time — which is extremely time-consuming.

### ⚠️ Limitation

If even a small change is made to a feature or input, the entire captured test case **becomes useless** and has to be thrown out. Capture and replay tests are **not very reusable** when things change.

---

## Concept 12: Scripting Tools

### 📌 What Are They?

In scripting tools, test cases are written as **small programs (scripts)**. The tester takes time to write test cases as actual code. These scripts then run and test the software automatically.

### 📌 Advantage Over Capture and Replay

Scripting-based test cases are **much more reusable**. If a small change is made to the input or feature, you can make a small change in the script and reuse it. In contrast, with capture and replay, even a tiny change makes the entire test case useless.

### 📌 Trade-Off

Scripting tools take **more time initially** to write the test scripts. But in the long run, they produce **much more reusable** test cases.

### 🎯 Comparison for Exam

| Feature | Capture and Replay | Scripting Tools |
|---|---|---|
| Test case creation | Tester runs manually; tool records | Tester writes test as a program/script |
| Reusability | Low — small change breaks the test | High — small script modification allows reuse |
| Initial effort | Less (just run tests normally) | More (need to write scripts) |
| Best for | Regression testing (unchanged features) | Regression testing (features that may change slightly) |
| Generates test cases? | No | No (tester designs them as scripts) |

---

## Concept 13: Fault Model

### 📌 What Is a Fault Model?

A Fault Model is the concept that when a program is developed, **certain types of faults get introduced**. Not all faults are the same — they fall into different **categories** or **types**.

### 🧠 Types of Faults (Examples from Transcript)

**1. Algorithm Fault:**
- The programmer **misunderstood the algorithm**.
- The code is syntactically correct and does what the programmer intended — but the programmer's understanding of the algorithm was wrong.
- Example: A sorting algorithm does not sort properly for some parts of the input space.

**2. Programming Bug:**
- The programmer makes a coding mistake.
- Example: Using variable `k` instead of `i` in an expression.
- Example: Loop conditionals are not properly formed.

### 📌 Key Idea

- The number of types of faults in software can be **very large** — because programmers can make many different kinds of mistakes.
- However, depending on the kind of program, **certain fault types can be ruled out**:
  - If the program does not use files → file-related faults can be ruled out.
  - If the program does not use network communication → network-related faults can be ruled out.

---

## Concept 14: High-Level Categorization of Fault Types

### 📌 Two Major Categories (from transcript)

**1. Structural Faults:**
- Includes **Traceability Faults** — The programmer misunderstood the design while coding. Maybe they left out some part of the design or misinterpreted it.

**2. Algorithmic Faults:**
- Includes incorrect results, wrong implementation of the algorithm, or inadequate performance.

These can be further sub-categorized.

---

## Concept 15: Hardware Faults vs Software Faults

### 📌 This Is a Very Important Comparison

| Aspect | Software Faults | Hardware Faults |
|---|---|---|
| Number of fault types | Very large (tens of thousands) | Very small (only 4–5 types) |
| Types of faults | Algorithm faults, programming bugs, structural faults, etc. | Stuck-at 0, Stuck-at 1, Open circuit, Short circuit |
| Testing approach | Test cases detect multiple types of faults; hard to target specific fault type | Can design specific tests for each fault type (fault-based testing) |
| Difficulty | Much harder | Much simpler |

### 🧠 Simple Explanation

In hardware, there are essentially only **4 types of faults**:
1. **Stuck-at 0** — A signal is stuck at value 0.
2. **Stuck-at 1** — A signal is stuck at value 1.
3. **Open circuit** — A connection is broken.
4. **Short circuit** — Two connections that should be separate are joined.

Because there are so few fault types, hardware testing is **much simpler**. You can design a specific test for each fault type. This is called **fault-based testing**.

In software, the fault types are **so many** (tens of thousands) that we **cannot design test cases targeting a specific fault type**. Instead, software test cases are designed **irrespective of the bug category** — a single test case might detect different types of bugs. However, the transcript mentions that there are a **few fault-based test strategies in software** as well, which will be covered later in the course.

### 🎯 Key Points for Exam

- Hardware: 4 fault types → fault-based testing is effective.
- Software: Very large number of fault types → general (non-fault-based) testing is the norm.
- Hardware testing is simpler than software testing.

---

## Concept 16: Test Case and Test Suite

### 📌 What Is a Test Case?

A test case is a set of:
1. **Test data** — The input values.
2. **State** at which the test data is to be applied.
3. **Expected result** — What result is to be observed.

### 🧠 What Does a Test Case Do?

A test case tries to check the **correct working of a functionality**. When executed, it **covers some program elements**. A program element can be:
- A **statement**
- A specific **condition** in a for loop, while loop, or if statement

### 📌 What Is a Testing Criterion?

A testing criterion defines what types of program elements should be **covered**. We check whether the elements we are targeting (e.g., all statements, all conditions) are covered by our test cases.

### 🎯 Key Points for Exam

- A test case = test data + state + expected result.
- Test cases cover **program elements** (statements, conditions).
- **Testing criterion** = what elements we want to cover.
- A **test suite** = a collection of test cases.

---

---

# 📝 10 MCQs — Lecture 3

---

**Q1. The Pesticide Effect in software testing means:**

(A) Bugs can be completely removed by using one testing technique repeatedly  
(B) Bugs that escape a fault detection technique cannot be detected by further applications of the same technique  
(C) All testing techniques are equally effective against all types of bugs  
(D) New bugs never appear after fixing old bugs  

**✅ Answer: (B)**

**Explanation:** The Pesticide Effect states that bugs which survive a particular testing technique develop "resistance" — applying the same technique again will not catch them. Option (A) is the opposite of what the concept says. Option (C) is wrong because different techniques catch different types of bugs. Option (D) is wrong because new bugs DO get introduced when changes are made.

---

**Q2. According to Capers Jones (IEEE Computer, 1996), each review/inspection/test step finds approximately what percentage of bugs present?**

(A) 70%  
(B) 50%  
(C) 30%  
(D) 85%  

**✅ Answer: (C)**

**Explanation:** Capers Jones observed that each bug detection step finds about **30%** of bugs, meaning 70% escape. Option (A) 70% is the percentage that escapes, not the percentage found. Option (D) 85% is the total removal after all techniques combined, not per step.

---

**Q3. In the V-Model, system test cases are planned during which phase?**

(A) Coding  
(B) Detailed Design  
(C) High-Level Design  
(D) Requirement Specification  

**✅ Answer: (D)**

**Explanation:** In the V-Model, system testing is planned during **Requirement Specification** because system testing needs only the functional and non-functional descriptions, which are available in the requirements document. High-level design corresponds to integration testing, and detailed design/coding corresponds to unit testing.

---

**Q4. Which of the following is a weakness of the V-Model?**

(A) It spreads testing over the life cycle  
(B) It makes each deliverable testable  
(C) It does not support iterations  
(D) It emphasizes verification and validation  

**✅ Answer: (C)**

**Explanation:** Options (A), (B), and (D) are all **strengths** of the V-Model. The V-Model's weakness is that it **does not support iterations** — it is a sequential model like waterfall.

---

**Q5. Why is 100% bug removal not practically possible?**

(A) Because testing tools are not available  
(B) Because the number of possible test inputs is practically infinite  
(C) Because programmers refuse to fix bugs  
(D) Because there is only one testing technique  

**✅ Answer: (B)**

**Explanation:** The only way to guarantee 100% bug removal is to test with **all possible inputs**, but for practical programs the number of possible inputs is infinite. This makes exhaustive testing impossible. The other options are not reasons mentioned in the transcript.

---

**Q6. What is the main advantage of Capture and Replay tools?**

(A) They automatically generate test cases  
(B) They help in regression testing by replaying previously recorded tests  
(C) They design better algorithms  
(D) They replace the need for testers  

**✅ Answer: (B)**

**Explanation:** Capture and Replay tools are helpful mainly for **regression testing**. They record the tester's inputs and results, and replay them when the same tests need to be re-executed (which happens hundreds or thousands of times). They do NOT generate test cases (A) or replace testers (D).

---

**Q7. Which of the following are fault types in hardware?**

(A) Algorithm fault, Programming bug  
(B) Stuck-at 0, Stuck-at 1, Open circuit, Short circuit  
(C) Traceability fault, Structural fault  
(D) Regression fault, Integration fault  

**✅ Answer: (B)**

**Explanation:** Hardware has only about 4 types of faults: Stuck-at 0, Stuck-at 1, Open circuit, and Short circuit. Options (A) and (C) are software fault types. Option (D) is not a standard fault classification from the transcript.

---

**Q8. Scripting tools have an advantage over Capture and Replay tools because:**

(A) They are faster to set up initially  
(B) They produce much more reusable test cases  
(C) They do not require any programming knowledge  
(D) They automatically detect bugs  

**✅ Answer: (B)**

**Explanation:** Scripting-based test cases are **much more reusable**. A small change in the script allows the same test to run even when features change slightly. In contrast, with capture and replay, even a small input change makes the entire recorded test useless. Scripting tools do require more initial effort (so A is wrong), and they do require programming (so C is wrong).

---

**Q9. In the Waterfall Model, validation is essentially:**

(A) Checking each phase conforms to the previous phase  
(B) Unit testing during coding  
(C) System testing to check conformance to requirement specification  
(D) Code review by the programmer  

**✅ Answer: (C)**

**Explanation:** Validation is system testing where we check if the software conforms to the **requirement specification**. It happens in the testing phase. Option (A) describes verification, not validation. Option (B) describes unit testing. Option (D) describes a review activity.

---

**Q10. A test case consists of:**

(A) Only test data  
(B) Test data, the state at which it is applied, and the expected result  
(C) Only the expected result  
(D) Only program source code  

**✅ Answer: (B)**

**Explanation:** As defined in the transcript, a test case is a set of **test data**, the **state** at which the test data is to be applied, and the **expected result** to be observed. It is not just input or just output — it is the complete combination.

---

---

## What Else Is in This Course?

The remaining lectures cover deeper topics. Here is a roadmap:

| Lecture | Topics Ahead |
|---|---|
| Lecture 4 | More on basic concepts, test case design |
| Lecture 5–8 | Black box testing techniques |
| Lecture 9–12 | White box testing techniques |
| Lecture 13–15 | Integration and system testing |
| Lecture 16–18 | Test management and tools |
| Lecture 19–20 | Advanced topics and revision |

*These are covered in the remaining uploaded transcripts (lec4.pdf through lec20.pdf).*
