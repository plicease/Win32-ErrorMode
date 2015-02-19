# Win32::ErrorMode

Set and retrieves the error mode for the current process.

# SYNOPSIS

    use Win32::ErrorMode qw( :all );
    
    my $error_mode = GetErrorMode();
    SetErrorMode(SEM_FAILCRITICALERRORS | SEM_NOGPFAULTERRORBOX);
    
    system "program_that_would_normal_produce_an_error_dialog.exe";

If you are using Windows 7 or better:

    use Win32::ErrorMode qw( :all );
    
    # The "Thread" versions are safer if you are using threads,
    # which includes the use of fork() on Windows.
    my $error_mode = GetThreadErrorMode();
    SetThreadErrorMode(SEM_FAILCRITICALERRORS | SEM_NOGPFAULTERRORBOX);
    
    system "program_that_would_normal_produce_an_error_dialog.exe";

# DESCRIPTION

The main motivation for this module is to povide an interface for
turning off those blasted dialog boxes when you try to run .exe
with missing symbols or .dll files.  This is useful when you have
a long running process or a test suite where such failures are
expected, or part of the configuration process.

It may have other applications.  It also attempts to smooth over
the variously incompatible versions of Windows while maintaing
binary compatibility.

# FUNCTIONS

## SetErrorMode

    SetErrorMode($mode);

Controls whether Windows will handle the specified type of serious erros 
or whether the process wil handle them.

`$mode` can be zero or more of the following values, bitwise or'd 
together:

- SEM\_FAILCRITICALERRORS

    Do not display the critical error message box.

- SEM\_NOALIGNMENTFAULTEXCEPT

    Automatically fix memory alignment faults.

- SEM\_NOGPFAULTERRORBOX

    Do not display the windows error reporting dialog.

- SEM\_NOOPENFILEERRORBOX

    Do not display a message box when the system fails to find a file.

## GetErrorMode

    my $mode = GetErrorMode();

Retrieves the error mode for the current process.

## SetThreadErrorMode

    SetThreadErrorMode($mode);

Same as ["SetErrorMode"](#seterrormode) above, except it only changes the error mode
on the current thread.  Only available when running under Windows 7 or
newer.

## GetThreadErrorMode

    my $mode = GetThreadErrorMode();

Same as ["GetErrorMode"](#geterrormode) above, except it only gets the error mode
for the current thread.  Only available when running under Windows 7
or newer.

# CAVEATS

`GetErrorMode` was introduced in Windows Vista / 2008, but will be
emulated on XP using `SetErrorMode`, but there may be a race 
condition if you are using threads / forking as the emulation
temporarily sets the error mode.  Then again there is probably a
race condition anyway since you are using the global version in a
threaded application, but you should keep this in mind if you must
support old versions of Windows.

# SEE ALSO

[Win32API::File](https://metacpan.org/pod/Win32API::File) includes an interface to `SetErrorMode`, but not
`GetErrorMode`.  The interface for this function appears to be a
side effect of the main purpose of the module.  The inteface to
`SetErrorMode` is not well documented in [Win32API::File](https://metacpan.org/pod/Win32API::File), but is
usable.

# AUTHOR

Graham Ollis <plicease@cpan.org>

# COPYRIGHT AND LICENSE

This software is copyright (c) 2015 by Graham Ollis.

This is free software; you can redistribute it and/or modify it under
the same terms as the Perl 5 programming language system itself.
