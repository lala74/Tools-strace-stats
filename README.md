## Description
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

    https://jabrcx.github.io/stracestats/

Copyright (c) 2012-2013, John A. Brunelle

All rights reserved.

## How to use
### Capture strace
Strace must be captured with (-t or -tt or -ttt) and -T format
```
# Ex
$ strace -tt -T -o /data/strace.out -f -p 1514
```
File example
```
$ cat /data/strace.out
5032  08:23:41.299622 getcpu([1], NULL, NULL) = 0 <0.000105>
```
### Running analyze
```
# Create virtual env
python3 -m venv venv
source venv/bin/activate
# Install package
pip3 install -r requirements.txt

# Run analyze
./bin/stracestats -f strace.out
```
Result should be
```
$ ./bin/stracestats -f strace.out
getcpu
	    num calls:       40  17% of syscalls
	     tot time: 0.006079   0% of syscall time,  0% of wall time
	avg call time: 0.000152
	          +/-: 0.000317
	med call time: 0.000072

	 min/hist/max: 0.000031 [37  1  0  0  0  0  1  0  0  1] 0.001798

recvmsg
	    num calls:       15   6% of syscalls
	     tot time: 0.006767   0% of syscall time,  0% of wall time
	avg call time: 0.000451
	          +/-: 0.000637
	med call time: 0.000101

	 min/hist/max: 0.000035 [11  0  0  0  0  1  0  0  0  3] 0.001686

clock_nanosleep
	    num calls:        3   1% of syscalls
	     tot time: 0.012371   0% of syscall time,  0% of wall time
	avg call time: 0.004124
	          +/-: 0.004243
	med call time: 0.001139

	 min/hist/max: 0.001108 [2 0 0 0 0 0 0 0 0 1] 0.010124

futex
	    num calls:      107  45% of syscalls
	     tot time: 3.105238  99% of syscall time, 69% of wall time
	avg call time: 0.029021
	          +/-: 0.057666
	med call time: 0.000256

	 min/hist/max: 0.000042 [78 18  2  5  1  1  1  0  0  1] 0.373245
```
