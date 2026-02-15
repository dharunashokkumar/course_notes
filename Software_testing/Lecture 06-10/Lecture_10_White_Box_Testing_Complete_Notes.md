# Lecture 10 – White Box Testing

**Course:** NPTEL Software Testing | **Instructor:** Prof. Rajib Mall, IIT Kharagpur

---

## Overview of This Lecture

This lecture moves from Black-box testing (covered in earlier lectures) to **White-box testing**. It covers what white-box testing is, why we need both black-box and white-box testing, and then explains several **coverage-based** white-box testing strategies one by one: Statement Coverage, Branch Coverage, Condition Coverage, Basic Condition Testing, Multiple Condition Coverage, and an introduction to MC/DC. It also introduces the concepts of **stronger**, **weaker**, and **complementary** testing.

---

## Concept 1: What is White-Box Testing?

📌 **Concept Name:** White-Box Testing (also called Structural Testing)

🧠 **Simple Explanation:**

In black-box testing, you design test cases by looking at the **requirements** or the **specification** — you do not care about how the code is written inside. But in white-box testing, you design test cases by looking at the **code structure** of the program itself.

Think of it this way: Imagine a machine. In black-box testing, you only look at what goes in and what comes out. In white-box testing, you open the machine and look at all the internal parts, wires, and connections, and then you test based on what you see inside.

White-box testing is also called **structural testing** because the test cases are developed by examining the structure of the code.

As per the transcript, there are about a dozen white-box testing strategies, but this lecture focuses on 6–7 important ones.

🎯 **Exam Important Points:**
- White-box testing = Structural testing
- Test cases are designed based on the **code structure** of the program
- There are many white-box testing strategies (about a dozen); this course covers 6–7 important ones

---

## Concept 2: White-Box Testing Strategies — Two Categories

📌 **Concept Name:** Coverage-Based vs. Fault-Based Testing

🧠 **Simple Explanation:**

All white-box testing strategies can be broadly classified into two categories:

**1. Coverage-Based Testing:**
Here, you design test cases so that certain **program elements** get executed (or "covered"). For example, you might want every statement to be executed, or every branch to be taken. The goal is: "Have I covered enough of the code?"

Examples mentioned in the transcript: Statement Coverage, Branch Coverage, Path Coverage, Condition Coverage, MC/DC Coverage, Data Flow-Based Testing.

**2. Fault-Based Testing:**
Here, you design test cases to **expose a specific category of faults**. For example, you know that programmers commonly make certain types of mistakes (like using the wrong variable type — writing `float` instead of `int`). So you design test cases that specifically check whether those types of faults exist.

Example mentioned in the transcript: **Mutation Testing**.

🛠 **Small Example:**
- Coverage-based: "I want to make sure every line of my code runs at least once" → Statement Coverage
- Fault-based: "Programmers often type `>` instead of `>=`. Let me check if that mistake exists" → Mutation Testing

🎯 **Exam Important Points:**
- White-box strategies = Coverage-based + Fault-based
- Coverage-based → cover program elements (statements, branches, conditions, paths)
- Fault-based → target specific types of faults
- Mutation testing is the main example of fault-based testing mentioned in this lecture

---

## Concept 3: Why Do We Need Both Black-Box AND White-Box Testing?

📌 **Concept Name:** Why Both BB and WB Testing Are Necessary (Complementary Testing Strategies)

🧠 **Simple Explanation:**

A very important question: If you do thorough black-box testing, can you skip white-box testing? Or if you do thorough white-box testing, can you skip black-box testing?

The answer is **NO — you need both**. Here is why:

**Limitations of Black-Box Testing:**
- It is **impossible** to write a test case for every possible set of inputs and outputs (the input space is too large).
- Some parts of the code may not be reachable through normal input testing.
- It **does not tell** if extra functionality (like hidden code or Trojans) has been implemented in the code.
- Example from transcript: A programmer may have written hidden code (a Trojan) that activates only for certain specific input combinations. Black-box testing won't find this because you don't know what those specific inputs are.

