# Lecture 7 — Special Value Testing (Boundary Value Testing)

**Course:** NPTEL Software Testing  
**Instructor:** Prof. Rajib Mall, IIT Kharagpur  
**Topic:** Special Value Testing — A Black-Box Testing Strategy

---

## Quick Recap (What We Already Know)

Before this lecture, we had already studied:

- Basic concepts of software testing.
- **Black-box testing techniques** — where we test without looking at the code.
- **Equivalence Class Partitioning (ECP)** — where we divide input data into classes/groups and test one value from each class.
- We also discussed **weak equivalence testing**, **strong equivalence testing**, and **robust testing**.

Now, in Lecture 7, we move to the next black-box testing strategy: **Special Value Testing**.

---

## Concept 1: What is Special Value Testing?

📌 **Concept Name:** Special Value Testing

🧠 **Simple Explanation:**

Special value testing is a black-box testing technique where the tester uses their experience and intuition ("hunch") to guess which input values are most likely to cause the program to fail. These are called **special values**.

Think of it like this: an experienced tester, after working on many projects, develops a gut feeling about which inputs are "risky" — that is, which values are most likely to expose a bug. Special value testing is about deliberately picking those risky values.

There are **two types** of special values discussed in the transcript:

**Type 1 — General Risk (Boundary Values):**

This applies to **all programs**, regardless of what they do. The idea is that programmers very commonly make mistakes at the **boundaries** (edges) of equivalence classes. This is because they use `if` statements or `switch` statements to separate different equivalence classes, and in those logical expressions, they often confuse things like `<` vs `<=`, or `>` vs `>=`. So, testing at boundaries catches these very common errors.

**Type 2 — Special Risk (Problem-Specific Values):**

This depends on the **specific problem**. An experienced tester might realize that certain edge cases are likely to be missed. For example, if a program computes the day of the week for a given date, a smart tester would check: "Has the programmer correctly handled **leap years**?" Because converting dates to day-of-the-week requires careful arithmetic, and leap years are a common source of bugs.

🎯 **Exam Important Points:**
- Special value testing relies on the **tester's experience and intuition**.
- **Boundary value testing** is a general-purpose special value test applicable to ALL programs.
- **Special risk values** are problem-specific and depend on the nature of the software.
- The reason programmers make errors at boundaries is because of confusion in **logical expressions** (like `<` vs `<=`).

---

## Concept 2: Why Do Errors Happen at Boundaries?

📌 **Concept Name:** Genesis of Boundary Errors

🧠 **Simple Explanation:**

When a programmer writes code, they need to separate different equivalence classes. They do this using **`if` statements** or **`switch` statements**. Inside these statements, they write **logical expressions** like:

- `if (age < 18)` → child
- `if (age >= 18 && age < 65)` → working adult

Now, the tricky part is: should it be `<` or `<=`? Should it be `>` or `>=`? This is where confusion happens. The programmer might accidentally write `< 18` when they meant `<= 18`, or vice versa. This means the value right at the boundary (like 18 itself) might be handled incorrectly.

That is why **boundary value testing** exists — we deliberately test the values that sit right on these edges, because that is where the most common mistakes are.

🛠 **Small Example from Transcript:**

Suppose a range-based equivalence class is defined as integers from **-3 to 10**. The boundaries are -3 and 10. If the programmer writes `if (x < 10)` instead of `if (x <= 10)`, then the input 10 would be wrongly classified. Testing with x = 10 would catch this bug.

🎯 **Exam Important Points:**
- Boundaries are error-prone because programmers confuse `<`, `<=`, `>`, `>=`.
- Special processing is required at the edges of equivalence classes.
- Boundary value testing checks whether the programmer correctly handled these edges.

---

## Concept 3: How to Pick Boundary Value Test Cases

📌 **Concept Name:** Selecting Boundary Values for a Range

🧠 **Simple Explanation:**

Given an equivalence class defined as a range bounded by values **a** and **b**, we pick the following test values:

