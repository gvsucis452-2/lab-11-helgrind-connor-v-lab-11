# Lab 10 - Connor VanDrunen

Q1 - The output points to a race condition occurring due to the read and write actions taken on lines 8 and 15 of `main-race.c`, which are the exact lines where the race condition occurs. Specifically, it mentions how the main program's read instruction could conflict with the thread's write instruction, and how the thread's write instruction could conflict with the main program's write instruction. Some of the additional information it includes is the line where the second thread was created and a trace of the underlying code that caused the issue in that thread. 

Q2 - If I comment out the offending lines of code in the main program, Helgrind reports no errors. The same is true if I comment out the offending code in the thread.  

Q3 - By placing a lock around only one of the offending lines of code, Helgrind's output two errors in much the same format as the first output, with the main difference being that in the second output, Helgrind made note that the lock existed and documented where it was in the code. 

Q4 - By placing locks around both offending lines of code, Helgrind reported 0 errors. 

Q5 - While using this code, I found it was much easier to spot the deadlocking behavior by placing a `sleep(1)` command between the two lock statements. When I did so, the program would run indefinitely. The reason for this was that one of the threads would lock the first lock, then yield, allowing the second thread to lock the second lock before it yielded. Following this, the thread that ran first would wake up, attempt to lock the other lock, find it locked, and begin waiting for it to be unlocked. Next, the thread that ran second would wake up, attempt to lock the other lock, notice that it was also locked, and begin waiting for it to be unlocked. This led me to conclude that a deadlock is a situation in which threads become stuck waiting on each other to release a lock before releasing their own lock.  
  
Q6 - Helgrind reported 1 error and specified that the order in which the locks were acquired must be consistent to prevent this behavior.

Q7 - This program does not have the same error as the previous program, because when one thread acquires and releases locks, the other thread must wait on lock `g`. This prevents the behavior seen in the previous version, as only one thread can acquire and release locks at a time. Despite the problem being fixed, Helgrind still reports the error. This tells me that tools like Helgrind are not perfect, as it can identify errors that don't exist, but still valuable to identify bad behavior and poorly written code (as even though the issue is 'solved', it would be much safer to aquire the locks in a consistent sequence like Helgrind suggested, as then we wouldn't need lock g and would be protected in the case where another programmer removed lock g without considering the potential consequences of such an action).  

Q8 - This program has essentially implemented its own simple spin-lock, which wastes CPU resources by spinning in the while loop while waiting for the child to finish. If the child takes a long time to complete, the main program will waste significant CPU resources on spinning every time it's given the CPU. 

Q9 - Running Helgrind on this program, it reports 1 error: a race condition between our print statements. This is incorrect, as the program's spin-lock behavior will prevent the aforementioned race condition.

Q10 - Whilst both `main-signal.c` and `main-signal-cv.c` will both ensure the order of the print statements (and are thereby correct), `main-signal-cv.c` is preffered as it has much better performance for long-running workers than `main-signal.c`, as instead of wasting CPU resources on an empty loop, the main program enters a waiting state, where it waits for the required lock to be released before doing anything. 

Q11 - Helgrind reports 0 errors. 
