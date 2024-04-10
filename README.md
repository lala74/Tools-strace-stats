strace is an invaluable tool for figuring out what a Linux process is doing.
With its options for logging how much time is spent in each system call, it's 
also an easy way to profile i/o-bound programs and components in distributed 
systems.  However, sometimes it can be a challenge getting the big picture from 
all the data, without losing details (such as you do with `strace -c`).  That's 
what stracestats is for -- it aggregates and reports statistics about the 
system calls in the output of an strace run.

Specifically, for each system call that appears it'll report:

* occurrences (number of times called, percentage of overall calls made)
* time spent in the call (total time, percentage of system call time, 
  percentage of wall time)
* average call time and standard deviation
* median call time
* a 10-bin histogram of call times, from min to max

It can report separate statistics for each call with a different first argument 
(helpful for all those calls that take a file/socket descriptor), and it can 
make plots of the distribution of time spent in each call.

stracestats is written in python and requires only numpy and, if plotting is 
desired, matplotlib.

For more information and examples, please see:

    http://jabrcx.github.com/stracestats


Copyright (c) 2012-2013, John A. Brunelle
All rights reserved.
