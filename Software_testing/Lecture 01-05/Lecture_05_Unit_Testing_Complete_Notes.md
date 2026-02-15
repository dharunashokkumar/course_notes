# Lecture 5: Unit Testing — Complete Notes

**Course:** NPTEL Software Testing  
**Instructor:** Prof. Rajib Mall, IIT Kharagpur  
**Focus:** Unit Testing, Drivers & Stubs, Black-Box Testing, Equivalence Class Partitioning

---

## Concept 1: What is Unit Testing?

### 📌 Definition

Unit Testing means testing **individual methods, modules, classes, or components in isolation**. Each unit is tested **independently** of the other units.

### 🧠 Simple Explanation

Imagine you are building a machine with many parts. Before you put all the parts together, you check each part separately — does this gear work? Does this motor spin correctly? That is unit testing. You test **one piece at a time**, not the whole machine.

In software, a "unit" can be a single function, a method, a module, or a class. You test it **before integrating it** with the rest of the software.

### 🎯 Exam Important Points

- Unit testing is done **before integration** with other software parts.
- Each unit is tested **in isolation** (alone, separately).
- Unit testing checks if the code matches the **detailed design**, NOT the full system requirements.

---

## Concept 2: Drivers and Stubs

### 📌 What are they?

Since we are testing a unit **in isolation**, that unit may normally be called by some other unit and may also call some other units. To handle this, we write two kinds of small helper software:

- **Driver** — Simulates the unit that **calls** the unit under test and possibly supplies data to it.
- **Stub** — Simulates the unit that the unit under test **calls** (a function that has not yet been written or is not available).

### 🧠 Simple Explanation

Think of it like this: You want to test a worker (unit under test). Normally, a manager (another unit) gives the worker instructions, and the worker asks a helper (another unit) for some information.

- The **Driver** plays the role of the **manager** — it calls the worker and gives it input data.
- The **Stub** plays the role of the **helper** — when the worker asks for information, the stub gives a pre-stored, simple answer.

Both the driver and stub are **very simple software**. A stub might just be a lookup table — if the unit calls it with value 2, the stub returns 11. That is it.

### 🛠 How it works (from transcript)

- The unit under test might need **global variables** or **nonlocal data** — the Driver provides those.
- The Stub may just have some **sample stored values** and does a simple lookup when called.

### 🎯 Exam Important Points

- Driver = simulates the **caller** of the unit under test.
- Stub = simulates a function that the unit under test **calls**.
- Both are simple, temporary pieces of software.
- They exist because we are testing the unit **in isolation**.

### ⚠️ Common Confusion

Many students confuse driver and stub. Remember:

| | Driver | Stub |
|---|---|---|
| Role | Calls the unit under test | Gets called by the unit under test |
| Simulates | The calling function (above) | The called function (below) |
| Provides | Input data, global variables | Pre-stored return values |

---

## Concept 3: Unit Testing is a Verification Activity

### 📌 Key Question: Is unit testing Verification or Validation?

**Answer: Unit Testing is a Verification activity.**

### 🧠 Simple Explanation

- **Validation** = Testing the **entire system** against the **requirements specification** (Are we building the right product?).
- **Verification** = Testing a **single unit** against the **detailed design** (Are we building the product right?).

In unit testing, we are checking whether the **code of one unit** matches its **detailed design specification** — not the full system requirements. That is why it is **verification**, not validation.

### 🎯 Exam Important Points

- Unit testing checks code against **detailed design** (not requirements).
- Validation checks the **whole system** against **requirement specification**.
- Unit testing = **Verification**.

---

## Concept 4: Three Main Approaches to Design Unit Test Cases

### 📌 The Three Approaches

1. **Black-Box Approach** — Look only at the **input-output behavior** (specification). Do NOT look at the code.
2. **White-Box Approach (Glass-Box)** — Look at the **code** (internal structure). Design test cases based on whether code elements like statements, decisions, and conditions are covered.
3. **Grey-Box Approach** — Look at the **design** of the unit (not the code, not just input-output). Design test cases based on the design.

