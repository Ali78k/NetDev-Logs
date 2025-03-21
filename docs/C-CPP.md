# C/CPP Training

This page will contain C/CPP trainings. There are many problems to solve. let's go!  
## Basic
In C or any other programming language, there are basic definitions for input/output, variables, mathematical operations, flow control and loops.  
This is a simple C program that simulates a network packet counter, a tool you might find in Linux or telecommunications systems to track data packets. It demonstrates fundamental C programming concepts—**primitive variables**, **input/output**, **control flow**, and **loops**—all working together in a practical example.  
### Code: Network Packet Counter Simulator
The code for network packet counter simulator:  
```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

int main(){
    int cycles;
    int pkt_per_cycle;
    int lost_pkt;
    float pkt_loss;
    // Seed random number generator
    srand(time(NULL));

    while (1){
        printf("Enter number of cycles (1-10) to process (0 to quit): ");
        scanf("%d", &cycles);
        if (cycles == 0){
            printf("Good Bye!\n");
            break;
        }
        else if (cycles < 1 || cycles > 10){
            printf("Invalid number of cycles. Please enter a number between 1 and 10.\n");
            continue;
        }
        else{
            int pkt_counter = 0;
            int pkt_processed = 0;
            for (int i = 0; i < cycles; i++){
                pkt_per_cycle = rand() % 20 + 1; // 1 to 20
                // Ensure denominator for rand() % n is at least 1
                int max_lost = (pkt_per_cycle / 2) > 0 ? pkt_per_cycle / 2 : 1;
                lost_pkt = rand() % max_lost; // Max loss is half, min 1
                pkt_counter += pkt_per_cycle;
                pkt_processed += pkt_per_cycle - lost_pkt;
                pkt_loss = (float)lost_pkt / pkt_per_cycle * 100;
                printf("Cycle %d: %d packets generated, %d packets lost (%.2f%% loss)\n", i + 1, pkt_per_cycle, lost_pkt, pkt_loss);
            }
            float avg_loss = (float)(pkt_counter - pkt_processed) / pkt_counter * 100;
            printf("Total packets generated: %d\n", pkt_counter);
            printf("Total packets processed: %d\n", pkt_processed);
            printf("Average packet loss: %.2f%%\n", avg_loss);
        }
    }
    return 0;
}
```

to compile this code, simply use:  
```bash
 gcc pkt_counter.c -o pkt_counter
```
The expected output is:
```
./pkt_counter 
Enter number of cycles (1-10) to process (0 to quit): 18
Invalid number of cycles. Please enter a number between 1 and 10.
Enter number of cycles (1-10) to process (0 to quit): 6
Cycle 1: 15 packets generated, 6 packets lost (40.00% loss)
Cycle 2: 16 packets generated, 5 packets lost (31.25% loss)
Cycle 3: 9 packets generated, 2 packets lost (22.22% loss)
Cycle 4: 13 packets generated, 0 packets lost (0.00% loss)
Cycle 5: 15 packets generated, 1 packets lost (6.67% loss)
Cycle 6: 10 packets generated, 1 packets lost (10.00% loss)
Total packets generated: 78
Total packets processed: 63
Average packet loss: 19.23%
Enter number of cycles (1-10) to process (0 to quit): 0
Good Bye!
```

## Arrays
Working with several separated element isn't so practical. So we utilize arrays. An array is a bunch of ordered elements with same type. It starts with 0 index. Here is an example.
### Code: Bubble-Sort
Bubbble sort is a method to sort elements. At each iteration, it fixes the biggest existed number. Time complexity of this algorithm is O(n^2^).
```c
#include <stdio.h>

int main()
{
    int arr[] = {7,2,3,8,6,9,12};
    int n = sizeof(arr)/sizeof(arr[0]);
    int temp;
    for (int i=0; i<n; i++)
        for(int j=0; j<n-i; j++)
            if (arr[j] > arr[j+1])
            {
                temp = arr[j];
                arr[j] = arr[j+1];
                arr[j+1] = temp;
            }
    for (int i=0; i<n; i++)
        printf("%d\n", arr[i]);
}
```

