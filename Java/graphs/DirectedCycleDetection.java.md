```java
/**
Cycle detection in a directed graph
using DFS. Here, status array shows if the node is in the path that we are currently on. This is used to detect any presence of back edges.
backedge present <=> cycle present
**/

import java.io.*;
import java.util.*;

public class DirectedCycleDetection {
	private static boolean cycle;
	private static void dfs_visit(ArrayList<ArrayList<Integer> > graph, int src, boolean[] status, boolean[] visit) {
		visit[src] = true;
		status[src] = true;
		for(int i = 0; i < graph.get(src).size(); i++)
			if(visit[graph.get(src).get(i)] == false)
			    dfs_visit(graph, graph.get(src).get(i), status, visit);
			else
			    if(status[graph.get(src).get(i)] == true)
				    cycle = true;
	    status[src] = false;
	}

	private static void dfs(ArrayList<ArrayList<Integer> > graph, int vertex, boolean[] status) {
		boolean[] visit = new boolean[v];
		for(int i = 0; i < vertex; i++)
			if(visit[i] == false)
				dfs_visit(graph, i, status, visit);
	}

	private static void detect_cycle(ArrayList<ArrayList<Integer> > graph, int vertex) {
		boolean[] status = new boolean[vertex];
		dfs(graph, vertex, status);
		if(cycle == false)
		    System.out.println("No cycle exits in the given graph");
	    else
		    System.out.println("Cycle exists in the given graph");
	}

	public static void main(String[] args) {
		cycle = false;
		// vertex denotes number of vertices
		// edge denotes number of edges
		// a, b are temporary variables to take input
		int vertex, edge, a, b;
		// all vertices are labelled from 0 to v-1
		Scanner sc= new Scanner(System.in);
		vertex = sc.nextInt();
		edge = sc.nextInt();
		ArrayList<ArrayList<Integer> > graph = new ArrayList<ArrayList<Integer> >(vertex);
		for (int i = 0; i < vertex; i++)
            graph.add(new ArrayList<Integer>());
		// all directed edges
		for(int i = 0; i < edge; i++) {
			a = sc.nextInt();
			b = sc.nextInt();
			// edge a -> b
			graph.get(a).add(b);
		}
		detect_cycle(graph, vertex);
	}
}

/**
Input :
6 8
0 3
0 4
5 0
1 5
1 0
2 1
3 4
4 5
Output :
Cycle exists in the given graph

Time Complexity : O(vertex+edge)
Space Complexity : O(vertex)
**/

```