### 🧠 Simple Explanation

| Approach | What you look at | Also called |
|---|---|---|
| Black-Box | Input/Output specification only | Functional Testing |
| White-Box | Source code (internal structure) | Structural Testing |
| Grey-Box | Design of the unit | — |

Think of a vending machine:

- **Black-box:** You put in a coin (input), and check if you get the right drink (output). You do not open the machine.
- **White-box:** You open the machine and check every gear, wire, and motor to make sure each part works.
- **Grey-box:** You look at the **blueprint** of the machine (the design) and test based on that.

### 🎯 Exam Important Points

- Black-box = Functional Testing = No code knowledge needed.
- White-box = Structural Testing = Code knowledge needed.
- Grey-box = Design-based = Look at unit design only.

---

## Concept 5: Black-Box Testing (Detailed)

### 📌 Definition

In Black-Box testing, the software (unit) is treated as a **black box**. We design test cases using **only the functional specification** — without any knowledge of the internal structure (code) of the software.

### 🧠 Simple Explanation

You only know: "If I give input X, the output should be Y." You do not know or care how the software produces Y from X. The goal is to test the software thoroughly based on what it is supposed to do.

### 📌 Other Names for Black-Box Testing

- **Functional testing**
- **Data-driven testing**
- **Input/Output driven testing**

### 📌 Goal of Black-Box Testing

To achieve the **thoroughness of exhaustive input testing** (testing with ALL possible inputs), but with **as few test cases as possible**.

### 🎯 Exam Important Points

- No code knowledge needed.
- Based on functional specification only.
- Also called functional testing, data-driven testing, and input/output driven testing.
- Goal: maximum coverage with minimum test cases.

---

## Concept 6: What is Hard About Black-Box Testing?

### 📌 The Problem

The **data domain is extremely large**. For any practical software, the number of possible input values is huge. Testing all possible values is impossible.

### 🛠 Example from Transcript

Consider a simple function:

```
int check_equal(int x, int y)
```

This function takes two integers and returns 1 if they are equal, 0 if they are not.

On a **64-bit computer**:
- Domain of x = 2⁶⁴ values
- Domain of y = 2⁶⁴ values
- Total combinations = 2⁶⁴ × 2⁶⁴ = **2¹²⁸ combinations**

If you take **10 seconds** to manually enter each pair, it would take about **a billion years** to enter all possible values!

### 📌 Why not automate it?

Even automatic testing has problems:
- Each execution takes finite time, so it would still run for a very long time.
- If a result is produced automatically, we may not know if it is **correct** — the only thing we can detect easily is a **crash**.

### 📌 The Real Objective

Design a set of test cases that is **as effective as testing with all possible values**, but uses a **minimum number of test cases**.

### 🎯 Exam Important Points

- Data domain for practical software is extremely large.
- Multiple parameters make it worse (combinations).
- Exhaustive testing is impractical.
- Our goal: be as effective as exhaustive testing, but with fewer test cases.

---

## Concept 7: Solution — Domain-Based Testing (Domain Model)

### 📌 The Main Idea

Since the data domain is too large, we **construct a model of the domain**. This is called **Domain-Based Testing**. We select test data based on this **domain model** instead of trying all possible values.

### 🧠 Simple Explanation

Instead of testing every single input, we create a simplified "map" (model) of all possible inputs. Then, we pick smart representatives from this map. This way, we cover the important cases without testing everything.

### 🎯 Exam Important Points

- We construct a **domain model** of the input space.
- Test data is selected based on this model.
- This is the foundation of all black-box testing techniques.

---

## Concept 8: White-Box Testing (Brief Introduction)

### 📌 Definition

In White-Box testing, we need **knowledge of the internal structure** (source code) of the software to design test cases.

### 📌 Also Called

**Structural Testing** — because we look at the code structure.

### 🧠 Simple Explanation

You open up the software and look at the code. You design tests to make sure specific parts of the code — statements, decisions, conditions — are actually executed during testing.

### 🎯 Exam Important Points

