# Lecture 2: Levels of Testing — Complete Notes

**Course:** NPTEL Software Testing  
**Instructor:** Prof. Rajib Mall, IIT Kharagpur  
**Lecture Topic:** Levels of Testing and Life Cycle Models for Testing

---

## Topics Covered in This Lecture

1. Verification vs Validation (Recap from Lecture 1)
2. Four Levels of Testing
3. Unit Testing
4. Integration Testing
5. System Testing
6. Regression Testing
7. Testing Activities and Who Does Them
8. Why Unit Testing is Needed (Why Not Just System Testing?)
9. Smoke Testing
10. Types of System Testing (Alpha, Beta, Acceptance)
11. Functional Testing vs Performance Testing
12. Types of Persons Who Do Testing
13. Testing Activities on the Waterfall Model
14. The Pesticide Effect

---

## 📌 Concept 1: Verification vs Validation (Recap)

### 🧠 Simple Explanation

These are two very important terms. They sound similar but mean different things.

**Verification** asks: **"Are we building the software RIGHT?"**
This means — are we following the correct process? Is the current work matching the previous stage's output? For example, if the design says "do X," is the code actually doing X?

**Validation** asks: **"Have we built the RIGHT software?"**
This means — does the final software match what the customer actually wanted? Is the final product correct according to the original requirements?

### 🛠 How to Remember

Think of it this way:
- **Verification** = checking at every stage during development (done by developers)
- **Validation** = checking the final product at the end (done by testers and customers)

### 📊 Key Differences

| Feature | Verification | Validation |
|---|---|---|
| Question | Are we building the software right? | Have we built the right software? |
| When done? | During development stages | At the end (system testing) |
| Who does it? | Developers | Testers |
| Techniques | Review, analysis, simulation (static) + unit testing (dynamic) | Only dynamic (execute and check) |
| Nature | Both static and dynamic activities | Only dynamic activity |
| Checks against | Previous artifact/stage output | Requirements specification (SRS) |

### 🎯 Exam Important Points

- Verification includes both static (review, analysis) AND dynamic (unit testing) activities.
- Validation is ONLY dynamic — you execute the software and check results against requirements.
- Verification is done by developers; Validation is done by testers.

### ⚠️ Common Confusion

Many students think both are "just testing." They are not. Verification includes non-execution activities like reviewing documents. Validation always involves actually running the software.

---

## 📌 Concept 2: Four Levels of Testing

### 🧠 Simple Explanation

Testing is not done just once at the end. It is done at **4 different levels** as the software is being developed:

1. **Unit Testing** — Test each small piece (unit) separately
2. **Integration Testing** — Combine the tested units and test if they work together
3. **System Testing** — Test the complete system as a whole
4. **Regression Testing** — Test again after any change is made (during maintenance)

### 📊 Step-by-Step Flow

```
Step 1: Unit Testing → Each unit is tested individually
Step 2: Integration Testing → Units are combined and tested together
Step 3: System Testing → The whole system is tested against requirements
Step 4: Regression Testing → After any change/maintenance, test again
```

### 🎯 Exam Important Points

- Unit testing is the FIRST level of testing.
- Regression testing happens during MAINTENANCE phase (after software is released).
- These 4 levels are done in order during the software lifecycle.

---

## 📌 Concept 3: Unit Testing

### 🧠 Simple Explanation

A "unit" can be a **function**, a **module**, or a **component**. Unit testing means testing each unit **independently** — that is, separately from other units.

Unit testing is done by the **developers themselves** (not by a separate test team). The developer who wrote the code tests it.

### 🛠 Example of Bugs Found in Unit Testing

- **Algorithm errors** — the logic is wrong
- **Programming errors** — the programmer wrote variable `j` instead of `i`, or did not implement the algorithm correctly

### 🎯 Exam Important Points

- Unit = function, module, or component.
- Tested independently (in isolation from other units).
- Done by developers, NOT by testers.
- Detects algorithm errors and programming errors.

---

