# Lecture 12: MC/DC Testing (Continued) & Introduction to Path Coverage

## NPTEL — Software Testing | Prof. Rajib Mall | IIT Kharagpur

---

## Topics Covered in This Lecture

1. Recap of Condition Testing (Basic, Condition/Decision, Multiple Condition Coverage)
2. Why Basic Condition Coverage is Weak
3. Shortcomings of Condition Testing
4. Short-Circuit Evaluation
5. Multiple Condition Coverage (MCC) and its Exponential Problem
6. Modified Condition/Decision Coverage (MC/DC) — Definition and Motivation
7. Three Requirements of MC/DC
8. MC/DC Requirement 1 — Decision Coverage (Branch Coverage)
9. MC/DC Requirement 2 — Every Condition Takes T/F Values
10. MC/DC Requirement 3 — Independent Effect of Each Condition
11. Number of Test Cases in MC/DC: N+1 Formula
12. How to Create MC/DC Test Cases (Step-by-Step with Examples)
13. MC/DC Example 1: `if (A and B)`
14. MC/DC Example 2: `if ((A ∨ B) ∧ C)`
15. Minimal Set of MC/DC Test Cases
16. MC/DC Subsumption Hierarchy and Summary
17. Introduction to Path Coverage Testing
18. What is a Control Flow Graph (CFG)?
19. How to Draw a Control Flow Graph
20. Three Basic Statement Types: Sequence, Selection, Iteration

---

## Concept 1: Recap of Condition Testing

### 📌 What Was Covered Before This Lecture

Before jumping into MC/DC, the professor recaps the types of condition testing already discussed in earlier lectures. This sets the foundation for understanding why MC/DC is needed.

There are several levels of condition-based testing:

**Basic (Simple) Condition Coverage:** In this technique, we design test cases so that each atomic condition (also called a component condition or basic condition) takes both TRUE and FALSE values at least once. For example, consider the decision: `if (a > 10 && b < 50)`. Here, `a > 10` is one atomic condition and `b < 50` is another. To achieve basic condition coverage, we need test cases where `a > 10` is TRUE in one test and FALSE in another, and similarly `b < 50` is TRUE in one test and FALSE in another. A pair like `a=15, b=30` (both TRUE) and `a=5, b=60` (both FALSE) would achieve this.

**Condition/Decision Coverage:** This combines two things — each atomic condition must take TRUE and FALSE values (that is the "condition" part), AND the overall decision (the compound condition) must also evaluate to TRUE and FALSE (that is the "decision" part, which is the same as branch coverage).

**Multiple Condition Coverage (MCC):** This is the most thorough form. Here, all possible combinations of truth values of the atomic conditions must be tested. If there are n atomic conditions, we need 2^n test cases.

### 🎯 Exam Important Points

- Basic condition coverage only ensures each atomic condition gets T and F — it does NOT guarantee the decision itself gets both T and F.
- Condition/Decision coverage adds the requirement that the decision must also take T and F.
- Multiple condition coverage requires ALL combinations — it is the strongest but most expensive.

---

## Concept 2: Why Basic Condition Coverage is Weak

### 📌 The Key Weakness

The professor asks an important question: **Does basic condition coverage subsume decision coverage?** The answer is **NO**.

Here is why. Consider the decision `if (a > 10 && b < 50)`. Suppose we choose these test cases:

- Test 1: `a > 10` = TRUE, `b < 50` = FALSE → Decision = FALSE (because TRUE AND FALSE = FALSE)
- Test 2: `a > 10` = FALSE, `b < 50` = TRUE → Decision = FALSE (because FALSE AND TRUE = FALSE)

In both tests, the decision evaluates to FALSE. We never see the decision become TRUE. So we have achieved basic condition coverage (each condition got both T and F), but we have NOT achieved decision coverage (the decision never became TRUE).

### 🧠 Simple Explanation

Think of it this way: basic condition coverage only looks at the individual conditions in isolation. It does not care about what happens to the overall decision. That makes it a very weak form of testing — in fact, the transcript says it can be even weaker than statement coverage.

### 🎯 Exam Important Points

