# Key Concepts of OS

- **Process Management**:
    - What is process?
        - How do processes schedule?
            
            ![Untitled](Key%20Concepts%20of%20OS%20d0703d6dc67844959b74a6eee8931731/Untitled.png)
            
        - How do processes communicate?
    - What is thread?
        
        A thread is a basic unit of CPU utilization.
        
        ![Untitled](Key%20Concepts%20of%20OS%20d0703d6dc67844959b74a6eee8931731/Untitled%201.png)
        
        - What is concurrency?
            
            it is possible to have concurrency without parallelism.
            
        - Multithreading Models
            - What are user threads and kernel threads?
            - Example of Multithreading Models?
                - Many-to-One Model
                    
                    will block the process if only one user thread makes a blocking system call.
                    
                
                One-to-One Model
                
                Many-to-Many Model
                
            - 
        
    - What is CPU scheduling?
        
        CPU scheduling deals with the problem of deciding which of the processes in the ready queue is to be allocated the CPU’s core.
        
        - Example of Scheduling Algorithms
            
            FCFS
            
            SJF
            
            RR
            
            Priority
            
            Multilevel Queue
            
            Multilevel Feedback Queue
            
        - Thread scheduling
            
            On most modern operating systems it is kernel-level threads—not processes—that are being scheduled by the operating system.
            
            User-level threads are managed by a thread library, and the kernel is unaware of them.
            
            When we say the thread library schedules user threads onto available LWPs(lightweight process), we do not mean that the threads are actually running on a CPU as that further requires the operating system to schedule the LWP’s kernel thread onto a physical CPU core.
            
    - 
- **Process Synchronization:**
    - What is race condition?
    - What is a critical section?
        
        Mutual exclusion
        
        Progress
        
        Bounded Waiting
        
        ### Peterson’s solution
        
        Peterson’s solution is not guaranteed to work on modern computer architectures for the primary reason that, to improve system performance, processors and/or compilers may reorder read and write operations that have no dependencies
        
        ```cpp
        while (true) {
        // don't reorder the following two lines
        // if reordered, critical section won't work
        	flag[i] = true;
        	turn = j;
        
        	while (flag[j] && turn == j) ;// wait
        
        	/* critical section */
        
        	flag[i] = false;
        
        	/*remainder section */
        }
        ```
        
    - hardware
        
        ```cpp
        boolean test_and_set(boolean *target) { 
        	boolean rv = *target;
        	*target = true;
        	return rv;
        }
        
        do{ 
        	while (test and set(&lock)); /* do nothing */
        	/* critical section */ 
        	lock = false;
        	/* remainder section */ 
        } while (true);
        ```
        
    - Mutex Locks
        
        ```cpp
        acquire() { 
        	while (!available) ; /* busy wait */ 
        	available = false;
        }
        
        release() { 
        	available = true;
        }
        
        while (true) {
        	acquire()
        	// critical section
        	release()
        	// remainder section
        }
        ```
        
    - Semaphores
        
        ```cpp
        wait(S) { 
        	while (S <= 0) ; // busy wait 
        	S--;
        }
        signal(S) {
        	S++;
        }
        ```
        
    - Synchronization example
        
        producer-consumer
        
        ```cpp
        // producer
        while (true) {
        /* produce an item in next produced */
        
        wait(empty); 
        wait(mutex);
        
        /* add next produced to the buffer */
        
        signal(mutex); 
        signal(full);
        
        }
        
        // consumer
        while (true) {
        	wait(full); 
        	wait(mutex);
        	/* remove an item from buffer to next consumed */
        	
        	signal(mutex); 
        	signal(empty);
        	
        	/* consume the item in next consumed */
        }
        ```
        
        reader-writers
        
        ```cpp
        // writer
        while (true) {
        
        	wait(rw_mutex);
        	
        	/* writing is performed */
        	
        	signal(rw_mutex);
        }
        
        while (true) {
        
        wait(mutex); 
        read_count++; 
        if (read count == 1)
        	wait(rw_mutex); 
        signal(mutex);
        
        /* reading is performed */
        
        wait(mutex); 
        read_count--; 
        if (read count == 0)
        	signal(rw_mutex); 
        signal(mutex);
        
        }
        ```
        
        Dining-Philosophers Problem
        
        ```cpp
        while (true) {
        
        wait(chopstick[i]); 
        wait(chopstick[(i+1) % 5]);
        
        /* eat for a while */
        
        signal(chopstick[i]); 
        signal(chopstick[(i+1) % 5]);
        
        /* think for awhile */
        
        }
        ```
        
    - Deadlocks
        
        ### conditions
        
        1. Mutual exclusion
        2. Hold and wait
        3. No preemption
        4. Circular wait
        
        ### handling
        
        1. ignore
        2. prevent or avoid (not allow deadlock)
            
            **prevent**
            
            1. take all or none
            2. allow preemption
            3. request k ⇒ release all i, where i≥k
            
            **avoid**
            
            Banker's Algorithm
            
            ![Untitled](Key%20Concepts%20of%20OS%20d0703d6dc67844959b74a6eee8931731/Untitled%202.png)
            
        3. detect and recover (allow deadlock)
            
            ![Untitled](Key%20Concepts%20of%20OS%20d0703d6dc67844959b74a6eee8931731/Untitled%203.png)
            

Operating systems (OS) are complex software that serves as an intermediary between computer hardware and user applications. They manage various resources and provide an interface for users to interact with the system. Key concepts of operating systems include:

