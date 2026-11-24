# Batsman Statistics Calculator using LangGraph – Parallel Workflow

## Overview

This project demonstrates how to build a **Non-LLM Parallel Workflow using LangGraph** to calculate cricket batting statistics.

The application uses a graph-based execution approach in which multiple independent calculations are performed in parallel using separate workflow nodes.

Instead of executing each calculation sequentially, the workflow performs three independent calculations simultaneously:

1. **Strike Rate Calculation**
2. **Balls Per Boundary Calculation**
3. **Boundary Percentage Calculation**

Once all three calculations are completed, their results are combined in a final Summary Node to generate a consolidated batting performance summary.

The main objective is to understand how LangGraph handles parallel execution, shared state management, multiple nodes, and workflow synchronization without using an LLM.

## Workflow Architecture

```text
                       START
                         |
            +------------+------------+
            |            |            |
            v            v            v
       Strike Rate   Balls Per     Boundary
       Calculation   Boundary      Percentage
         Node        Calculation   Calculation
                         Node          Node
            |            |            |
            +------------+------------+
                         |
                         v
                    Summary Node
                         |
                         v
                        END
```

## How It Works

### 1. Define Workflow State

A `TypedDict` named `BatsmanState` defines the data structure shared across workflow nodes.

It contains the following fields:

**Input Fields:**
- `runs`: Total runs scored by the batsman.
- `balls`: Total balls faced.
- `fours`: Number of fours scored.
- `sixes`: Number of sixes scored.

**Calculated Fields:**
- `strick_rate`: Calculated batting strike rate.
- `balls_per_boundry`: Average number of balls faced per boundary.
- `boundry_percent`: Percentage of total runs scored through boundaries.

**Output Field:**
- `summary`: Consolidated summary of the calculated statistics.

### 2. Create Independent Calculation Nodes

The workflow contains three independent Python functions.

**Node 1: Calculate Strike Rate**

Calculates the batsman's strike rate based on total runs and balls faced.

Formula:

```text
Strike Rate = (Runs / Balls Faced) × 100
```

**Node 2: Calculate Balls Per Boundary**

Calculates the average number of balls faced per boundary.

Formula:

```text
Balls Per Boundary = Balls Faced / (Fours + Sixes)
```

**Node 3: Calculate Boundary Percentage**

Calculates the percentage of total runs scored through fours and sixes.

Formula:

```text
Boundary Percentage =
((Fours × 4) + (Sixes × 6)) / Total Runs × 100
```

All three nodes independently access the input state and return their respective calculated results.

### 3. Create Summary Node

The `summary` function receives the updated workflow state containing all three calculated statistics.

It combines the results into a readable summary containing:

- Strike Rate
- Balls Per Boundary
- Boundary Percentage

### 4. Build the LangGraph Workflow

- Initialize a `StateGraph` using `BatsmanState`.
- Register the three calculation functions and the summary function as nodes.
- Connect `START` to all three calculation nodes.
- Connect all three calculation nodes to the Summary Node.
- Connect the Summary Node to `END`.

### 5. Compile and Execute

Compile the graph using `graph.compile()`.

Execute the workflow using `workflow.invoke()` with the initial batsman statistics.

The final state contains the original input values, calculated statistics, and consolidated summary.

## Workflow Pattern: Parallel Workflow

This project implements the **Parallel Workflow Pattern**.

Parallel workflows allow multiple independent tasks to execute in the same execution step rather than waiting for one another sequentially.

In this project:

- Strike Rate Calculation runs independently.
- Balls Per Boundary Calculation runs independently.
- Boundary Percentage Calculation runs independently.
- Summary Node executes after all three calculation branches complete.

LangGraph synchronizes the parallel branches before executing the summary node.

## Technologies Used

- Python
- LangGraph
- TypedDict
- Jupyter Notebook

## Project Structure

```text
Batsman_Statistics_Parallel_Workflow/
│
├── batsman_statistics.ipynb
├── README.md
└── requirements.txt
```

## Installation

Install the required dependency:

```bash
pip install -U langgraph
```

## Key LangGraph Concepts Covered

| Concept | Description |
|---|---|
| StateGraph | Defines the graph-based workflow |
| TypedDict | Defines the shared state structure |
| Nodes | Represent individual calculation tasks |
| Edges | Define connections between workflow nodes |
| START | Entry point of the workflow |
| END | Exit point of the workflow |
| Parallel Execution | Executes independent calculation nodes in the same superstep |
| State Management | Shares and updates data across nodes |
| Synchronization | Waits for parallel branches before executing the summary node |
| Graph Compilation | Converts the graph definition into an executable workflow |

## Learning Outcomes

Through this project, I learned how to:

- Build a non-LLM workflow using LangGraph.
- Implement parallel execution using multiple independent nodes.
- Define and manage shared workflow state using `TypedDict`.
- Connect multiple nodes using graph edges.
- Synchronize parallel branches before executing a downstream node.
- Combine outputs from multiple nodes into a single shared state.
- Compile and execute a parallel LangGraph workflow.

## Conclusion

This project demonstrates how LangGraph can orchestrate multiple independent tasks through a parallel execution pattern.

By calculating three batting statistics independently and combining their outputs in a Summary Node, the project illustrates the practical implementation of a **Non-LLM Parallel Workflow in LangGraph**.