- Basic condition coverage does NOT subsume decision coverage.
- You can have basic condition coverage achieved without ever making the decision TRUE (or FALSE).
- Basic condition coverage is considered a very weak testing technique.

### ⚠️ Common Confusion

Students sometimes assume "if I test each condition with T and F, I must have tested all branches." That is wrong. The combination matters for the overall decision outcome, not just the individual conditions.

---

## Concept 3: Shortcomings of Condition Testing

### 📌 Two Main Shortcomings

The professor identifies two major problems with condition-based testing (especially multiple condition coverage):

**Problem 1 — Redundancy of Test Cases:** Because of how compilers use short-circuit evaluation, many of the test cases in a truth table actually lead to the same conditional expression evaluation. This means some test cases are redundant — they do not test anything new.

**Problem 2 — Unachievable Coverage:** Some combinations of condition values may be impossible to achieve. For example, consider the condition `(chr == 'A') || (chr == 'E')`. Both cannot be TRUE at the same time because a single character cannot equal both 'A' and 'E' simultaneously. So the combination TRUE-TRUE is unachievable.

### 🎯 Exam Important Points

- Short-circuit evaluation causes redundancy in test cases.
- Some condition combinations may be logically impossible (infeasible).
- These problems make multiple condition coverage impractical.

---

## Concept 4: Short-Circuit Evaluation

### 📌 What Is Short-Circuit Evaluation?

Short-circuit evaluation is a technique used by compilers where the compiler does not evaluate the entire compound condition if the result can be determined from the first part.

**For AND (&&):** If the first condition is FALSE, the compiler already knows the entire AND expression is FALSE. So it does not evaluate the second condition. Example: `if (a > 30 && b < 50)` — if `a > 30` is FALSE, the compiler skips evaluating `b < 50`.

**For OR (||):** If the first condition is TRUE, the compiler already knows the entire OR expression is TRUE. So it does not evaluate the second condition. Example: `if (a > 30 || b < 50)` — if `a > 30` is TRUE, the compiler skips evaluating `b < 50`.

### 🧠 Simple Explanation

The compiler is being smart — it stops early when it already knows the answer. This is called "short-circuit" because it shortcuts the evaluation process.

### 🎯 Exam Important Points

- In AND: first condition FALSE → skip rest, result is FALSE.
- In OR: first condition TRUE → skip rest, result is TRUE.
- Short-circuit evaluation reduces the effective number of test cases needed.
- The short-circuit behavior may differ between compilers.

---

## Concept 5: Multiple Condition Coverage — The Exponential Problem

### 📌 Why MCC Becomes Impractical

For a Boolean expression with n component conditions, multiple condition coverage requires 2^n test cases. This grows exponentially.

Examples from the transcript:

| Number of Conditions (n) | Test Cases Required (2^n) |
|---|---|
| 3 | 8 |
| 5 | 32 |
| 10 | 1,024 (about 1 thousand) |
| 20 | 1,048,576 (about 1 million) |
| 30 | 1,073,741,824 (about 1 billion) |

The professor mentions that 20 or 30 component conditions are very common in real applications, especially in control applications (like controllers). So MCC becomes completely impractical for such cases.

However, using short-circuit evaluation, the actual number may be reduced. In the transcript's example, 5 conditions needing 32 test cases were reduced to just 13 through short-circuit evaluation. But this reduction is not always enough.

### 🎯 Exam Important Points

- MCC requires 2^n test cases — exponential growth.
- Impractical when n is large (10, 20, 30+ conditions).
- Large compound conditions are common in control applications.
- This is the main motivation for developing MC/DC testing.

---

## Concept 6: Modified Condition/Decision Coverage (MC/DC) — Definition and Motivation

### 📌 What Is MC/DC?

MC/DC stands for **Modified Condition/Decision Coverage**. It is a condition coverage technique that achieves almost the same thoroughness as Multiple Condition Coverage (MCC), but with far fewer test cases.

**Motivation:** We want to effectively test important combinations of conditions without the exponential blow-up of the test suite size.

**What does "important" mean here?** Each basic condition must be shown to **independently affect** the outcome of the decision. This is the core idea. When we change a single atomic condition (from T to F or F to T), while keeping all other conditions at fixed values, the output of the entire decision should toggle (change from T to F or F to T).

