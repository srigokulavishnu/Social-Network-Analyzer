#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>
#include <limits.h>

// Function to create and initialize an adjacency matrix
int** createGraph(int numUsers) {
    int** graph = (int**)malloc(numUsers * sizeof(int*));
    for (int i = 0; i < numUsers; i++) {
        graph[i] = (int*)malloc(numUsers * sizeof(int));
        for (int j = 0; j < numUsers; j++) {
            graph[i][j] = 0; // Initialize with 0 (no connection)
        }
    }
    return graph;
}

// Add a friendship (edge) between two users with a given weight
void addFriendship(int** graph, int user1, int user2, int weight) {
    graph[user1][user2] = weight;
    graph[user2][user1] = weight; // Undirected graph
}

// Display the adjacency matrix
void displayGraph(int** graph, int numUsers) {
    printf("Adjacency Matrix:\n");
    for (int i = 0; i < numUsers; i++) {
        for (int j = 0; j < numUsers; j++) {
            printf("%d ", graph[i][j]);
        }
        printf("\n");
    }
}

// Function to find and display isolated users
void findIsolatedUsers(int** graph, int numUsers) {
    printf("\nIsolated Users:\n");
    bool isolatedFound = false;

    for (int i = 0; i < numUsers; i++) {
        bool isIsolated = true;

        // Check if there are no connections for this user (row in matrix)
        for (int j = 0; j < numUsers; j++) {
            if (graph[i][j] != 0) {
                isIsolated = false;
                break;
            }
        }

        if (isIsolated) {
            printf("User %d is isolated.\n", i);
            isolatedFound = true;
        }
    }

    if (!isolatedFound) {
        printf("No isolated users found.\n");
    }
}

// Function to recommend friends for a given user
void recommendFriends(int** graph, int numUsers, int user) {
    printf("\nFriend Recommendations for User %d:\n", user);

    bool* isFriend = (bool*)malloc(numUsers * sizeof(bool));
    for (int i = 0; i < numUsers; i++) {
        isFriend[i] = (graph[user][i] != 0); // Check if connected
    }

    for (int friend = 0; friend < numUsers; friend++) {
        // Check if the node is a direct friend
        if (isFriend[friend]) {
            // Look for friends of this friend (second-degree connections)
            for (int potential = 0; potential < numUsers; potential++) {
                // If not already a friend and not the user itself
                if (!isFriend[potential] && potential != user && graph[friend][potential] != 0) {
                    printf("-> User %d (via User %d)\n", potential, friend);
                }
            }
        }
    }

    free(isFriend);
}

// Function to reconstruct and display the path 
void printPath(int* prev, int start, int end) {
    if (end == -1) {
        printf("No path found.");
        return;
    }

    int path[100];
    int count = 0;
    for (int current = end; current != -1; current = prev[current]) {
        path[count++] = current;
    }

    printf("Path: ");
    for (int i = count - 1; i > 0; i--) {
        printf("User %d -> ", path[i]);
    }
    printf("User %d\n", path[0]);
}

// Find the shortest path using Dijkstra's Algorithm
void dijkstra(int** graph, int numUsers, int start, int end) {
    int* dist = (int*)malloc(numUsers * sizeof(int));
    bool* visited = (bool*)malloc(numUsers * sizeof(bool));
    int* prev = (int*)malloc(numUsers * sizeof(int));

    for (int i = 0; i < numUsers; i++) {
        dist[i] = INT_MAX;
        visited[i] = false;
        prev[i] = -1;
    }
    dist[start] = 0;

    for (int i = 0; i < numUsers; i++) {
        int minDist = INT_MAX, minIndex = -1;
        for (int j = 0; j < numUsers; j++) {
            if (!visited[j] && dist[j] < minDist) {
                minDist = dist[j];
                minIndex = j;
            }
        }

        if (minIndex == -1) break;
        visited[minIndex] = true;

        for (int j = 0; j < numUsers; j++) {
            if (!visited[j] && graph[minIndex][j] > 0 && dist[minIndex] + graph[minIndex][j] < dist[j]) {
                dist[j] = dist[minIndex] + graph[minIndex][j];
                prev[j] = minIndex;
            }
        }
    }

    if (dist[end] == INT_MAX) {
        printf("No path exists between User %d and User %d\n", start, end);
    } else {
        printf("Shortest path from User %d to User %d: %d steps\n", start, end, dist[end]);
        printPath(prev, start, end);
    }

    free(dist);
    free(visited);
    free(prev);
}


int main() {
    int numUsers, choice, user1, user2, weight;

    printf("Enter the number of users in the social network: ");
    scanf("%d", &numUsers);

    int** graph = createGraph(numUsers);

    while (1) {
        printf("\nMenu:\n");
        printf("1. Add friendship\n");
        printf("2. Display social network\n");
        printf("3. Find shortest path\n");
        printf("4. Recommend friends\n");
        printf("5.Isolated Users\n");
        printf("6. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                printf("Enter two user IDs to connect (0 to %d) and weight of the connection: ", numUsers - 1);
                scanf("%d %d %d", &user1, &user2, &weight);
                if (user1 >= 0 && user1 < numUsers && user2 >= 0 && user2 < numUsers && weight > 0) {
                    addFriendship(graph, user1, user2, weight);
                    printf("Friendship added between User %d and User %d with weight %d.\n", user1, user2, weight);
                } else {
                    printf("Invalid input. Please try again.\n");
                }
                break;

            case 2:
                displayGraph(graph, numUsers);
                break;

            case 3:
                printf("Enter the start and end user IDs for shortest path: ");
                scanf("%d %d", &user1, &user2);
                if (user1 >= 0 && user1 < numUsers && user2 >= 0 && user2 < numUsers) {
                    dijkstra(graph, numUsers, user1, user2);
                } else {
                    printf("Invalid user IDs. Please try again.\n");
                }
                break;

            case 4:
                printf("Enter the user ID to get friend recommendations: ");
                scanf("%d", &user1);
                if (user1 >= 0 && user1 < numUsers) {
                    recommendFriends(graph, numUsers, user1);
                } else {
                    printf("Invalid user ID. Please try again.\n");
                }
                break;

            case 5:
                 findIsolatedUsers(graph, numUsers);
                break;


            case 6:
                printf("Exiting program.\n");
                for (int i = 0; i < numUsers; i++) {
                    free(graph[i]);
                }
                free(graph);
                return 0;

            default:
                printf("Invalid choice. Please try again.\n");
        }
    }
}
