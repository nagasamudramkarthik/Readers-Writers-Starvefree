# Readers-Writers-Starvefree

The Reader-Writer Problem is a well-known challenge in Computer Science where multiple processes try to  access a shared memory concurrently . So to prevent this conflicts,the critical section can only be accessed by one writer at a time, while multiple readers can access it simultaneously.so there should be a proper synchronization should be maintained.synchronization is done such a way there shouldnt be any starvation either for readers or writers.this can be achievied by using three semaphores one variable `readcount` this is for counting no. of readers.the semaphore `wrt` is needed for the writer to access the critical section.the semaphore `rd` is needed for maintaining mutual exclusion among the readers so they increase or decrease the readcount atomically and the `mutex` semaphore is common for both readers as well as writers.whoever(reader or writer) acquire this `mutex` semaphore will start their own process execution and whenevr if reader reaches  critcal section he will releases the `mutex` semaphore but reader locks the `wrt` mutex.so if incase one more reader comes after first reader then he will enter the critical section as both `mutex` and `rd` semaphores are 1 for him.its possible that multiple readers can be in a critical section.but if writer comes in between readers then after the first reader releases the  `mutex`.the writer will acquire it and reader will releases the `wrt` whenever all the readers who came before the writer finishes their jobs in critical section so this leads to starve free solution and similarly for writers until writer exits critical section he wont releases the `mutex` semaphore.


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
----------------------

//critical section//

----------------------
wait(rd);        //for maintaining mutual exclusion b/w readers for decreasing readcount atomically
readcount--;
signal(rd);
if(readcount==0) signal(wrt);


Writer Process:

wait(mutex);
wait(wrt);   //for maintaining mutual exclusion b/w writers
------------------------
// crtical section//
------------------------
signal(mutex); //whenever it releases the mutex any one can aquire based on which came first either reader or writer
signal(wrt);                   
                   
```