**Limitations of White-Box Testing:**
- It **does not check** whether the program matches the specification (requirements).
- It **does not tell** if all the required functionality has actually been implemented.
- It **does not uncover** any missing program logic.
- Example from transcript: If the requirements say "when this input is given, this action should happen" but the programmer forgot to write that code entirely, white-box testing will never find this because there is no code to examine.

**Conclusion:** Both are necessary. They are called **complementary testing strategies** — each one catches bugs that the other one misses.

🎯 **Exam Important Points:**
- Black-box testing alone is NOT sufficient (cannot find Trojans, hidden code, unreachable code parts)
- White-box testing alone is NOT sufficient (cannot find missing functionality, does not check against specification)
- Both are **complementary** — each detects bugs the other cannot
- This is a very exam-important concept; expect direct questions on limitations of each

⚠️ **Common Confusion:**
Students sometimes think "if I test everything through code (white-box), I've covered everything." But remember: white-box testing only tests what IS in the code. If something is MISSING from the code, white-box testing will never find it. Only black-box testing (which checks against requirements) can find missing functionality.

---

## Concept 4: Types of Program Element Coverage

📌 **Concept Name:** Types of Program Element Coverage

🧠 **Simple Explanation:**

Within coverage-based testing, different strategies target different **program elements**. The transcript lists these types:

| Coverage Type | What It Means |
|---|---|
| **Statement** | Each statement is executed at least once |
| **Branch** | Each branch is traversed (and every entry point taken) at least once |
| **Condition** | Each condition takes True at least once and False at least once |
| **Multiple Condition** | All combinations of condition coverage are achieved |
| **Path** | All program paths are covered |
| **Dependency** | Data dependencies are covered |

The lecture then goes into detail on statement, branch, condition, basic condition, and multiple condition coverage.

🎯 **Exam Important Points:**
- Know all the types listed above and their one-line definitions
- These represent a hierarchy from weaker to stronger testing

---

## Concept 5: Stronger and Weaker Testing

📌 **Concept Name:** Stronger and Weaker Testing

🧠 **Simple Explanation:**

Among coverage-based strategies, some are "stronger" and some are "weaker."

**Stronger testing** means:
- Strategy A is stronger than Strategy B if A covers **all** the program elements that B covers, AND also covers **some additional elements** that B does not cover.
- In other words, stronger testing is a **superset** of weaker testing.

**Weaker testing** means:
- It covers only a **subset** of the program elements covered by the stronger testing.

**Why does this matter?** If you are doing a stronger testing, you do NOT need to also do the weaker testing separately. The stronger one has already covered everything the weaker one would cover, plus more.

🛠 **Small Example:**
If Branch Coverage is stronger than Statement Coverage, then by doing branch coverage, you automatically achieve statement coverage. No need to do statement coverage separately.

🎯 **Exam Important Points:**
- Stronger testing = superset of weaker testing
- Stronger covers all elements of weaker + additional elements
- If you do stronger testing, weaker testing becomes redundant

---

## Concept 6: Complementary Testing

📌 **Concept Name:** Complementary Testing

🧠 **Simple Explanation:**

Two testing strategies are called **complementary** if:
- They cover some program elements **in common** (there is overlap)
- But they also each cover some program elements that the **other does not cover**

Think of two overlapping circles (like a Venn diagram). The overlapping part is what both strategies cover. The non-overlapping parts are what each strategy covers uniquely.

This is different from stronger/weaker where one is a complete subset of the other. In complementary testing, neither is a subset of the other — each one brings something unique.

As discussed earlier, **black-box and white-box testing are complementary** — they overlap somewhat but each catches bugs the other misses.

🎯 **Exam Important Points:**
- Complementary = overlap exists, but each covers unique elements too
- Black-box and White-box testing are complementary to each other
- This is different from stronger/weaker relationship

---

## Concept 7: Statement Coverage

📌 **Concept Name:** Statement Coverage

🧠 **Simple Explanation:**

This is the **simplest** white-box testing strategy.

