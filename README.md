# Pintos - Priority Scheduling

## How to running pintos on your machine?

$>need pintos (optional)

$>cd pintos/src/threads

$>make

$>cd build

$>pintos

### Make functions:
$> make check (it will test your solution against a set of test programs. Make sure you 'make clean' between runs.)

$> make grade

## Files changed:
### /src/threads/thread.c
	- add define value
	- change thread_create
	- change thread_unblock
	- change thread_yield
	- change thread_set_priority
	- change thread_get_priority
	- change init_thread
	- add test_yield
	- add update_priority
	- add donate_priority
	- add lock_remove
	- add change_priority

### /src/threads/thread.h
	- change struct thread
	- add pre-declaration
		- add test_yield
		- add update_priority
		- add donate_priority
		- add lock_remove
		- add change_priority
	
### /src/threads/synch.c
	- change sema_down
	- change sema_up
	- change lock_acquire
	- change lock_try_acquire
	- change lock_release
	- change cond_wait
	- change cond_signal
	- add change_sem_priority
	
### /src/threads/synch.h
	- add pre-declaration
		- add change_sem_priority

## Test results:

pass tests/threads/alarm-priority

pass tests/threads/priority-change

pass tests/threads/priority-donate-one

pass tests/threads/priority-donate-multiple

pass tests/threads/priority-donate-multiple2

FAIL tests/threads/priority-donate-nest

pass tests/threads/priority-donate-sema

pass tests/threads/priority-donate-lower

pass tests/threads/priority-fifo

pass tests/threads/priority-preempt

pass tests/threads/priority-sema

pass tests/threads/priority-condvar

FAIL tests/threads/priority-donate-chain


2 of 13 tests failed.

## New code in file with following format:

/* My Code Begins */

CODES HERE

/* My Code Ends */

## Algorithms:

When one thread with higher priority adding to the ready queue, the current running thread should give it to the CPU, which means it should donate CPU to the thread has higher priority.
Suppose there has three threads H/M/L from high priority to low priority. If thread H is waiting for the lock, thread M is in the ready queue, so that thread H cannot get the CPU (or cannot running first) and continue waiting. Because the low priority thread L cannot get the CPU, so the thread M running first. So the problem is the thread H priority is higher than thread M, however, the thread M running first. It's called priority inversion problem.
It use next_thread_to_run to control the priority scheduling. 
Every time calling thread with highest priority to running first. Every time calling schedule, it need to check the thread priority. In this case, it should call test_yield(). Use lock_list to store every thread is waiting on lock. When a thread require lock, it will doing test_yield first to check whether it give first priority or not.

## Synchronization:

The data structure protected by the control the operation that are separately specified and implemented, in other words, it define the data structure no in “public”.
To avoid race conditions, it should disable the interrupt before add to waiting list. After that, re-enable the interrupt after the list is updated.