## 📌 Concept 4: Integration Testing

### 🧠 Simple Explanation

After each unit passes its unit test, the next step is to **combine (integrate) the units together** and test if they work correctly as a group.

**Why is this needed?** Because even if each unit works perfectly on its own, when you put them together, they may not communicate properly. The **interfaces** between units may have problems.

### 🛠 Example of a Bug Found in Integration Testing

- **Mismatch of parameters** — Suppose a function expects input as `(i, j)` but another function sends the values as `(j, i)`. The order is swapped. Each unit individually is correct, but together they fail because the interface is wrong.

### 📊 Key Point

Integration testing is based on the **design** of the software. The design tells us which modules exist and how they interface with each other.

### 🎯 Exam Important Points

- Done AFTER unit testing.
- Checks if units work together properly.
- Focuses on **interfacing errors** between modules.
- Based on the **design document**.

---

## 📌 Concept 5: System Testing

### 🧠 Simple Explanation

After all units are integrated and tested, the **entire system** is tested as a whole. System testing checks whether the software meets its **specification/requirements**.

System testing is done by a **separate test team** (not the developers who wrote the code).

### 🛠 Example of Bugs Found in System Testing

- **Performance bugs** — the system is too slow
- **Usability bugs** — the system is hard to use

The transcript says: if unit testing and integration testing were done perfectly, the bugs that system testing will detect are **performance bugs and usability bugs** — because unit and integration testing do NOT look at performance or usability aspects.

### 📊 Two Major Types of System Testing

1. **Functionality Testing** — checks if the functional requirements are correct
2. **Performance Testing** — checks if the non-functional requirements are correct

### 🎯 Exam Important Points

- Tests the entire system as a whole.
- Done by a **separate test team**, not developers.
- Based on the **requirements document (SRS)**.
- Detects performance and usability bugs that unit/integration testing cannot find.
- Acceptance testing is a PART of system testing.

---

## 📌 Concept 6: Regression Testing

### 🧠 Simple Explanation

After software is released, it will undergo **changes** — bug fixes, enhancements, new features, etc. Every software undergoes changes during its lifetime.

When a change is made, we need to check TWO things:
1. The **changed part** is working correctly.
2. The **unchanged parts** are STILL working correctly.

Why would unchanged parts stop working? Because a change in one part can **influence** other parts (for example, through shared data).

This re-testing is called **Regression Testing**.

### 🛠 What are Regression Bugs?

A regression bug is when the software **used to work correctly**, but after a small change somewhere, it **stops working** on tests it previously passed.

### 📊 Key Point

Regression testing checks whether software that previously passed certain test cases **continues to pass** those same test cases after a change.

### 🎯 Exam Important Points

- Done during the **maintenance** phase.
- Checks both changed AND unchanged parts.
- A change in one part can break other parts due to shared data or dependencies.
- Regression bugs = software that was working now fails after a change.

---

## 📌 Concept 7: Levels of Testing — Diagrammatic View

### 🧠 Simple Explanation

The transcript shows a diagram connecting development stages to testing levels:

```
What users really need  ←---  System testing  ---→  Acceptance Testing
Requirements            ←---  System testing  ---→  System Testing
Design                  ←---  Integration testing → Integration Testing
Code                    ←---  Unit testing    ---→  Unit Testing
Maintenance             ←---  ................---→  Regression Testing
```

What this means:
- **Acceptance testing** is based on what the users really need.
- **System testing** is based on the requirements.
- **Integration testing** is based on the design.
- **Unit testing** is based on the code.
- **Regression testing** is carried out during maintenance.

### 🎯 Exam Important Points

- Each testing level maps to a specific development artifact.
- This mapping is very important for exams.

---

## 📌 Concept 8: Testing Activities

### 🧠 Simple Explanation

A tester performs these activities:

1. **Test Suite Design** — create the set of test cases
2. **Run Test Cases** — execute them on the software
3. **Check Results** — see if there are any failures
4. **Prepare Failure List / Test Report** — document all failures

