
# Sirens: Benchmarking Job Scheduling Heuristics in Networked Environments

David Barsky, Ryan Marcus, Olga Papaemmanouil

---

# Schedule

1. Problem Statement
2. Introduction and Context
3. Definitions
4. Goals
5. Results
6. Conclusions

---

# Problem Statement

Cluster computing has allowed users with large volumes of data to distribute work over clusters of commodity hardware.

Running the cluster can be expensive, but efficient distribution can reduce costs significantly.

Sirens measures the efficacy of different distribution techniques.

---

# Introduction

Static scheduling of a program—as represented by by a directed acyclic graph—onto a multiprocessor system to minimize program completion time is a common problem in parallel processing.

Finding an optimal schedule is an NP-complete problem, where a brute-force solution can be $$O(n!)$$.

Heuristics make this intractable problem tractable, albeit at the expense of optimality.

^ Read Paragraph 1. “It’s been studied for ages. Very common problem with many solutions”.
  Read paragraph 2. “On smaller problem, NP-completeness isn’t that big an issue. Computers can handle it. On larger inputs, these problems become intractable.”
  Read paragraph 3. “This tradeoff allows us to solve problems that would otherwise be impossible.”

---

# Introduction

Sirens benchmarks how much optimality is traded across various graph shapes and heuristics.

Traditionally, research in this field focused on multi-processor environments.

Sirens approaches job scheduling through the lens of distributed environments, which are characterized by congested and unreliable networks.

^ Read paragraph 1. What tradeoffs we make are context-sensitive. We want to figure out these tradeoffs.
  Read Paragraph 2.
  Read paragraph 3.

---

# Definitions

Directed acyclic graphs are structures useful for representing arbitrary, freeform networks between entities.

Graphs consist of vertices $$V$$ and edges $$E$$, where $$V$$ are the entities and $$E$$ are the connections between the entities. Therefore, we can define a graph to be $$G=(V, E)$$.

Additionally, directed acyclic graphs have two additional properties: directionality and acyclically.

---

# Definitions

**Directionality**: Edges point from vertex to subsequent vertex, creating an explicit path.
**Acyclically**: A path that starts at vertex $$a$$ and ends at $$a$$ does not exist.

Taken together, directed acyclic graphs are uniquely adept at representing connected actions over time.

---

# Definitions

![inline 50%](images/erdos-sample.png)

^ This is a graph with 10 nodes generated using Erdos' technique.

---

# Definitions

Sirens modifies the terminology established for directed acyclic graphs. We redefine graphs to be jobs, vertices to be tasks, and edges to be communication costs.

^ I might use the terms interchangeably. They are the same thing.

---

# Definitions

Sirens makes frequent use of a structure known as a critical path. Critical paths are paths in a directed acyclic graph that represent the shortest time that a graph can be traversed from start to finish.

---

# Definitions

![inline 50%](images/erdos-sample.png)

---

# Definitions

Sirens models itself on Apache Spark, which is a library for large-scale cluster computing.

Spark describes itself as an "advanced [directed acyclic graph] execution engine".

Therefore, we will be a simple directed acyclic graph execution engine that provides robust reporting.

^ Read Paragraph one. “With a slight twist, many pathfinding/resource optimization problems can be expressed in terms of directed acyclic graphs.”
  Read paragraph two.

[^1]: http://spark.apache.org

---

# Definitions

Apache Spark allows for processing of large data sets in a declarative manner.

The user can declare their intention through code, and Spark will try to find an optimal schedule for that intention. Tools like SQL embody this  declarative mindset.

---

# Definitions

The code below runs on one machine.

```scala
def fancyWordCount(words: List[String]): Int = {
  words
    .filter(_.someCondition)
    .map(removePunctuation)
    .map(removeVerbs)
    .sum
}
```

---

# Definitions

The code below runs on many machines.

```scala
def fancyWordCount(words: List[String]): Int = {
  words
    .filter(_.someCondition)
    .map(removePunctuation)
    .map(removeVerbs)
    .sum
}
```

---

# Goals

We are interested in maintaining parallelism, minimizing cost, and reducing network communication costs. 

Sirens tries to determine which heuristics are best at achieving these goals and under what circumstances.

---

# Goals

We implemented several algorithms that share/allow for the following properties: 

- Arbitrary graph structure
- Arbitrary computational costs
- Communication between tasks
- No duplication between tasks
- No upper bound on the number of processors

---

# Goals

| Heuristic             | Time Complexity |
|-----------------------|-----------------|
| Dynamic Critical Path | $$O(v^3)$$      |
| Edge Zero             | $$O(v(e + v))$$ |
| Linear Cluster        | $$O(v(e + v))$$ |
| Round Robin           | $$O(n)$$        |

^ The time complexity alone does not reveal the behavior on graphs!

---

# Results

We measured the cost of a schedule (as generated by each scheduler) across graphs with different properties. We implemented $$n$$ schedulers across $$m$$ graphs for a $$n \times m$$ sets of experiments. 

"Cost" is an abstract unit, derived from Amazon Web Services' cost structure, that represents the time for a schedule to complete. For cloud providers like AWS, Azure, or GCP, billing follows a "pay-as-you-go" model.


---

# Results

| Graph Generation Technique | Notes                                          |
|----------------------------|------------------------------------------------|
| Cholesky                   | Uses a Cholesky Decomposition of a matrix.     |
| Erdős GNM                  | Uses Erdős–Rény's graph generation technique.  |
| Sparse LU                  | Uses the LU factorization technique.           |
| Poisson                    | Uses Poisson's matrix factorization technique. |
| Fork/Join                  | Uses a fan-in/fan-out technique.               |

^ We picked these because they showed the greatest variance.

--- 

# Samples 

---

![inline](images/graphviz/cholesky-sample.png)

^ Cholesky

---

![inline](images/graphviz/forkjoin-sample.png)

^ forkjoin

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

# Results

| Scheduler             | Time (in Milliseconds) |
|-----------------------|------------------------|
| Edge Zero             | 29                     |
| Linear Cluster        | 30                     |
| Round Robin           | 28                     |
| Dynamic Critical Path | 4200                   |

---

# Conclusions

We've seen two primary tradeoffs.

1. Time vs. Optimality (expected, this is an NP-complete problem).
2. Number of Critical Paths versus Optimality (context-sensitive, room for further work.)

---

# Conclusions

If time is not a concern or the task graph is sufficiently small, an $$O(n^3)$$ algorithm works wonders. 

Otherwise, graphs with few critical paths are best scheduled with Linear Cluster. Graphs with more critical paths are best handled by Edge Zero.

---

# Acknowledgements