**Goal:** Design test cases so that **every statement in the program is executed at least once**.

**Why?** The logic is simple: unless a statement is executed, you have no way of knowing if there is an error in that statement. If a line of code never runs during testing, any bug hiding in that line will go undetected.

**How to measure it:**

**Statement Coverage (%) = (Number of executed statements / Total number of statements) × 100**

So if your program has 100 statements and your test cases execute 80 of them, you have 80% statement coverage.

**Important limitation:** Even if a statement works correctly for one input value, there is **no guarantee** that it will work correctly for all input values. Statement coverage only ensures each line runs at least once — not that it runs for every possible scenario.

**Goal in practice:** Normally we expect **100% statement coverage** (unless there is unreachable code).

🛠 **Example from Transcript — Euclid's GCD Algorithm:**

```
int f1(int x, int y){
1   while (x != y){
2       if (x > y) then
3           x = x - y;
4       else y = y - x;
5   }
6   return x;
}
```

This program computes the Greatest Common Divisor (GCD) of two numbers. It has 6 statements.

To achieve 100% statement coverage, we need test cases that execute ALL 6 statements. We need:
- `x != y` to be true (to enter the loop) → so x and y must be different
- `x > y` to be true (to execute line 3) → so we need a case where x > y
- `x > y` to be false (to execute line 4) → so we need a case where x ≤ y
- `x == y` eventually (to exit the loop and reach line 6)

Test set that achieves 100% statement coverage: **{(x=3, y=3), (x=4, y=3), (x=3, y=4)}**

With these three test cases, all 6 statements get executed at least once.

**Practical note from transcript:** For very small programs, you can manually figure out which inputs achieve 100% statement coverage. But for larger programs, you typically give **random inputs** and use a **coverage tool** to check what percentage of statements got covered. You keep giving more random inputs until you reach 100% statement coverage.

📊 **Step-by-step for the GCD example:**
1. (x=3, y=3) → x == y, so loop does not execute, goes directly to line 6. Covers: lines 1, 6.
2. (x=4, y=3) → x != y, enters loop; x > y is true, so x = 4-3 = 1; now x != y, x < y, so y = 3-1 = 2; continues... Covers: lines 1, 2, 3, 4, 5, 6.
3. (x=3, y=4) → x != y, enters loop; x > y is false, so y = 4-3 = 1; continues... Covers lines 1, 2, 4, 5, 6.

Together, all statements are covered.

🎯 **Exam Important Points:**
- Statement coverage is the **simplest and weakest** white-box testing strategy
- Formula: (executed statements / total statements) × 100
- Goal: every statement executed at least once
- Rationale: a fault in a statement can only be revealed by executing that faulty statement
- Limitation: correct behavior for one input does not guarantee correct behavior for all inputs
- For large programs: use random inputs + coverage tools to achieve 100%

⚠️ **Common Confusion:**
"100% statement coverage means the program is bug-free" — **WRONG**. 100% statement coverage only means every line ran at least once. There could still be bugs that only appear with specific input combinations that were not tested.

---

## Concept 8: Branch Coverage (Decision Coverage)

📌 **Concept Name:** Branch Coverage (also called Decision Coverage)

🧠 **Simple Explanation:**

Branch coverage is a **stronger** testing strategy compared to statement coverage.

**Goal:** Design test cases so that each **branch condition** (like `if`, `while`, `for`) assumes **both true and false** values.

In any program, whenever there is a decision point (like an `if` statement or a `while` loop), the condition can evaluate to either true or false. Branch coverage requires that your test cases make each decision go **both ways** — take the true path at least once AND take the false path at least once.

**How to measure it:**

**Branch Coverage (%) = (Number of executed branches / Total number of branches) × 100**

**Adequacy criterion:** Each branch (edge in the Control Flow Graph) must be executed at least once.

🛠 **Example from Transcript — Same GCD Algorithm:**

In this program, there are **two branch conditions:**
1. `while (x != y)` → can be true or false
2. `if (x > y)` → can be true or false