For **system and integration testing**, these activities are done by the **testers**.

For **unit testing**, all these activities are done by the **developer** himself (no separate testers).

Once the failure list/test report is prepared, it is given to the **developers**, who then:
- **Debug** — find where the specific bug is in the code
- **Correct** — fix the bug

### 🎯 Exam Important Points

- Testers do: design tests, run tests, check results, prepare test report.
- Developers do: debug and fix based on the test report.
- For unit testing, the developer does all activities.

---

## 📌 Concept 9: Why Not Just Do System Testing? (Why is Unit Testing Needed?)

### 🧠 Simple Explanation

Some people ask: "Why can't we skip unit testing and just do thorough system testing?"

The answer: You CAN do that, but it will be **very expensive**.

**Why?** Because in system testing, the whole system (maybe thousands of modules) is running together. When a failure is detected:
- You have to **search through all thousands of modules** to find the bug.
- **Debugging becomes extremely difficult**.
- **Debugging and correcting become very costly**.

In unit testing, you test one small unit at a time, so finding and fixing bugs is **much cheaper and easier**.

### 📊 Key Point from Transcript

"Non-trivial software bugs are detected during **unit testing**. They are largely detected during unit testing because unit testing is a very cost-effective way of reducing bugs. System testing is a very expensive proposition to reduce bugs."

### 🎯 Exam Important Points

- Unit testing is **cost-effective** for finding and fixing bugs.
- System testing is **expensive** for finding bugs because debugging is difficult in a large system.
- Most bugs are found during unit testing, not system testing.

---

## 📌 Concept 10: Smoke Testing

### 🧠 Simple Explanation

Smoke testing is a special type of testing that is done **frequently** — daily or even several times a day.

**What is it?** It means putting all the modules together (doing a "build") and checking whether **at least the basic functionalities are working**.

**Why is it important?** If we wait until the end to integrate everything, we might find that the entire system is frozen — not responding at all. That would be a disaster. Smoke testing prevents this by catching severe integration problems early.

### 🛠 Origin of the Name

The name "smoke testing" comes from **pipe testing**. When testing large pipe installations, engineers would first put smoke through the pipes to check if smoke was escaping (meaning there are leaks) before they put water through for the actual test.

Similarly, in software, we do a quick "smoke" check before the real testing begins.

### 📊 Key Point

- Every organization recommends smoke testing be done frequently.
- Even though integration and system testing are done towards the end of an iteration, smoke testing is done **throughout**.

### 🎯 Exam Important Points

- Smoke testing = build the system + check basic functionalities.
- Done frequently (daily or multiple times a day).
- Catches severe integration problems early.
- Without smoke testing, final integration and system testing may take unduly long time and effort.

---

## 📌 Concept 11: Types of System Testing (Alpha, Beta, Acceptance)

### 🧠 Simple Explanation

System testing has **three types** depending on **WHO does the testing**:

1. **Alpha Testing** — Done by the **developing organization** itself. The testers here have access to the internals of the software.

2. **Beta Testing** — Done by a **friendly set of customers**. The software is given to selected users who use it and report bugs. They do NOT have access to the internals.

3. **Acceptance Testing** — Done by the **actual customer** before accepting delivery of the product. This is the final test before the customer says "Yes, I will accept this software."

### 📊 Summary Table

| Type | Done By | Access to Internals? |
|---|---|---|
| Alpha Testing | Developing organization | Yes |
| Beta Testing | Friendly customers | No |
| Acceptance Testing | Final customer | No |

### 🎯 Exam Important Points

- All three are types of system testing.
- The difference is WHO is doing the testing.
- Alpha = developer's organization, Beta = friendly customers, Acceptance = final customer.
- Alpha testers have access to software internals; Beta testers do not.

---

## 📌 Concept 12: Functional Testing vs Performance Testing

### 🧠 Simple Explanation

System testing checks two kinds of requirements:

**Functional Testing** checks the **functional requirements** — "Does the software do what it is supposed to do?"
These are the functions listed in the SRS (Software Requirements Specification) document.

**Performance Testing** checks the **non-functional requirements** — "Does the software perform well enough?"

### 📊 Types of Performance Tests (mentioned in transcript)

- **Response time** — how fast does the system respond?
- **Throughput** — how much work can it handle?
- **Usability** — how easy is it to use?
- **Stress testing** — does it work under heavy load (e.g., many inputs per unit time)?
- **Recovery** — can it recover from failures?
- **Configuration** — does it work in different configurations?

### 🎯 Exam Important Points

- Functional tests → check functional requirements from the SRS.
- Performance tests → check non-functional requirements from the SRS.
- Know the types of performance tests listed above.

---

## 📌 Concept 13: User Acceptance Testing

### 🧠 Simple Explanation

User Acceptance Testing (UAT) is a type of system testing. Based on this testing, the user either **accepts or rejects** the delivered software.

This is the final gate before the software is officially handed over.

### 🎯 Exam Important Points

- UAT is the customer's final check.
- Result: Accept or reject the software.

---

## 📌 Concept 14: Types of Persons Who Do Testing

### 🧠 Simple Explanation

Three types of people are involved in testing:

**1. Programmers (Developers)**
- They do **unit testing**.
- They test the code they have written.

**2. Testers**
- They do **all types of testing EXCEPT unit testing and acceptance testing**.
- So they do: integration testing and system testing.
- They also develop the **test plan and strategy**.

**3. Users (Customers)**
- They do **acceptance testing**.
- They also check **usability**.
- Some may volunteer for **beta testing**.

### 📊 Summary Table

| Person | Testing They Do |
|---|---|
| Programmers | Unit testing |
| Testers | Integration testing, System testing, Test plan & strategy |
| Users | Acceptance testing, Usability testing, Beta testing (volunteer) |

### 🎯 Exam Important Points

- Programmers ONLY do unit testing.
- Testers do everything EXCEPT unit testing and acceptance testing.
- Users do acceptance testing and may volunteer for beta testing.

---

## 📌 Concept 15: Testing Activities on the Waterfall Model

### 🧠 Simple Explanation

In the Waterfall model, development happens in sequential phases. If we map testing activities onto this model:

- During **coding phase** → unit testing is done by developers.
- During **testing phase** → the test team does integration and system testing. A large number of testers are needed ONLY at this stage.
- During **development phases** (requirements, design) → verification activities like review, simulation etc. are done by developers.

### ⚠️ Problem with the Waterfall Model

In the waterfall model, testers are needed in large numbers ONLY during the testing phase. **What do testers do during other phases?** They are idle, or they possibly work on other projects. This is a drawback of the waterfall model for testing.

### 🎯 Exam Important Points

- In waterfall model, testers are largely working ONLY during the testing phase.
- This is a disadvantage — testers are idle in other phases.
- Verification activities happen during development phases.
- Unit testing happens during coding phase.

---

## 📌 Concept 16: The Pesticide Effect

### 🧠 Simple Explanation

This is a very important concept. The name comes from farming.

**Farming Analogy:**
Imagine you are a farmer. Your cotton crop is infested with bugs (pests). You spray DDT. Most bugs die, but some 20-30% survive. Next season, the same bugs return. You spray DDT again, but it does NOT work anymore — the surviving bugs have become **immune** to DDT. So you need a new pesticide (like Malathion). But again, 30% survive and become immune to that too.

**Software Testing Analogy:**
The exact same thing happens in software testing. If you keep using the **same testing technique** again and again, the bugs that escape that technique will NEVER be caught by it again.

Each testing technique is like a **filter**. It catches some bugs, but some bugs pass through. If you apply the same filter again, the bugs that already escaped will escape again.

### 📊 Step-by-Step Understanding

