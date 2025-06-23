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


---------------------------------------
### INDEXED FILE ALLOCATION 

#include <stdio.h>
struct IndexedFile {
    char name;
    int index_block;
    int size;
    int blocks[10];
};
int main() {
    int n, i, j;
    struct IndexedFile files[10];
    printf("Indexed File Allocation\n");
    printf("Enter number of files: ");
    scanf("%d", &n);
    getchar(); // Clear newline
    for (i = 0; i < n; i++) {
        printf("\nEnter name of file %d: ", i + 1);
        scanf("%c", &files[i].name);
        getchar(); // Consume newline
        printf("Enter index block number for file %c: ", files[i].name);
        scanf("%d", &files[i].index_block);
        printf("Enter number of blocks used by file %c: ", files[i].name);
        scanf("%d", &files[i].size);
        printf("Enter block numbers: ");
        for (j = 0; j < files[i].size; j++) {
            scanf("%d", &files[i].blocks[j]);
        }
        getchar(); // Consume newline
    }
    printf("\nFile\tIndex Block\tSize\tBlocks\n");
    for (i = 0; i < n; i++) {
        printf("%c\t%d\t\t%d\t", files[i].name, files[i].index_block, files[i].size);
        for (j = 0; j < files[i].size; j++) {
            printf("%d", files[i].blocks[j]);
            if (j != files[i].size - 1)
                printf(", ");
        }
        printf("\n");
    }
    return 0;
}


## SEQUENTIAL 


#include <stdio.h>
struct SequentialFile {
    int start_block;
    int length;
};
int main() {
    int n, i, file_number;
    struct SequentialFile files[10];
    printf("Sequential File Allocation\n");
    printf("Enter number of files: ");
    scanf("%d", &n);
    for (i = 0; i < n; i++) {
        printf("Enter starting block of file %d: ", i + 1);
        scanf("%d", &files[i].start_block);
        printf("Enter length (no. of blocks) for file %d: ", i + 1);
        scanf("%d", &files[i].length);
    }
    printf("\nFile\tStart Block\tLength\n");
    for (i = 0; i < n; i++) {
        printf("%d\t%d\t\t%d\n", i + 1, files[i].start_block, files[i].length);
    }
    printf("\nEnter file number to display blocks: ");
    scanf("%d", &file_number);
    if (file_number >= 1 && file_number <= n) {
        int start = files[file_number - 1].start_block;
        int length = files[file_number - 1].length;
        printf("Blocks occupied by file %d: ", file_number);
        for (i = 0; i < length; i++) {
            printf("%d ", start + i);
        }
        printf("\n");
    } else {
        printf("Invalid file number.\n");
    }
    return 0;
}



#LINKED 

#include <stdio.h>
struct LinkedFile {
    char name;
    int start;
    int size;
    int blocks[10];
};
int main() {
    int n, i, j;
    struct LinkedFile files[10];
    printf("Linked File Allocation\n");
    printf("Enter number of files: ");
    scanf("%d", &n);
    getchar(); // Clear newline character
    for (i = 0; i < n; i++) {
        printf("Enter name of file %d: ", i + 1);
        scanf("%c", &files[i].name);
        getchar(); // To consume newline
        printf("Enter starting block of file %c: ", files[i].name);
        scanf("%d", &files[i].start);
        printf("Enter number of blocks: ");
        scanf("%d", &files[i].size);
        printf("Enter block numbers: ");
        for (j = 0; j < files[i].size; j++) {
            scanf("%d", &files[i].blocks[j]);
        }
        getchar(); // To consume newline
    }    printf("\nFile\tStart\tSize\tBlocks\n");
    for (i = 0; i < n; i++) {
        printf("%c\t%d\t%d\t", files[i].name, files[i].start, files[i].size);
        for (j = 0; j < files[i].size; j++) {
            printf("%d", files[i].blocks[j]);
            if (j != files[i].size - 1)
                printf(" --> ");
        }
        printf("\n");
    }
    return 0;
}
---------------------------------------


