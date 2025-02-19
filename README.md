# Tiny Shell (tsh) - Assignment 2

## Overview
This project involves implementing a small Unix shell (Tiny Shell - `tsh`) to gain hands-on experience with process control, signals, and pipes. The shell provides basic job control, input/output redirection, and signal handling.

## Video Demonstration
A video walkthrough of our implementation can be found here:
[YouTube Video](https://youtu.be/GSSE_0GVfGc)

## Features
The implemented shell supports the following functionalities:

### Process Execution
- Executes user-specified programs with command-line arguments.
- Supports both foreground and background job execution.

### Built-in Commands
- `quit` - Exits the shell.
- `jobs` - Lists background jobs.
- `bg <job>` - Moves a stopped job to the background and continues execution.
- `fg <job>` - Moves a job to the foreground and continues execution.

### Job Control
- Implements foreground (`fg`) and background (`bg`) job manipulation.
- Uses job IDs (JID) prefixed with `%` and process IDs (PID) for job referencing.
- Properly assigns process groups and handles process state transitions.

### Signal Handling
- Captures `SIGINT` (Ctrl-C) and sends it to the foreground job.
- Captures `SIGTSTP` (Ctrl-Z) and stops the foreground job.
- Implements `SIGCHLD` handler to properly clean up zombie processes.

### Input/Output Redirection
- Supports `<` for input redirection and `>` for output redirection.
- Handles errors related to file redirection.

### Pipelining
- Supports pipelines of commands connected by `|`.
- Implements necessary process communication for command chaining.

## Compilation and Execution
### Building the Shell
To compile the shell, run:
```sh
make
```

### Running the Shell
To start the shell, execute:
```sh
./tsh
```

### Running Tests
To test the shell against provided trace files:
```sh
./sdriver.pl -t trace01.txt -s ./tsh -a "-p"
```
You can also use:
```sh
make test01  # Runs trace01.txt on tsh
```

## Implementation Details
### Key Functions
- `eval`: Parses and executes command lines.
- `builtin_cmd`: Handles built-in commands.
- `do_bgfg`: Implements `bg` and `fg` commands.
- `waitfg`: Waits for foreground job completion.
- `sigchld_handler`: Cleans up terminated child processes.
- `sigint_handler`: Handles `SIGINT` (Ctrl-C) signals.
- `sigtstp_handler`: Handles `SIGTSTP` (Ctrl-Z) signals.

### Signal Handling Strategy
- Blocks signals when modifying job states.
- Uses `sigprocmask` to prevent race conditions.
- Uses `setpgid` to ensure proper job control.

## C Programs Called by the Trace Files
These helper programs are used in trace files to test different shell behaviors:
- `myspin.c` - Takes argument `<n>` and spins for `<n>` seconds.
- `mysplit.c` - Forks a child that spins for `<n>` seconds.
- `mystop.c` - Spins for `<n>` seconds and sends `SIGTSTP` to itself.
- `myint.c` - Spins for `<n>` seconds and sends `SIGINT` to itself.

## Known Issues & Limitations
- Some complex pipeline behaviors may not fully match Bash.
- Terminal-interactive programs (e.g., `vi`, `less`) do not work as expected due to process group handling.

## References
- [The Linux Programming Interface - Kerrisk](https://man7.org/tlpi/)
- Linux `man` pages for `fork()`, `execve()`, `waitpid()`, `kill()`, `setpgid()`, `sigprocmask()`, and `sigsuspend()`

## Contributors
Esha Pakalapati, Sahaj Malavia, Rayan Itani

## License
This project is for educational purposes and follows university academic policies.
