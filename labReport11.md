# Lab 10 - Connor VanDrunen

Q1 - The output points to a race condition occuring due to the read and write actions taken on lines 8 and 15 of `main-race.c`, which are the exact lines that the race condition occurs. Specifically, it mentions how the main program's read instruction could conflict with the thread's write instruction, and how the thread's write instruction could conflic with the main program's write instruction. Some of the additional information it includes, is the line where the second thread was created and some of the underlying code causing an issue in the thread itself. 

Q2 - If I comment out the offending lines of code in the main program, Helgrind reports no errors. The same is true if I comment out the offending code in the thread.  

Q3 - By placing a lock around only one of the offending lines of code, Helgrind's output two errors in much the same format as the first output, with the main difference being that in the second output Helgrind made note that the lock existed and documented where it was in the code. 

Q4 - By placing locks around both offending lines of code, Helgrind reported 0 errors. 

Q5 - While using this code, I found it was much easier to spot the deadlocking behavior by placing a `sleep(1)` command between the two lock statements. When I did so, the program would run indefinetly. The reason for this was that one of the threads would lock the first lock, then yeild allowing the second thread to lock the second lock before it yeilded. Following this, the thread that ran first would wake up, attempt to lock the other lock, find that it was locked, and begin wating for it to be unlocked. Next, the thread that ran second would wake up, attempt to lock the other lock, notice that it was also locked and begin waiting for it to be unlocked. This led me to conclude that a deadlock is a situation in which two locks become stuck waiting on each other to unlock before unlocking.  
  
Q6 - Helgrind reported 1 error and specified that the order the locks were aquired must be consistent to prevent this behavior.

Q7 - This program does not have the same error as the previous program, as when one thread is aquiring and releasing locks, the other thread must wait on lock `g`. This prevents the behavior seen in the previous version, as only one thread is able to aquire and release locks at a time. Despite the problem being fixed, Helgrind still reports the error. This tells me that tools like Helgrind are not perfect, as it can identify errors that don't exist, but still valuable to identify bad behavior and poorly written code (as even though the issue is 'solved', it would be much safer to aquire the locks in a consistent sequence like Helgrind suggested, as then we wouldn't need lock g and would be protected in the case where another programmer removed lock g without considering the potential consequences of such an action). 