1. **The minimum boundary itself** → value `a`
2. **Just above the minimum boundary** → value `a + 1` (for integers)
3. **The maximum boundary itself** → value `b`
4. **Just below the maximum boundary** → value `b - 1` (for integers)
5. **A normal/representative value** → any typical value from inside the range

So, for a single equivalence class, we pick **5 test cases** (without negative/invalid test cases).

🛠 **Example from Transcript:**

If the equivalence class is the integer range **-3 to 10**, the boundary value test cases are:

- **-3** (minimum boundary)
- **-2** (just above minimum)
- **10** (maximum boundary)
- **9** (just below maximum)
- **0** (a normal/representative value)

For **enumerated data** like {3, 5, 100, 102}, the boundary values would be:

- **3** (lower bound)
- **5** (just above lower bound)
- **102** (upper bound)
- **100** (just below upper bound)
- Plus a representative value

🎯 **Exam Important Points:**
- For one range equivalence class: pick **5 values** — min, min+1, max, max-1, and a representative.
- "Just above" and "just below" means the next valid value in the data type (for integers, +1 or -1).
- The representative value is any normal value from the middle of the range.

---

## Concept 4: HR Application Example — Boundary Errors in Specification

📌 **Concept Name:** Boundary Problems in Specifications (HR Application Example)

🧠 **Simple Explanation:**

The transcript gives an important example of an HR hiring application. The original (incorrect) specification says:

- Age 0 to 12 → Do not hire
- Age 12 to 18 → Hire as intern
- Age 18 to 65 → Hire full time
- Age above 65 → Do not hire

Now look at the problem: What happens when age = 12? It falls in BOTH the first class (0 to 12) AND the second class (12 to 18). Similarly, age = 18 falls in both "intern" and "full time". And age = 65 is also ambiguous.

This means the **specification itself has boundary errors**. If the programmer codes exactly what the spec says, the code will also have the same overlapping boundary problem. For instance, `if (age <= 12)` and `if (age >= 12 && age <= 18)` — both become true when age = 12.

📊 **The Corrected Specification:**

- Age 0 to 11 → Do not hire
- Age 12 to 17 → Hire as intern
- Age 18 to 64 → Hire full time
- Age 65 to 99 → Do not hire

Now there is no overlap. Each age value falls into exactly one category.

But notice — what about ages like **-3** or **101**? These are outside the defined ranges and also need to be tested (these are **negative test cases**).

📊 **Boundary Values to Test (from corrected spec):**

The values to test include: 0, -1, 11, 12, 17, 18, 64, 65, 99, 100, and nominal/representative values from each class.

🎯 **Exam Important Points:**
- Boundary errors can exist in the **specification** itself, not just in the code.
- Overlapping ranges at boundaries cause ambiguity.
- Always check that boundary values belong to exactly one equivalence class.
- Also test values outside the valid boundaries (negative test cases).

⚠️ **Common Confusion:**
- Students sometimes think boundary errors only happen in code. But this example clearly shows they can start from the **requirements/specification** stage.

---

## Concept 5: Pictorial Representation of Boundary Values

📌 **Concept Name:** Boundary Value Selection Pattern

🧠 **Simple Explanation:**

At the boundary of each equivalence class (both valid and invalid), we pick:

- **One value ON the boundary** (the exact boundary value)
- **One value JUST INSIDE the boundary** (one step into the valid zone)
- **One value JUST OUTSIDE the boundary** (one step into the invalid zone — this is a negative test case)

This pattern is applied at **both ends** (lower boundary and upper boundary) of each equivalence class.

🎯 **Exam Important Points:**
- The value just outside the boundary is used for **negative testing**.
- The value just inside the boundary is used for **positive testing**.
- This pattern is applied at BOTH the lower and upper boundary of each equivalence class.

---

## Concept 6: Example — Boundary 1 to 5000

📌 **Concept Name:** Boundary Values for Range 1 to 5000

🧠 **Simple Explanation:**

If the equivalence class is a range from **1 to 5000**, the boundary value test cases include:

- **1** (lower boundary)
- **0** (just outside lower boundary — negative test case)
- **2** (just inside lower boundary)
- **5000** (upper boundary)
- **5001** (just outside upper boundary — negative test case)
- **4999** (just inside upper boundary)
- **1000** (a representative/normal value from inside the range)

🎯 **Exam Important Points:**
- Total of 7 test cases when including both positive and negative test cases.
- Values outside the boundary are for checking whether the program properly rejects invalid input.

---

## Concept 7: Example — Average Age of Employees

📌 **Concept Name:** Boundary Value Testing for Employee Age Function

🧠 **Simple Explanation:**

Consider a function that reads the ages of employees from a file and computes their average age. Assume valid age range is **1 to 100**.

**Positive test cases (valid values)** — 5 test cases:

1. **1** (minimum boundary)
2. **2** (just above minimum)
3. **100** (maximum boundary)
4. **99** (just below maximum)
5. A **representative value** (e.g., some middle value)

**Negative test cases (invalid values):**

6. **0** (just below lower boundary)
7. **101** (just above upper boundary)

So, **5 test cases** if we only consider positive (valid) test cases, and **7 test cases** if we also include negative (invalid) test cases.

🎯 **Exam Important Points:**
- Without negative test cases → **5** test cases per variable.
- With negative test cases → **7** test cases per variable.
- Values outside the boundary are considered **negative test cases**.

---

## Concept 8: Boundary Value Testing with Multiple Parameters (Two Variables)

📌 **Concept Name:** Boundary Values for Multiple Independent Inputs

🧠 **Simple Explanation:**

What if a function takes **two input parameters** instead of one? The transcript gives this example:

- **Years of education:** range 1 to 23
- **Age:** range 1 to 100

If we treat them as **independent** (meaning the boundary of one does not affect the other), then:

- We need **5 boundary values** for years of education (1, 2, 23, 22, and a representative)
- We need **5 boundary values** for age (1, 2, 100, 99, and a representative)
- Plus **1 representative** that applies to both variables combined

Total = 5 + 5 + 1 = **11 test cases** (but can be reduced to **4n + 1** since the representative can be shared).

Wait — let's understand the formula properly.

📊 **The Formula for n Independent Parameters (without negative test cases):**

**Number of test cases = 4n + 1**

Where **n** = number of independent input parameters.

Why 4n? Because for each parameter, we have 4 special values: min, min+1, max, max-1. And then we add 1 for a shared representative value that covers all parameters.

For **n = 2**: 4 × 2 + 1 = **9** test cases.

But the transcript also mentions that if you count 5 per parameter plus a shared representative, you get 11. The formula 4n + 1 = 9 is used when the representative of one can serve as the nominal for the other. Either way, the key formula to remember is **4n + 1**.

🛠 **Pictorial Understanding:**

Imagine two axes: X-axis for parameter 1 (say, boundary between a and b) and Y-axis for parameter 2 (boundary between c and d). We pick special values along each axis independently, plus one point in the middle.

🎯 **Exam Important Points:**
- For **n independent inputs**, boundary value test cases (without negative) = **4n + 1**.
- This assumes we test ONE parameter at a time while keeping others at their nominal values.
- For n = 2: 4 × 2 + 1 = **9** test cases.

---

## Concept 9: When Both Parameters Have Combined Boundary Issues

📌 **Concept Name:** All Combinations of Boundary Values (Worst-Case Testing)

🧠 **Simple Explanation:**

The formula **4n + 1** assumes that at any one time, only **one parameter** has a boundary-related problem. That is, we are testing one variable's boundary while keeping the other at a normal value.

But what if the error only appears when **BOTH parameters** are at their boundary values simultaneously? In that case, we cannot test them independently. We must consider **all possible combinations** of the special values of both parameters.

For 2 parameters with 4 special values each:

- Instead of **4 + 4** (which gives 8 boundary values), we need **4 × 4 = 16** combinations.

So the assumption matters a lot:

- **Independent boundaries (single fault assumption):** 4n + 1 test cases
- **Combined boundaries (multiple fault assumption):** we need all combinations → 4 × 4 (not 4 + 4)

