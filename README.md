# Readers-Writers-Starvefree

The Reader-Writer Problem is a classical problem in Computer Science in which a data structure like database, storage area, etc is being accessed simultaneously by multiple processes concurrently. The critcal section is such that it can be accessed by only one one writer at any point of time while multiple readers can simultaneously access the critical section. We use semaphores to achieve this such that there would be no conflicts while accessing by writers and readers and process synchronization is properly met. But, this may give rise to starvation of either the readers or the writers, depending on their priorities. I have described a solution to the Reader-Writer problem that is starve-free and tackles the problem efficiently.

Firstly, I have decribed the classical solution followed by the starve-free solution to the same. Finally, I have described an even faster starve-free solution, that speeds up the reader process' execution in the critical section.


## Data Structures Used : 

Semaphores are used to solve the problem of process synchronization. The semaphore is linked to a critical section and contains a Queue (FIFO structure) that stores the list of processes that are blocked and waiting to acquire the semaphore. While entering the `blockedQueue` the process is blocked and once a process signals a semaphore, the process in the front of the `blockedQueue` gets activated. The following code describes the implementation of the same.

```cpp
initialize:
wrt = 1,mutex = 1,rd = 1,readcount = 0;

Reader process:

wait(mutex);   //this mutex is saem for readers as well as for writers
wait(rd);   // this is for maintaining mutual exclusion b/w readers for increasing readcount atomically
readcount++;
if(readcount==1) wait(wrt);
signal(rd);
signal(mutex);


//critical section//


wait(rd);        //for maintaining mutual exclusion b/w readers for decreasing readcount atomically
readcount--;
signal(rd);
if(readcount==0) signal(wrt);


Writer Process:

wait(mutex);
wait(wrt);   //for maintaining mutual exclusion b/w writers

// crtical section//

signal(mutex); //whenever it releases the mutex any one can aquire based on which came first either reader or writer
signal(wrt);                   
                   
```
