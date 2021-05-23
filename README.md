**PintOS: Project 1 - Threads**

**Problems: Provided in cs140, Project 1**

	1. Alarm clock
	2. Priority scheduling
	3. Advanced scheduler
My additional modifications to the files only focus on the **3rd problem, implementing the Advanced scheduler,** as listed in cs140 course.

**Changelog:**

These following files have been modified within the scope of this Project:

	- *./src/threads/thread.c*
	- *./src/threads/thread.h*
	- *./src/threads/synch.c*
	- Added 1 additional file: *./src/threads/fixed_point.h*

In order for this Project to run properly, these below files have been modified to meet the Project's requirements.
The details on why the changes are added got explained precisely using comments within corresponding files.
Each changes are enclosed within the structure:

	//Edit: Advanced scheduler
		*Additional changes*
	//End of edit

**Details:**

**_./src/threads/fixed_point.h:_**
This header file was added to define some functions on converting floating-point real number to fixed-point real number, and calculations on those numbers.

>From B.6 Fixed-Point Real Arithmetic:
>In the formulas, priority, nice and ready_threads are integers, but recent_cpu and load_avg are real numbers. Unfortunately, PintOS does not support floating-point arithmetic in the kernel, because it would complicate and slow the kernel... This means that calculations on real quantities must be simulated using integers.

	Added functions:
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
>**Advanced Scheduler FAQ:** _How does priority donation interact with the advanced scheduler?_ It doesn't have to. We won't test priority donation and the advanced scheduler at the same time.

	Modifications: Line 236, 243 & 292. 

**_./src/threads/thread.h:_**
This header file was modified to add new variable/function declarations needed for **thread_mlfqs** to work properly. The functions are implemented in *thread.c* file.

	Modifications:

	Variables: Edits in thread's structure, adding line 116, 117
	1. int nice
	2. fixed_t recent_cpu

	Functions: Definition of function prototypes, adding line 182 to 185
	1. void thread_mlfqs_incr_recent_cpu (void)
	2. void thread_mlfqs_calc_recent_cpu (void)
	3. void thread_mlfqs_update_priority (struct thread *)
	4. void thread_mlfqs_refresh (void)

**_./src/threads/thread.c:_**
This file was modified for the implementation of needed functions for **thread_mlfqs** to work properly.

	Modifications:
	1. Add line 16, include the *fixed_point.h* file for value manipulation
	2. Add line 68, declare the fixed_t load_avg variable, holding the CPU average load at one time.
	3. Add line 124, initiate the load_avg value at thread start
	4. Modify line 476 to 479, if running mlfqs, return control to main program to update priority first
	5. Modify line 511 to 515, set the nice value to current thread & update priority
	6. Modify line 524: return nice value
	7. Modify line 533: return load_avg value
	8. Modify line 542: return recent_cpu value
	9. Added line 546 to 630, implement some needed functions:
		- thread_mlfqs_incr_recent_cpu (void): Check current thread is idle or not. If not, increase current thread's recent_cpu by 1
		- thread_mlfqs_calc_recent_cpu (struct thread *t): Calculate thread's recent CPU. Formula is defined in B.3 Calculating recent_cpu
		- thread_mlfqs_update_priority (struct thread *t): Check if thread is idle or not. If not, update thread's priority. Formula is defined in B.2 Calculating priority
		- thread_mlfqs_refresh (void): Invoked once per second to refresh load_avg and recent_cpu of all threads
	
*end of file*