### Code: One-Line `for` Loop
In this short code, we want to count number of array elemnts smaller than `4`.  
```c
#include <stdio.h>

int main()
{
    int counter = 0;
    int arr[] = {1,2,3,4,5,6,7,8,9};
    for (int i = 0; i < sizeof(arr)/sizeof(arr[0]); i++)
        counter = arr[i] < 4 ? ++counter : counter;
    printf("Number of elements less than 4: %d\n", counter);
    return 0;
}
```

It could be rewrite as a is shortened code.
```c
#include <stdio.h>

int main()
{
    int counter = 0;
    int arr[] = {1,2,3,4,5,6,7,8,9};
    for (int i = 0; i < sizeof(arr)/sizeof(arr[0]); counter += arr[i++] < 4){}
    printf("Number of elements less than 4: %d\n", counter);
    return 0;
}
```
## Functions
Functions are functions! they take some parameters(arguments) and give the value. Functions are __defined__ and are __called__; If a function is defined after `main()` it should be __declared__ before the it. Functions help to code in a structured manner.
```C
#include <stdio.h>

// Function Declaration
void myFunction(int a, int b)

// main
int main()
{
    /* some
    code here
    */
    
    //Function Call
    myFunction()

}

//Function definition
void myFunction(int a, int b)
{
    /* some
    code here
    */
}

```
Functions don't have to be called inside main, they can call each other and even the can be called by themselves. These functions are __Recursive Functions__.  

### Code: Hanoi Tower
In this code we can define a recursive function to solve the **tower of hanoi** Problem. The time complexity of this solution is O(2^n^).  
Tower of Hanoi is a mathematical puzzle where we have three towers and `n` disks. The objective of the puzzle is to move the entire stack to another tower, obeying the following simple rules:  
1) Only one disk can be moved at a time.  
2) Each move consists of taking the upper disk from one of the stacks and placing it on top of another stack i.e. a disk can only be moved if it is the uppermost disk on a stack.  
3) No disk may be placed on top of a smaller disk.
```C
#include <stdio.h>

void hanoiTower (int n, char src, char helping, char dst);

int main ()
{
    int n = 8;
    char src='S'; char dst='D'; char helping='H';
    hanoiTower(n, src, helping, dst);
    return 0;

}

void hanoiTower (int n, char src, char helping, char dst)
{
    if (n==1)
        printf("Disk %d: %c -> %c\n", n, src, dst);
    else
    {
        hanoiTower(n-1, src, dst, helping);
        printf("Disk %d: %c -> %c\n", n, src, dst);
        hanoiTower(n-1, helping, src, dst);
    }

}

```


## Pointers
Pointer's Placeholder

## File Handling and String Manipulation in C
File handling in C allows programs to interact with files on the system for reading, writing, or modifying data. This is achieved using standard library functions like `fopen`, `fclose`, `fread`, `fwrite`, `fprintf`, and `fscanf`. These functions enable operations such as opening a file in different modes (e.g., read, write, append), reading data line by line or in chunks, and writing formatted or raw data to files.  
Strings in C are represented as **arrays of characters terminated by a null character (`'\0'`)**. Common string manipulation functions include `strlen`, `strcpy`, `strcat`, and `strcmp`, which allow developers to calculate string length, copy strings, concatenate them, or compare them. Combining file handling and string manipulation is essential for tasks like parsing text files, searching for specific patterns, or processing logs.  
### Code: grep-like app  
Below is an example of a simple `grep-like` application in C. This program reads a file line by line via `fgets()`and prints lines containing a specified search string using `strstr`.


```c
#include <stdio.h>
#include <string.h>

#define MAX_LINE_LENGTH 1024

int main (int argc, char *argv[])
{
    if (argc != 3)
    {
        printf ("Usage: %s <filename> <search_string>. \n", argv[0]);
        return 1;
    }

    const char *filename = argv[1];
    const char *search_string = argv[2];

    FILE *file = fopen(filename, "r");

    if (!file)
    {
        perror("cannot open the file");
        return 1;
    }

    char line[MAX_LINE_LENGTH];
    int line_number = 0;

    while (fgets(line, sizeof(line), file))
    {
        line_number++;
        // Remove newline character if present -- It is related to printf() function to prevent printing empty line
        line[strcspn(line, "\n")] = '\0';

        // Check if the line contains the search string
        if (strstr(line, search_string)) {
            printf("Line %d: %s\n", line_number, line);
        }
    }

    fclose(file);
    return 0;
}
```

