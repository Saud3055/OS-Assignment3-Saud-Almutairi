# Assignment 3 - Complete Documentation

**Student Name**: SAUD ALHUMAIDI ALMUTAIRI 
**Student ID**: 445050108 
**Date Submitted**: 3 May 2026 9:30 AM

---

## 🎥 VIDEO DEMONSTRATION LINK (REQUIRED)

> **⚠️ IMPORTANT: This section is REQUIRED for grading!**
> 
> Upload your 3-5 minute video to your **PERSONAL Gmail Google Drive** (NOT university email).
> Set sharing to "Anyone with the link can view".
> Test the link in incognito/private mode before submitting.

**Video Link**: [Paste your personal Gmail Google Drive link here]

**Video filename**: `[YourStudentID]_Assignment3_Synchronization.mp4`

**Verification**:
- [ ] Link is accessible (tested in incognito mode)
- [ ] Video is 3-5 minutes long
- [ ] Video shows code walkthrough and commits
- [ ] Video has clear audio
- [ ] Uploaded to PERSONAL Gmail (not @std.psau.edu.sa)

---

## Part 1: Development Log (1 mark)

Document your development process with **minimum 3 entries** showing progression:

### Entry 1 – 2 May 2026 9:00 PM
**What I implemented**: 
examined the codebase structure for Assignment 1.
Critical portions were identified.
Imports from synchronization libraries were added 
**Challenges encountered**: 
Recognizing which shared resources needed to be protected
Differentiating between common independent and dependent variables
**How I solved it**: 
examined every global shared variable.
Critical sections include the execution log and classified counters.
**Testing approach**: 
Code compilation was confirmed following import additions.
carefully examined thread interactions
**Time spent**: 
30 minutes
---

### Entry 2 – 2 May 2026 9:30 PM
**What I implemented**: 
ReentrantLock mechanisms were added.
Defended:
contextSwitchCount totalWaitingTime executionLog completedProcessCount
**Challenges encountered**: 
Selecting between fine-grained and coarse-grained locking
Avoiding race situations without drastically lowering concurrency
**How I solved it**: 
put in place distinct locks for every independent shared resource.
Try-finally blocks were used to release locks safely.
**Testing approach**: 
conducted several compilation tests.
Confirmed that counters were updated correctly
**Time spent**: 
40 minutes
---

### Entry 3 – 2 May 2026 10:15 PM
**What I implemented**: 
CPU semaphore addition
**Challenges encountered**: 
Managing the InterruptedException
Making sure the semaphore is released in every situation
**How I solved it**: 
outer try-catch-finally added
used the final block to ensure the release of the semaphore
**Testing approach**: 
carried out the simulation
confirmed the order of CPU execution
No deadlocks were confirmed.
**Time spent**: 
1 hour
---

### Entry 4 – 2 May 2026 11:15 PM
**What I implemented**: 
Enhanced execution logging and increased synchronization with runToCompletion()
Generation of validated statistics
**Challenges encountered**: 
Keeping all execution paths consistent
Maintaining synchronization in the execution of the final process
**How I solved it**: 
consistently used the same synchronization model
Confirmed the integrity of shared data
**Testing approach**: 
Complete runs of the simulation
Verification of output
Validation through statistics
**Time spent**: 
40 minutes
---

### Entry 5 – 2 May 2026 11:55 PM
**What I implemented**: 
Last debugging
Records
Commit organization on GitHub
Planning for video preparation
**Challenges encountered**: 
Verifying that all assignment conditions were met
Making scholarly justifications
**How I solved it**: 
Assignment rubric cross-checked
reviewed the ideas in the textbook
verified that every TODO was completed.
**Testing approach**: 
completed several final executions.
confirmed consistent, synchronized conduct
**Time spent**: 
45 minutes
---

## Part 2: Technical Questions (1 mark)

### Question 1: Race Conditions
**Q**: Identify and explain TWO race conditions in the original code. For each:
- What shared resource is affected?
- Why is concurrent access a problem?
- What incorrect behavior could occur?

**Your Answer**:
ContextSwitchCount is impacted by the first race condition, which allows multiple threads to run contextSwitchCount++ at the same time. Incorrect context switch totals could result from some changes being lost because incrementing is not atomic.