```
Step 1: Apply testing technique A (e.g., equivalence partitioning)
        → Catches many bugs, but 20-30% escape

Step 2: Fix the caught bugs. But new bugs may get introduced by:
        - Bug fixes themselves
        - New development

Step 3: Apply the SAME technique A again
        → The escaped bugs will NOT be caught (they are immune)

Step 4: Solution → Apply a DIFFERENT technique B 
        (e.g., decision table testing, white box testing, 
         path testing, MCDC testing)
```

### 📊 Key Points from Transcript

- There are **dozens of testing techniques** and each one is a "bug filter."
- Bugs that survive a filter cannot be eliminated by further application of that same filter.
- New bugs get introduced by bug corrections and newer development.
- As the system development proceeds, we need to use **many types of filters** to effectively reduce bugs.
- Just one methodology used for a long time may not really help.

### 🛠 Testing Techniques Mentioned as Examples of Different Filters

- Equivalence partitioning
- Decision table testing
- White box testing
- Path testing
- MCDC testing

### 🎯 Exam Important Points

- The Pesticide Effect = bugs that escape a testing technique cannot be caught by the same technique again.
- Solution: Use MULTIPLE different testing techniques.
- Each testing technique is a "filter."
- There are dozens of testing techniques, and many must be used together.
- New bugs are introduced when old bugs are fixed and when new development happens.

### ⚠️ Common Confusion

Students sometimes think "just test more with the same method." That does NOT work. The pesticide effect says you MUST change the testing method to find new bugs.

---

## 📌 Concept 17: When to Stop Testing?

### 🧠 Simple Explanation

(Briefly revisited from Lecture 1 in this lecture)

Two approaches to decide when to stop testing:

**Approach 1: Rate of Bug Detection**
Initially, many bugs are found per unit time. As testing continues, the rate falls. When you test for a day or two and NO new bugs are reported, it may be time to stop.

**Approach 2: Bug Seeding**
You intentionally plant (seed) known bugs into the software. Then you test. The percentage of seeded bugs that are detected tells you how thorough your testing is.

### ⚠️ Important Caution about Bug Seeding

The seeded bugs **must match the type and percentage of real bugs** in an actual program. You cannot just seed trivial, easy-to-find bugs. If you seed only easy bugs, detecting them does not tell you anything useful about the remaining hard bugs.

### 🎯 Exam Important Points

- Bug seeding must be realistic — types and percentages of seeded bugs must match what a real program has.
- Trivially easy seeded bugs give misleading results.

---

---

# 📝 10 MCQs from Lecture 2

---

**Q1.** Verification is concerned with:

(a) Checking whether we have built the right software  
(b) Checking whether we are building the software right  
(c) Only executing the software and checking output  
(d) Only testing done by the customer  

**Answer: (b)**  
Verification asks "Are we building the software right?" — it checks whether the current work conforms to its previous artifact/stage output. Option (a) is validation, not verification.

---

**Q2.** Which of the following is TRUE about Validation?

(a) It is both a static and dynamic activity  
(b) It is done during the development stages by developers  
(c) It is only a dynamic activity  
(d) It involves reviewing design documents  

**Answer: (c)**  
The transcript clearly states that validation is ONLY a dynamic activity — you execute the software and check it against requirements. You do not review documents for validation. Verification is the one that includes both static and dynamic activities.

---

**Q3.** The correct order of testing levels is:

(a) System testing → Unit testing → Integration testing → Regression testing  
(b) Unit testing → System testing → Integration testing → Regression testing  
(c) Unit testing → Integration testing → System testing → Regression testing  
(d) Integration testing → Unit testing → System testing → Regression testing  

**Answer: (c)**  
First individual units are tested (unit testing), then they are combined and tested together (integration testing), then the full system is tested (system testing), and finally after changes during maintenance, regression testing is done.

---

**Q4.** A mismatch of parameters between two modules is an example of a bug detected during:

(a) Unit testing  
(b) Integration testing  
(c) System testing  
(d) Regression testing  

**Answer: (b)**  
The transcript gives the specific example: passing `(j, i)` instead of `(i, j)` — a parameter mismatch between modules — is detected during integration testing because it is an interfacing error between units.

