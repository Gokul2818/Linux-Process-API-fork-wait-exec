# Linux-Process-API-fork-wait-exec-
Ex02-Linux Process API-fork(), wait(), exec()
# Ex02-OS-Linux-Process API - fork(), wait(), exec()
Operating systems Lab exercise


# AIM:
To write C Program that uses Linux Process API - fork(), wait(), exec()

# DESIGN STEPS:

### Step 1:

Navigate to any Linux environment installed on the system or installed inside a virtual environment like virtual box/vmware or online linux JSLinux (https://bellard.org/jslinux/vm.html?url=alpine-x86.cfg&mem=192) or docker.

### Step 2:

Write the C Program using Linux Process API - fork(), wait(), exec()

### Step 3:

Test the C Program for the desired output. 

# PROGRAM:

## C Program to create new process using Linux API system calls fork() and getpid() , getppid() and to print process ID and parent Process ID using Linux API system calls
```
#include <stdio.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/wait.h>  // <-- This fixes the warning
#include <stdlib.h>

int main() {
    pid_t pid = fork();

    if (pid == 0) {
        // Child
        printf("I am child, my PID is %d\n", getpid());
        printf("My parent PID is: %d\n", getppid());
        sleep(10);  // Pause so we can see the process
    } else if (pid > 0) {
        // Parent
        printf("I am parent, my PID is %d\n", getpid());
        wait(NULL);  // Wait for child to finish
        sleep(10);
    } else {
        perror("fork failed");
        exit(1);
    }

    return 0;
}
```

##OUTPUT

![OS OUTPUT 1](https://github.com/user-attachments/assets/bb749cfa-6b32-4c1a-8653-a6f1231b0ea7)

## C Program to execute Linux system commands using Linux API system calls exec() , exit() , wait() family
```
#include <stdio.h>
#include <stdlib.h>
#include <sys/types.h>
#include <sys/wait.h>
#include <unistd.h>

int main() {
    int status;
    
    printf("Running ps with execl\n");
    if (fork() == 0) {
        execl("ps", "ps", "-f", NULL);
        perror("execl failed");
        exit(1);
    }
    wait(&status);
    
    if (WIFEXITED(status)) {
        printf("Child exited with status: %d\n", WEXITSTATUS(status));
    } else {
        printf("Child did not exit successfully\n");
    }
    
    printf("Running ps with execlp (without full path)\n");
    if (fork() == 0) {
        execlp("ps", "ps", "-f", NULL);
        perror("execlp failed");
        exit(1);
    }
    wait(&status);
    
    if (WIFEXITED(status)) {
        printf("Child exited for execlp with status: %d\n", WEXITSTATUS(status));
    } else {
        printf("Child did not exit successfully\n");
    }
    
    printf("Done.\n");
    return 0;
}
```

##OUTPUT

![OS OUTPUT 2](https://github.com/user-attachments/assets/1bfa5e63-f96d-4a37-89bf-8a746513ea3b)

# RESULT:
The programs are executed successfully.