### 📌 Key Terminology

- **Condition** = Atomic condition in the compound expression (e.g., `a > 10`, `b < 50`).
- **Decision** = The entire compound condition that controls the program flow (e.g., `if (a > 10 && b < 50)`).

### 📌 Where Does MC/DC Fit in the Hierarchy?

The transcript provides a clear **subsumption hierarchy**:

```
Multiple Condition Coverage (MCC)
        ↑ (stronger)
    MC/DC
        ↑ (stronger)
Condition/Decision Coverage
        ↑ (stronger)
   Branch Coverage
        ↑ (stronger)
 Statement Coverage
```

MC/DC is weaker than MCC but stronger than all others (statement, branch, condition/decision).

### 🎯 Exam Important Points

- MC/DC stands for Modified Condition/Decision Coverage.
- It is stronger than condition/decision coverage but weaker than MCC.
- Key idea: each condition must independently affect the decision's outcome.
- Number of test cases is LINEAR (not exponential) in the number of conditions.
- Bug-detection effectiveness is almost similar to MCC.
- MC/DC provides a good balance of thoroughness and test size.
- It is widely used and mandated by many testing standards.

---

## Concept 7: Three Requirements of MC/DC

### 📌 The Three Requirements

MC/DC has exactly three requirements that must be satisfied:

**Requirement 1 — Decision Coverage (DC):** Every decision in the program must take both TRUE and FALSE values. This is the same as branch coverage.

**Requirement 2 — Condition Coverage (C):** Every atomic condition in each decision must take both TRUE and FALSE values at least once. This is the same as basic condition coverage.

**Requirement 3 — Modified Condition (M):** Each condition in a decision must **independently affect** the decision's outcome. This means: when we toggle one atomic condition (from T to F or F to T), while holding ALL other atomic conditions at constant values, the decision outcome must also toggle.

### 🧠 Simple Explanation

Think of Requirement 3 like this: if you flip just ONE switch while keeping all other switches unchanged, the light (the decision) should also flip. If flipping a switch does not change the light, then that switch did not independently affect the result.

### 🎯 Exam Important Points

- Three requirements: Decision T/F + Condition T/F + Independent effect.
- Requirement 1 = Branch coverage.
- Requirement 2 = Basic condition coverage.
- Requirement 3 = The "Modified" part that makes MC/DC special.
- The combination of all three gives MC/DC its power.

### 📝 MC/DC Summary Formula

MC/DC = Basic Condition Coverage (C) + Branch Coverage (DC) + Independent Effect of Each Condition (M)

---

## Concept 8: MC/DC Requirement 1 — Decision Coverage

### 📌 What It Means

The first requirement says the compound condition (the decision) must evaluate to both TRUE and FALSE across the test cases. If we have a decision like `if ((a > 10) && (b < 50) || (c == 0))`, then our test suite must include at least one case where this entire expression is TRUE and at least one case where it is FALSE.

This is exactly the same as **branch coverage** — we must take both the TRUE branch and the FALSE branch.

### 🎯 Exam Important Points

- MC/DC Requirement 1 = Branch Coverage = Decision takes both T and F.

---

## Concept 9: MC/DC Requirement 2 — Condition Coverage

### 📌 What It Means

The second requirement says every atomic condition must individually take both TRUE and FALSE values across the test cases.

Example: For the decision `if ((a > 10) && (b < 50) || (c == 0))`, we need:

- `a > 10` must be TRUE in some test and FALSE in some other test.
- `b < 50` must be TRUE in some test and FALSE in some other test.
- `c == 0` must be TRUE in some test and FALSE in some other test.

### 🎯 Exam Important Points

- MC/DC Requirement 2 = Basic Condition Coverage = Each condition gets T and F.

---

## Concept 10: MC/DC Requirement 3 — Independent Effect

### 📌 What It Means

This is the most important and unique requirement of MC/DC. For each atomic condition, we need to find a **pair of test cases** where:

1. Only that one atomic condition changes its value (from T to F or F to T).
2. All other atomic conditions stay at the same values in both tests.
3. The decision outcome toggles (changes from T to F or F to T).

This pair of test cases proves that this particular atomic condition **independently affects** the decision outcome.

