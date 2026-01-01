# Process Signals

## Process : 
A process is running program in computer system.  
process has its own identity -> PID (Process ID)

## Signal : 
Short message OS or User sends to process to take certain actions.

---

**Process Signals** means sending signals to running process to control their behaviour. That enabels communication between user and OS to manage process in real time.

### Types
+ **Standard signals** : Pre Defined signals that OS sends to process to control the behaviour. This has fixed meaning (Like Start, Stop and Terminat).
    + SIGINT ( Signal Interrupt ) : Used to interup the process.  
        `kill -SIGINT <pid>` or **ctrl + C**
    + SIGTERM ( Signal Terminate ) : Process to terminate gracefully ( Save all the data and clean up before cloasing ) .   
        `kill -SIGTERM <pid>`  
        Here the process is saved in swap file.
        So to get that data use `vim -r <file>` recovery.
    + SIGKILL ( Signal Kill ) : Forcefully kill process instently.  
        `kill -SIGKILL <pid>`
    + SIGSTOP ( Signal Stop ) : Pause the running process (can be resumed when needed).  
        `kill -SIGKILL <pid>` 
    + SIGCONT ( Signal Continue ) : Resume to stop process.  
        `kill -SIGCONT <pid>`  
        Here if we continue the process it will be running in bg, to bring to foregrount use cmd `fg`.
    + SIGSEGV ( Signal Segmentation Fault ) : kills the process as if it had caused an invalid memory access (Segmentation fault).  
        `kill -SIGSEGV <pid>` 
+ **Real time signals** : Sends Extra information with signal.
    Real time signals are queued ( Sends exact order then arrived ).
    1. SIGRTMIN : Marks beginning of the signal range.
    2. SIGRTMAX : Marks highest range of the signal.
    3. Intermidiate signals : Used for custorm process.