So there are **4 possible branch outcomes** (each condition × true/false = 2 × 2 = 4).

For 100% branch coverage, we need test cases that make:
- `while (x != y)` → true AND false
- `if (x > y)` → true AND false

Test set for branch coverage: **{(x=3, y=3), (x=3, y=2), (x=4, y=3), (x=3, y=4)}**

These test cases ensure both conditions take both true and false values.

**Practical note from transcript:** For small programs, you can manually design test cases for branch coverage. For larger programs, give random inputs and use a **coverage tool** that reports branch coverage percentage. Open source tools are available for this.

🎯 **Exam Important Points:**
- Branch coverage = Decision coverage (both names are used)
- Each branch condition must take both true and false values
- Formula: (executed branches / total branches) × 100
- Branch coverage is **stronger** than statement coverage
- For GCD: 2 branch conditions → 4 branch outcomes

---

## Concept 9: Branch Coverage is Stronger than Statement Coverage

📌 **Concept Name:** Branch Coverage is Stronger than Statement Coverage

🧠 **Simple Explanation:**

This is a very important relationship to understand for the exam.

**Claim:** Branch coverage is a **stronger** testing strategy compared to statement coverage.

**To prove this, two things must be shown:**

**Part 1: Branch coverage guarantees statement coverage.**
The argument: Every statement lies on some branch. So, if all branches are covered (both true and false paths are taken), then all statements must have been executed. Therefore, achieving 100% branch coverage automatically gives you 100% statement coverage.

**Part 2: Statement coverage does NOT guarantee branch coverage.**
We need to show at least one example where statement coverage is achieved but branch coverage is NOT.

**Example from transcript:**
Consider this simple code:
```
if (a > b)
    printf("...");
```

If we give a test case where `a > b` is true, the `printf` statement executes. Since `printf` is the only statement inside the `if`, we achieve **100% statement coverage**.

But we have NOT tested the case where `a > b` is false (the condition taking the false path). So **branch coverage is NOT achieved** — we need another test case where `a ≤ b`.

This proves that statement coverage does NOT guarantee branch coverage.

**Conclusion:** Branch coverage is stronger than statement coverage. If you achieve branch coverage, you automatically get statement coverage. But statement coverage does NOT give you branch coverage.

**Practical implication:** If you are doing branch coverage testing, you do NOT need to separately do statement coverage testing.

🎯 **Exam Important Points:**
- Branch coverage → automatically gives statement coverage (stronger includes weaker)
- Statement coverage → does NOT automatically give branch coverage
- The `if(a>b) printf(...)` example is key: only true path covers all statements but misses false branch
- This is a very likely exam question

⚠️ **Common Confusion:**
"If I have 100% statement coverage, I must have 100% branch coverage too" — **WRONG**. Statement coverage can be 100% even when some branches (like the false branch of an `if` without `else`) have never been tested.

---

## Concept 10: Limitations of Branch Coverage — All Branches Can Still Miss Conditions

📌 **Concept Name:** Branch Coverage Can Still Miss Condition-Level Bugs

🧠 **Simple Explanation:**

Even if you achieve **100% branch coverage**, there can still be bugs that are not detected! How?

When a branch condition is a **compound condition** (made up of multiple sub-conditions joined by AND, OR), branch coverage only checks whether the overall condition becomes true and false. It does NOT check whether **each individual sub-condition** has been tested independently.

🛠 **Example from Transcript:**

Consider this condition:
```
digit_high == 1 || digit_low == -1
```

This is a compound condition with two parts joined by OR (`||`).

For **branch coverage**, we just need the overall expression to become true once and false once.

We can achieve this by **only varying `digit_low`:**
- `digit_low = -1` → expression is true (because of OR, the second part being true makes the whole thing true)
- `digit_low = 0` and `digit_high = 0` → expression is false

Branch coverage is achieved! But notice: we never tested what happens when `digit_high == 1` is true. If there is a bug related to `digit_high` (say, a missing negation operator), branch coverage would **not detect it**.

