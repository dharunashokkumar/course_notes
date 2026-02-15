# Lecture 13 – Path Testing

## NPTEL Software Testing | Prof. Rajib Mall | IIT Kharagpur

---

## What This Lecture Covers

This lecture introduces **Path Testing**, a white-box testing technique. You will learn what path coverage means, how to draw a Control Flow Graph (CFG), what paths and linearly independent paths are, how to compute McCabe's Cyclomatic Complexity, and how to practically apply path testing to design test cases.

---

## Concept 1: What is Path Testing?

### 📌 Definition

Path testing is a white-box testing technique where we design test cases to achieve **path coverage**. The goal is to execute all the linearly independent paths in the program.

### 🧠 Simple Explanation

Imagine a program as a network of roads. Each road is a possible route from the start to the end. Path testing means we want to make sure every unique route (path) gets traveled at least once during testing.

Here, "program" means a **unit**, and a unit is a **function**. So path testing works at the function level.

### 🎯 Key Definition (Exam Important)

> **Path Coverage**: Design test cases such that **all linearly independent paths** in the program are executed at least once.

Path coverage is defined in terms of the **Control Flow Graph (CFG)** of the program.

### ⚠️ Important Note from Transcript

It may not always be possible to design test cases that achieve full path coverage. But at least, we should have test cases (even randomly selected) that achieve the required path coverage.

---

## Concept 2: Control Flow Graph (CFG)

### 📌 What is a CFG?

A Control Flow Graph is a **graph representation** of a program where:

- The **statements** of the program become the **nodes** of the graph.
- The **edges** represent the **sequence in which control flows** through the program — i.e., the order in which statements get executed.

### 🧠 Simple Explanation

Think of a CFG like a flowchart, but simpler. Each numbered statement in your code becomes a circle (node). If one statement can lead to another, you draw an arrow (edge) between them.

For a trivial program with no branches (just straight-line code), the CFG is just a straight line of nodes. For realistic programs with if-else and loops, the graph becomes more complex.

### 📊 How to Draw a CFG — Step by Step

**Step 1:** Number all the statements in the program.

**Step 2:** Each numbered statement becomes a **node** in the graph.

**Step 3:** Draw an **edge** from one node to another **if execution of the first statement can result in transfer of control to the other statement**.

### 🎯 Exam Important

Every structured program is composed of only **three types of statements**:

1. **Sequence** statements
2. **Selection** statements (if-then-else, switch)
3. **Iteration** statements (while, for, do-while)

If you know how to draw CFG edges for these three types, you can draw the CFG for **any program**.

---

## Concept 3: CFG for Sequence Statements

### 📌 What is a Sequence Statement?

A sequence statement is when one statement finishes and the very next statement executes — there is no branching or choice involved.

### 🛠 Example from Transcript

```
1  a = 5;
2  b = a * b - 1;
```

**CFG:** Simply draw an edge from node 1 to node 2.

```
  (1)
   |
   ↓
  (2)
```

### 🧠 Simple Explanation

This is the easiest case. As soon as statement 1 completes, statement 2 executes. No hesitation, no ambiguity — just draw a single edge from 1 to 2.

---

## Concept 4: CFG for Selection Statements

### 📌 What is a Selection Statement?

A selection statement is an if-then-else or switch statement, where the program can go in one of two (or more) directions based on a condition.

### 🛠 Example from Transcript

```
1  if (a > b) then
2      c = 3;
3  else c = 5;
4  c = c * c;
```

**CFG:**

```
      (1)
      / \
     ↓   ↓
   (2)   (3)
     \   /
      ↓ ↓
      (4)
```

### 🧠 Simple Explanation

From node 1 (the if-condition), control can go to either node 2 (if condition is true) or node 3 (if condition is false). After either 2 or 3 finishes, control goes to node 4 (the next statement after the if-else block). So we draw:

- Edge from 1 → 2
- Edge from 1 → 3
- Edge from 2 → 4
- Edge from 3 → 4

---

## Concept 5: CFG for Iteration Statements (⚠️ Tricky — Common Mistake Area)

### 📌 What is an Iteration Statement?

An iteration statement is a loop — while, for, do-while. The body of the loop repeats as long as the condition is true.

### 🛠 Example from Transcript

```
1  while (a > b) {
2      b = b * a;
3      b = b - 1;
   }
4  c = b + d;
```

**CFG:**