ExecutionLog, which makes use of the non-thread-safe ArrayList object, is impacted by the second race situation. Concurrent add() calls can result in missing or inconsistent execution records, corrupt data, or a ConcurrentModificationException.
---

### Question 2: Locks vs Semaphores
**Q**: Explain the difference between ReentrantLock and Semaphore. Where did you use each in your code and why?

**Your Answer**:

nly one thread can access a shared resource at a time thanks to ReentrantLock's mutual exclusion feature. I used it to safeguard the execution log and shared counters.

Permits are used by Semaphore to regulate access. I ensured that only one process runs at a time by simulating a single CPU using Semaphore(1). Semaphores regulate resource access, whereas locks safeguard data consistency.

---

### Question 3: Deadlock Prevention
**Q**: What is deadlock? Explain TWO prevention techniques and what you did to prevent deadlocks in your code.

**Your Answer**:

When threads wait endlessly for resources owned by one another, deadlock occurs.

By employing try-finally blocks to ensure lock and semaphore release even in the event of errors, I was able to avoid deadlocks. In order to lower the danger of deadlock, I additionally employed straightforward lock structures devoid of circular dependencies.

---

### Question 4: Lock Granularity Design Decision 
**Q**: For Task 1 (protecting the three counters), explain your lock design choice:
- Did you use ONE lock for all three counters (coarse-grained) OR separate locks for each counter (fine-grained)?
- Explain WHY you made this choice
- What are the trade-offs between the two approaches?
- Given that the three counters are independent, which approach provides better concurrency and why?

**Your Answer**:

With distinct locks for every shared counter, I employed fine-grained locking. Because independent counters can be updated concurrently, concurrency is improved.

Because all counter updates would have to wait on a single lock, coarse-grained locking would be less efficient even if it would be simpler.

Fine-grained locking improves efficiency and scalability because the counters are independent.

---

## Part 3: Synchronization Analysis (1 mark)

### Critical Section #1: Counter Variables

**Which variables**: 
contextSwitchCount, completedProcessCount, totalWaitingTime
**Why they need protection**: 
Multiple threads can access these shared counters. Race circumstances could result in lost updates and inaccurate statistics if there is no synchronization.
**Synchronization mechanism used**: 
ReentrantLock
**Code snippet**:
```java
 counterLock.lock();
        try {
            contextSwitchCount++;
        } finally {
            counterLock.unlock();
        }
```

**Justification**: 
Locks preserve precise scheduling statistics and guarantee atomic updates.
---

### Critical Section #2: Execution Log

**What resource**: 
executionLog (ArrayList<String>)
**Why it needs protection**: 
Because ArrayList is not thread-safe, concurrent changes could result in errors or damaged data.
**Synchronization mechanism used**: 
ReentrantLock
**Code snippet**:
```java
logLock.lock();
        try {
            executionLog.add(message);
        } finally {
            logLock.unlock();
        }
```

**Justification**: 
Preventing runtime problems and ensuring consistent execution history are two benefits of protecting log access.
---

### Critical Section #3: CPU Semaphore

**Purpose of semaphore**: 
to limit CPU access and guarantee that only one task runs concurrently.
**Number of permits and why**: 
One license to mimic a CPU with a single core.
**Where implemented**: 
both prior to and following process execution inside run().
**Code snippet**:
```java
cpuSemaphore.acquire();
try {
    // Process execution
} finally {
    cpuSemaphore.release();
}
```

**Effect on program behavior**: 

---

## Part 4: Testing and Verification (2 marks)

### Test 1: Consistency Check
**What I tested**: Running program multiple times to verify consistent results
To guarantee steady synchronization behavior and consistent output, run the scheduler several times
**Testing procedure**: 
```bash
javac SchedulerSimulationSync.java
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync
```

**Results**: 
Every execution resulted in consistent behavior with regard to:

Every procedure is successfully finished.
No deadlocks or crashes
Proper context-switching actions
Stable statistics and logs of execution

The end outcomes (finished processes, counters, and waiting time) remained constant even if thread scheduling may cause a little variation in execution order.

**Why synchronization is necessary**: 
Shared variables like contextSwitchCount, totalWaitingTime, and executionLog may experience race situations in the absence of synchronization. Inaccurate statistics, lost updates, and damaged logs would result from this. Atomic updates and a uniform system state throughout all threads are guaranteed by synchronization.