**The faulty sub-expression might not be tested even though we test both outcomes of the branch.**

This motivates the need for **condition coverage** — a stronger strategy.

🎯 **Exam Important Points:**
- 100% branch coverage does NOT guarantee all individual sub-conditions are tested
- Compound conditions (using AND/OR) can hide untested sub-conditions
- This is WHY we need condition coverage — to test each sub-condition individually

---

## Concept 11: Condition Coverage (Multiple Condition — MC — Coverage)

📌 **Concept Name:** Condition Coverage (also called Multiple Condition Coverage)

🧠 **Simple Explanation:**

Condition coverage addresses the limitation of branch coverage we just discussed.

**Goal:** Design test cases so that **each component (sub-condition)** of a composite conditional expression is made to assume **both true and false values**.

So if you have a condition like `if (c1 AND c2 OR c3)`, condition coverage requires:
- c1 must take true and false
- c2 must take true and false
- c3 must take true and false

This is called **condition coverage** or **multiple condition (MC) coverage** in the transcript.

**Important note:** When there is only ONE condition in an expression (no compound condition), then condition coverage and branch coverage are the **same thing**. The difference only matters when there are **multiple sub-conditions** in a single expression.

🛠 **Example from Transcript:**

Consider: `(c1 AND c2) OR c3`

For condition coverage, we need:
- c1 → true at least once, false at least once
- c2 → true at least once, false at least once
- c3 → true at least once, false at least once

🎯 **Exam Important Points:**
- Condition coverage = each component condition takes both true and false
- Also called Multiple Condition (MC) coverage in the transcript
- Stronger than branch coverage
- If only one condition → same as branch coverage
- Difference shows up only with compound/composite conditions

---

## Concept 12: Basic Condition Testing

📌 **Concept Name:** Basic Condition Testing

🧠 **Simple Explanation:**

Basic condition testing is a specific way to achieve condition coverage.

**Adequacy criterion:** Each **basic condition** (each individual sub-condition) must be executed at least once with a true value and at least once with a false value.

**How to measure it:**

**Coverage (%) = (Number of truth values taken by all basic conditions) / (2 × Number of basic conditions)**

The denominator is `2 × n` because each of the `n` basic conditions needs to take 2 values (true and false). So the maximum number of truth values to be covered is `2n`.

🛠 **Example from Transcript:**

Consider: `if (c1 AND c2 OR c3)`

Here there are 3 basic conditions: c1, c2, c3.

For basic condition testing, we need only **2 test cases** to ensure each takes true and false:
- Test case 1: c1=T, c2=T, c3=T
- Test case 2: c1=F, c2=F, c3=F

OR alternatively:
- Test case 1: c1=T, c2=F, c3=T
- Test case 2: c1=F, c2=T, c3=F

In either case, each condition takes true once and false once. So 2 test cases are enough for basic condition testing.

The total number of truth values needed = 2 × n. For 3 basic conditions = 2 × 3 = 6 truth values.

🎯 **Exam Important Points:**
- Basic condition testing: each basic condition → true at least once and false at least once
- Coverage formula: (truth values taken by all basic conditions) / (2 × number of basic conditions)
- For n basic conditions, minimum 2 test cases needed (in the best case)
- Does NOT require all possible combinations

---

## Concept 13: Multiple Condition Coverage (All Combinations)

📌 **Concept Name:** Multiple Condition Coverage

🧠 **Simple Explanation:**

Multiple condition coverage takes things further than basic condition testing.

**Goal:** Test **ALL possible combinations** of truth values of all sub-conditions.

If you have `n` component conditions, each can be true or false (2 values each). So the total number of combinations = **2^n** (2 to the power n).

🛠 **Example from Transcript:**

Consider: `(c1 AND c2) OR c3` — there are 3 conditions.

For multiple condition coverage, you need 2³ = **8 test cases:**
- (T, T, T), (T, T, F), (T, F, T), (T, F, F), (F, T, T), (F, T, F), (F, F, T), (F, F, F)

Every single combination of c1, c2, c3 must be tested.