### 🛠 Example from Transcript

Consider: `if ((a > 10) && ((b < 50) || (c == 0)))`

**For condition `a > 10`:**
- Test A: `a > 10` = TRUE, `b < 50` = TRUE, `c == 0` = FALSE → Decision = TRUE
- Test B: `a > 10` = FALSE, `b < 50` = TRUE, `c == 0` = FALSE → Decision = FALSE

Here, only `a > 10` changed. The others stayed the same. The decision toggled from TRUE to FALSE. So `a > 10` independently affects the decision.

**For condition `b < 50`:**
- Test C: `a > 10` = TRUE, `b < 50` = TRUE, `c == 0` = FALSE → Decision = TRUE
- Test D: `a > 10` = TRUE, `b < 50` = FALSE, `c == 0` = FALSE → Decision = FALSE

Only `b < 50` changed. Decision toggled. So `b < 50` independently affects the decision.

**For condition `c == 0`:**
- Test E: `a > 10` = TRUE, `b < 50` = FALSE, `c == 0` = TRUE → Decision = TRUE
- Test F: `a > 10` = TRUE, `b < 50` = FALSE, `c == 0` = FALSE → Decision = FALSE

Only `c == 0` changed. Decision toggled. So `c == 0` independently affects the decision.

### 🎯 Exam Important Points

- For each condition, you must find a test case PAIR.
- In that pair, only that ONE condition changes.
- All other conditions STAY FIXED.
- The decision outcome MUST TOGGLE.
- This is what "independently affect" means.

---

## Concept 11: Number of Test Cases — The N+1 Formula

### 📌 How Many Test Cases Does MC/DC Need?

The transcript states that for N basic conditions in a compound conditional expression, MC/DC requires **N+1 test cases**.

This is the key advantage of MC/DC over MCC:

| Technique | Test Cases for N conditions |
|---|---|
| Multiple Condition Coverage (MCC) | 2^N (exponential) |
| MC/DC | N+1 (linear) |

Examples:

| N (conditions) | MCC (2^N) | MC/DC (N+1) |
|---|---|---|
| 3 | 8 | 4 |
| 5 | 32 | 6 |
| 10 | 1,024 | 11 |
| 20 | 1,048,576 | 21 |

The difference is dramatic! For 20 conditions, MCC needs over a million test cases, but MC/DC needs only 21.

### 🎯 Exam Important Points

- MC/DC requires N+1 test cases for N basic conditions.
- This is linear — a huge improvement over MCC's exponential growth.
- This makes MC/DC practical even for large compound conditions.

---

## Concept 12: How to Create MC/DC Test Cases — Step-by-Step

### 📌 The Process

The professor explains a clear process for designing MC/DC test cases:

**Step 1:** Write the truth table for all the atomic conditions and compute the decision outcome for each row.

**Step 2:** For each atomic condition, find a pair of rows in the truth table where:
- Only that condition's value changes (T↔F).
- All other conditions remain the same.
- The decision outcome toggles.

**Step 3:** Collect all the required test cases from the pairs found. Remove duplicates to get the minimal set.

---

## Concept 13: MC/DC Example 1 — `if (A and B)`

### 📌 Full Worked Example

**Decision:** `if (A AND B)`

**Step 1 — Truth Table:**

| Test Case | A | B | Decision (A AND B) |
|---|---|---|---|
| 1 | T | T | T |
| 2 | T | F | F |
| 3 | F | T | F |
| 4 | F | F | F |

**Step 2 — Find Independent Pairs:**

**For condition A:**
We need two rows where A changes but B stays the same, and the decision toggles.

- Test 1 (A=T, B=T → Decision=T) and Test 3 (A=F, B=T → Decision=F): A changes from T to F, B stays T, decision toggles from T to F. ✅
- So the pair for A is **(1, 3)**.

Can we use Test 2 and Test 4? Test 2 (A=T, B=F → F) and Test 4 (A=F, B=F → F). Here A changes, B stays F, but decision does NOT toggle (both are F). ❌ So (2, 4) does NOT work.

**For condition B:**
We need two rows where B changes but A stays the same, and the decision toggles.

