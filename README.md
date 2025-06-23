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


#FCFS page replacement


#include <stdio.h>
int main() { int pages[100], frames[10], n, f, i, j, k = 0, faults = 0, hit = 0, found;
printf("Enter number of pages: "); 
scanf("%d", &n); 
printf("Enter the page reference string:\n"); 
for(i = 0; i < n; i++) { 
    scanf("%d", &pages[i]); 
} 
printf("Enter number of frames: "); 
scanf("%d", &f); 
// Initialize all frames to -1 (empty) 
for(i = 0; i < f; i++) { 
    frames[i] = -1; 
} 
printf("\nPage\tFrames\t\tPage Fault\n"); 
for(i = 0; i < n; i++) { 
    found = 0; 
    // Check if page is already in frame 
    for(j = 0; j < f; j++) { 
        if(frames[j] == pages[i]) { 
            found = 1; 
            hit++; 
            break; 
        } 
    } 
    if(!found) { 
        // Replace the oldest page using FIFO 
        frames[k] = pages[i]; 
        k = (k + 1) % f; // circular queue 
        faults++; 
    } 
    // Display frame contents 
    printf("%d\t", pages[i]); 
    for(j = 0; j < f; j++) { 
        if(frames[j] != -1) 
            printf("%d ", frames[j]); 
        else 
            printf("- "); 
    } 
    printf("\t%s\n", found ? "Hit" : "Miss"); 
} 
printf("\nTotal Page Faults: %d", faults); 
printf("\nTotal Page Hits: %d\n", hit); 
return 0; 
}



#LRU PAGE 

#include <stdio.h>
int findLRU(int time[], int f) {
    int min = time[0], pos = 0;
    for (int i = 1; i < f; i++) {
        if (time[i] < min) {
            min = time[i];
            pos = i;
        }
    }
    return pos;
}
int main() {
    int pages[100], frames[10], time[10], n, f;
    int i, j, pos, faults = 0, counter = 0, hits = 0;
    int found;
    printf("Enter number of pages: ");
    scanf("%d", &n);
    printf("Enter the page reference string:\n");
    for (i = 0; i < n; i++) {
        scanf("%d", &pages[i]);
        }
    printf("Enter number of frames: ");
    scanf("%d", &f);
    for (i = 0; i < f; i++) {
        frames[i] = -1;
        time[i] = 0;
    }    printf("\nPage\tFrames\t\tStatus\n");
    for (i = 0; i < n; i++) {
        found = 0;
        for (j = 0; j < f; j++) {
            if (frames[j] == pages[i]) {
                counter++;
                time[j] = counter;
                found = 1;
                hits++;
                break;
            }
        }
       if (!found) {
            int empty_found = 0;
            for (j = 0; j < f; j++) {
                if (frames[j] == -1) {
                    counter++;
                    frames[j] = pages[i];
                    time[j] = counter;
                    faults++;
                    empty_found = 1;
                    break;
                }
            }
            if (!empty_found) {
                pos = findLRU(time, f);
                counter++;
                frames[pos] = pages[i];
                time[pos] = counter;
                faults++;
            }
        }
        // Print page and frame status
        printf("%d\t", pages[i]);
        for (j = 0; j < f; j++) {
            if (frames[j] != -1)
                printf("%d ", frames[j]);
            else
                printf("- ");
        }
        printf("\t%s\n", found ? "Hit" : "Miss");
    }
    printf("\nTotal Page Faults: %d", faults);
    printf("\nTotal Page Hits: %d\n", hits);
    return 0;
}


# OPTIMAL PAGE REPLACEMENT