1. **Process Management**:
    
    [Process & Thread](Key%20Concepts%20of%20OS%20d0703d6dc67844959b74a6eee8931731/Process%20&%20Thread%20237cfc0271bf4c1ab17f1454d8ba6f13.md)
    
    [Heap vs Stack](Key%20Concepts%20of%20OS%20d0703d6dc67844959b74a6eee8931731/Heap%20vs%20Stack%200edc610ea31a429fbc937409b1698351.md)
    
    [**Synchronization**](Key%20Concepts%20of%20OS%20d0703d6dc67844959b74a6eee8931731/Synchronization%204382cc684a17437cb61398bcb5a706e7.md)
    
    - **Processes**: Units of execution that represent running programs. The OS manages processes, scheduling them for execution, allocating resources, and providing inter-process communication.
    - **Scheduling**: Deciding which processes to run when, using scheduling algorithms like First Come First Serve (FCFS), Round Robin, etc.
    - **Synchronization**: Ensuring proper coordination between concurrent processes, preventing issues like race conditions and deadlocks.
2. **Memory Management**:
    
    [Paging](Key%20Concepts%20of%20OS%20d0703d6dc67844959b74a6eee8931731/Paging%20577d8da7f66b4441aada1935ce2af2eb.md)
    
    [Virtual Memory](Key%20Concepts%20of%20OS%20d0703d6dc67844959b74a6eee8931731/Virtual%20Memory%20893434acd5f94120958594688e7ea231.md)
    
    [Memory Protection](Key%20Concepts%20of%20OS%20d0703d6dc67844959b74a6eee8931731/Memory%20Protection%2063e362a3de784a03a2790d0143ddba7b.md)
    
    - **Virtual Memory**: Mechanism for efficiently managing memory by using secondary storage (like hard drives) as an extension of RAM.
    - **Address Translation**: Converting logical addresses used by programs into physical addresses in memory.
    - **Memory Allocation**: Assigning portions of memory to processes, and managing allocation and deallocation.
3. **File System Management**:
    
    [File System](Key%20Concepts%20of%20OS%20d0703d6dc67844959b74a6eee8931731/File%20System%20f1ca245f5adc4469b625c8817ac312e0.md)
    
    - **File Systems**: Organizing and managing data stored on secondary storage devices like hard drives.
    - **File Operations**: Providing mechanisms for creating, reading, writing, and deleting files.
    - **Directory Structures**: Organizing files into hierarchical structures for easy navigation.
4. **Device Management**:
    - **Device Drivers**: Software components that facilitate communication between the OS and hardware devices.
    - **I/O Operations**: Managing input and output operations to and from devices such as keyboards, printers, disks, etc.
    - **Interrupt Handling**: Responding to hardware interrupts generated by devices.
5. **Security**:
    
    [Security](Key%20Concepts%20of%20OS%20d0703d6dc67844959b74a6eee8931731/Security%20760a78cc08c24237bffcfed60682fc0b.md)
    
    - **Authentication and Authorization**: Verifying the identity of users and determining their level of access.
    - **Data Protection**: Ensuring the confidentiality, integrity, and availability of data.
    - **User Privileges**: Controlling what actions users can perform on the system.
6. **Networking**:
    
    [Networking](Key%20Concepts%20of%20OS%20d0703d6dc67844959b74a6eee8931731/Networking%20c2121ad4084a43b485e21821f0b32c32.md)
    
    [Socket](Key%20Concepts%20of%20OS%20d0703d6dc67844959b74a6eee8931731/Socket%204d604d2274044909842ab2d9830a13e3.md)
    
    - **Network Protocols**: Implementing communication protocols for networking tasks like sending and receiving data over networks.
    - **Socket Programming**: Providing interfaces for applications to communicate over a network using sockets.
    - **Resource Sharing**: Facilitating the sharing of network resources among multiple users and applications.
7. **User Interface**:
    
    [User mode and kernel mode](Key%20Concepts%20of%20OS%20d0703d6dc67844959b74a6eee8931731/User%20mode%20and%20kernel%20mode%20e466c91516754e7584735b6944de9801.md)
    
    - **Command Line Interface (CLI)**: Text-based interface for issuing commands to the operating system.
    - **Graphical User Interface (GUI)**: Visual interface with windows, icons, menus, and pointers for user interaction.
    - **System Calls**: Interfaces for applications to request services from the operating system, like file operations or memory allocation.

These concepts collectively form the foundation of modern operating systems, which enable computers to efficiently manage resources, run multiple programs simultaneously, and provide a user-friendly environment for interaction.

# Virtualization

[Virtualization (1)](Key%20Concepts%20of%20OS%20d0703d6dc67844959b74a6eee8931731/Virtualization%20(1)%20bc017076695b446c8399be84fd0cdc57.md)

[Containerization (1)](Key%20Concepts%20of%20OS%20d0703d6dc67844959b74a6eee8931731/Containerization%20(1)%20e03de150d904440aa3a5771d163c2b78.md)

[Virtualization vs containerization (1)](Key%20Concepts%20of%20OS%20d0703d6dc67844959b74a6eee8931731/Virtualization%20vs%20containerization%20(1)%2088ed572c32684d169eaaa9ba541d52ee.md)

[VM vs Container (1)](Key%20Concepts%20of%20OS%20d0703d6dc67844959b74a6eee8931731/VM%20vs%20Container%20(1)%20dd96a90f52d04c9888bd63920b116bd5.md)

# OS Structure

[OS Structure (1)](Key%20Concepts%20of%20OS%20d0703d6dc67844959b74a6eee8931731/OS%20Structure%20(1)%206889f16b8b3b4273a12261afcf3fa48b.md)