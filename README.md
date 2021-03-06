# User-Level-Thread-Library
A User-Level Thread Library

Ubuntu 14.04 Linux 3.19.0-28-generic  X86_64

The preemptive scheduler is implemented through time and signal functions. 
In gtthread_init(long period) function, a timer is initiated through "setitimer(ITIMER_PROF, &timer, NULL)".
When the timer expires, signal SIGPROF is sent to the process.
sigaction() is used to invoke the scheduling procedure "void scheduler(int signum)" to run, which choose a thread from a run queue to run.
The scheduler maintains a run queue which is implemented as a FIFO queue using "steque.c".
The new created threads are put into the end of the queue.
The first thread in the head of the queue is the thread currently running. 
Each time the scheduler receives signal SIGPRO, it interrupts the running thread at the head by putting this thread into the end of the queue, 
swap the context between this thread and next thread which is the new head of the queue.

Two queues are used for the scheduler. One queue is "readyqueue" which stores the threads ready to run.
Another queue is "terminatequeue", which stores all the threads have been terminated.


To prevent deadlocks, I use the resource hierachy solution. The chopsticks are number from 0 to 4 and each philosopher always first pick up the one that has the lower number among his left and right chopsticks, and then pick up the higher one. In this way, circular wait cannot be formed. Suppose that if there is a circular wait,
each philosopher must hold the lower numbered chopstick and wait for the higher numbered one. But for the number 4 chopstick, no larger numbered chopstick need to be waited on. For 4th chopstick, philosopher 4 waits on it, means that philosopher 0 holds it. We have contradiction. Thus, this method prevents the deadlocks by breaking circular wait through ordered lock.
 

For the resource hierachy solution to prevent deadlocks, the experiment results actually show unbalanced eaten meals among philosophers. The philosoper 4 has the most number of meals. The reason can be caused by number 0 chopstick, for which both philosopher 0 and 1 are contended with each other on this chopstick. Similarly, the highest numbered chopstick 4 has the least contention.