#include <stdio.h>
int predict(int pages[], int frames[], int n, int index, int f) {
    int farthest = index;
    int result = -1;
    int i, j;
    for (i = 0; i < f; i++) {
        int j;
        for (j = index; j < n; j++) {
            if (frames[i] == pages[j]) {
                if (j > farthest) {
                    farthest = j;
                    result = i;
                }
                break;
            }
        }
        // If a page is never used again
        if (j == n)
            return i;
    }
    // If all pages are going to be used again
    return (result == -1) ? 0 : result;
}
int main() {
    int pages[100], frames[10], n, f;
    int i, j, faults = 0, hits = 0;
    int found;
    printf("Enter number of pages: ");
    scanf("%d", &n);
    printf("Enter the page reference string:\n");
    for (i = 0; i < n; i++) {
        scanf("%d", &pages[i]);
    }
    printf("Enter number of frames: ");
    scanf("%d", &f);
    for (i = 0; i < f; i++)
        frames[i] = -1;
printf("\nPage\tFrames\t\tStatus\n");
    for (i = 0; i < n; i++) {
        found = 0;
        // Page Hit
        for (j = 0; j < f; j++) {
            if (frames[j] == pages[i]) {
                found = 1;
                hits++;
                break;
            }
        }
        // Page Miss
        if (!found) {
            int replaced = 0;
            // Empty slot found
            for (j = 0; j < f; j++) {
                if (frames[j] == -1) {
                    frames[j] = pages[i];
                    faults++;
                    replaced = 1;
                    break;
                }
            }
            // No empty frame -> Replace optimal
            if (!replaced) {
                int pos = predict(pages, frames, n, i + 1, f);
                frames[pos] = pages[i];
                faults++;
            }
        }
        // Output current status
        printf("%d\t", pages[i]);
        for (j = 0; j < f; j++) {
            if (frames[j] != -1)
                printf("%d ", frames[j]);
            else
                printf("- ");
        }
        printf("\t%s\n", found ? "Hit" : "Miss");
    }
    printf("\nTotal Page Faults: %d", faults);
    printf("\nTotal Page Hits: %d\n", hits);
    return 0;
}


---------------------------------------


#FCFS CPU SCHEDULING 

#include <stdio.h>
int main() {
    int n, i, j;
    int at[20], bt[20], ct[20], tat[20], wt[20], pid[20];
    int total_tat = 0, total_wt = 0;
    int temp;
    printf("Enter number of processes: ");
    scanf("%d", &n);
    for(i = 0; i < n; i++) {
        pid[i] = i + 1;
        printf("Enter Arrival Time for Process P%d: ", pid[i]);
        scanf("%d", &at[i]);
        printf("Enter Burst Time for Process P%d: ", pid[i]);
        scanf("%d", &bt[i]);
    }
    for(i = 0; i < n - 1; i++) {
        for(j = i + 1; j < n; j++) {
            if(at[i] > at[j]) {
                temp = at[i]; at[i] = at[j]; at[j] = temp;
                temp = bt[i]; bt[i] = bt[j]; bt[j] = temp;
                temp = pid[i]; pid[i] = pid[j]; pid[j] = temp;
            }
        }
    }
    int current_time = 0;
    for(i = 0; i < n; i++) {
        if(current_time < at[i])
            current_time = at[i];
        current_time += bt[i];
        ct[i] = current_time;
        tat[i] = ct[i] - at[i];        
        wt[i] = tat[i] - bt[i];       
        total_tat += tat[i];
        total_wt += wt[i];
    }
printf("\nProcess\tAT\tBT\tCT\tTAT\tWT\n");
    for(i = 0; i < n; i++) {        printf("P%d\t%d\t%d\t%d\t%d\t%d\n", pid[i], at[i], bt[i], ct[i], tat[i], wt[i]);
    }
    printf("\nAverage Turnaround Time = %.2f", (float)total_tat / n);
    printf("\nAverage Waiting Time = %.2f\n", (float)total_wt / n);
    printf("\nGantt Chart:\n|");
    for(i = 0; i < n; i++) {
        printf("  P%d  |", pid[i]);
    }
    printf("\n%d", at[0] < 0 ? 0 : at[0]);
    for(i = 0; i < n; i++) {
        printf("     %d", ct[i]);
    }
    printf("\n");
    return 0;
}