- White-box testing = Structural testing.
- Requires knowledge of the code.
- Focuses on covering code elements (statements, decisions, conditions).

---

## Concept 9: Black-Box Testing Strategies (List)

### 📌 Strategies mentioned in the transcript

The transcript lists these black-box testing strategies:

1. **Scenario Coverage**
2. **Equivalence Class Partitioning**
3. **Special Value (Risk-Based) Testing**
4. **Boundary Value Testing**
5. **Cause-Effect (Decision Table) Testing**
6. **Combinatorial Testing**
7. **Orthogonal Array Testing**

### 🎯 Exam Important Points

- Know the names of all these strategies.
- This lecture focuses in detail on **Equivalence Class Partitioning** and briefly on **Scenario Coverage**.
- The others are listed for awareness and may be covered in later lectures.

---

## Concept 10: Equivalence Class Partitioning (Main Topic)

### 📌 Definition

The input values to a program are **partitioned into equivalence classes**. The partitioning is done such that the **program behaves in a similar way for every input value belonging to an equivalence class**.

### 🧠 Simple Explanation

Imagine you have a huge bag of inputs. You sort them into groups (classes). Within each group, every input makes the program behave the same way. So instead of testing every input, you just pick **one input from each group**. If it works for one value in the group, it works for all values in that group.

### 📌 Why does this work? (The Assumption/Premise)

The key assumption is: **all values in one equivalence class exercise the same set of program elements (same code paths)**. So:

- If the program succeeds for one value → it succeeds for all values in that class.
- If the program fails for one value → it fails for all values in that class.

### 📌 Relationship with Scenarios

At the very least, there should be **as many equivalence classes as there are scenarios** of operation. Each scenario corresponds to a different equivalence class because different code paths are exercised.

### 🎯 Exam Important Points

- Equivalence class partitioning divides the input domain into groups.
- Each group: program behaves the same for all values.
- Test with just **one value per class**.
- Minimum equivalence classes = number of scenarios.
- Based on the assumption that values in a class exercise the **same program elements**.

---

## Concept 11: How to Identify Equivalence Classes

### 📌 Methods to identify equivalence classes

1. **Identify scenarios** of operation.
2. **Examine the input data** — what ranges, types, or conditions does it have?
3. **Examine the output** — what different outputs can be produced?

---

## Concept 12: Guidelines for Designing Equivalence Classes

### 📌 Guideline 1: Input condition specifies a RANGE

→ You get **1 valid** and **2 invalid** equivalence classes.

**Example:** Area code must be between 10000 and 90000.

| Class | Description | Type |
|---|---|---|
| Class 1 | Value < 10000 | Invalid |
| Class 2 | 10000 ≤ Value ≤ 90000 | Valid |
| Class 3 | Value > 90000 | Invalid |

### 📌 Guideline 2: Input condition specifies a MEMBER OF A SET (specific value/format)

→ You get **1 valid** and **1 invalid** equivalence class.

**Example:** Password must be exactly a 6-character string.

| Class | Description | Type |
|---|---|---|
| Class 1 | String with fewer than 6 characters | Invalid |
| Class 2 | String with exactly 6 characters | Valid |
| Class 3 | String with more than 6 characters | Invalid |

(Note: The transcript mentions 1 valid and 1 invalid for set membership. For the password example specifically, the transcript describes 3 classes — less than 6, exactly 6, and more than 6.)

### 📌 Guideline 3: Input condition is BOOLEAN

→ You get **1 valid** and **1 invalid** equivalence class.

(True and False — one will be valid, one invalid depending on context.)

### 🎯 Exam Important Points

- Range → 1 valid + 2 invalid classes.
- Set membership → 1 valid + 1 invalid class.
- Boolean → 1 valid + 1 invalid class.
- These are the standard guidelines — memorize them for the exam.

---

## Concept 13: Equivalence Partitioning Example — Triangle Problem

### 📌 Problem Statement

A function takes **3 integers** (representing 3 sides of a triangle) and determines the **type of triangle**: Isosceles, Scalene, Equilateral, or Not a Triangle.