**The problem:** When the number of conditions is large, the number of test cases grows **exponentially**. For example, in embedded control applications, you might have 20 or 25 conditions in one expression. That would need 2²⁰ = over 1 million test cases, or 2²⁵ = over 33 million. This is simply **too many** test cases.

🎯 **Exam Important Points:**
- Multiple condition coverage = test ALL combinations of sub-conditions
- Number of test cases = 2^n (where n = number of component conditions)
- Exponential growth makes it impractical for large n
- This motivates the need for MC/DC (discussed in next lecture)

⚠️ **Common Confusion:**
Basic condition testing ≠ Multiple condition coverage.
- Basic condition testing: each condition takes T and F (minimum 2 test cases, does NOT test all combinations)
- Multiple condition coverage: ALL 2^n combinations tested

---

## Concept 14: The Strength Hierarchy of Coverage Strategies

📌 **Concept Name:** Hierarchy: Weakest to Strongest

🧠 **Simple Explanation:**

The transcript presents a clear hierarchy from weakest to strongest:

```
Multiple Condition (Strongest)
       ↓
Basic Condition
       ↓
Decision / Branch Coverage
       ↓
Statement Coverage (Weakest)
```

What this means:
- **Multiple condition** is the strongest — it covers everything the others cover, plus more.
- If you do **multiple condition** testing, you do NOT need to do basic condition, decision, or statement coverage separately.
- **Statement coverage** is the weakest — it covers the least.

**Key relationships explained in the transcript:**
- Branch testing is the **simplest condition testing strategy** — it only checks if the entire compound condition becomes true and false, NOT individual sub-conditions.
- Basic condition is stronger than branch testing because it checks individual sub-conditions.
- Multiple condition is the strongest because it checks ALL combinations.

**The practical problem:** Multiple condition coverage requires 2^n test cases, which is too many for real programs. This is why **MC/DC (Multiple Condition/Decision Coverage)** was developed — it tries to achieve almost the same bug-detection capability as multiple condition coverage but with far fewer test cases. MC/DC is introduced at the end of this lecture and will be covered in detail in the next lecture.

🎯 **Exam Important Points:**
- Hierarchy (weakest → strongest): Statement → Branch/Decision → Basic Condition → Multiple Condition
- Each stronger level includes everything from weaker levels
- MC/DC is mentioned as a practical solution to the exponential problem of multiple condition coverage
- MC/DC is mandated by many certifying agencies (it is a very popular strategy)

---

## Concept 15: Introduction to MC/DC (Multiple Condition/Decision Coverage)

📌 **Concept Name:** MC/DC — Multiple Condition Decision Coverage (Introduction)

🧠 **Simple Explanation:**

MC/DC is briefly introduced at the end of this lecture (details come in the next lecture).

The motivation: Multiple condition coverage is very powerful but needs 2^n test cases, which is too many. Can we achieve testing that is **almost as effective** as multiple condition coverage but with a **much smaller number of test cases**?

That is exactly what MC/DC does. It tries to:
- Achieve as much bug detection capability as multiple condition coverage
- Keep the number of test cases down (not exponential)

MC/DC is so useful that it is **mandated by many certifying agencies** — programs must have MC/DC coverage to be considered acceptable (especially in safety-critical software).

🎯 **Exam Important Points:**
- MC/DC = aims for near-multiple-condition effectiveness with fewer test cases
- Mandated by certifying agencies for safety-critical software
- Detailed coverage comes in the next lecture (Lecture 11)

---

## Concept 16: Coverage Report Tools

📌 **Concept Name:** Coverage Report Tools

🧠 **Simple Explanation:**

The transcript mentions that there are **open source tools** (like Cobertura, shown in the slides) that generate coverage reports. These tools:
- Show how much **line coverage** (statement coverage) has been achieved for each package/class
- Show how much **branch coverage** has been achieved
- Highlight which specific statements have **not been executed**
- Help developers see exactly which parts of their code still need testing