### NON PREEMPTIVE SJFFF

#include <stdio.h>
int main() {
    int n, i, shortest, time = 0, completed_count = 0;
    int at[20], bt[20], ct[20], tat[20], wt[20], pid[20], completed[20] = {0};
    int total_tat = 0, total_wt = 0;
    // For Gantt Chart
    int gantt_pid[20], gantt_start[20], gantt_end[20], gantt_index = 0;
    // Input number of processes
    printf("Enter number of processes: ");
    scanf("%d", &n);
    // Input arrival time and burst time
    for(i = 0; i < n; i++) {
        pid[i] = i + 1;
        printf("Enter Arrival Time for Process P%d: ", pid[i]);
        scanf("%d", &at[i]);
        printf("Enter Burst Time for Process P%d: ", pid[i]);
        scanf("%d", &bt[i]);
    }
    // Main SJF scheduling loop
    while(completed_count < n) {
        int min_bt = 9999;
        shortest = -1;
        // Find the process with the shortest burst time at current time
        for(i = 0; i < n; i++) {
            if(at[i] <= time && completed[i] == 0 && bt[i] < min_bt) {
                min_bt = bt[i];
                shortest = i;
            }
        }
        if(shortest == -1) {
            time++; // CPU is idle
        } else {
            gantt_pid[gantt_index] = pid[shortest];
            gantt_start[gantt_index] = time;
            time += bt[shortest];               // Process runs to completion
            ct[shortest] = time;
            completed[shortest] = 1;
            completed_count++;
            tat[shortest] = ct[shortest] - at[shortest];   // TAT = CT - AT
            wt[shortest] = tat[shortest] - bt[shortest];
            total_tat += tat[shortest];
            total_wt += wt[shortest];
            gantt_end[gantt_index] = time;
            gantt_index++;
        }
    }
printf("\nProcess\tAT\tBT\tCT\tTAT\tWT\n");
    for(i = 0; i < n; i++) {        printf("P%d\t%d\t%d\t%d\t%d\t%d\n", pid[i], at[i], bt[i], ct[i], tat[i], wt[i]);
    }
    printf("\nAverage Turnaround Time = %.2f", (float)total_tat / n);
    printf("\nAverage Waiting Time = %.2f\n", (float)total_wt / n);
    // Gantt chart display
    printf("\nGantt Chart:\n");
    printf("|");
    for(i = 0; i < gantt_index; i++) {
        printf("  P%d  |", gantt_pid[i]);
    }
    printf("\n");
    printf("%d", gantt_start[0]);
    for(i = 0; i < gantt_index; i++) {
        printf("     %d", gantt_end[i]);
    }
    printf("\n");
    return 0;
}



### PREEMPTIVE SJFF (SRTF) 


