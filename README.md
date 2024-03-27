<h2>Description (Post Office)</h2>

In the system, we have 3 types of processes: (0) main process, (1) postal clerk, and (2) customer. Each customer goes to the post office to handle one of three types of requests: postal services, packages, and money services. Each request is uniquely identified by a number (letters: 1, packages: 2, money services: 3). Upon arrival, a customer joins the queue according to the activity they intend to handle. Each clerk serves all queues (randomly selecting one queue each time). If no customer is currently waiting, the clerk takes a short break. After closing the post office, the clerks finish serving all customers in the queue, and once all queues are empty, they go home. Any customers who arrive after the post office is closed also go home (tomorrow is another day).

<h2>Detailed Task Description</h2>
Execution:

$ ./proj2 NZ NU TZ TU F

• NZ: number of customers

• NU: number of clerks

• TZ: Maximum time in milliseconds that a customer waits before entering the post office (otherwise leaves without being served). 0<=TZ<=10000

• TU: Maximum length of clerk's break in milliseconds. 0<=TU<=100

• F: Maximum time in milliseconds after which the post office is closed to new arrivals. 0<=F<=10000


                                                          
<h2>Error Conditions:</h2>

• If any of the inputs do not match the expected format or are out of the allowed range, the program prints an error message to the standard error output, releases all allocated resources, and exits with exit code 1.

• If any semaphore or shared memory operation fails, the program follows the same process—printing an error message to the standard error output, releasing all allocated resources, and exiting with exit code 1.


<h2>Main Process</h2>

• Upon execution, the main process immediately creates NZ customer processes and NU clerk processes.

 • Waits for a random time interval <F/2,F> using the usleep function.

 • Prints: A: closing

• Then waits for the termination of all processes created by the application. Once these processes are terminated, the main process also terminates with exit code 0.


<h2>Customer Process</h2>

  • Each customer is uniquely identified by the number idZ, 0<idZ<=NZ

  • Upon execution, prints: A: Z idZ: started

  • Then waits for a random time interval <0,TZ> using the usleep function.

  • If the post office is closed:

    ◦ Prints: A: Z idZ: going home
  
    ◦ Process terminates
  
  • If the post office is open, randomly selects activity X---a number from the interval <1,3>
  
    ◦ Prints: A: Z idZ: entering office for a service X
  
    ◦ Joins queue X and waits to be called by a clerk.
  
    ◦ Prints: Z idZ: called by office worker
  
    ◦ Then waits for a random time interval <0,10> using the usleep function (no synchronization with the clerk's completion of the request is required).
  
    ◦ Prints: A: Z idZ: going home
  
    ◦ Process terminates

<h2>Clerk Process</h2>

  • Each clerk is uniquely identified by the number idU, 0<idU<=NU

  • Upon execution, prints: A: U idU: started

[beginning of cycle]

  • Clerk serves a customer from queue X (randomly selects any non-empty one).

    ◦ Prints: A: U idU: serving a service of type X
  
    ◦ Then waits for a random time interval <0,10> using the usleep function.
  
    ◦ Prints: A: U idU: service finished
  
    ◦  Proceeds to [beginning of cycle]
  
• If no customer is waiting in any queue and the post office is open:

    ◦ Prints: A: U idU: taking break
  
    ◦ Then waits for a random time interval <0,TU> using the usleep function.
  
    ◦ Prints: A: U idU: break finished
  
    ◦ Proceeds to [beginning of cycle]
  
• If no customer is waiting in any queue and the post office is closed:

    ◦ Prints: A: U idU: going home
  
    ◦ Process terminates
  

<h2>Given requirements</h2>

• Implement the project in the C language. Comment the source code and program neatly. The quality of the source code will also be evaluated.

• Ensure that all processes terminate correctly and that all allocated resources are properly released upon termination.

• Adhere to the syntax of specified names, file format, and output data format. Use a basic script to verify the correctness of the output format (available from the assignment website).

• Queries regarding the assignment: Address all ambiguities and questions only through the project 2 discussion forum.

• Note for testing: You can simulate more frequent process switching, for example, by inserting short sleeps after releasing semaphores, etc. For testing purposes only; do not include in the final solution.

• Each process performs its actions and simultaneously writes information about the actions to a file named proj2.out. The output information includes the sequential number A of the action being performed (see output description). Actions are numbered starting from one.

• Use shared memory to implement the action counter and shared variables necessary for synchronization.

• Use semaphores for process synchronization.

• Do not use active waiting (including cyclically sleeping the process) for synchronization purposes.

• Work with processes, not threads.

<h2>Compilation</h2>

• Use the make tool for compilation. The submission will include a Makefile.

• Compilation is performed by running the make command in the directory where the Makefile is located.

• After compilation, an executable file named proj2 will be created in the same directory as the Makefile.

• The executable file may depend only on system libraries—it must not assume the existence of any other student-created files (e.g., clerk executable, configuration file, customer dynamic library, etc.).

• Compile source codes with the options -std=gnu99 -Wall -Wextra -Werror -pedantic

• If your solution requires it, you can add additional options for the linker (e.g., for semaphores or shared memory, -pthread, -lrt, etc.).

• Your solution must be compilable and runnable on the merlin server.

<h2>Example Output</h2>

Example output file proj2.out for the following command:

```
$ ./proj2 3 2 100 100 100
1: U 1: started
2: Z 3: started
3: Z 1: started
4: Z 1: entering office for a service 2
5: U 2: started
6: Z 2:

 started
7: Z 3: entering office for a service 1
8: Z 1: called by office worker
9: U 1: serving a service of type 2
10: U 1: service finished
11: Z 1: going home
12: Z 3: called by office worker
13: U 2: serving a service of type 1
14: U 1: taking break
15: closing
16: U 1: break finished
17: U 1: going home
18: Z 2: going home
19: U 2: service finished
20: U 2: going home
21: Z 3: going home
```
