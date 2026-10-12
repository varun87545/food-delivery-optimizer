# Food Delivery Route Optimizer

A small data structures and algorithms project that models food delivery on a weighted city graph. Orders enter a priority queue, a dispatcher chooses the rider who can reach the restaurant soonest, and Dijkstra's algorithm finds the fastest route from rider to restaurant to customer.

## Features

- Weighted undirected graph stored as an adjacency list.
- Heap-based Dijkstra search with early exit and stale-entry skipping.
- Priority queue ordering: urgent orders first, then older orders, then insertion order.
- Rider assignment based on earliest restaurant arrival.
- Interactive SVG city map with animated delivery routes.
- Click any two map locations to see the shortest path and travel time.
- Step-by-step visualization of Dijkstra's settled nodes and edge relaxations.
- Rider-count selector for one to four riders.
- Optional nearest-neighbour batching for up to two orders, with a comparison against one-order assignments.
- Python unit tests using only the standard library.

## Files

| File | Purpose |
|---|---|
| `index.html` | Self-contained interactive website for GitHub Pages |
| `delivery_optimizer.py` | Python reference implementation; standard library only |
| `test_delivery_optimizer.py` | Unit tests for graph, queue, dispatcher, and batching behavior |
| `website-screenshot.png` | Screenshot used in the README and presentation |
| `Delivery_Route_Optimizer_updated.pptx` | Revised five-slide project presentation with screenshot and complexity slide |
| `project-report.md` | Short written project report |
| `project-report.pdf` | PDF version of the report |

## How to run

### Website

Open `index.html` directly in a browser, or publish the folder through GitHub Pages. No build step or JavaScript package is required.

1. Add individual orders or press **Add sample orders**.
2. Choose the number of riders and a dispatch strategy.
3. Press **Dispatch orders** to animate the routes.
4. Click two locations on the map to inspect a shortest path.
5. Read the exploration panel to see Dijkstra's algorithm work through the graph.

### Python

```bash
python3 delivery_optimizer.py
```

Run the tests with:

```bash
python3 -m unittest -v
```

## Screenshot

![Food Delivery Route Optimizer website](https://private-us-east-1.manuscdn.com/sessionFile/VEBuL67Hz5dPDfd2n4ZOlq/sandbox/E2HeVxolpZRU5MhJky32TJ-images_1790746025307_na1fn_L2hvbWUvdWJ1bnR1L2RlbGl2ZXJ5LXJvdXRlLW9wdGltaXplci93ZWJzaXRlLXNjcmVlbnNob3Q.png?Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9wcml2YXRlLXVzLWVhc3QtMS5tYW51c2Nkbi5jb20vc2Vzc2lvbkZpbGUvVkVCdUw2N0h6NWRQRGZkMm40Wk9scS9zYW5kYm94L0UySGVWeG9scFpSVTVNaEpreTMyVEotaW1hZ2VzXzE3OTA3NDYwMjUzMDdfbmExZm5fTDJodmJXVXZkV0oxYm5SMUwyUmxiR2wyWlhKNUxYSnZkWFJsTFc5d2RHbHRhWHBsY2k5M1pXSnphWFJsTFhOamNtVmxibk5vYjNRLnBuZyIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTc5MzQ5MTIwMH19fV19&Key-Pair-Id=K2QY5QTL8JSY6C&Signature=MEUCIQCrs9Kk~ABJ87GVTnnyQXh-NeHAmatr2COmPoqb8oVKlQIgFaAgNrScBFpCSdGM3ichLAcgi2xPii5I~C8mT1YyzJE_)

The live page is designed to work as a static GitHub Pages site. The screenshot shows the city graph, restaurant and home nodes, weighted roads, and the interactive controls below the map.

## Data structures and algorithms

- **Graph:** `Graph.adj` is a dictionary-like adjacency list. Each edge stores a neighbor and a non-negative travel time.
- **Dijkstra:** A min-heap stores frontier entries `(distance, node)`. The implementation ignores stale entries and can stop as soon as the target is removed from the heap.
- **Order queue:** `OrderQueue` stores `Order` objects in a min-heap with the key `(priority, created_at, sequence)`.
- **Rider assignment:** For each pending order, the dispatcher computes each rider's arrival time at the restaurant and selects the smallest one.
- **Batching:** The optional heuristic tests whether adding one nearby pending order to a rider's route reduces total route work. It is deliberately limited to two orders so the idea stays explainable.

## Complexity analysis

Let `V` be the number of locations, `E` the number of roads, `R` the number of riders, and `O` the number of orders.

- `Graph.add_edge`: **O(1)** per direction; adjacency-list space is **O(V + E)**.
- Dijkstra with a binary heap: **O((V + E) log V)** time and **O(V + E)** graph/working space.
- Shortest-path reconstruction: **O(V)** in the worst case.
- Regular dispatch: up to `R` shortest-path searches per order, so **O(O · R · (V + E) log V)** in the straightforward implementation.
- Priority queue insertion and removal: **O(log O)** per operation.
- Batching comparison: the two-order nearest-neighbour heuristic checks candidate pairs and shortest paths, so it adds an extra factor proportional to the number of pending candidates. It is useful for a classroom demonstration, but it is not a guaranteed global optimum.

## Future work

- Add time-dependent traffic weights.
- Compare Dijkstra with A* on a larger map.
- Load graph and order data from CSV or JSON.
- Add rider capacity, restaurant preparation time, and delivery time windows.
- Replace the nearest-neighbour batching heuristic with a more rigorous vehicle-routing optimization method.
- Add automated browser tests for map clicks and dispatch animation.