- Test 1 (A=T, B=T → Decision=T) and Test 2 (A=T, B=F → Decision=F): B changes from T to F, A stays T, decision toggles from T to F. ✅
- So the pair for B is **(1, 2)**.

**Step 3 — Collect Test Cases:**
- Pair for A: Test cases 1 and 3.
- Pair for B: Test cases 1 and 2.
- Combined (removing duplicates): **Test cases 1, 2, 3** → Only 3 test cases needed!

This follows the N+1 formula: 2 conditions → 2+1 = 3 test cases. ✅

### 🎯 Exam Important Points

- For `A AND B`, MC/DC test cases are {1, 2, 3} (i.e., TT, TF, FT).
- The pair for A is (1, 3), the pair for B is (1, 2).
- This gives the minimum number of test cases: N+1 = 3.

---

## Concept 14: MC/DC Example 2 — `if ((A ∨ B) ∧ C)`

### 📌 Full Worked Example

**Decision:** `if ((A OR B) AND C)`

**Step 1 — Truth Table:**

| Test Case | A | B | C | Decision ((A OR B) AND C) |
|---|---|---|---|---|
| 1 | T | T | T | T |
| 2 | T | T | F | F |
| 3 | T | F | T | T |
| 4 | T | F | F | F |
| 5 | F | T | T | T |
| 6 | F | T | F | F |
| 7 | F | F | T | F |
| 8 | F | F | F | F |

**Step 2 — Find Independent Pairs:**

**For condition A:**
We need rows where only A changes (B and C stay same), and decision toggles.

- Test 3 (A=T, B=F, C=T → T) and Test 7 (A=F, B=F, C=T → F): A changes, B and C stay same (F, T), decision toggles. ✅
- So the pair for A is **(3, 7)**.

(The professor checks other pairs too: Test 1 (A=T, B=T, C=T → T) vs Test 5 (A=F, B=T, C=T → T) — decision does NOT toggle because B=T makes (A OR B) = T regardless of A. So (1, 5) does NOT work.)

**For condition B:**
- Test 5 (A=F, B=T, C=T → T) and Test 7 (A=F, B=F, C=T → F): B changes, A and C stay same (F, T), decision toggles. ✅
- Another valid pair: Test 4 (A=T, B=F, C=F → F) and... we would need to check.
- The professor identifies the pair for B as **(5, 7)** — but notes from the transcript the pair is given as **(4, 7)** with outcome toggling.

**For condition C:**
- Test 1 (A=T, B=T, C=T → T) and Test 2 (A=T, B=T, C=F → F): C changes, A and B stay same, decision toggles. ✅
- So the pair for C is **(1, 2)**.

**Step 3 — Collect Test Cases:**
Combined set: **{1, 2, 3, 4, 7}** or **{1, 2, 3, 5, 7}** — this gives 5 test cases.

This follows N+1: 3 conditions → 3+1 = 4. But here we got 5 because of the structure of the expression. The N+1 is the minimum, but sometimes a few more may be needed depending on the expression structure.

### 🎯 Exam Important Points

- For more complex expressions, you build the truth table and systematically check pairs.
- Not every pair of rows where one condition changes will produce a toggle in the decision.
- You must verify the decision actually toggles before accepting a pair.
- The expression structure affects which pairs work.

---

## Concept 15: Minimal Set of MC/DC Test Cases

### 📌 What Is a Minimal Set?

When you find the independent pairs for each condition, there may be multiple valid pairs for each condition. Different choices of pairs lead to different final test case sets. The **minimal set** is the one with the fewest total test cases.

### 🛠 Example from Transcript

**Decision:** `if (A AND (B OR C))`

After finding all valid pairs for independent evaluation:

**For condition A:** Multiple valid pairs exist — e.g., (1, 5), (2, 6), (3, 7), etc.
**For condition B:** Valid pair — (2, 4) and (4, 2)
**For condition C:** Valid pair — (3, 4) and (4, 3)

Possible MC/DC test case sets:
- **{2, 3, 4, 6}** → 4 test cases (MINIMAL) ✅
- **{2, 3, 4, 7}** → 4 test cases (MINIMAL) ✅
- **{1, 2, 3, 4, 5}** → 5 test cases (NON-MINIMAL) ❌

