# Assignment 3 - Complete Documentation

**Student Name**: [Raed Faez Al-Dawsari]  
**Student ID**: [445050283]  
**Date Submitted**: [30 April, 2026]

---

## 🎥 VIDEO DEMONSTRATION LINK (REQUIRED)

> **⚠️ IMPORTANT: This section is REQUIRED for grading!**
> 
> Upload your 3-5 minute video to your **PERSONAL Gmail Google Drive** (NOT university email).
> Set sharing to "Anyone with the link can view".
> Test the link in incognito/private mode before submitting.

**Video Link**: [https://drive.google.com/file/d/1RfggtHkO2OVUW93m6C3-EaOt2a-vFgtQ/view?usp=sharing]

**Video filename**: 445050283_Assignment3_Synchronization.mp4

**Verification**:
- [ ] Link is accessible (tested in incognito mode)
- [ ] Video is 3-5 minutes long
- [ ] Video shows code walkthrough and commits
- [ ] Video has clear audio
- [ ] Uploaded to PERSONAL Gmail (not @std.psau.edu.sa)

---

## Part 1: Development Log (1 mark)

Document your development process with **minimum 3 entries** showing progression:

### Entry 1 - [28 April, 2026 , 9:40 PM]
**What I implemented**: 
Cloned the GitHub repository and explored the code.
**Challenges encountered**: 
Understanding the scheduler and how processes are managed using threads.
**How I solved it**: 
Reviewed README file. Used YouTube as a guide and used the codes provided in slides
**Testing approach**: 
Ran the original code
**Time spent**: 
45 minutes
---

### Entry 2 - [28 April, 2026 , 10;35 PM]
**What I implemented**: 
Added  static final ReentrantLock for shared counters (context switches, completed processes, waiting time).
**Challenges encountered**: 
Confusion about where race conditions occur.
**How I solved it**: 
Used YouTube and codes in slides to understand.
**Testing approach**: 
Ran program multiple times and checked the output.
**Time spent**: 
1 hour
---

### Entry 3 - [28 April, 2026 , 11;00 PM]
**What I implemented**: 
Implemented Semaphore to control CPU access with 1 permit
**Challenges encountered**: 
Ensuring correct placement of acquire() and release() to avoid deadlocks.
**How I solved it**: 
Used try-finally blocks to make sure semaphore release even if interruption occurs.
**Testing approach**: 
Executed program  multiple times to make sure only one runs in CPU.
**Time spent**: 
1 hour
---

### Entry 4 - [29 April, 2026 , 2:00 PM]
**What I implemented**: 
completing the Assignment Documentation file by answering the technical questions related to synchronization, race conditions, locks, semaphores, and design decisions.
**Challenges encountered**: 
I had some difficulty understanding the difference between ReentrantLock and Semaphore and how to explain lock
**How I solved it**: 
Reviewed my implementation in the code and matched each concept with its practical usage in the scheduler
**Testing approach**: 
checked answers with the actual code behavior to ensure explanations matched implementation
**Time spent**: 
1.5 hour
---

### Entry 5 - [30 April, 2026 , 12:00 PM]
**What I implemented**: 
Final review of the code, recorded the video demonstration, and completed the assignment submission.
**Challenges encountered**: 
Ensuring full understanding of synchronization concepts and explaining them clearly in the video.
**How I solved it**: 
Reviewed the code multiple times, read related materials, and used YouTube tutorials to better understand locks and semaphores
**Testing approach**: 
Ran the program several times to confirm consistent and correct results before recording the video.
**Time spent**: 
1 hour
---

## Part 2: Technical Questions (1 mark)

### Question 1: Race Conditions
**Q**: Identify and explain TWO race conditions in the original code. For each:
- What shared resource is affected?
- Why is concurrent access a problem?
- What incorrect behavior could occur?

**Your Answer**:

There are two race conditions. The first one occurs in contextSwitchCount. Some threads could update the value at the same time. The second one may occur in executionLog ArrayList, multiple threads can change it concurrently, which lead to incorrect results. Concurrent access is a problem because incrementing isn't atomic. And we solved it by using locks.

---

### Question 2: Locks vs Semaphores
**Q**: Explain the difference between ReentrantLock and Semaphore. Where did you use each in your code and why?

**Your Answer**:

Lock is used for mutual exclusion to protect shared resources like counters and logs. It ensures only one thread can access a critical section at a time. Semaphore controls access based on permits. I used ReentrantLock for protecting shared variables and Semaphore to control CPU access.

---

### Question 3: Deadlock Prevention
**Q**: What is deadlock? Explain TWO prevention techniques and what you did to prevent deadlocks in your code.

**Your Answer**:

Deadlock occurs when two or more threads are waiting for each other to release resources, causing the program to stop. One prevention technique is using try-finally blocks to ensure locks are always released. Another technique is avoiding nested locks. In my code, I used try-finally blocks to release locks and avoided multiple locks in the same section, preventing deadlocks.

---

### Question 4: Lock Granularity Design Decision 
**Q**: For Task 1 (protecting the three counters), explain your lock design choice:
- Did you use ONE lock for all three counters (coarse-grained) OR separate locks for each counter (fine-grained)?
- Explain WHY you made this choice
- What are the trade-offs between the two approaches?
- Given that the three counters are independent, which approach provides better concurrency and why?

**Your Answer**:

I used fine-grained locking by separating locks for different shared counters. Such as waiting time and process count, is protected by its own lock. This approach improves concurrency because multiple threads can update different counters at the same time without blocking each other. The trade-off is that it increases code complexity compared to using a single lock. However, since the counters are independent, fine-grained locking provides better performance and efficiency in a multi-threaded environment.

---

## Part 3: Synchronization Analysis (1 mark)

### Critical Section #1: Counter Variables

**Which variables**: 
contextSwitchCount
completedProcessCount
totalWaitingTime
**Why they need protection**: 
Without synchronization, concurrent updates can cause race conditions
**Synchronization mechanism used**: 
ReentrantLock (multiple locks - fine-grained locking)
**Code snippet**:
contextSwitchLock.lock();
try {
    contextSwitchCount++;
} finally {
    contextSwitchLock.unlock();
}
**Justification**:
Each counter is protected using its own lock to ensure thread-safe updates. This prevents race conditions.
---

### Critical Section #2: Execution Log

**What resource**: 
executionLog (ArrayList)
**Why it needs protection**: 
it could cause race conditions.
**Synchronization mechanism used**: 
ReentrantLock (logLock)
**Code snippet**:
```java
logLock.lock();
try {
    executionLog.add(message);
} finally {
    logLock.unlock();
}

**Justification**: 
The log is protected using a dedicated lock
---

### Critical Section #3: CPU Semaphore

**Purpose of semaphore**: 
To control access to the CPU so that only one process executes at a time.
**Number of permits and why**: 
1 permit (binary semaphore), to ensure only one process can access the CPU at a time.
**Where implemented**: 
Inside Process.run() and runToCompletion() methods.
**Code snippet**:
```java
SharedResources.cpuSemaphore.acquire();

try {
    // process execution
} finally {
    SharedResources.cpuSemaphore.release();
}

**Effect on program behavior**: 
Ensures that only one thread executes in the CPU at a time.
---

## Part 4: Testing and Verification (2 marks)

### Test 1: Consistency Check
**What I tested**: Running program multiple times to verify consistent results

**Testing procedure**: 
Executed the program at least 5 times with the same student ID and compared outputs such as context switches, completed processes, and waiting time.
```

**Results**: 
The results were consistent across multiple runs.

**Why synchronization is necessary**: 
Without synchronization, race conditions could occur in shared resources such as contextSwitchCount, completedProcessCount, totalWaitingTime, and executionLog. 
This could lead to incorrect output.

**Conclusion**: 
Synchronization ensured data consistency and correct execution results across multiple runs.
---

### Test 2: Exception Testing
**What I tested**: Checking for ConcurrentModificationException

**Testing procedure**: 
Ran the program multiple times while processes were executing concurrently.
**Results**: 
No ConcurrentModificationException occurred.
**What this proves**: 
The execution log is properly synchronized.
---

### Test 3: Correctness Verification
**What I tested**: Verifying correct final values (total burst time, context switches, etc.)

**Expected values**: 
1. Context switches should match number of process executions.
2. Completed processes should equal total number of processes.
3. Waiting time should be non-negative and consistent.
**Actual values**: 
Values were consistent with expected behavior and matched program execution results
**Analysis**: 
Synchronization ensured atomic updates to shared counters, preventing incorrect increments.
---

### Test 4: Different Scenarios
**Scenario tested**: [e.g., different time quantum, more processes, etc.]

**Purpose**: 
To verify that the scheduler works correctly.
**Results**:
correct scheduling behavior and stable output across all scenarios.

**What I learned**: 
The synchronization works correctly in different situations and keeps the program safe when multiple threads run at the same time.
---

## Part 5: Reflection and Learning

### What I learned about synchronization:

I learned that synchronization is important when multiple threads share the same data. Without synchronization, race conditions can occur and cause incorrect results. I understood how locks like ReentrantLock protect critical sections by allowing only one thread at a time. I also learned how semaphores control access to limited resources like CPU. One challenge I faced was understanding where to place locks correctly. I also learned that using too many locks can make the code more complex.

---

### Real-world applications:

Give TWO examples where synchronization is critical:

**Example 1**: 
Bank systems, where multiple users access and update the same account balance at the same time.
**Example 2**: 
Online booking systems (like flight or hotel reservations), where multiple users try to book the same seat or room at the same time.
---

### How I would explain synchronization to others:

Synchronization is like organizing people who want to use the same resource. Imagine many people trying to use one bathroom. Without rules, it will be messy and unfair. Synchronization acts like a lock on the door, allowing only one person at a time. This prevents problems and keeps everything working correctly.

---

## Part 6: GitHub Repository Information

**Repository URL**: 
https://github.com/Raed-Faez-277/OS-Assignment3-Raed-Faez.git
**Number of commits**: 
4
**Commit messages**: 
1. Set my student ID: 444050283
2. Method to increment context switch counter using lock
3. Acquire semaphore before executing.
4. Update race conditions explanation in documentation.

---

## Summary

**Total time spent on assignment**: 
4 hours
**Key takeaways**: 
1. Synchronization is important to prevent race conditions
2. Locks and semaphores help to control access to shared resources
3. Multi-threading requires careful design to avoid errors

**Most challenging aspect**: 
Understanding where to place locks correctly in synchronization.
**What I'm most proud of**: 
coding synchronization without errors.
---

**End of Documentation**