🎯 **Exam Important Points:**
- **4n + 1** assumes **single fault** — problem in one parameter at a time.
- If errors can occur only with a **specific combination** of boundary values across multiple parameters, we need **all combinations** (multiplicative, not additive).
- This is a very important distinction for the exam.

⚠️ **Common Confusion:**
- 4 + 4 = 8 (independent/additive) vs 4 × 4 = 16 (combinatorial/multiplicative) — students often mix these up.

---

## Concept 10: Robustness Testing

📌 **Concept Name:** Robustness Testing (Negative Test Cases at Boundaries)

🧠 **Simple Explanation:**

Robustness testing is when we **also include the negative test cases** — that is, values that are **outside** the valid equivalence classes (beyond the boundaries).

Purpose of robustness testing: to check whether the programmer has properly handled invalid inputs. Specifically, it checks:

- Did the programmer **expect** that invalid values could be given?
- Does the program issue an **informative error message** (like "Please enter a valid value")?
- Does the program have a **recovery mechanism** — does it just ignore the bad input and let the user try again, or does the user have to start from scratch?

📊 **Test Cases for Robustness Testing (per variable):**

For each variable, we now need **7 values** (instead of 5):

1. Minimum boundary
2. Just above minimum (inside)
3. Just below minimum (outside — **negative test case**)
4. Maximum boundary
5. Just below maximum (inside)
6. Just above maximum (outside — **negative test case**)
7. A representative/nominal value

📊 **Formula for Robustness Testing with n Parameters:**

**Number of test cases = 6n + 1**

Why 6n? Because for each variable, there are 3 values at the lower boundary (on, inside, outside) and 3 values at the upper boundary (on, inside, outside), giving 6 per variable. Plus 1 representative.

🎯 **Exam Important Points:**
- Robustness testing = boundary value testing **WITH negative test cases**.
- Per variable: **7 test cases** (5 positive + 2 negative).
- Formula for n parameters: **6n + 1**.
- It checks whether the program handles **invalid inputs gracefully**.

---

## Concept 11: Boolean and Non-Numerical Variables (Introduction)

📌 **Concept Name:** Special Value Testing for Boolean and Non-Numerical Variables

🧠 **Simple Explanation:**

The transcript raises an important question: everything we discussed so far is about **numerical boundaries**. But what about:

- **Boolean variables** — like radio buttons in a user interface that can be either ON or OFF (marked or unmarked)?
- **Non-numerical variables** — like strings?

How do we apply boundary value testing to these types?

The transcript mentions this as an open question and hints that it will be discussed further. It notes that Boolean values are very common in user interfaces (radio buttons, checkboxes).

🎯 **Exam Important Points:**
- Boundary value testing was discussed mainly for **numerical** inputs.
- For **Boolean** variables (e.g., radio buttons): there are only two values — true/false.
- For **string** variables: the concept of boundaries is different (e.g., empty string, very long string, special characters).
- The transcript introduces this topic but does not provide full details here — it says "let us discuss these cases."

> ⚠️ *The transcript does not provide further details on Boolean/string boundary testing in this lecture.*

---

## Concept 12: Quiz — Black-Box Test Suite for Quadratic Equation Solver

📌 **Concept Name:** Designing Equivalence Classes for a Quadratic Equation Solver

🧠 **Simple Explanation:**

The lecture ends with a quiz problem. Consider a function called **quad_solver** that takes three floating-point parameters **a, b, c** and displays the solution of the equation **ax² + bx + c = 0**.

For example: quad_solver(5.0, 7.0, 2.0)

The quiz asks: what would be the **equivalence classes** for testing this function?

📊 **Step-by-Step Breakdown:**

**Step 1 — Identify Valid Equivalence Classes (Based on Scenarios):**

The key is the **discriminant**: b² - 4ac

- **b² - 4ac > 0** → Two **distinct real roots** (unique roots)
- **b² - 4ac = 0** → Two **coincident roots** (same/repeated roots)
- **b² - 4ac < 0** → Two **imaginary/complex roots**

So there are **3 valid equivalence classes**: unique roots, coincident roots, and complex roots.

