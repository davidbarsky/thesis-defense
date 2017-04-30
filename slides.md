build-lists: true

# Sirens: Benchmarking Job Scheduling Heuristics in Networked Environments

David Barsky, Ryan Marcus, Olga Papaemmanouil

---

# Problem Statement

- A computer is really good processing data of all kinds. It's their main job!
- What happens when the data you want to process doesn't fit on your computer? Get a bigger computer!

---

# Problem Statement

- The cost of getting a “bigger” computer is non-linear.
- It's far cheaper to distribute

---

# Introduction

- Static scheduling of a program—as represented by by a directed acyclic graph—onto a multiprocessor system to minimize program completion time is a common problem in parallel processing.
- Finding an optimal schedule is an NP-complete problem.
- Heuristics make this intractable problem tractable, albeit at the expense of optimality.

^ - Read Paragraph 1. “It’s been studied for ages. Very common problem with many solutions”.
  - Read paragraph 2. “On smaller problem, NP-completeness isn’t that big an issue. Computers can handle it. On larger inputs, these problems become intractable.”
  - Read paragraph 3. “This tradeoff allows us to solve problems that would otherwise be impossible.”

---

# Introduction

- Sirens benchmarks how much optimality is traded across various graph shapes and heuristics.
- Traditionally, research in this field focused on multi-processor environments.
- Sirens approaches job scheduling through the lens of networked—or cloud—environments, which tend to have long communication times between machines.

^ - Read paragraph 1. What tradeoffs we make are context-sensitive. We want to figure out these tradeoffs.
  - Read Paragraph 2.
  - Read paragraph 3.

---

## What are some  “real-world” applications of this work?

--- 

# Introduction

- Scheduling problems are everywhere! Consider examples like Amazon’s delivering packages and and Lyft pooling passengers into a single car—all scheduling.
- Sirens is more closely related to Apache Spark, which is a library for large-scale data processing for analytics or machine learning.

^ - Read Paragraph one. “With a slight twist, many pathfinding/resource optimization problems can be expressed in terms of directed acyclic graphs.”
  - Read paragraph two. “A bit more abstract, it tends to run software that is everywhere yet invisible.”

---

# Introduction

- For many companies, analytics are critical for understanding revenue generation and product usage. Spark is great for analytics processing!
- Facebook uses Spark for a pipeline where each job would scales robuts 90 TB per job[^1].

[^1]: For more details, read: code.facebook.com/posts/1671373793181703

---

# Introduction

^ - This is an sample directed acyclic graph, generated through Erdos’ technique.
  - We’re interested in critical paths, which in our model, represent the shortest amount of time that a job—represented by this DAG—can take.