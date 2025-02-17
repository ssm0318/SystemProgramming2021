```bash
SETPGID(2)                  BSD System Calls Manual                 SETPGID(2)

NAME
     setpgid, setpgrp -- set process group

SYNOPSIS
     #include <unistd.h>

     int
     setpgid(pid_t pid, pid_t pgid);

     pid_t
     setpgrp(void);

DESCRIPTION
     setpgid() sets the process group of the specified process pid to the specified pgid.  If pid is zero, then the call applies to the current process.

     If the invoker is not the super-user, then the affected process must have the same effective user-id as the invoker or be a descendant of the invoking process.

RETURN VALUES
     setpgid() returns 0 when the operation was successful.  If the request failed, -1 is returned and the global variable errno indicates the reason.

ERRORS
     setpgid() will fail and the process group will not be altered if:

     [EACCES]           The value of the pid argument matches the process ID of a child process of the calling process, and the child process has successfully executed one of the exec functions.

     [EINVAL]           The value of the pgid argument is less than 0 or is not a value supported by the implementation.

     [EPERM]            The process indicated by the pid argument is a session leader.

     [EPERM]            The effective user ID of the requested process is different from that of the caller and the process is not a descendant of the calling process.

     [EPERM]            The value of the pgid argument is valid, but does not match the process ID of the process indicated by the pid argument and there is no process with a process group ID that matches the
                        value of the pgid argument in the same session as the calling process.

     [ESRCH]            The value of the pid argument does not match the process ID of the calling process or of a child process of the calling process.

LEGACY SYNOPSIS
     #include <unistd.h>

     int
     setpgrp(pid_t pid, pid_t pgid);

     The legacy setpgrp() function is a clone of the setpgid() function, retained for calling convention compatibility with historical versions of BSD.

COMPATIBILITY
     Use of the legacy version of the setpgrp() call will cause compiler diagnostics.  Use setpgid() instead.

     Use of private (and conflicting) prototypes for setpgrp() will cause compiler diagnostics.  Delete the private prototypes and include <unistd.h>.

SEE ALSO
     getpgrp(2), compat(5)

STANDARDS
     The setpgid() function conforms to IEEE Std 1003.1-1988 (``POSIX.1'').

4th Berkeley Distribution        June 4, 1993        4th Berkeley Distribution
(END)
```