**Step 2 — Identify Invalid Equivalence Classes:**

- **a = 0, b = 0, c = 0** → All coefficients are zero — this is not a valid equation (invalid input)
- **a, b, c are character strings** instead of numbers — this is also invalid input

**Step 3 — Summary of Equivalence Classes:**

*Valid:*
1. Complex roots (b² - 4ac < 0)
2. Coincident roots (b² - 4ac = 0)
3. Unique/distinct real roots (b² - 4ac > 0)

*Invalid:*
4. a, b, c are all 0
5. a, b, c are character strings (non-numeric)

**Step 4 — Pick Representative Test Values:**

Choose values of a, b, c such that b² - 4ac is:
- **Positive** (for distinct real roots)
- **Zero** (for coincident roots)
- **Negative** (for complex roots)
- Also pick invalid inputs

🎯 **Exam Important Points:**
- For quadratic solver, equivalence classes are based on the **discriminant** (b² - 4ac).
- Three valid classes: distinct real, coincident, imaginary.
- Two invalid classes: all zeros, character strings.
- This is a classic NPTEL exam question — designing equivalence classes for a given function.
- Note: the quiz asks for a **black-box test suite** (not just special value testing), so the **first step is always equivalence class partitioning**.

---

## Concept 13: Summary of the Lecture

📌 **Concept Name:** Lecture Summary and Next Steps

🧠 **Key Takeaways from Lecture 7:**

1. **Special value testing** is a black-box technique that uses the tester's experience to identify risky input values.
2. **Boundary value testing** is the most common form of special value testing and is applicable to ALL programs.
3. Programmers make mistakes at boundaries due to confusion in logical operators (`<`, `<=`, `>`, `>=`).
4. For a single variable with a range [a, b], we test: a, a+1, b, b-1, and a representative value (**5 test cases**).
5. Including negative test cases (outside the boundary): **7 test cases** per variable.
6. For **n independent parameters** (without negative): **4n + 1** test cases.
7. For **n independent parameters** with robustness testing: **6n + 1** test cases.
8. If errors can only occur with a **combination** of boundary values from multiple parameters, we need **multiplicative** combinations (e.g., 4 × 4), not additive (4 + 4).
9. Robustness testing checks whether the program handles invalid inputs gracefully (error messages, recovery).
10. Boolean and non-numerical variables in boundary testing were introduced but not fully covered in this lecture.
11. For complex problems like the quadratic equation solver, start with **equivalence class partitioning** (based on scenarios), then apply special value testing.

The lecture concludes by saying that equivalence testing and special value testing require **a lot of practice**, and the course will next move to **combinatorial testing**.

---

## All Important Formulas at a Glance

| Scenario | Formula | Example (n=2) |
|---|---|---|
| BVT per single variable (positive only) | 5 test cases | — |
| BVT per single variable (with negative) | 7 test cases | — |
| BVT for n independent inputs (positive only) | **4n + 1** | 4(2)+1 = 9 |
| BVT for n independent inputs (robustness) | **6n + 1** | 6(2)+1 = 13 |
| Combined boundary (worst case, 2 variables) | **4 × 4 = 16** (multiplicative) | Not 4+4=8 |

---

---

# 10 MCQs — Lecture 7: Special Value Testing

---

**Q1.** What is the main reason programmers commit errors at the boundaries of equivalence classes?

(A) They do not understand the problem requirements  
(B) They get confused between logical operators like `<`, `<=`, `>`, `>=` in if/switch statements  
(C) They do not test their code at all  
(D) They use wrong programming languages  

**Answer: (B)**  
**Explanation:** The transcript explicitly states that the likely confusion is between less than, less than equal to, greater than, and greater than equal to in the logical expressions used in if/switch statements to distinguish equivalence classes.

---

**Q2.** For a single equivalence class with range 1 to 100, how many boundary value test cases are needed WITHOUT considering negative test cases?

(A) 3  
(B) 4  
(C) 5  
(D) 7  

**Answer: (C)**  
**Explanation:** The 5 test cases are: minimum (1), just above minimum (2), maximum (100), just below maximum (99), and one representative value. This is directly from the transcript's employee age example.