#include <stdio.h>
int main() {
    int n, i, time = 0, smallest = -1;
    int at[20], bt[20], rt[20], ct[20], wt[20], tat[20], pid[20];
    int completed = 0, min_rt = 9999;
    float total_wt = 0, total_tat = 0;
    int gantt_pid[100], gantt_time[100], gantt_index = 0;
    printf("Enter number of processes: ");
    scanf("%d", &n);
    for(i = 0; i < n; i++) {
        pid[i] = i + 1;
        printf("Enter Arrival Time for Process P%d: ", pid[i]);
        scanf("%d", &at[i]);
        printf("Enter Burst Time for Process P%d: ", pid[i]);
        scanf("%d", &bt[i]);
        rt[i] = bt[i];
    }
    int prev_process = -1;
    while(completed < n) {
        smallest = -1;
        min_rt = 9999;
        for(i = 0; i < n; i++) {
            if(at[i] <= time && rt[i] > 0 && rt[i] < min_rt) {
                min_rt = rt[i];
                smallest = i;
            }
        }
        if(smallest == -1) {
            if(prev_process != -1) {
                gantt_pid[gantt_index] = -1;                gantt_time[gantt_index++] = time;
                prev_process = -1;
            }
            time++;
            continue;
        }
        if(prev_process != pid[smallest]) {
            gantt_pid[gantt_index] = pid[smallest];
            gantt_time[gantt_index++] = time;
            prev_process = pid[smallest];
        }
        rt[smallest]--;
        time++;
        if(rt[smallest] == 0) {
            completed++;
            ct[smallest] = time;
            tat[smallest] = ct[smallest] - at[smallest];
            wt[smallest] = tat[smallest] - bt[smallest];
            total_tat += tat[smallest];
            total_wt += wt[smallest];
        }
    }
    gantt_time[gantt_index] = time;    printf("\nProcess\tAT\tBT\tCT\tTAT\tWT\n");
    for(i = 0; i < n; i++) {        printf("P%d\t%d\t%d\t%d\t%d\t%d\n", pid[i], at[i], bt[i], ct[i], tat[i], wt[i]);
    }
    printf("\nAverage Waiting Time = %.2f", total_wt / n);
    printf("\nAverage Turnaround Time = %.2f\n", total_tat / n);
m
    printf("\nGantt Chart:\n");
    for(i = 0; i < gantt_index; i++) {
        if(gantt_pid[i] == -1)
            printf("| idle ");
        else
            printf("| P%d ", gantt_pid[i]);
    }
    printf("|\n");
    printf("%d", gantt_time[0]);
    for(i = 1; i <= gantt_index; i++) {
        printf("     %d", gantt_time[i]);
    }
    printf("\n");
    return 0;
}



### NON PREEMPTIVE PRIORITY 


#include <stdio.h>
int main() {
    int n, i, j, time = 0;
    int at[20], bt[20], ct[20], tat[20], wt[20], pid[20], priority[20];
    int completed[20] = {0}, total_completed = 0;
    float total_wt = 0, total_tat = 0;
    int gantt_pid[20], gantt_start[20], gantt_end[20], gantt_index = 0;
    printf("Enter number of processes: ");
    scanf("%d", &n);
    for(i = 0; i < n; i++) {
        pid[i] = i + 1;
        printf("Enter Arrival Time for P%d: ", pid[i]);
        scanf("%d", &at[i]);
        printf("Enter Burst Time for P%d: ", pid[i]);
        scanf("%d", &bt[i]);
        printf("Enter Priority for P%d (lower number = higher priority): ", pid[i]);
        scanf("%d", &priority[i]);
    }
    while(total_completed < n) {
        int highest = -1;
        int min_priority = 9999;
        for(i = 0; i < n; i++) {
            if(at[i] <= time && !completed[i]) {
                if(priority[i] < min_priority) {
                    min_priority = priority[i];
                    highest = i;
                } else if(priority[i] == min_priority) {
                    if(at[i] < at[highest]) {
                        highest = i;
                    }
                }
            }
        }
        if(highest == -1) {
            time++;
        } else {
            gantt_pid[gantt_index] = pid[highest];
            gantt_start[gantt_index] = time;
            time += bt[highest];
            ct[highest] = time;
            completed[highest] = 1;
            total_completed++;
            tat[highest] = ct[highest] - at[highest];
            wt[highest] = tat[highest] - bt[highest];
            total_tat += tat[highest];
            total_wt += wt[highest];
            gantt_end[gantt_index] = time;
            gantt_index++;
        }
    }    printf("\nProcess\tAT\tBT\tPri\tCT\tTAT\tWT\n");
    for(i = 0; i < n; i++) {        printf("P%d\t%d\t%d\t%d\t%d\t%d\t%d\n", pid[i], at[i], bt[i], priority[i], ct[i], tat[i], wt[i]);
    }
    printf("\nAverage Waiting Time = %.2f", total_wt / n);
    printf("\nAverage Turnaround Time = %.2f\n", total_tat / n);
    printf("\nGantt Chart:\n|");
    for(i = 0; i < gantt_index; i++) {
        printf(" P%d |", gantt_pid[i]);
    }
    printf("\n%d", gantt_start[0]);
    for(i = 0; i < gantt_index; i++) {
        printf("    %d", gantt_end[i]);
    }
    printf("\n");
    return 0;
}




