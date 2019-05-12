#### 题目：MST: Minimum Spanning Tree
Given a connected graph with V nodes and E edges. Find a collection of edges that connects all nodes together without any cycle and with the minimum total edge weight. 

hint: what’s the time complexity of your algorithm? Can you make the running time O(E * log(E)) by using Union Find? 

### 解答：
```c++
#include "stdafx.h"
#include <vector>
#include <iostream>
#include <algorithm>

using namespace std;

struct Edge
{
	Edge(int s, int d, int w)
	{
		src = s;
		dest = d;
		weight = w;
	}

	int src, dest, weight;
};

struct Graph
{
	int V, E;
	vector<Edge> edge;
};

struct subset
{
	subset(int p, int r)
	{
		parent = p;
		rank = r;
	};

	int parent;
	int rank;
};

int find(vector<subset> & subsets, int i)
{
	if (subsets[i].parent != i)
		subsets[i].parent = find(subsets, subsets[i].parent);

	return subsets[i].parent;
}

void Union(vector<subset> & subsets, int x, int y)
{
	int xroot = find(subsets, x);
	int yroot = find(subsets, y);

	if (subsets[xroot].rank < subsets[yroot].rank)
		subsets[xroot].parent = yroot;
	else if (subsets[xroot].rank > subsets[yroot].rank)
		subsets[yroot].parent = xroot;
	else {
		subsets[yroot].parent = xroot;
		subsets[xroot].rank++;
	}
}

void KruskalMST(struct Graph* graph)
{
	std::sort(graph->edge.begin(), graph->edge.end()
		, [](struct Edge & l, struct Edge & r) { return (l.weight < r.weight); } );

	vector<subset> subsets;
	for (int v = 0; v < graph->V; ++v) {
		subsets.push_back(subset(v, 0));
	}

	vector<Edge> result;
	for ( auto & next_edge : graph->edge) {
		if (result.size() >= graph->V - 1)
			break;

		int x = find(subsets, next_edge.src);
		int y = find(subsets, next_edge.dest);

		if (x != y) {
			result.push_back(next_edge);
			Union(subsets, x, y);
		}
	}

	for (auto element : result) {
		printf("%d -- %d = %d\n", element.src, element.dest, element.weight);
	}
}

int main()
{
	Graph graph;
	graph.E = 5;
	graph.V = 4;
	graph.edge.push_back(Edge(0, 1, 10));
	graph.edge.push_back(Edge(0, 2, 6));
	graph.edge.push_back(Edge(0, 3, 5));
	graph.edge.push_back(Edge(1, 3, 15));
	graph.edge.push_back(Edge(2, 3, 4));

	KruskalMST(&graph);

	return 0;
}
···
