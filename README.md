## xv6 Scheduling

### GWU CSCI 3411 - Fall 2021 - Lab 8

---

## Topics

1.  Introduction to Scheduling

2.  Scheduling in xv6

3.  Exercises

    1. Yield System Call

    2. High-Priority Scheduling

# 1. Introduction to Scheduling

This week we study xv6 scheduling. We will add system calls to xv6 much like we have done in earlier xv6 labs and assignments

Remember, to add a system call to the kernel, you will need to alter several files and add an implementation to your new system call in the kernel. You will also need to create a user space program to test and use any new system calls which will require adding a `C` file and updating the `Makefile` to build your program.

This section is presented by your instructor on the slides. The slides will be made available following the last lab section.

# 2. Scheduling in xv6

This section is presented by your instructor on the slides. The slides will be made available following the last lab section.

# 3. Exercises

These exercises are designed to help understand the XV6 scheduler. Remember that the xv6 scheduler is vastly different from schedulers in most modern OSes. Keep those differences in mind as you move through the exercises.

## 3.1 Exercise 1 - Yield System Call

Add a `yield` system call. `yield` already has a kernel space implementation in `proc.c`; however, `yield` does not currently have a user space interface. To make `yield` available to user space, add a `sys_yield` system call in `sysproc.c` and call `yield` from `sys_yield`. Once you have added `sys_yield`, add the necessary declarations and definitions to expose the system call to user space.

To test `yield`, add a user space program named `twoproc.c`. `twoproc` should fork a single child process. After the fork, both parent and child processes enter a loop that iterates fifty times. Inside the child loop, print a plus sign (`+`) and then call `yield` each iteration. Inside the parent loop, print a minus sign (`-`) and then call `yield` each iteration. 

Compile xv6 and run `twoproc`. Verify that your `twoproc` produces the expected output.

### Questions

0.  **BEFORE** writing any code, what do we expect the output of `twoproc` to be?

1.  How does `yield` result in the current process being rescheduled?

## 3.2 Exercise 2 - Simple High Priority Scheduling Policy

On a standard Linux distribution, a user can elevate the priority of a particular process. The xv6 scheduler does not support prioritization. In this exercise, we will incorporate a level of high priority into the xv6 scheduler.

Add a system call named `sethipriority`. This system call elevates the calling process to high priority through the kernel. Only one process will be elevated to high priority at a time, so we will maintain the identity of that process using a variable that stores a pointer to the current high priority process.

Adjust the scheduler such that if a high priority process is `RUNNABLE` then the system executes it above all other processes. If the high priority process is not `RUNNABLE`, then the scheduler runs the next process according to the default scheduling policy.

Copy `twoproc.c` to `hiproc.c`, as `twoproc` provides a good testing model. In `hiproc.c`, raise the priority of the child process after forking and before iterating. Compile `hiproc` and test your implementation.

### Questions

0.  **BEFORE** writing any code, what do you think the behavior will be?

1.  How does the behavior of `twoproc` differ from `hiproc`?

2.  If you raise the priority of the parent process in `hiproc` before forking instead of in the child process after forking, how does the behavior change?

# 3. Discussion

While `sethipriority` facilitates some level of prioritization in xv6, this model does not allow more than one process to have increased priority. Thus, we only have two priority levels which has limited utility. Consider the priority queue assignment from earlier in the semester where we ordered data with varying priorities based on that priority and the order in which data was placed into the queue.

Imagine you are adding priority queue based scheduling to xv6. Consider how you would address the following questions to support priority scheduling:

1.  What data structure(s) would you add?

2.  What variables would you add?

3.  How would you modify the XV6 process definition?

4.  How would you modify the XV6 scheduler?

5.  How would you support both the existing scheduling policy and a priority scheduling policy?