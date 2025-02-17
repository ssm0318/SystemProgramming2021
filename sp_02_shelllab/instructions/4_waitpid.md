```bash
WAIT(2)                     BSD System Calls Manual                    WAIT(2)

NAME
     wait, wait3, wait4, waitpid -- wait for process termination

SYNOPSIS
     #include <sys/wait.h>

     pid_t
     wait(int *stat_loc);

     pid_t
     wait3(int *stat_loc, int options, struct rusage *rusage);

     pid_t
     wait4(pid_t pid, int *stat_loc, int options, struct rusage *rusage);

     pid_t
     waitpid(pid_t pid, int *stat_loc, int options);

DESCRIPTION
     The wait() function suspends execution of its calling process until stat_loc information is available for a terminated child process, or a signal is received.  On return from a successful wait() call, the
     stat_loc area contains termination information about the process that exited as defined below.

     The wait4() call provides a more general interface for programs that need to wait for certain child processes, that need resource utilization statistics accumulated by child processes, or that require
     options.  The other wait functions are implemented using wait4().

     The pid parameter specifies the set of child processes for which to wait.  If pid is -1, the call waits for any child process.  If pid is 0, the call waits for any child process in the process group of the
     caller.  If pid is greater than zero, the call waits for the process with process id pid.  If pid is less than -1, the call waits for any process whose process group id equals the absolute value of pid.

     The stat_loc parameter is defined below.  The options parameter contains the bitwise OR of any of the following options.  The WNOHANG option is used to indicate that the call should not block if there are no
     processes that wish to report status.  If the WUNTRACED option is set, children of the current process that are stopped due to a SIGTTIN, SIGTTOU, SIGTSTP, or SIGSTOP signal also have their status reported.

     If rusage is non-zero, a summary of the resources used by the terminated process and all its children is returned (this information is currently not available for stopped processes).

     When the WNOHANG option is specified and no processes wish to report status, wait4() returns a process id of 0.

     The waitpid() call is identical to wait4() with an rusage value of zero.  The older wait3() call is the same as wait4() with a pid value of -1.

     The following macros may be used to test the manner of exit of the process.  One of the first three macros will evaluate to a non-zero (true) value:

     WIFEXITED(status)
             True if the process terminated normally by a call to _exit(2) or exit(3).

     WIFSIGNALED(status)
             True if the process terminated due to receipt of a signal.

     WIFSTOPPED(status)
             True if the process has not terminated, but has stopped and can be restarted.  This macro can be true only if the wait call specified the WUNTRACED option or if the child process is being traced (see
             ptrace(2)).

     Depending on the values of those macros, the following macros produce the remaining status information about the child process:

     WEXITSTATUS(status)
             If WIFEXITED(status) is true, evaluates to the low-order 8 bits of the argument passed to _exit(2) or exit(3) by the child.

     WTERMSIG(status)
             If WIFSIGNALED(status) is true, evaluates to the number of the signal that caused the termination of the process.

     WCOREDUMP(status)
             If WIFSIGNALED(status) is true, evaluates as true if the termination of the process was accompanied by the creation of a core file containing an image of the process when the signal was received.

     WSTOPSIG(status)
             If WIFSTOPPED(status) is true, evaluates to the number of the signal that caused the process to stop.

NOTES
     See sigaction(2) for a list of termination signals.  A status of 0 indicates normal termination.

     If a parent process terminates without waiting for all of its child processes to terminate, the remaining child processes are assigned the parent process 1 ID (the init process ID).

     If a signal is caught while any of the wait() calls is pending, the call may be interrupted or restarted when the signal-catching routine returns, depending on the options in effect for the signal; see
     intro(2), System call restart.

RETURN VALUES
     If wait() returns due to a stopped or terminated child process, the process ID of the child is returned to the calling process.  Otherwise, a value of -1 is returned and errno is set to indicate the error.

     If wait3(), wait4(), or waitpid() returns due to a stopped or terminated child process, the process ID of the child is returned to the calling process.  If there are no children not previously awaited, -1 is
     returned with errno set to [ECHILD].  Otherwise, if WNOHANG is specified and there are no stopped or exited children, 0 is returned.  If an error is detected or a caught signal aborts the call, a value of -1
     is returned and errno is set to indicate the error.

ERRORS
     The wait() system call will fail and return immediately if:

     [ECHILD]           The calling process has no existing unwaited-for child processes.

     [EFAULT]           The status or rusage argument points to an illegal address (may not be detected before the exit of a child process).

     [EINVAL]           Invalid or undefined flags are passed in the options argument.

     The wait3() and waitpid() calls will fail and return immediately if:

     [ECHILD]           The process specified by pid does not exist or is not a child of the calling process, or the process group specified by pid does not exist or does not have any member process that is a
                        child of the calling process.

     The waitpid() call will fail and return immediately if:

     [EINVAL]           The options argument is not valid.

     Any of these calls will fail and return immediately if:

     [EINTR]            The call is interrupted by a caught signal or the signal does not have the SA_RESTART flag set.

STANDARDS
     The wait() and waitpid() functions are defined by POSIX; wait3() and wait4() are not specified by POSIX.  The WCOREDUMP() macro and the ability to restart a pending wait() call are extensions to the POSIX
     interface.

LEGACY SYNOPSIS
     #include <sys/types.h>
     #include <sys/wait.h>

     The include file <sys/types.h> is necessary.

SEE ALSO
     sigaction(2), exit(3), compat(5)

HISTORY
     A wait() function call appeared in Version 6 AT&T UNIX.

4th Berkeley Distribution       April 19, 1994       4th Berkeley Distribution
(END)
```