The first two are minimal sets with only 4 test cases. The third set works but has an extra test case, so it is non-minimal.

### 🎯 Exam Important Points

- Multiple valid MC/DC test sets may exist for the same expression.
- Always look for the MINIMAL set.
- A non-minimal set still achieves MC/DC coverage, but it is not optimal.
- The exam may ask you to identify which set is minimal.

---

## Concept 16: MC/DC — Another Example with 5 Conditions

### 📌 Example from Transcript

**Decision:** `if ((((a>10 || b<50) && c==0) || d<5) && e==10)`

This has 5 basic conditions: `a>10`, `b<50`, `c==0`, `d<5`, `e==10`.

The professor shows that MC/DC coverage can be achieved with just **6 test cases** (matching the N+1 = 5+1 = 6 formula).

The test cases from the transcript:

| Test | a>10 | b<50 | c==0 | d<5 | e==10 | Outcome |
|---|---|---|---|---|---|---|
| 6 | T | F | T | F | T | T |
| 2 | F | T | T | F | T | T |
| 3 | T | F | F | T | T | T |
| 8 | T | F | T | F | F | F |
| 11 | T | F | F | F | T | F |
| 13 | F | F | T | F | T | F |

In each pair, the underlined (toggled) condition independently affects the decision output while the others remain constant.

### 🎯 Exam Important Points

- 5 conditions → 6 test cases with MC/DC (not 32 as in MCC).
- N+1 formula confirmed: linear, not exponential.
- The specific test case numbers depend on the truth table and which pairs achieve independent evaluation.

---

## Concept 17: MC/DC Summary and Subsumption

### 📌 Key Summary Points from the Transcript

MC/DC essentially combines three things:

1. **Basic condition coverage (C):** Each condition takes T and F.
2. **Branch coverage (DC):** The decision takes T and F.
3. **Plus one additional requirement (M):** Every condition must independently affect the decision's output.

**Subsumption:**
- MC/DC is **subsumed by** MCC (MCC is stronger).
- MC/DC **subsumes** all other criteria discussed (stronger than statement, branch, condition/decision coverage).

**Practical Value:**
- It provides a good balance of thoroughness and test size.
- It is widely used in industry.
- It is mandated by many testing standards.

### 🎯 Exam Important Points

- MC/DC = C + DC + M (three components).
- Subsumed by MCC, subsumes all others.
- Good balance of thoroughness and cost.
- Widely used and mandated by standards.

---

## Concept 18: Introduction to Path Coverage Testing

### 📌 What Is Path Coverage?

After completing MC/DC, the lecture introduces path coverage as a different white-box testing strategy.

**Path Coverage Definition:** Design test cases such that all **linearly independent paths** in the program are executed at least once.

Note: The professor says we cannot cover ALL paths (there are too many, especially with loops), so we focus on linearly independent paths.

Path coverage is defined in terms of the **Control Flow Graph (CFG)** of a program. The paths in a program are defined with respect to this graph.

### 🎯 Exam Important Points

- Path coverage covers all linearly independent paths, not ALL possible paths.
- Paths are defined on the Control Flow Graph (CFG).
- Path testing is different from MC/DC — they are complementary.

---

## Concept 19: What Is a Control Flow Graph (CFG)?

### 📌 Definition

A **Control Flow Graph (CFG)** is a graph representation of a program that describes:

1. **The sequence in which different instructions of a program get executed.**
2. **The way control flows through the program.**

In simpler words, a CFG shows you the order in which program statements run, and how the program jumps from one statement to another based on conditions, loops, etc.

### 🎯 Exam Important Points

- CFG is a graph where nodes = statements, edges = flow of control.
- It shows the execution order of statements.
- It is the foundation for path coverage testing.

---

## Concept 20: How to Draw a Control Flow Graph

### 📌 Step-by-Step Process

The professor gives a clear process:

**Step 1:** Number all the statements of the program. Each statement gets a unique number.

**Step 2:** Each numbered statement becomes a **node** in the control flow graph.

**Step 3:** Draw an **edge** from one node to another node if execution of the statement at the first node can result in transfer of control to the statement at the other node. In other words, if after executing statement X, the program can go to statement Y, then draw an edge from X to Y.