After running a set of test cases, the tool produces a report showing coverage percentages. The developer then adds more test cases to cover the uncovered parts.

🎯 **Exam Important Points:**
- Coverage tools exist that measure both statement and branch coverage
- They show which lines/branches are not yet covered
- The transcript mentions Cobertura as an example of such a tool
- In practice: give random inputs → check coverage → add more tests until 100%

---

## Summary Table: All Coverage Strategies at a Glance

| Strategy | What to Cover | Strength | Formula |
|---|---|---|---|
| Statement Coverage | Every statement at least once | Weakest | Executed statements / Total statements |
| Branch/Decision Coverage | Every branch condition → true & false | Stronger than Statement | Executed branches / Total branches |
| Basic Condition Testing | Each sub-condition → true & false | Stronger than Branch | Truth values taken / (2 × basic conditions) |
| Multiple Condition Coverage | All combinations of sub-conditions | Strongest | All 2^n combinations tested |
| MC/DC | Efficient subset of multiple condition | Between Basic & Multiple | (Details in next lecture) |

---

## Key Takeaways for Exam

1. White-box testing = structural testing = test cases designed from code structure.
2. Two categories: coverage-based (cover program elements) and fault-based (target specific faults).
3. Both black-box and white-box testing are necessary — they are complementary.
4. Black-box cannot find: Trojans, hidden code, unreachable code.
5. White-box cannot find: missing functionality, specification mismatches.
6. Statement coverage is the simplest and weakest strategy.
7. Branch coverage is stronger than statement coverage (branch → statement, but NOT vice versa).
8. Branch coverage can still miss bugs in individual sub-conditions of compound expressions.
9. Condition coverage / basic condition testing checks each sub-condition for T and F.
10. Multiple condition coverage checks ALL combinations → 2^n test cases → exponential.
11. MC/DC is introduced as a practical alternative (details in next lecture).
12. Strength hierarchy: Statement < Branch < Basic Condition < Multiple Condition.

---

---

# 10 MCQs — Lecture 10: White Box Testing

---

**Q1.** White-box testing is also known as:

(a) Functional testing
(b) Structural testing
(c) Acceptance testing
(d) Integration testing

**Answer: (b) Structural testing**

**Explanation:** As stated in the transcript, white-box testing is also called structural testing because test cases are designed by examining the structure of the code. Functional testing is another name for black-box testing, not white-box.

---

**Q2.** White-box test strategies can be broadly classified into:

(a) Static and Dynamic testing
(b) Unit and System testing
(c) Coverage-based and Fault-based testing
(d) Manual and Automated testing

**Answer: (c) Coverage-based and Fault-based testing**

**Explanation:** The transcript clearly states that white-box test strategies are classified into coverage-based (design test cases to cover program elements) and fault-based (design test cases to expose specific categories of faults). The other options are general testing classifications, not specific to white-box strategies as discussed in this lecture.

---

**Q3.** Which of the following is a limitation of white-box testing as discussed in the transcript?

(a) It cannot find Trojans in the code
(b) It does not check whether the program matches the specification
(c) It cannot cover all statements
(d) It is more expensive than black-box testing

**Answer: (b) It does not check whether the program matches the specification**

**Explanation:** The transcript states that white-box testing does not address whether a program matches the specification, does not tell if all functionality has been implemented, and does not uncover missing program logic. Finding Trojans is a limitation of black-box testing, not white-box testing.

---

**Q4.** Statement coverage is measured as:

(a) Number of test cases / Total test cases
(b) Number of executed statements / Total number of statements
(c) Number of branches / Total number of branches
(d) Number of paths / Total number of paths

**Answer: (b) Number of executed statements / Total number of statements**

**Explanation:** The transcript explicitly defines statement coverage as the number of executed statements divided by the total number of statements. This gives the percentage of statements that have been exercised by the test cases.

---

**Q5.** For the Euclid's GCD algorithm discussed in the transcript, which test set achieves 100% statement coverage?

(a) {(x=3, y=3)}
(b) {(x=4, y=3)}
(c) {(x=3, y=3), (x=4, y=3), (x=3, y=4)}
(d) {(x=0, y=0)}

