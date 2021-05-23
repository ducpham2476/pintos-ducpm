**PintOS: Project 1 - Threads**

**Problems: Provided in cs140, Project 1**
1. Alarm clock
2. Priority scheduling
3. Advanced scheduler
My additional modifications to the files only focus on the **3rd problem, implementing the Advanced scheduler,** as listed in cs140 course.

**Changelog:**

These following files have been modified within the scope of this Project:
*./src/threads/thread.c*
*./src/threads/thread.h*
*./src/threads/synch.c*
*./src/threads/synch.h*
Added 1 additional file: *./src/threads/fixed_point.h*

In order for this Project to run properly, these below files have been modified to meet the Project's requirements.
The details on why the changes are added got explained precisely using comments within corresponding files.
Each changes are enclosed within the structure:

	//Edit: Advanced scheduler
		*Additional changes*
	//End of edit

**Details:**

**_./src/threads/fixed_point.h:_**
This header file was added to define some functions on converting floating-point real number to fixed-point real number, and calculations on those numbers.

**From B.6 Fixed-Point Real Arithmetic:**
>In the formulas, priority, nice and ready_threads are integers, but recent_cpu and load_avg are real numbers. Unfortunately, PintOS does not support floating-point arithmetic in the kernel, because it would complicate and slow the kernel... This means that calculations on real quantities must be simulated using integers.


Hence, the implementation of these functions is necessary.

Defined functions:
1. FP_CONST(val): Convert a value to fixed-point value
2. FP_ADD(val1, val2): Add two fixed-point values
3. FP_ADD_MIX(val1, int val2): Add a fixed-point value with an int value
4. FP_SUB(val1, val2): Subtract two fixed-point values
5. FP_SUB_MIX(val1, int val2): Subtract an int value from a fixed-point value
6. FP_MUL(val1, val2): Multiply two fixed-point values
7. FP_MUL_MIX(val1, int val2): Multiply a fixed-point value with an int value
8. FP_DIV(val1, val2): Divide a fixed-point value from an another fixed-point value
9. FP_DIV_MIX(val1, int val2): Divide an int value from the fixed-point value
10. FP_INT_PART(val): Get the integer part of a fixed-point value
11. FP_ROUND(val): Get the rounded integer value of a fixed-point value

**_./src/threads/synch.c:_**
Additional changes to pre-existing functions to disable **thread_mlfqs** (Multilevel Feedback Queue) scheduling, prevent thread's priority donating interfere with mlfqs
>Advanced Scheduler FAQ:
- How does priority donation interact with the advanced scheduler?
- It doesn't have to. We won't test priority donation and the advanced scheduler at the same time.

Modifications: Line 236, 243 & 292. 
Line 224: 