### 🧠 Simple Explanation

Imagine your program is a sequence of instructions. You number each instruction. Then you draw arrows showing "after this instruction, which instruction can run next?" If there is a condition (like an if-statement), you draw two arrows from the condition node — one for the TRUE branch and one for the FALSE branch. If there is a loop, you draw an arrow going back to an earlier node.

### 🎯 Exam Important Points

- Nodes = numbered program statements.
- Edges = possible transfer of control between statements.
- Edge from node A to node B means "statement A can transfer control to statement B."

---

## Concept 21: Three Basic Statement Types for CFG

### 📌 Every Program Is Made of Three Types

The professor states that every program is composed of three basic types of statements:

1. **Sequence** — Statements that execute one after another in order (no branching, no looping). The CFG for sequence is just a straight line of nodes connected by edges.

2. **Selection** — Conditional statements like `if-then-else`. These create branching in the CFG where control can go to one of two (or more) paths based on a condition.

3. **Iteration** — Loop statements like `while`, `for`, `do-while`. These create a cycle (backward edge) in the CFG where control goes back to an earlier node.

### 🧠 Simple Explanation

If you know how to draw CFG for these three basic types of statements, you can draw the CFG for ANY program — because every program, no matter how large, is just a combination of sequences, selections, and iterations.

### 🎯 Exam Important Points

- Three types: Sequence, Selection, Iteration.
- CFG for any program can be built using these three building blocks.
- Sequence = straight line, Selection = branching, Iteration = cycle/loop.
- The detailed CFG drawing for each type is covered in the next lecture (Lecture 13).

---

## Quick Revision Summary

| Topic | Key Point |
|---|---|
| Basic Condition Coverage | Each condition gets T/F — very weak, does NOT subsume decision coverage |
| Condition/Decision Coverage | Conditions get T/F AND decision gets T/F |
| Multiple Condition Coverage (MCC) | All 2^n combinations — strongest but impractical |
| Short-Circuit Evaluation | Compiler skips evaluating remaining conditions when result is already determined |
| MC/DC Definition | Modified Condition/Decision Coverage — each condition independently affects decision |
| MC/DC Requirement 1 | Decision takes both T and F (= branch coverage) |
| MC/DC Requirement 2 | Each condition takes both T and F (= basic condition coverage) |
| MC/DC Requirement 3 | Each condition independently affects the decision outcome |
| MC/DC Test Cases | N+1 for N conditions (linear, not exponential) |
| MC/DC vs MCC | MC/DC nearly as thorough as MCC but with far fewer test cases |
| MC/DC Hierarchy | MCC > MC/DC > Condition/Decision > Branch > Statement |
| Creating MC/DC Tests | Build truth table → find independent pairs → collect minimal set |
| Path Coverage | All linearly independent paths executed at least once |
| CFG | Graph with nodes (statements) and edges (control flow) |
| Drawing CFG | Number statements → make nodes → draw edges for control transfer |
| Three Statement Types | Sequence, Selection, Iteration — building blocks for any CFG |

---

## 10 MCQs — Strictly from Lecture 12

---

**Q1.** Does basic condition coverage subsume decision coverage?

(A) Yes, basic condition coverage is always stronger than decision coverage
(B) No, basic condition coverage does not subsume decision coverage
(C) They are equivalent
(D) Decision coverage subsumes basic condition coverage, but not the other way

**Answer: (B)**

**Explanation:** The transcript clearly states that basic condition coverage does NOT subsume decision coverage. We can have each condition take T and F values, but the decision may never become TRUE (or never become FALSE). The professor demonstrates this with the `if (a > 10 && b < 50)` example where both test cases make the decision FALSE.

---

**Q2.** What is the main motivation behind MC/DC testing?

(A) To replace statement coverage
(B) To achieve the thoroughness of MCC without exponential test cases
(C) To eliminate the need for branch coverage
(D) To test only the most important conditions

**Answer: (B)**

**Explanation:** The transcript states that the motivation for MC/DC is to effectively test important combinations of conditions without the exponential blow-up in test suite size, achieving almost the same thoroughness as MCC.

---

**Q3.** How many test cases are required for MC/DC when there are N basic conditions?