---

**Q5.** Why is unit testing preferred over directly doing system testing to find bugs?

(a) System testing cannot find any bugs  
(b) Unit testing is more expensive but finds more bugs  
(c) Debugging is very difficult in system testing because you have to search through thousands of modules  
(d) System testing does not execute the software  

**Answer: (c)**  
The transcript explains that when a bug is found during system testing, you must search through thousands of modules to find it, making debugging extremely difficult and expensive. Unit testing is a cost-effective way to find and fix bugs because you are looking at only one small unit.

---

**Q6.** Smoke testing is best described as:

(a) Testing the software under heavy stress conditions  
(b) Building the entire system and checking if at least the basic functionalities are working  
(c) Testing individual units in isolation  
(d) Testing the software by end users before acceptance  

**Answer: (b)**  
Smoke testing means putting all modules together (performing a build) and checking whether at least the basic functionalities are working. It is done frequently — daily or several times a day — to catch severe integration problems early.

---

**Q7.** Alpha testing is done by:

(a) End users / customers  
(b) A friendly set of customers  
(c) The developing organization  
(d) Independent third-party testers  

**Answer: (c)**  
Alpha testing is done by the developing organization, and they have access to the internals of the software. Beta testing is done by friendly customers, and acceptance testing is done by the final customer.

---

**Q8.** Performance testing checks:

(a) Functional requirements in the SRS document  
(b) Non-functional requirements in the requirements document  
(c) Whether the code compiles without errors  
(d) Whether individual units pass their test cases  

**Answer: (b)**  
Functional testing checks functional requirements, while performance testing checks non-functional requirements such as response time, throughput, usability, stress, recovery, and configuration.

---

**Q9.** The Pesticide Effect in software testing means:

(a) Bugs multiply with each testing cycle  
(b) Bugs that escape a testing technique cannot be detected by further application of that same technique  
(c) All bugs can be found by applying one technique multiple times  
(d) Testing should only use one method for consistency  

**Answer: (b)**  
The pesticide effect states that bugs that survive a particular testing technique (filter) cannot be eliminated by further application of that same filter. Just like pests becoming immune to a pesticide, software bugs become "immune" to a testing technique. The solution is to use multiple different testing techniques.

---

**Q10.** In the Waterfall model, the main problem for testing is:

(a) No testing is done at all  
(b) Unit testing is not possible  
(c) Testers are needed in large numbers only during the testing phase and are idle otherwise  
(d) Developers cannot participate in testing  

**Answer: (c)**  
The transcript explains that in the waterfall model, testers are largely working only during the testing phase. They are not needed in other phases, which means they are idle and possibly working on other projects. This is a drawback of the waterfall model.

---

## Quick Revision Summary

| Concept | Key Takeaway |
|---|---|
| Verification | "Building the software right?" — static + dynamic — by developers |
| Validation | "Built the right software?" — only dynamic — by testers |
| Unit Testing | Test individual units in isolation — by developers — detects algorithm/programming errors |
| Integration Testing | Test combined units — based on design — detects interfacing errors (e.g., parameter mismatch) |
| System Testing | Test whole system — based on requirements — detects performance/usability bugs |
| Regression Testing | Re-test after changes during maintenance — checks changed AND unchanged parts |
| Smoke Testing | Frequent builds + basic functionality check — done daily — prevents integration disasters |
| Alpha Testing | By developing organization — has access to internals |
| Beta Testing | By friendly customers — no access to internals |
| Acceptance Testing | By final customer — accept or reject software |
| Functional Testing | Checks functional requirements |
| Performance Testing | Checks non-functional requirements (response time, throughput, usability, stress, recovery, configuration) |
| Pesticide Effect | Same technique cannot catch bugs that already escaped it — use multiple techniques |
| Waterfall Problem | Testers idle except during testing phase |
| Bug Seeding | Seeded bugs must match real bug types and percentages |

---

*End of Lecture 2 Notes*
