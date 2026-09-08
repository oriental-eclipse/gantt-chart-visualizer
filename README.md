# OS Project: Dispatch Bench (CPU Scheduling Visualizer)

This is a college project for our Operating Systems course. It simulates and visualizes how a CPU dispatcher schedules a set of processes using two different scheduling algorithms: **Round Robin (RR)** and **Preemptive Priority Scheduling**.

## Features
- **Interactive Process Editor**: Add processes and edit their Arrival Time (AT), Burst Time (BT), and Priority on the fly.
- **Live Gantt Chart**: Visualizes the timeline of process execution.
- **Live Ready Queue State**: Watch the simulation step-by-step to see exactly which process is running and which ones are waiting in the queue at any given time unit `t`.
- **Metrics Table**: Automatically calculates Completion Time (CT), Turnaround Time (TAT), and Waiting Time (WT) alongside their averages.

## How it works & How calculations are done

The simulation logic runs entirely in vanilla JavaScript. Based on the processes you input, it calculates a timeline of "segments" (which process runs at what time).

### 1. Round Robin (RR)
- **Concept**: Each process gets a small unit of CPU time (time quantum).
- **How it's simulated**: 
  - The dispatcher maintains a First-In-First-Out (FIFO) ready queue.
  - When a process arrives, it is added to the back of the queue.
  - The CPU picks the first process from the queue and runs it.
  - If the process completes within the time quantum, it leaves the system.
  - If it requires more time than the quantum, it gets preempted (paused) and is pushed back to the end of the ready queue.
  - This repeats until all processes have finished their burst times.

### 2. Preemptive Priority Scheduling
- **Concept**: The CPU always executes the process with the highest priority (in this project, a **lower number = higher priority**).
- **How it's simulated**:
  - At every time unit, the dispatcher checks the ready queue for all arrived processes that still have remaining burst time.
  - It selects the process with the smallest priority number.
  - If a new process arrives with a higher priority (smaller number) than the currently running process, the CPU *preempts* the current process and switches to the new one.
  - **Tie-breaking**: If two processes have the same priority, the algorithm favors the process that is already running (to avoid unnecessary context switching). If neither is running, it favors the one that arrived earlier.

### 3. Scheduling Metrics
Once the execution segments are generated, the following metrics are calculated for each process:
- **Completion Time (CT)**: The exact time unit when the process finishes its final burst of execution.
- **Turnaround Time (TAT)**: The total time the process spent in the system. 
  - `TAT = Completion Time - Arrival Time`
- **Waiting Time (WT)**: The total time the process spent waiting in the ready queue without using the CPU. 
  - `WT = Turnaround Time - Burst Time`

The tables at the bottom of the tool dynamically aggregate these numbers and compute the **Average Turnaround Time** and **Average Waiting Time** to help compare the efficiency of both algorithms for a given set of processes.

## How to run
Since this is built with standard HTML, CSS, and JS, there are no dependencies or build steps required. Simply open `index.html` in any modern web browser to view and interact with the visualizer!
