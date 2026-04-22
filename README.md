#include <stdio.h>
#include <stdlib.h>
#include <limits.h>

#define MAX 100
#define INF INT_MAX

typedef struct Edge {
    int dest, weight;
    struct Edge* next;
} Edge;

typedef struct Graph {
    int V;
    Edge* adjList[MAX];
} Graph;

Graph* createGraph(int V) {
    Graph* g = malloc(sizeof(Graph));
    g->V = V;
    for(int i=0;i<V;i++) g->adjList[i] = NULL;
    return g;
}

void addEdge(Graph* g, int src, int dest, int weight) {
    Edge* e = malloc(sizeof(Edge));
    e->dest = dest;
    e->weight = weight;
    e->next = g->adjList[src];
    g->adjList[src] = e;
}

int minDistance(int dist[], int visited[], int V) {
    int min = INF, index = -1;
    for(int i=0;i<V;i++) {
        if(!visited[i] && dist[i] < min) {
            min = dist[i];
            index = i;
        }
    }
    return index;
}

void dijkstra(Graph* g, int src) {
    int V = g->V;
    int dist[MAX], visited[MAX];

    for(int i=0;i<V;i++) {
        dist[i] = INF;
        visited[i] = 0;
    }

    dist[src] = 0;

    for(int i=0;i<V-1;i++) {
        int u = minDistance(dist, visited, V);
        visited[u] = 1;

        Edge* temp = g->adjList[u];
        while(temp) {
            int v = temp->dest;
            if(!visited[v] && dist[u] != INF &&
               dist[u] + temp->weight < dist[v]) {
                dist[v] = dist[u] + temp->weight;
            }
            temp = temp->next;
        }
    }

    printf("\nShortest distances from source %d:\n", src);
    for(int i=0;i<V;i++) {
        printf("Vertex %d -> %d\n", i, dist[i]);
    }
}

int main() {
    int V, E, src;

    printf("Enter vertices: ");
    scanf("%d", &V);

    printf("Enter edges: ");
    scanf("%d", &E);

    Graph* g = createGraph(V);

    printf("Enter edges (src dest weight):\n");
    for(int i=0;i<E;i++) {
        int u,v,w;
        scanf("%d %d %d", &u,&v,&w);
        addEdge(g,u,v,w);
    }

    printf("Enter source: ");
    scanf("%d", &src);

    dijkstra(g, src);

    return 0;
}