```
   → (1) ←──┐
     |       |
     ↓       |
    (2)      |
     |       |
     ↓       |
    (3) ─────┘
     
    (1) ───→ (4)
```

### 🧠 Simple Explanation

- From node 1 (the while condition), if the condition is **true**, control goes to node 2.
- From node 2, control goes to node 3.
- **After node 3, control does NOT go to node 4.** Instead, control goes **back to node 1** (the while condition is re-evaluated).
- Only when the while condition becomes **false** does control go from node 1 to node 4.

### ⚠️ Common Mistake (Exam Important!)

Many students draw an edge from node 3 → node 4. **This is WRONG.** After the last statement inside the loop (node 3), control always goes back to the loop condition (node 1). From node 1, if the condition is false, then control goes to node 4.

### 🎯 Key Difference: CFG vs Flowchart

In a **flowchart**, we write conditions like "true" and "false" on the edges. In a **CFG**, we do NOT write conditions. As long as control can transfer from one node to another, we simply draw the edge.

---

## Concept 6: What is a Path?

### 📌 Definition

> **A path** through a program is a **node and edge sequence from the start node to a terminal node** in the Control Flow Graph.

### 🧠 Simple Explanation

A path is any route you can take through the CFG, starting from the very first node (entry point) and ending at a terminal node (exit point, like a return statement).

### 🎯 Key Points (Exam Important)

- A program may have **several terminal nodes** (due to multiple return statements, exit calls, etc.).
- A path is any complete route from entry to exit through the CFG.

---

## Concept 7: Why All-Path Testing is Impractical

### 📌 The Problem with Loops

When loops are present in a program, the number of paths can become **extremely large**.

### 🛠 Example from Transcript

For a while loop with statements 1, 2, 3, 4:

