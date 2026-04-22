# Directed Graph Acyclicity Checker

A Java-based tool for determining whether a directed graph is acyclic (DAG) using the **Sink Elimination Algorithm** and detecting cycles via **Depth-First Search**.

## Overview

This project implements efficient algorithms to:
- ✓ Check if a directed graph is acyclic using sink elimination
- ✓ Detect and return an actual cycle if one exists
- ✓ Process large-scale benchmark graphs (up to 10,240 vertices)

## Features

| Feature | Details |
|---------|---------|
| **Algorithm** | Sink Elimination (O(V + E)) |
| **Cycle Detection** | DFS with 3-color marking |
| **Data Structure** | Adjacency list with HashSets |
| **Input Format** | Plain text: `u v` (edge from u to v) |
| **Scalability** | Handles graphs with thousands of vertices |

## Project Structure

```
dsa-directed-graph-acyclicity/
├── src/
│   ├── Main.java                 # Entry point with demo and file modes
│   ├── DirectedGraph.java        # Graph representation and operations
│   ├── AcyclicityChecker.java    # Core algorithms
│   └── GraphParser.java          # File parsing utility
├── bin/                          # Compiled class files (.class)
├── benchmarks/
│   ├── acyclic/                  # 40 acyclic test graphs
│   └── cyclic/                   # 40 cyclic test graphs
└── README.md                     # This file
```

## Getting Started

### Compilation

```bash
javac -d bin src/*.java
```

This compiles all Java files from `src/` and outputs `.class` files to `bin/`.

#### Using Build Scripts (Easier!)

**Windows**:
```bash
build.bat
```

**Linux/Mac**:
```bash
bash build.sh
```

#### Alternative (individual compilation):
```bash
cd src
javac *.java
cd ..
```

### Usage

**Run with demo mode** (two built-in examples):
```bash
java -cp bin Main
```

**Run on a benchmark file**:
```bash
java -cp bin Main benchmarks/acyclic/a_40_0.txt
java -cp bin Main benchmarks/cyclic/c_40_0.txt
```

#### Quick Start (Linux/Mac):
```bash
javac -d bin src/*.java
java -cp bin Main
```

#### Quick Start (Windows PowerShell):
```powershell
javac -d bin src\*.java
java -cp bin Main
```

### Example Output

```
============================================================
DEMO 1: Acyclic graph  |  Edges: 0->1, 1->2, 0->2
============================================================
--- Running Sink Elimination Algorithm ---
Step 1: Sink found -> vertex 2 (out-degree = 0). Removing it. Vertices remaining after removal: 2
Step 2: Sink found -> vertex 1 (out-degree = 0). Removing it. Vertices remaining after removal: 1
Step 3: Sink found -> vertex 0 (out-degree = 0). Removing it. Vertices remaining after removal: 0
=> Result: YES - graph IS acyclic (all 3 vertices successfully eliminated as sinks).
```

## Algorithm Details

### Sink Elimination (Task 4)

The algorithm works by repeatedly removing sink vertices (vertices with out-degree 0):

1. If the graph is empty → **acyclic** ✓
2. If no sink exists → **cycle detected** ✗
3. Remove a sink and repeat

**Complexity**: O(V + E)

```java
DirectedGraph graph = GraphParser.parse("input.txt");
boolean isAcyclic = AcyclicityChecker.isAcyclic(graph);
```

### Cycle Detection (Task 5)

If the graph is cyclic, find the actual cycle using DFS with vertex coloring:

- **WHITE (0)**: Not yet visited
- **GREY (1)**: Currently on DFS stack
- **BLACK (2)**: Fully explored

A back edge to a **GREY** vertex indicates a cycle.

```java
DirectedGraph graphCopy = GraphParser.parse("input.txt");
List<Integer> cycle = AcyclicityChecker.findCycle(graphCopy);
AcyclicityChecker.printCycle(cycle);
```

## Design Decisions

### Why Adjacency List Over Matrix?

| Property | Matrix | List |
|----------|--------|------|
| **findSink()** | O(n²) | O(V) |
| **Memory** | O(n²) | O(V + E) |
| **Sparse Graphs** | Inefficient | Efficient |

The adjacency list approach with out-degree tracking is optimal for sparse and dense graphs.

### Key Data Structures

- **outEdges**: List of HashSets tracking outgoing edges
- **inEdges**: Reverse map for efficient vertex removal
- **outDegree[]**: Array tracking active out-degree (enables O(V) sink finding)
- **active[]**: Boolean array marking removed vertices

## Benchmark Tests

The project includes **80 test graphs**:
- **40 acyclic graphs** (a_N_i.txt): Variable sizes from 40 to 10,240 vertices
- **40 cyclic graphs** (c_N_i.txt): Same sizes for comparison

Test sizes: 40, 80, 160, 320, 640, 1,280, 2,560, 5,120, 10,240 (5 variations each)

## Time & Space Complexity

| Operation | Time | Space |
|-----------|------|-------|
| Build graph | O(E) | O(V + E) |
| Find sink | O(V) | O(1) |
| Remove vertex | O(in-degree) | O(1) |
| Acyclicity check | O(V + E) | O(V) |
| Cycle detection | O(V + E) | O(V) |

## Input File Format

**Plain text format** - each line contains a directed edge:
```
u v
```
Where `u` is the source vertex and `v` is the destination vertex.

**Supported formats**:
- ✓ 0-based numbering: `0 1`, `1 2`
- ✓ 1-based numbering: `1 2`, `2 3`
- ✓ Windows (`\r\n`) and Unix (`\n`) line endings
- ✓ Blank lines and extra whitespace

**Example**:
```
1 2
3 1
2 5
```

## Implementation Highlights

- **Modular design**: Separate concerns (parsing, graph, algorithms)
- **Detailed output**: Every step printed for reproducibility
- **Robust parsing**: Handles various input formats automatically
- **Memory efficient**: Removes vertices without graph reconstruction
- **Comprehensive comments**: Each class and method fully documented

## Module & Academic Info

- **Module**: 5SENG003W Algorithms Coursework 2025/26
- **Student ID**: 20240856
- **Name**: Siddiha Rimzan

## Build Files

- **build.bat** - Windows build script (one-command compilation)
- **build.sh** - Linux/Mac build script (one-command compilation)
- **.gitignore** - Excludes compiled files and IDE configs from version control

---