### PREEMPTIVE PRIORITYY 


#include <stdio.h>
int main() {
    int n, i, time = 0, highest = -1, completed = 0;
    int at[20], bt[20], rt[20], pr[20], ct[20], tat[20], wt[20], pid[20];
    float total_tat = 0, total_wt = 0;
    int gantt_pid[100], gantt_time[100], gantt_index = 0;
    printf("Enter number of processes: ");
    scanf("%d", &n);
    for(i = 0; i < n; i++) {
        pid[i] = i + 1;
        printf("Enter Arrival Time for P%d: ", pid[i]);
        scanf("%d", &at[i]);
        printf("Enter Burst Time for P%d: ", pid[i]);
        scanf("%d", &bt[i]);
        printf("Enter Priority for P%d (lower number = higher priority): ", pid[i]);
        scanf("%d", &pr[i]);
        rt[i] = bt[i];
    }
    int prev_process = -1;
    while(completed < n) {
        highest = -1;
        int min_priority = 9999;
        for(i = 0; i < n; i++) {
            if(at[i] <= time && rt[i] > 0) {
                if(pr[i] < min_priority) {
                    min_priority = pr[i];
                    highest = i;
                } else if(pr[i] == min_priority) {
                    if(at[i] < at[highest]) {
                        highest = i;
                    }
                }
            }
        }
        if(highest == -1) {
            if(prev_process != -1) {
                gantt_pid[gantt_index] = -1;                gantt_time[gantt_index++] = time;
                prev_process = -1;
            }
            time++;
            continue;
        }
        if(prev_process != pid[highest]) {
            gantt_pid[gantt_index] = pid[highest];
            gantt_time[gantt_index++] = time;
            prev_process = pid[highest];
        }
        rt[highest]--;
        time++;
        if(rt[highest] == 0) {
            completed++;
            ct[highest] = time;
            tat[highest] = ct[highest] - at[highest];
            wt[highest] = tat[highest] - bt[highest];
            total_tat += tat[highest];
            total_wt += wt[highest];
        }
    }
    gantt_time[gantt_index] = time;    printf("\nProcess\tAT\tBT\tPri\tCT\tTAT\tWT\n");
    for(i = 0; i < n; i++) {
        printf("P%d\t%d\t%d\t%d\t%d\t%d\t%d\n", pid[i], at[i], bt[i], pr[i], ct[i], tat[i], wt[i]);
    }
    printf("\nAverage Waiting Time = %.2f", total_wt / n);
    printf("\nAverage Turnaround Time = %.2f\n", total_tat / n);
    printf("\nGantt Chart:\n");
    for(i = 0; i < gantt_index; i++) {
        if(gantt_pid[i] == -1)
            printf("| idle ");
        else
            printf("| P%d ", gantt_pid[i]);
    }
    printf("|\n");
    printf("%d", gantt_time[0]);
    for(i = 1; i <= gantt_index; i++) {
        printf("     %d", gantt_time[i]);
    }
    printf("\n");
    return 0;
}




#### ROUND ROBIN 


