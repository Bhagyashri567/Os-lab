# Os-lab
###FIRST FIT 


#include <stdio.h>
int main() {
    int m, n;
    printf("Enter number of blocks: ");
    scanf("%d", &m);
    int b[m], bf[m];
    printf("Enter sizes of blocks:\n");
    for (int i = 0; i < m; i++) {
        printf("Block %d: ", i + 1);
        scanf("%d", &b[i]);
        bf[i] = b[i]; // initialize available sizes
    }
    printf("Enter number of processes: ");
    scanf("%d", &n);
    int p[n];
    printf("Enter sizes of processes:\n");
    for (int i = 0; i < n; i++) {
        printf("Process %d: ", i + 1);
        scanf("%d", &p[i]);
    }
    printf("\n--- First Fit Allocation ---\n");
    printf("Process Size\tBlock Allocated\n");
    for (int i = 0; i < n; i++) {
        int allocated = 0;
        for (int j = 0; j < m; j++) {
            if (bf[j] >= p[i]) {
                printf("%d\t\tBlock %d\n", p[i], j + 1);
                bf[j] -= p[i];
                allocated = 1;
                break;
            }
        }
        if (!allocated) {
            printf("%d\t\tNot Allocated\n", p[i]);
        }
    }
    return 0;
}


##BEST FIT 


#include <stdio.h>
int main() {
    int m, n;
    printf("Enter number of blocks: ");
    scanf("%d", &m);
    int b[m], bf[m];
    printf("Enter sizes of blocks:\n");
    for (int i = 0; i < m; i++) {
        printf("Block %d: ", i + 1);
        scanf("%d", &b[i]);
        bf[i] = b[i];
    }
    printf("Enter number of processes: ");
    scanf("%d", &n);
    int p[n];
    printf("Enter sizes of processes:\n");
    for (int i = 0; i < n; i++) {
        printf("Process %d: ", i + 1);
        scanf("%d", &p[i]);
    }
    printf("\n--- Best Fit Allocation ---\n");
    printf("Process Size\tBlock Allocated\n");
    for (int i = 0; i < n; i++) {
        int best = -1;
        for (int j = 0; j < m; j++) {
            if (bf[j] >= p[i]) {
                if (best == -1 || bf[j] < bf[best])
                    best = j;
            }
        }
        if (best != -1) {
            printf("%d\t\tBlock %d\n", p[i], best + 1);
            bf[best] -= p[i];
        } else {
            printf("%d\t\tNot Allocated\n", p[i]);
        }
    }
    return 0;
}


#### WORST FIT


#include <stdio.h>
int main() {
    int m, n;
    printf("Enter number of blocks: ");
    scanf("%d", &m);
    int b[m], bf[m];
    printf("Enter sizes of blocks:\n");
    for (int i = 0; i < m; i++) {
        printf("Block %d: ", i + 1);
        scanf("%d", &b[i]);
        bf[i] = b[i];
    }
    printf("Enter number of processes: ");
    scanf("%d", &n);
    int p[n];
    printf("Enter sizes of processes:\n");
    for (int i = 0; i < n; i++) {
        printf("Process %d: ", i + 1);
        scanf("%d", &p[i]);
    }
    printf("\n--- Worst Fit Allocation ---\n");
    printf("Process Size\tBlock Allocated\n");
    for (int i = 0; i < n; i++) {
        int worst = -1;
        for (int j = 0; j < m; j++) {
            if (bf[j] >= p[i]) {
                if (worst == -1 || bf[j] > bf[worst])
                    worst = j;
            }
        }
        if (worst != -1) {
            printf("%d\t\tBlock %d\n", p[i], worst + 1);
            bf[worst] -= p[i];
        } else {
            printf("%d\t\tNot Allocated\n", p[i]);
        }
    }
    return 0;
}



--------------------------------------


#### BANKERS ALGORITHM

#include <iostream>
using namespace std;
int main() {
    int n, r;
    int alloc[5][5], max[5][5], need[5][5], avail[5], done[5] = {}, safe[5], count = 0;
    cout << "Enter number of processes and resources: ";
    cin >> n >> r;
    cout << "Enter Allocation Matrix:\n";
    for (int i = 0; i < n; i++)
        for (int j = 0; j < r; j++)
            cin >> alloc[i][j];
    cout << "Enter Maximum Matrix:\n";
    for (int i = 0; i < n; i++)
        for (int j = 0; j < r; j++) {
            cin >> max[i][j];
            need[i][j] = max[i][j] - alloc[i][j];
        }
    cout << "Enter Available Resources:\n";
    for (int i = 0; i < r; i++)
        cin >> avail[i];
    while (count < n) {
        bool found = false;
        for (int i = 0; i < n; i++) {
            if (!done[i]) {
                bool canRun = true;
                for (int j = 0; j < r; j++)
                    if (need[i][j] > avail[j])
                        canRun = false;
                if (canRun) {
                    for (int j = 0; j < r; j++)
                        avail[j] += alloc[i][j];
                    safe[count++] = i;
                    done[i] = 1;
                    found = true;
                }
            }
        }
        if (!found) break;
    }
    if (count == n) {
        cout << "Safe Sequence: ";
        for (int i = 0; i < n; i++)
            cout << "P" << safe[i] << (i < n - 1 ? " -> " : "\n");
        cout << "System is in a SAFE state.\n";
    } else {
        cout << "System is NOT in a safe state.\n";
    }
    return 0;
}