**Conclusion**: 
The system generates dependable scheduling results under numerous executions and is thread-safe.
---

### Test 2: Exception Testing
**What I tested**: Checking for ConcurrentModificationException
determining whether runtime exceptions like ConcurrentModificationException are caused by concurrent access to shared resources.
**Testing procedure**: 
ran several processes at once.
shared counter adjustments and observed updates to the execution log
checked for exceptions in the terminal output.
**Results**: 
There were no runtime crashes or ConcurrentModificationExceptions. Throughout execution, the execution log and counters stayed the same.
**What this proves**: 
Shared data structures are successfully shielded from concurrent modification problems by appropriate locking mechanisms (ReentrantLock).
---

### Test 3: Correctness Verification
**What I tested**: Verifying correct final values (total burst time, context switches, etc.)
Confirmation of the final statistics calculated:

Switching contexts
Finalized procedures
Calculating waiting times
**Expected values**: 
Completed Processes = Total number of processes created
Quantum executions = Context switches
Waiting time equals a positive total.
**Actual values**: 
Completed Processes: 20
Context Switches: 43
Total Waiting Time: 1380809ms
**Analysis**: 
The outcomes are consistent with Round Robin execution's anticipated scheduling behavior. Every process gets CPU time slices until it is finished, and every time a quantum expires, context shifts take place.
---

### Test 4: Different Scenarios
**Scenario tested**: [e.g., different time quantum, more processes, etc.]
Modifying:

Quantum value of time
The quantity of procedures
Student ID 
**Purpose**: 
to assess the resilience of the system under various scheduling pressures.
**Results**: 
Greater time quantum → fewer context changes
Increased waiting times due to more processes
Higher CPU switching overhead due to smaller quantum
**What I learned**: 
Time quantum selection has a significant impact on system performance. While greater quantum decreases overhead but lengthens waiting times, smaller quantum improves responsiveness but increases overhead.
---

## Part 5: Reflection and Learning

### What I learned about synchronization:
conditions and guarantee the accuracy of the data. I was aware that concurrent, unprotected access to shared resources, such as counters and logs, can result in inaccurate outcomes.

I also discovered the practical distinction between semaphores and reentrant locks: semaphores regulate access to restricted resources, such as a CPU core, whereas locks guarantee mutual exclusion. The use of try-finally blocks to ensure resource release and avoid deadlocks was another crucial idea.

Overall, this project assisted me in making the connection between actual Java threading implementation and theoretical ideas from Operating System Concepts (Silberschatz et al.).

---

### Real-world applications:

Give TWO examples where synchronization is critical:

**Example 1**: 
banking systems that allow several users to concurrently view and modify the same account balance. Synchronization guards against data corruption and guarantees accurate transactions
**Example 2**: 
Operating system process scheduling in actual CPUs requires the safe management of numerous programs that vie for CPU time through the use of scheduling algorithms and synchronization techniques.
---

### How I would explain synchronization to others:

Controlling access to a shared bathroom in a home is analogous to synchronization. It is locked to keep others out until it is free because only one person can use it at a time. A semaphore is similar to having a restricted number of identical rooms that can only accommodate a certain number of people at once. Similar to racial conditions in programming, anarchy and disputes would arise in the absence of these restrictions.

---

## Part 6: GitHub Repository Information

**Repository URL**: 
https://github.com/Saud3055/OS-Assignment3-Saud-Almutairi.git
**Number of commits**: 
6 commits
**Commit messages**: 
1. Set my student ID: 445050108
2. Added synchronization libraries for locks and semaphores
3. Implemented ReentrantLock and Semaphore shared synchronization resources
4. Protected shared counters and execution log using mutex locks
5. Implemented binary semaphore for CPU process synchronization
6. Added synchronization to final process completion execution
---

## Summary

**Total time spent on assignment**: 
10 hours
**Key takeaways**: 
1. Recognizing multi-threaded systems' race circumstances
2. Using semaphores and locks in actual scheduling simulation
3. The significance of effective resource management using try-finally
4. Putting CPU scheduling principles into practice

**Most challenging aspect**: 
managing synchronization appropriately while preserving proper program flow and preventing deadlocks.
**What I'm most proud of**: 
successfully putting into practice a synchronized CPU scheduler that replicates actual operating system behavior while maintaining thread safety and accurate statistics
---

**End of Documentation**
