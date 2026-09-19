# Metasploit Bind and Reverse Shell Lab

A controlled Metasploit lab demonstrating payload generation, reverse and bind TCP connections, Meterpreter sessions, and read-only post-exploitation enumeration.

## Scope

All payload execution and network callbacks in this lab were performed locally on a Kali Linux virtual machine using the loopback address `127.0.0.1`. No external hosts or third-party systems were targeted.

## Environment

- OS: Kali GNU/Linux Rolling 2026.3
- Metasploit Framework: 6.5.0-dev
- Architecture: x86-64
- Callback/listener address: `127.0.0.1`
- Reverse TCP port: `4444`
- Bind TCP port: `5555`

## Objectives

- Verify the Metasploit Framework installation.
- Generate staged and stageless Meterpreter reverse TCP payloads.
- Configure and run `exploit/multi/handler`.
- Demonstrate a local reverse TCP connection.
- Generate and execute a local bind TCP Meterpreter payload.
- Establish a Meterpreter session through the bind listener.
- Perform limited, read-only session enumeration.

## Lab Results

### Reverse TCP

Two Linux x64 Meterpreter reverse TCP payload variants were generated:

- Staged: `linux/x64/meterpreter/reverse_tcp`
- Stageless: `linux/x64/meterpreter_reverse_tcp`

The local handler on `127.0.0.1:4444` successfully received a Meterpreter stage and opened a session for both tests. In both cases, the session subsequently terminated with the framework reporting `Reason: Died`.

Therefore, this lab records **successful reverse TCP connection/session creation**, but does not claim a stable reverse Meterpreter session.

### Bind TCP

A Linux x64 `meterpreter/bind_tcp` payload was generated with the local bind endpoint `127.0.0.1:5555`.

The bind payload remained running locally and the Metasploit handler successfully connected to it. A Meterpreter session was established and used for read-only enumeration.

Evidence included:

- `getuid` — identified the local session user as `t3tra`.
- `sysinfo` — reported the Kali Linux system and x64 architecture.
- `pwd` — confirmed the session working directory.
- `ls` — enumerated the lab directory contents.
- `getpid` — returned the payload process PID.
- `ps` — displayed the local process list.

No destructive actions, persistence, credential collection, or external targeting were performed.

## Evidence

| Evidence | Description |
|---|---|
| [01](screenshots/01-metasploit-console.png) | Metasploit console and version |
| [02](screenshots/02-payload-generation.png) | Reverse TCP payload generation |
| [03](screenshots/03-handler-configuration.png) | Reverse handler configuration |
| [04](screenshots/04-reverse-handler-running.png) | Reverse handler running |
| [05](screenshots/05-stageless-handler.png) | Stageless reverse handler configuration |
| [06](screenshots/06-bind-payload-generation.png) | Bind TCP payload generation |
| [07](screenshots/07-bind-handler-configuration.png) | Bind TCP handler configuration |
| [08](screenshots/08-bind-meterpreter-session.png) | Established bind Meterpreter session |
| [09](screenshots/09-meterpreter-post-exploitation.png) | Read-only Meterpreter enumeration |
| [10](screenshots/10-meterpreter-session-summary.png) | Final session summary |

## Repository Structure

```text
.
├── .gitignore
├── README.md
├── docs/
│   └── methodology.md
├── reports/
│   └── Metasploit_Bind_Reverse_Shell_Report.md
├── payloads/
│   └── *.elf                 # ignored; generated locally
└── screenshots/
    ├── 01-metasploit-console.png
    ├── 02-payload-generation.png
    ├── 03-handler-configuration.png
    ├── 04-reverse-handler-running.png
    ├── 05-stageless-handler.png
    ├── 06-bind-payload-generation.png
    ├── 07-bind-handler-configuration.png
    ├── 08-bind-meterpreter-session.png
    ├── 09-meterpreter-post-exploitation.png
    └── 10-meterpreter-session-summary.png
```

Generated ELF payloads are intentionally excluded from version control through `.gitignore`.

## Methodology

See [docs/methodology.md](docs/methodology.md) for the lab procedure and evidence mapping.

## Report

See [reports/Metasploit_Bind_Reverse_Shell_Report.md](reports/Metasploit_Bind_Reverse_Shell_Report.md) for the formal lab report.

## Safety

This repository documents a local, isolated training exercise. The techniques demonstrated are dual-use and should only be performed against systems for which explicit authorization has been obtained.