#include <stdio.h>
int main() {
    int n, i, time = 0, tq, count = 0;
    int at[20], bt[20], rt[20], ct[20], wt[20], tat[20], pid[20];
    int completed = 0;
    int gantt_pid[100], gantt_time[100], gantt_index = 0;
    printf("Enter number of processes: ");
    scanf("%d", &n);
    for(i = 0; i < n; i++) {
        pid[i] = i + 1;
        printf("Enter Arrival Time for P%d: ", pid[i]);
        scanf("%d", &at[i]);
        printf("Enter Burst Time for P%d: ", pid[i]);
        scanf("%d", &bt[i]);
        rt[i] = bt[i];
    }
    printf("Enter Time Quantum: ");
    scanf("%d", &tq);
    int queue[100], front = 0, rear = 0;
    int visited[20] = {0};
    // Enqueue processes which have arrived at time 0
    for(i = 0; i < n; i++) {
        if(at[i] == 0) {
            queue[rear++] = i;
            visited[i] = 1;
        }
    }
    while(completed < n) {
        if(front == rear) {
            // CPU idle, increment time and enqueue newly arrived processes
            time++;
            for(i = 0; i < n; i++) {
                if(at[i] == time && visited[i] == 0) {
                    queue[rear++] = i;
                    visited[i] = 1;
                }
            }
            continue;
        }
        int idx = queue[front++];
        if(rt[idx] > 0) {
            // Record start time for Gantt chart if process changed
            if(gantt_index == 0 || gantt_pid[gantt_index - 1] != pid[idx]) {
                gantt_pid[gantt_index] = pid[idx];                gantt_time[gantt_index++] = time;
            }
          int exec_time = (rt[idx] < tq) ? rt[idx] : tq;
            rt[idx] -= exec_time;
            time += exec_time;
            // Enqueue newly arrived processes during execution
            for(i = 0; i < n; i++) {
                if(at[i] > time - exec_time && at[i] <= time && visited[i] == 0) {
                    queue[rear++] = i;
                    visited[i] = 1;
                }
            }
            if(rt[idx] == 0) {
                completed++;
                ct[idx] = time;
                tat[idx] = ct[idx] - at[idx];
                wt[idx] = tat[idx] - bt[idx];
            } else {
                queue[rear++] = idx; // Re-queue the process if not finished
            }
        }
    }
    gantt_time[gantt_index] = time;   printf("\nProcess\tAT\tBT\tCT\tTAT\tWT\n");
    for(i = 0; i < n; i++) {    printf("P%d\t%d\t%d\t%d\t%d\t%d\n", pid[i], at[i], bt[i], ct[i], tat[i], wt[i]);
    }
    float total_wt = 0, total_tat = 0;
    for(i = 0; i < n; i++) {
        total_wt += wt[i];
        total_tat += tat[i];
    }
    printf("\nAverage Waiting Time = %.2f", total_wt / n);
    printf("\nAverage Turnaround Time = %.2f\n", total_tat / n);
    printf("\nGantt Chart:\n|");
    for(i = 0; i < gantt_index; i++) {
        printf(" P%d |", gantt_pid[i]);
    }
    printf("\n%d", gantt_time[0]);
    for(i = 1; i <= gantt_index; i++) {
        printf("    %d", gantt_time[i]);
    }
    printf("\n");
    return 0;
}


_______________________________________



#### FORKKKK GETPID 


#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
int main() {
    pid_t pid;
    printf("Before the Fork process ID: %d\n", getpid());
    pid = fork();
    if (pid < 0) {
        // fork failed
        perror("fork failed");
        exit(1);
    }
    else if (pid == 0) {
        // child process
        printf("After the Fork, Child Process ID = %d\n", getpid());
    }
    else {
        // parent process
        printf("After the Fork, Parent Process ID = %d\n", getpid());
    }
    return 0;
}


##### SLEEP 



#!/bin/bash
# Simulate getpid in the parent process
echo "Parent PID: $$"
# Start a background process to simulate the child
sleep 1 &
# Get the PID of the last background process (child)
child_pid=$!
# Simulate getpid in the child process
echo "Child PID: $child_pid"
# Print parent PID from the child's perspective (background process)
echo "Parent PID from child: $$" &
# Wait for the background child process to finish
wait $child_pid


______________________________________