(A) 2^N
(B) N^2
(C) N+1
(D) 2N

**Answer: (C)**

**Explanation:** The transcript states that MC/DC requires N+1 test cases for N basic conditions. This is linear, making it practical even for large expressions. For 5 conditions, we need 6 test cases (not 32 as in MCC).

---

**Q4.** Which of the following is NOT a requirement of MC/DC?

(A) Every decision must take both T and F values
(B) Every atomic condition must take both T and F values
(C) All possible combinations of condition truth values must be tested
(D) Each condition must independently affect the decision's outcome

**Answer: (C)**

**Explanation:** Option (C) describes Multiple Condition Coverage (MCC), not MC/DC. MC/DC has three requirements: decision takes T/F, each condition takes T/F, and each condition independently affects the decision. Testing all combinations is MCC, which requires 2^N test cases.

---

**Q5.** In short-circuit evaluation of `if (a > 30 && b < 50)`, when will the compiler NOT evaluate `b < 50`?

(A) When `a > 30` is TRUE
(B) When `a > 30` is FALSE
(C) When `b < 50` is TRUE
(D) The compiler always evaluates both conditions

**Answer: (B)**

**Explanation:** In AND (&&), if the first condition is FALSE, the compiler knows the entire expression is FALSE regardless of the second condition. So it skips evaluating `b < 50`. This is short-circuit evaluation as explained in the transcript.

---

**Q6.** For the expression `if (A AND B)`, which test case pairs show the independent effect of condition A?

(A) Test cases (1, 2) where row 1 is TT and row 2 is TF
(B) Test cases (1, 3) where row 1 is TT and row 3 is FT
(C) Test cases (2, 4) where row 2 is TF and row 4 is FF
(D) Test cases (3, 4) where row 3 is FT and row 4 is FF

**Answer: (B)**

**Explanation:** For independent effect of A, we need A to change (T→F) while B stays constant, and the decision must toggle. In (1, 3): A changes (T→F), B stays T, decision toggles (T→F). In (2, 4): A changes but decision doesn't toggle (both F). The transcript confirms the pair for A is (1, 3).

---

**Q7.** In the MC/DC subsumption hierarchy, MC/DC is:

(A) Stronger than MCC
(B) Weaker than statement coverage
(C) Subsumed by MCC and subsumes condition/decision coverage
(D) Equivalent to branch coverage

**Answer: (C)**

**Explanation:** The transcript shows the hierarchy: MCC > MC/DC > Condition/Decision > Branch > Statement. MC/DC is subsumed by MCC (MCC is stronger) and MC/DC subsumes all other criteria discussed in the course.

---

**Q8.** What does "independently affect" mean in MC/DC Requirement 3?

(A) Each condition is tested separately in different programs
(B) Changing one condition while holding others constant causes the decision to toggle
(C) Each condition must be TRUE independently
(D) Conditions should not depend on each other

**Answer: (B)**

**Explanation:** The transcript defines "independently affect" as: when we change a single atomic condition (T↔F) while keeping all other conditions at constant values, the decision outcome must also toggle (change from T to F or F to T).

---

**Q9.** A Control Flow Graph (CFG) consists of:

(A) Nodes representing variables and edges representing assignments
(B) Nodes representing numbered statements and edges representing possible transfer of control
(C) Nodes representing test cases and edges representing test results
(D) Nodes representing conditions and edges representing truth values

**Answer: (B)**

**Explanation:** The transcript states: number all statements (these become nodes), and draw an edge from one node to another if execution of the first statement can result in transfer of control to the other. CFG represents the execution sequence of program statements.

---

**Q10.** According to the transcript, every program is composed of which three basic types of statements?

(A) Assignment, Input, Output
(B) Sequence, Selection, Iteration
(C) Declaration, Definition, Execution
(D) Conditional, Unconditional, Loop

**Answer: (B)**

**Explanation:** The transcript clearly states that every program is composed of three types: Sequence, Selection, and Iteration. If we know how to draw CFG for these three basic statement types, we can draw CFG for any program because all programs are composed of these three building blocks.

---

*End of Lecture 12 Notes — MC/DC Testing (Continued) & Introduction to Path Coverage*