**Answer: (c) {(x=3, y=3), (x=4, y=3), (x=3, y=4)}**

**Explanation:** The transcript shows this exact test set as achieving 100% statement coverage for the GCD algorithm. We need: (x=3,y=3) to execute the exit and return; (x=4,y=3) to take the x>y true branch; (x=3,y=4) to take the x>y false branch. Together all 6 statements are executed.

---

**Q6.** Which of the following is TRUE about the relationship between branch coverage and statement coverage?

(a) Statement coverage is stronger than branch coverage
(b) Branch coverage guarantees statement coverage
(c) Statement coverage guarantees branch coverage
(d) They are completely independent of each other

**Answer: (b) Branch coverage guarantees statement coverage**

**Explanation:** The transcript clearly explains that branch coverage is stronger than statement coverage. The argument is: every statement lies on some branch, so if all branches are covered, all statements must have been executed. The reverse is NOT true — statement coverage does not guarantee branch coverage (the `if(a>b) printf(...)` example proves this).

---

**Q7.** Consider the code: `if (a > b) printf("...");` — with no else part. If we only test with a > b being true, which of the following is correct?

(a) Both statement coverage and branch coverage are achieved
(b) Statement coverage is achieved but branch coverage is NOT achieved
(c) Branch coverage is achieved but statement coverage is NOT achieved
(d) Neither statement coverage nor branch coverage is achieved

**Answer: (b) Statement coverage is achieved but branch coverage is NOT achieved**

**Explanation:** This is a key example from the transcript. With only a > b being true, the printf statement executes, so all statements are covered (100% statement coverage). But the false branch of the if condition has not been taken, so branch coverage is not achieved. We need another test case where a ≤ b to achieve branch coverage.

---

**Q8.** In the expression `digit_high == 1 || digit_low == -1`, branch coverage can be satisfied by only varying `digit_low`. What does this demonstrate?

(a) Branch coverage is always sufficient
(b) Branch coverage can miss faults in individual sub-expressions of compound conditions
(c) Statement coverage is stronger than branch coverage
(d) Condition coverage and branch coverage are the same

**Answer: (b) Branch coverage can miss faults in individual sub-expressions of compound conditions**

**Explanation:** The transcript uses this exact example to show that even with 100% branch coverage, the faulty sub-expression `digit_high == 1` might not be tested. By varying only digit_low, the overall condition becomes true and false (satisfying branch coverage), but the digit_high part is never independently tested. This motivates the need for condition coverage.

---

**Q9.** For a conditional expression with n basic conditions, how many test cases does multiple condition coverage require?

(a) n
(b) 2n
(c) 2^n
(d) n^2

**Answer: (c) 2^n**

**Explanation:** The transcript states that for multiple condition coverage, all possible combinations of truth values of the component conditions must be tested. With n conditions, each having 2 possible values (true/false), the total combinations are 2^n. For example, 3 conditions need 2³ = 8 test cases, and 20 conditions would need 2²⁰ = over 1 million test cases.

---

**Q10.** What is the correct strength hierarchy of white-box testing strategies from weakest to strongest, as discussed in the transcript?

(a) Branch → Statement → Basic Condition → Multiple Condition
(b) Statement → Branch/Decision → Basic Condition → Multiple Condition
(c) Multiple Condition → Basic Condition → Branch → Statement
(d) Statement → Condition → Branch → Multiple Condition

**Answer: (b) Statement → Branch/Decision → Basic Condition → Multiple Condition**

**Explanation:** The transcript presents this exact hierarchy. Statement coverage is the weakest (simplest), then Branch/Decision coverage, then Basic Condition coverage, and finally Multiple Condition coverage is the strongest. Each level covers all elements of the levels below it plus additional elements. Option (c) is the reverse order (strongest to weakest, not weakest to strongest as asked). Option (d) incorrectly places condition between statement and branch.

---

*End of Lecture 10 Notes — White Box Testing*
