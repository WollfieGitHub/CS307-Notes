### while(!condition)
- If progress cannot be made because a resource cannot be acquired, but it's desirable to free up exec. resources for another thread.
**Preferable if** 
- Scheduling overhead is larger than expected wait time
	- Scheduling overhead is in the order of microseconds
	- While a few spinning iterations is nanoseconds
- Processor's resources not needed for other tasks

### block_until_true();
- The os deschedules the thread 
	- Preempt the running thread (suspend)
	- OS schedules another thread to run
	- The other thread can run and make progress
**Preferable if** 
- Waiting time is significant & There are other tasks that neeed processor's resources
