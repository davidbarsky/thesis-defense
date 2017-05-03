
# Sirens: Benchmarking Job Scheduling Heuristics in Networked Environments

David Barsky, Ryan Marcus, Olga Papaemmanouil

---

# Schedule

1. Problem Statement
2. Introduction and Context
3. Definitions
4. Goals
5. Demo
6. Conclusions

---

# Problem Statement

Cluster computing has allowed users with large volumes of data to distribute workflows over clusters of commodity hardware.

Running the cluster can be expensive, but efficent distribution can reduce costs significantly.

Sirens measures how the efficacy of different distribution techniques.

---

# Introduction

Static scheduling of a program—as represented by by a directed acyclic graph—onto a multiprocessor system to minimize program completion time is a common problem in parallel processing.

Finding an optimal schedule is an NP-complete problem.

Heuristics make this intractable problem tractable, albeit at the expense of optimality.

^ Read Paragraph 1. “It’s been studied for ages. Very common problem with many solutions”.
  Read paragraph 2. “On smaller problem, NP-completeness isn’t that big an issue. Computers can handle it. On larger inputs, these problems become intractable.”
  Read paragraph 3. “This tradeoff allows us to solve problems that would otherwise be impossible.”

---

# Introduction

Sirens benchmarks how much optimality is traded across various graph shapes and heuristics.

Traditionally, research in this field focused on multi-processor environments.

Sirens approaches job scheduling through the lens of distributed enviroments, which are characterized by congested and unreliable networks.

^ Read paragraph 1. What tradeoffs we make are context-sensitive. We want to figure out these tradeoffs.
  Read Paragraph 2.
  Read paragraph 3.

---

# Definitions

Directed acyclic graphs are structures useful for representing arbitrary, freeform networks between entities.

Graphs consist of verticies $$V$$ and edges $$E$$, where $$V$$ are the entities and $$E$$ are the connections between the entities. Therefore, we can define a graph to be $$G=(V, E)$$.

Additionally, directed ayclic graphs have two additional properties: directionality and acyclically.

---

# Definitions

**Directionality**: Edges point to subsequent verticies, creating an explicit path.
**Acyclically**: It is impossible to follow a path from vertex $$a$$ and end at $$a$$.

Taken together, directed ayclic graphs are uniquely adept at representing connected actions over time.

---

# Definitions

![inline 50%](images/erdos-sample.png)

^ This is a grpah with 10 nodes generated using Erdos' technique.

---

# Definitions

Sirens modifies the terminology established for directed acyclic graphs. We redefine graphs to be jobs, verticies to be tasks, and edges to be communication costs.

^ I might use the terms interchagably. They are the same thing.

---

# Definitions

Sirens frequently intersects with critical paths. Critical paths are paths in a directed acyclic graph that represent the shortest time that a graph can be completed in.

They span from start to finish.

---

# Definitions

Sirens models itself on Apache Spark, which is a library for large-scale cluster computing.

Spark describes itself as an “advanced [directed acylic graph] execution engine”.

Therefore, we will be a simple directed ayclic graph execution engine that provides robust reporting.

^ Read Paragraph one. “With a slight twist, many pathfinding/resource optimization problems can be expressed in terms of directed acyclic graphs.”
  Read paragraph two.

[^1]: http://spark.apache.org

---

# Goals

We are interested in maintaining parallelism, minimizing cost, and reducing network communication costs. 

Sirens tries to determine which algorithms are best at achieving these goals and under what circumstances.

---

# Goals

We implemented several algorithms that share/allow for the following properties: 

- Arbitrary graph structure
- Arbitrary computational costs
- Communication between tasks
- No duplication between tasks
- No upper bound on the number of processors

---

# Results

![inline](images/results-cholesky.png)

---

# Results

![inline](images/results-erdos.png)

---

# Results

![inline](images/results-poisson.png)

---

# Results

![inline](images/results-sparse.png)


---

# Results

![inline](images/results-fork.png)

---

# Conclusions

We've seen two primary tradeoffs.

1. Time vs. Optimality (expected, this is an NP-complete problem).
2. Number of Critical Paths versus Optimality (context-sensative, room for further work.)

---

# Conclusions

If time is not a concern or the task graph is sufficently small, an $$O(n^3)$$ algorithm works wonders. 

Otherwise, graphs with few critical paths are best scheduled with Linear Cluster. Graphs with more critical paths are best handled by Edge Zero.