#include <iostream>

#define MAX_VERTICES 100

class Graph {
private:
    int V;
    int graph[MAX_VERTICES][MAX_VERTICES];

    int minDistance(int dist[], bool sptSet[]) {
        int min = __INT_MAX__, min_index;

        for (int v = 0; v < V; v++) {
            if (!sptSet[v] && dist[v] <= min) {
                min = dist[v];
                min_index = v;
            }
        }

        return min_index;
    }

public:
    Graph(int v) : V(v) {
        for (int i = 0; i < V; i++) {
            for (int j = 0; j < V; j++) {
                graph[i][j] = 0;
            }
        }
    }

    void addEdge(int source, int destination, int weight) {
        graph[source][destination] = weight;
        graph[destination][source] = weight;
    }

    void dijkstra(int src) {
        int dist[MAX_VERTICES];
        bool sptSet[MAX_VERTICES] = { false };

        for (int i = 0; i < V; i++) {
            dist[i] = __INT_MAX__;
        }

        dist[src] = 0;

        for (int count = 0; count < V - 1; count++) {
            int u = minDistance(dist, sptSet);
            sptSet[u] = true;

            for (int v = 0; v < V; v++) {
                if (!sptSet[v] && graph[u][v] && dist[u] != __INT_MAX__ && dist[u] + graph[u][v] < dist[v]) {
                    dist[v] = dist[u] + graph[u][v];
                }
            }
        }

        std::cout << "Shortest Distances from Vertex " << src << ":" << std::endl;
        for (int i = 0; i < V; i++) {
            std::cout << "Vertex " << i << ": ";
            if (dist[i] == __INT_MAX__) {
                std::cout << "INFINITY" << std::endl;
            } else {
                std::cout << dist[i] << std::endl;
            }
        }
    }
};

int main() {
    int V = 9;
    Graph g(V);

    g.addEdge(0, 1, 4);
    g.addEdge(0, 7, 8);
    g.addEdge(1, 2, 8);
    g.addEdge(1, 7, 11);
    g.addEdge(2, 3, 7);
    g.addEdge(2, 8, 2);
    g.addEdge(2, 5, 4);
    g.addEdge(3, 4, 9);
    g.addEdge(3, 5, 14);
    g.addEdge(4, 5, 10);
    g.addEdge(5, 6, 2);
    g.addEdge(6, 7, 1);
    g.addEdge(6, 8, 6);
    g.addEdge(7, 8, 7);

    int source = 0;

    g.dijkstra(source);

    return 0;
}