- Path 1: 1 → 4 (loop doesn't execute)
- Path 2: 1 → 2 → 3 → 1 → 4 (loop executes once)
- Path 3: 1 → 2 → 3 → 1 → 2 → 3 → 1 → 4 (loop executes twice)
- Path 4: 1 → 2 → 3 → 1 → 2 → 3 → 1 → 2 → 3 → 1 → 4 (three times)
- ... and so on, infinitely!

### 🧠 Simple Explanation

Each extra iteration of the loop creates a brand new path. If a loop can run 1000 times, you'd need 1000+ test cases just for that one loop. For any practical program, **all-path testing is impractical**.

### 🎯 Key Conclusion (Exam Important)

> **All-path testing is impractical** because of loops. That is why we need the concept of **linearly independent paths** — they give us almost the same thoroughness as all-path testing, but with a **manageable number of test cases**.

---

## Concept 8: Linearly Independent Paths

### 📌 Definition

> A path **p** is a **linear combination** of paths p₁, p₂, ..., pₙ if there are integers a₁, a₂, ..., aₙ such that p = Σ(aᵢ × pᵢ), where aᵢ can be negative, zero, or positive.

> A **set of paths is linearly independent** if **no path in the set is a linear combination of any other paths in the set**.

### 🧠 Simple Explanation (The Easy Way to Think About It)

Don't worry too much about the mathematical formula. Here's the simple idea:

A **linearly independent path** is any complete path through the program (from start node to terminal node) that introduces **at least one new edge** that is not included in any other linearly independent paths already identified.

Think of it this way: if a path doesn't bring anything "new" (no new edge), then it's not an independent path — it's just a combination of paths you've already covered.

### 🛠 Example from Transcript

For a loop program with nodes 1, 2, 3, 4:

- Path: 1 → 4 ✅ (this is one independent path)
- Path: 1 → 2 → 3 → 4 — Is this independent? We check: does it introduce a new edge compared to 1 → 4? **The second path doesn't introduce a truly new edge** in the example shown, so they are NOT linearly independent as a pair in that specific context.

The key rule: **each independent path must have at least one edge that no other independent path has**.

### 🎯 Exam Important

- Linearly independent paths are defined on the **Control Flow Graph**.
- Each independent path introduces **at least one new edge**.
- For small programs (5–7 lines), you can identify them by examining the CFG visually.
- For larger programs (20–30 nodes, 50 edges), it becomes **extremely complicated** to identify them manually — you could spend 1–2 weeks trying!

### ⚠️ Key Insight from Transcript

> Nobody really tries to manually identify linearly independent paths for large programs. Instead, we use **McCabe's Cyclomatic Metric** to know **how many** such paths exist, and then we use practical techniques to test them.

---

## Concept 9: McCabe's Cyclomatic Metric (V(G))

### 📌 What is it?

McCabe's Cyclomatic Metric is a number that provides an **upper bound** on the number of linearly independent paths in a program. If the metric evaluates to 10, we would look for **at most 10** linearly independent paths.

### 🧠 Simple Explanation

Think of it as a "difficulty score" for your code. The higher the number, the more paths there are, the more test cases you need, and the more complex the code is.

### 🎯 Three Ways to Compute Cyclomatic Complexity

---

### Method 1: Formula — V(G) = E − N + 2

Where:
- **E** = Number of **edges** in the Control Flow Graph
- **N** = Number of **nodes** in the Control Flow Graph

### 🛠 Example from Transcript

For the example CFG shown in the lecture:
- Edges (E) = 7
- Nodes (N) = 6
- V(G) = 7 − 6 + 2 = **3**

### 🎯 Exam Tip

This method is **easy to automate**. A tool can easily draw the CFG from code, count edges and nodes, and compute V(G).

---

### Method 2: Bounded Areas + 1

Look at the CFG visually and count the number of **bounded areas** (regions enclosed by nodes and edges), then add 1.

> **V(G) = Number of bounded areas + 1**

### 📌 What is a Bounded Area?

A bounded area is a **region enclosed by nodes and edges** in the CFG. Think of it like a closed shape or enclosed region in the graph.

### 🛠 Example from Transcript

In the example CFG:
- Bounded area 1: The region enclosed by the if-true branch
- Bounded area 2: The region enclosed by the loop back-edge

Number of bounded areas = 2
V(G) = 2 + 1 = **3**

This matches exactly with Method 1: 7 − 6 + 2 = 3 ✅

### 🎯 Exam Tip

This method is useful for **visual/manual inspection** — just look at the CFG drawing, count enclosed regions, add 1.

---

### Method 3: Count Branch Expressions + 1

Simply look through the program code (you don't even need the CFG!) and count how many **branch expressions** (if, while, for, etc.) are there, then add 1.

> **V(G) = Number of branch/decision statements + 1**

### 🛠 Example from Transcript

If a program has 2 decision statements (one while and one if):
V(G) = 2 + 1 = **3** ✅

### 🎯 Exam Tip

This is the **quickest method** for exam-style questions. Just count if/while/for/do-while statements in the code and add 1.

---

## Concept 10: What McCabe's Metric Tells Us

### 📌 Multiple Uses

McCabe's Cyclomatic Metric provides:

1. **Minimum number of test cases** needed to achieve path coverage.
2. **A quantitative measure of testing difficulty** — higher metric means more effort needed.
3. **An indication of reliability** — code with high cyclomatic complexity is likely to have bugs even after testing.
4. **Code quality indicator** — companies normally require code to have **less than 10** McCabe's metric.

### 🧠 Simple Explanation

If V(G) = 50, it means:
- You need at least 50 test cases
- The code is extremely complicated (too many branches)
- It will likely still have bugs even after testing
- The code is NOT good — it should be simplified

If V(G) = 5, it means:
- You need only about 5 test cases
- The code is fairly simple and manageable

### 🎯 Exam Important

> Companies normally require that the code should have **McCabe's metric less than 10**.

### ⚠️ Critical Understanding

Knowing the cyclomatic metric tells us **how many** linearly independent paths exist, but it does **NOT tell us what those paths are**. Even after knowing the number, finding those actual paths in a large CFG is very hard.

---

## Concept 11: Practical Path Testing

### 📌 How is Path Testing Done in Practice?

Since we cannot easily identify the actual linearly independent paths for large programs, here's what is done practically:

### 📊 Step-by-Step Process

**Step 1:** The tester proposes an **initial set of test data** based on experience and judgment.

**Step 2:** A tool called a **Dynamic Program Analyzer** is used.

**Step 3:** The Dynamic Program Analyzer measures **which parts of the program have been tested** — specifically, how many linearly independent paths have been covered.

**Step 4:** The result is used to **determine when to stop testing**.

### 🧠 How the Dynamic Program Analyzer Works

1. Given a program, it generates its **Control Flow Graph**.
2. The CFG nodes are the statements, and edges are determined by statement types.
3. As the program executes with test cases, the tool **marks which edges are executed**.
4. As long as a test case executes **at least one new edge**, we know a **new path** has been covered.
5. Using McCabe's metric, we know the total number of paths expected.
6. The tool computes the **percentage path coverage** achieved.

### 🎯 Key Point (Exam Important)

> Path testing does NOT require us to find the actual paths and manually write test cases for each. We execute with random/chosen test cases and **measure the path coverage achieved**. As long as we achieve high path coverage, we say path testing has been done.

---

## Concept 12: Derivation of Test Cases (For Small Programs)

### 📌 For Small Programs Only

For very small programs (3–4 lines), we CAN manually derive test cases:

### 📊 Steps for Small Program Path Testing

**Step 1:** Draw the Control Flow Graph.

**Step 2:** Determine V(G) — the cyclomatic complexity.

**Step 3:** Determine the set of linearly independent paths.

**Step 4:** Prepare test cases that **force execution along each path**.

### 🛠 Example from Transcript

For the program:
```
int f1(int x, int y) {
1  while (x != y) {
2      if (x > y) then
3          x = x - y;
4      else y = y - x;
5  }
6  return x;
}
```

**Number of decision statements:** 2 (while and if)
**V(G) = 2 + 1 = 3** → We might expect up to 3 paths.

**Actual linearly independent paths found:** 2 paths (note: McCabe's gives an upper bound, actual may be fewer)

- **Path 1:** 1 → 2 → 3 → 5 → 1 → 6
- **Path 2:** 1 → 2 → 4 → 5 → 1 → 6

**Number of independent paths: 3** (from the slide)
- Path: 1 → 6 (test case: x=1, y=1)
- Path: 1 → 2 → 3 → 5 → 1 → 6 (test case: x=1, y=2)
- Path: 1 → 2 → 4 → 5 → 1 → 6 (test case: x=2, y=1)

### ⚠️ Important Note

This manual method is **NOT practical for larger programs**. It only works for small programs where you can easily trace all paths.

---

## Concept 13: Application of Cyclomatic Complexity — Psychological Complexity

### 📌 Interesting Application

McCabe's metric is not just useful for testing — it is also a measure of the **psychological complexity** of a program.

### 🧠 What Does This Mean?

A relationship exists between:
- **McCabe's metric**
- **The number of errors existing in the code**
- **The time required to find and correct the errors**

Additionally, cyclomatic complexity indicates:
- **Psychological complexity** of a program
- **Difficulty level** of understanding the program

### 🛠 How It Works

If you give a piece of code to a third person (who was NOT involved in developing it) and ask them to understand it:

- If V(G) = 50 → They will have to spend **substantial effort** understanding the code.
- If V(G) = 5 or 1 → They will spend **very little time** understanding the code.

### 🧠 Why Does This Happen?

To understand a program, a person **unconsciously traverses paths** through the code. They either:
- Look at outputs and trace back which inputs produce them, OR
- Look at inputs and trace forward to see what output is produced.

Both approaches require traversing paths. If there are more paths (higher cyclomatic complexity), it takes longer to understand the code.

### 🎯 Exam Important

> McCabe's metric = Measure of **psychological complexity** = **Difficulty level** of understanding the program.

> Higher V(G) → More errors likely → More effort to understand → More effort to test.

---

## Quick Revision Summary

| Concept | Key Point |
|---|---|
| Path Testing | Design test cases to cover all linearly independent paths |
| Path Coverage | All linearly independent paths executed at least once |
| CFG | Graph where nodes = statements, edges = control flow |
| Three Statement Types | Sequence, Selection, Iteration |
| Iteration CFG Mistake | After loop body, control goes back to loop condition — NOT to next statement |
| Path Definition | Node-edge sequence from start node to terminal node |
| All-Path Testing | Impractical due to loops creating infinite paths |
| Linearly Independent Path | A path that introduces at least one new edge |
| McCabe's V(G) | Upper bound on number of linearly independent paths |
| Formula Method | V(G) = E − N + 2 |
| Visual Method | V(G) = Number of bounded areas + 1 |
| Code Inspection Method | V(G) = Number of branch expressions + 1 |
| Industry Standard | Companies require V(G) < 10 |
| Dynamic Program Analyzer | Tool that measures path coverage during testing |
| Psychological Complexity | V(G) correlates with difficulty of understanding code |

---

## 10 MCQs — Strictly from Lecture 13 Transcript

---

### Q1. What is the definition of path coverage?

(A) All statements in the program are executed at least once
(B) All branches in the program are taken at least once
(C) All linearly independent paths in the program are executed at least once
(D) All possible paths in the program are executed at least once

**Answer: (C)**

**Explanation:** The transcript defines path coverage as designing test cases such that all linearly independent paths in the program are executed at least once. Option (A) describes statement coverage, (B) describes branch coverage, and (D) describes all-path testing which is impractical.

---

### Q2. In a Control Flow Graph, what do the nodes represent?

(A) Variables in the program
(B) Statements of the program
(C) Conditions in the program
(D) Functions in the program

**Answer: (B)**

**Explanation:** As stated in the transcript, a CFG is a graph representation where the statements of the program are nodes of the graph, and edges represent the sequence in which control flows.

---

### Q3. Every structured program is composed of which three types of statements?

(A) Input, Output, Processing
(B) Sequence, Selection, Iteration
(C) Declaration, Assignment, Return
(D) Read, Write, Compute

**Answer: (B)**

**Explanation:** The transcript explicitly states that every structured program consists of three types of statements: Sequence, Selection (if-then-else, switch), and Iteration (while, for, do-while).

---

### Q4. In drawing a CFG for a while loop, after the last statement inside the loop body, where does the control go?

(A) To the next statement after the loop
(B) To the loop condition (back to the while node)
(C) To the first statement of the loop body
(D) The program terminates

**Answer: (B)**

**Explanation:** The transcript specifically warns about this common mistake. After the last statement in the loop body, control does NOT go to the next statement after the loop. Instead, it goes back to the loop condition node for re-evaluation. Only if the condition becomes false does control transfer to the next statement after the loop.

---

### Q5. Why is all-path testing impractical?

(A) Because programs have too many variables
(B) Because of the presence of loops which create an extremely large number of paths
(C) Because test data is hard to generate
(D) Because programs are too long

**Answer: (B)**

**Explanation:** The transcript explains that in the presence of loops, each iteration of the loop creates a new path, making the number of paths extremely large. This makes all-path testing impractical, which is why we use linearly independent paths instead.

---

### Q6. If a Control Flow Graph has 7 edges and 6 nodes, what is the cyclomatic complexity?

(A) 1
(B) 2
(C) 3
(D) 4

**Answer: (C)**

**Explanation:** Using the formula V(G) = E − N + 2 = 7 − 6 + 2 = 3. This exact example is given in the transcript.

---

### Q7. Which of the following is NOT a valid method to compute McCabe's Cyclomatic Complexity as mentioned in the transcript?

(A) E − N + 2
(B) Number of bounded areas + 1
(C) Number of branch expressions + 1
(D) Number of statements + 1

**Answer: (D)**

**Explanation:** The transcript mentions three methods: (A) E − N + 2, (B) number of bounded areas + 1, and (C) number of branch expressions (if, while, etc.) + 1. "Number of statements + 1" is not a valid method — statements include non-branching statements which don't affect complexity.

---

### Q8. What does a Dynamic Program Analyzer do?

(A) It automatically generates test cases
(B) It measures which parts of the program have been tested and determines path coverage
(C) It fixes bugs in the code
(D) It draws the control flow graph only

**Answer: (B)**

**Explanation:** The transcript describes the Dynamic Program Analyzer as a tool that measures which parts of the program have been tested. It marks which edges are executed by test cases, tracks how many linearly independent paths have been covered, and its result is used to determine when to stop testing.

---

### Q9. Companies normally require that the McCabe's metric for code should be less than:

(A) 5
(B) 10
(C) 20
(D) 50

**Answer: (B)**

**Explanation:** The transcript states that companies normally require that the code should have less than 10 McCabe's metric. A value of 50 would indicate extremely complicated code that is not good.

---

### Q10. McCabe's Cyclomatic Complexity is also a measure of:

(A) The number of variables in the program
(B) The memory usage of the program
(C) The psychological complexity and difficulty level of understanding the program
(D) The execution time of the program

**Answer: (C)**

**Explanation:** The transcript explicitly states that McCabe's metric is a measure of the psychological complexity of the program or the difficulty level of understanding the program. A higher cyclomatic complexity means a person would need more effort to understand the code because they unconsciously traverse all paths while trying to comprehend it.

---

*End of Lecture 13 Notes — Path Testing*