---

**Q3.** For the same range 1 to 100, how many test cases are needed if we ALSO include negative test cases?

(A) 5  
(B) 6  
(C) 7  
(D) 9  

**Answer: (C)**  
**Explanation:** We add 2 negative test cases (0 and 101 — just outside each boundary) to the 5 positive ones, making it 7. The transcript explicitly states this for the employee age example.

---

**Q4.** For n independent input parameters, the number of boundary value test cases (without negative test cases) is:

(A) 5n  
(B) 4n  
(C) 4n + 1  
(D) 6n + 1  

**Answer: (C)**  
**Explanation:** The transcript gives the formula 4n + 1 for boundary value testing without negative test cases. The 4 comes from: min, min+1, max, max-1 for each parameter, and +1 for a shared representative value.

---

**Q5.** In robustness testing for n independent parameters, the number of test cases is:

(A) 4n + 1  
(B) 5n + 1  
(C) 6n + 1  
(D) 7n  

**Answer: (C)**  
**Explanation:** The transcript states: "for robustness test, we need 6n + 1." For each variable, there are 3 values at each boundary (on, inside, outside) × 2 boundaries = 6, plus 1 representative.

---

**Q6.** In the HR application example, what was the problem with the original specification (ages 0–12, 12–18, 18–65, 65+)?

(A) The ranges did not cover all possible ages  
(B) The ranges overlapped at boundary values (12, 18, 65)  
(C) The specification had too many equivalence classes  
(D) The specification was written in the wrong format  

**Answer: (B)**  
**Explanation:** The transcript clearly points out that age 12 falls in both "do not hire" and "hire as intern," age 18 falls in both "intern" and "full time," and 65 is also ambiguous. The specification has overlapping boundaries.

---

**Q7.** If errors can only occur when BOTH parameters have specific boundary values simultaneously (not independently), we need:

(A) 4 + 4 = 8 combinations  
(B) 4 × 4 = 16 combinations  
(C) 4n + 1 test cases  
(D) 6n + 1 test cases  

**Answer: (B)**  
**Explanation:** The transcript explicitly states: "we will have to consider all possible combinations, so that is 4 into 4, not 4 plus 4." Multiplicative, not additive.

---

**Q8.** What does robustness testing specifically check?

(A) Whether the program runs fast enough  
(B) Whether the programmer handled invalid inputs, issued informative error messages, and provided recovery  
(C) Whether the code has enough comments  
(D) Whether the program passes all positive test cases  

**Answer: (B)**  
**Explanation:** The transcript says robustness testing checks: did the programmer expect invalid values, did they issue an informative message, and does the program have recovery or does the user have to start fresh.

---

**Q9.** For the quadratic equation solver (ax² + bx + c = 0), which of the following is NOT a valid equivalence class?

(A) Distinct real roots (b² - 4ac > 0)  
(B) Coincident roots (b² - 4ac = 0)  
(C) Imaginary roots (b² - 4ac < 0)  
(D) Infinite roots (b² - 4ac = ∞)  

**Answer: (D)**  
**Explanation:** The transcript identifies exactly three valid equivalence classes: distinct real roots, coincident roots, and imaginary roots — all based on the discriminant (b² - 4ac) being positive, zero, or negative. "Infinite roots" is not mentioned in the transcript.

---

**Q10.** What is the special value that an experienced tester would check when testing a "day of the week for a given date" program?

(A) Very large dates like year 9999  
(B) Dates involving leap years  
(C) Dates in different time zones  
(D) Dates stored in different formats  

**Answer: (B)**  
**Explanation:** The transcript gives this exact example — an experienced tester would check whether leap years have been taken into account, because the date-to-day conversion requires careful arithmetic and leap years are a common source of bugs.

---

## What Else & Remaining Topics in Lecture 7

All topics from the Lecture 7 transcript have been fully covered above. The lecture concludes by stating that the next topic will be **Combinatorial Testing** (covered in subsequent lectures). No topics from Lecture 7 have been skipped.