### 📌 How to design equivalence classes

Use **Scenario Coverage** first. The different scenarios (outputs) are:

| Scenario | Output |
|---|---|
| Scenario 1 | Isosceles |
| Scenario 2 | Scalene |
| Scenario 3 | Equilateral |
| Scenario 4 | Not a triangle |

Each scenario forms a separate equivalence class because **different program elements are exercised** for each type.

You pick **one test case from each class**:
- One set of sides that makes it Isosceles (e.g., 3, 3, 5)
- One set that makes it Scalene (e.g., 3, 4, 5)
- One set that makes it Equilateral (e.g., 5, 5, 5)
- One set that is Not a Triangle (e.g., 1, 2, 10)

### 🎯 Exam Important Points

- For the triangle problem, scenarios directly give equivalence classes.
- At minimum, number of equivalence classes = number of scenarios.
- In this case, at least 4 equivalence classes.

---

## Concept 14: Valid vs. Invalid Equivalence Classes (First-Level Partitioning)

### 📌 Definition

The **first level** of equivalence partitioning divides the input into two broad sets:

1. **Valid equivalence classes** — inputs that are legitimate and should be accepted.
2. **Invalid equivalence classes** — inputs that are illegal or outside the expected range.

### 🧠 Simple Explanation

Before going deeper, first separate the good inputs from the bad inputs. Then, within the valid set, find further sub-groups. Within the invalid set, find further sub-groups.

### 📌 How to create the test suite

Once all valid and invalid equivalence classes are identified:

→ **Pick one value randomly from each equivalence class** (both valid and invalid).

→ These selected values form the **equivalence class test suite**.

### 🎯 Exam Important Points

- First partition: Valid vs. Invalid.
- Then further partition within each.
- Pick **one value from each class** to form the test suite.
- This gives maximum coverage with minimum test cases.

---

## Concept 15: Scenario-Based Testing

### 📌 Definition

In scenario-based testing, you look at the **requirement specification** and identify all possible **scenarios of operation** for the functionality. Then you write test cases to **execute each scenario**.

### 🛠 Example from Transcript — Book Return System

| Scenario | Description |
|---|---|
| Scenario 1 | Book returned successfully |
| Scenario 2 | Book could not be returned because someone else had reserved it |
| Scenario 3 | Membership expired — cannot renew, must return the book |

Each scenario tests a different path through the software.

### 📌 Relationship to Equivalence Classes

- Each scenario corresponds to at least one equivalence class.
- If you know the scenarios, you know the minimum number of equivalence classes.
- There may be **more** equivalence classes than scenarios (because within a scenario, further partitioning may be possible).

### 🎯 Exam Important Points

- Scenario-based testing = testing each scenario of operation from the requirements.
- Minimum equivalence classes ≥ number of scenarios.
- Each scenario exercises different program elements.

---

# Summary Table: All Key Concepts of Lecture 5

| # | Concept | Key Point |
|---|---|---|
| 1 | Unit Testing | Testing individual units in isolation |
| 2 | Driver | Simulates the caller of the unit under test |
| 3 | Stub | Simulates a function called by the unit under test |
| 4 | Verification vs Validation | Unit testing = Verification (checks code against detailed design) |
| 5 | Black-Box Testing | Test using specification only; also called functional testing |
| 6 | White-Box Testing | Test using code; also called structural testing |
| 7 | Grey-Box Testing | Test using design of the unit |
| 8 | Hard about BB Testing | Data domain is too large; combinations explode |
| 9 | Domain-Based Testing | Build a model of the domain, select test data from it |
| 10 | Equivalence Class Partitioning | Divide input into classes where program behaves same |
| 11 | Guidelines for EC | Range → 1 valid + 2 invalid; Set → 1 valid + 1 invalid; Boolean → 1 valid + 1 invalid |
| 12 | Triangle Example | Scenarios = equivalence classes (Isosceles, Scalene, Equilateral, Not a Triangle) |
| 13 | Valid vs Invalid Classes | First-level partition; pick one value from each |
| 14 | Scenario-Based Testing | Test each scenario from requirements specification |

