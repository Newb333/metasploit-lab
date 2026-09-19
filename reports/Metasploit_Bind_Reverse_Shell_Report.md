# Metasploit Bind and Reverse Shell Lab Report

## Executive Summary

This controlled lab examined Metasploit payload generation, reverse TCP and bind TCP communication, Meterpreter session establishment, and limited post-exploitation enumeration.

The work was performed locally on a Kali GNU/Linux Rolling 2026.3 virtual machine. All network endpoints used the loopback address `127.0.0.1`.

The reverse TCP tests demonstrated successful communication with the local Metasploit handler and Meterpreter session creation, but both sessions terminated shortly after opening. The bind TCP test established a working Meterpreter session that remained available for read-only enumeration.

## Environment

| Item | Value |
|---|---|
| Operating system | Kali GNU/Linux Rolling 2026.3 |
| Architecture | x86-64 |
| Metasploit Framework | 6.5.0-dev |
| Lab interface | Loopback |
| Loopback address | 127.0.0.1 |
| Reverse TCP port | 4444 |
| Bind TCP port | 5555 |

## Objectives

1. Verify Metasploit Framework.
2. Generate a staged reverse TCP Meterpreter payload.
3. Generate a stageless reverse TCP Meterpreter payload.
4. Configure a multi/handler.
5. Demonstrate reverse TCP communication locally.
6. Generate a bind TCP Meterpreter payload.
7. Establish a bind Meterpreter session.
8. Perform controlled, read-only Meterpreter enumeration.

## Procedure and Results

### 1. Metasploit Verification

Metasploit Framework was available through the Kali installation and reported version `6.5.0-dev`.

Evidence: [01-metasploit-console.png](../screenshots/01-metasploit-console.png)

### 2. Staged Reverse TCP

The staged payload used:

```text
linux/x64/meterpreter/reverse_tcp
```

The callback endpoint was configured as:

```text
127.0.0.1:4444
```

Evidence: [02-payload-generation.png](../screenshots/02-payload-generation.png)

A matching multi/handler was configured and started.

Evidence:

- [03-handler-configuration.png](../screenshots/03-handler-configuration.png)
- [04-reverse-handler-running.png](../screenshots/04-reverse-handler-running.png)

The handler received the Meterpreter stage and reported a session opening. The session subsequently closed with `Reason: Died`.

### 3. Stageless Reverse TCP

A stageless payload used:

```text
linux/x64/meterpreter_reverse_tcp
```

The corresponding handler was configured for the same loopback endpoint.

Evidence: [05-stageless-handler.png](../screenshots/05-stageless-handler.png)

The local test again demonstrated reverse TCP communication and Meterpreter session creation, followed by session termination.

### 4. Bind TCP

The bind payload used:

```text
linux/x64/meterpreter/bind_tcp
```

The payload listened locally on:

```text
127.0.0.1:5555
```

Evidence: [06-bind-payload-generation.png](../screenshots/06-bind-payload-generation.png)

The multi/handler was configured with the local host as the remote endpoint:

```text
RHOST 127.0.0.1
LPORT 5555
```

Evidence: [07-bind-handler-configuration.png](../screenshots/07-bind-handler-configuration.png)

The handler successfully established a Meterpreter session.

Evidence: [08-bind-meterpreter-session.png](../screenshots/08-bind-meterpreter-session.png)

### 5. Meterpreter Enumeration

The established bind session was used for the following read-only commands:

```text
getuid
sysinfo
pwd
ls
getpid
ps
```

Observed evidence included:

- Session user: `t3tra`
- Computer name: `kali`
- Architecture: x64
- Meterpreter type: x64/linux
- Working directory: `/home/t3tra/Projects/Metasploit`
- Process identification through `getpid`
- Process enumeration through `ps`

Evidence: [09-meterpreter-post-exploitation.png](../screenshots/09-meterpreter-post-exploitation.png)

A final session summary was captured separately.

Evidence: [10-meterpreter-session-summary.png](../screenshots/10-meterpreter-session-summary.png)

## Findings

### Finding 1 — Reverse TCP connectivity demonstrated

The local reverse TCP handler successfully received the payload stage and reported Meterpreter session creation.

**Limitation:** the session terminated immediately, so the test does not demonstrate a stable reverse Meterpreter session.

### Finding 2 — Bind TCP session demonstrated

The local bind TCP payload accepted a connection from the Metasploit handler and provided an active Meterpreter session.

### Finding 3 — Read-only enumeration demonstrated

The established session permitted basic identity, system, filesystem, and process enumeration using Meterpreter's read-only commands.

## Security and Ethical Controls

- All endpoints were restricted to `127.0.0.1`.
- No external target was used.
- No persistence mechanism was configured.
- No credential dumping was performed.
- No destructive commands were executed.
- Generated executable payloads were excluded from the public repository.

## Limitations and Reproducibility Notes

The reverse TCP sessions were not stable. This is an observed lab result and should be reported as such rather than presented as a successful persistent reverse shell.

The bind TCP demonstration was local to the Kali VM and therefore does not establish behavior across a real network.

## Evidence Index

| ID | Evidence |
|---|---|
| 01 | Metasploit console |
| 02 | Reverse TCP payload generation |
| 03 | Reverse handler configuration |
| 04 | Reverse handler running |
| 05 | Stageless reverse handler |
| 06 | Bind TCP payload generation |
| 07 | Bind handler configuration |
| 08 | Bind Meterpreter session |
| 09 | Meterpreter post-exploitation enumeration |
| 10 | Final Meterpreter session summary |

## Conclusion

The lab demonstrated the requested Metasploit workflow in a controlled local environment: payload generation, multi/handler configuration, reverse TCP connection attempts, bind TCP session establishment, Meterpreter access, and basic read-only post-exploitation enumeration.

The evidence distinguishes the reverse TCP result from the bind TCP result: reverse sessions were created but terminated, while the bind TCP test provided the working Meterpreter session used for enumeration.
