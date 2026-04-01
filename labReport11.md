# Lab 10 - Connor VanDrunen

Q1 - The output points to a race condition occuring due to the read and write actions taken on lines 8 and 15 of `main-race.c`, which are the exact lines that the race condition occurs. Specifically, it mentions how the main program's read instruction could conflict with the thread's write instruction, and how the thread's write instruction could conflic with the main program's write instruction. Some of the additional information it includes, is the line where the second thread was created and some of the underlying code causing an issue in the thread itself. 

Q2 - If I comment out the offending lines of code in the main program, Helgrind reports no errors. The same is true if I comment out the offending code in the thread.  

Q3 - By placing a lock around only one of the offending lines of code, Helgrind's output two errors in much the same format as the first output, with the main difference being that in the second output Helgrind made note that the lock existed and documented where it was in the code. 

Q4 - 