---

# 10 MCQs — Strictly from Lecture 5

---

### Q1. What is unit testing?

(a) Testing the entire system against requirements  
(b) Testing individual units in isolation  
(c) Testing the system after deployment  
(d) Testing only the user interface  

**Answer: (b)**  
**Explanation:** The transcript defines unit testing as testing individual methods, modules, classes, or components in isolation — before integrating them with other parts.

---

### Q2. What is the role of a "Driver" in unit testing?

(a) It simulates a function that the unit under test calls  
(b) It runs the entire software system  
(c) It simulates the function that calls the unit under test and supplies data to it  
(d) It is used only in integration testing  

**Answer: (c)**  
**Explanation:** A driver simulates the behavior of the function that calls the unit under test and possibly supplies some data to it. A stub (not driver) simulates the function that the unit calls.

---

### Q3. Unit testing is which type of activity?

(a) Validation  
(b) Verification  
(c) Both validation and verification  
(d) Neither  

**Answer: (b)**  
**Explanation:** Unit testing checks code against the detailed design specification, which is verification. Validation checks the entire system against requirements, which is not what unit testing does.

---

### Q4. Black-box testing is also known as:

(a) Structural testing  
(b) Grey-box testing  
(c) Functional testing  
(d) Code-based testing  

**Answer: (c)**  
**Explanation:** Black-box testing uses only the functional specification (input/output behavior) without looking at the code. Hence it is called functional testing. Structural testing is the name for white-box testing.

---

### Q5. What is the main difficulty in black-box testing?

(a) The source code is too complex  
(b) The data domain is extremely large  
(c) The software cannot be executed  
(d) There are no specifications available  

**Answer: (b)**  
**Explanation:** The transcript explains that the data domain is very large, and when multiple parameters are involved, the combinations make exhaustive testing impractical. For example, two 64-bit integers give 2¹²⁸ combinations.

---

### Q6. In equivalence class partitioning, if an input condition specifies a RANGE, how many equivalence classes are formed?

(a) 1 valid and 1 invalid  
(b) 2 valid and 1 invalid  
(c) 1 valid and 2 invalid  
(d) 2 valid and 2 invalid  

**Answer: (c)**  
**Explanation:** The guideline states: if the input specifies a range, there is 1 valid class (within the range) and 2 invalid classes (below the range and above the range).

---

### Q7. What is the key premise (assumption) behind equivalence class partitioning?

(a) All input values produce the same output  
(b) Any one value from an equivalence class exercises the same program elements as any other value from that class  
(c) Invalid inputs should never be tested  
(d) Each input value must be tested at least twice  

**Answer: (b)**  
**Explanation:** The transcript states that the basis for equivalence partitioning is that all values in one class exercise the same set of program elements. So testing one value is as good as testing all values in that class.

---

### Q8. For the triangle classification problem, what is the MINIMUM number of equivalence classes based on scenarios?

(a) 2  
(b) 3  
(c) 4  
(d) 6  

**Answer: (c)**  
**Explanation:** The scenarios are: Isosceles, Scalene, Equilateral, and Not a Triangle. Each is a different equivalence class, giving at least 4 classes.

---

### Q9. White-box testing is also known as:

(a) Functional testing  
(b) Data-driven testing  
(c) Structural testing  
(d) Scenario testing  

**Answer: (c)**  
**Explanation:** White-box testing requires knowledge of the internal structure (code) of the software. Hence it is called structural testing. Functional testing and data-driven testing are names for black-box testing.

---

### Q10. In equivalence class partitioning, the first-level partitioning divides input into:

(a) Boundary values and non-boundary values  
(b) Valid equivalence classes and invalid equivalence classes  
(c) Scenarios and non-scenarios  
(d) Code paths and data paths  

**Answer: (b)**  
**Explanation:** The transcript states that the first level of partitioning creates two sets — valid equivalence classes and invalid equivalence classes. Further partitioning is done within each set.

---

*End of Lecture 5 Complete Notes*
