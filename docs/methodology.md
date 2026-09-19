# Metasploit Lab Methodology

## 1. Scope and Controls

The exercise was conducted on a Kali GNU/Linux Rolling 2026.3 virtual machine.

All callback and bind traffic was restricted to the loopback interface:

```text
127.0.0.1
```

This prevented the payload demonstrations from requiring an external target.

## 2. Framework Verification

Metasploit Framework and `msfvenom` were verified as installed.

The recorded framework version was:

```text
6.5.0-dev
```

Evidence: [01-metasploit-console.png](../screenshots/01-metasploit-console.png)

## 3. Reverse TCP Payload Generation

A staged Linux x64 Meterpreter payload was generated with:

```text
linux/x64/meterpreter/reverse_tcp
```

The payload was configured to call back to:

```text
127.0.0.1:4444
```

A second, stageless payload used:

```text
linux/x64/meterpreter_reverse_tcp
```

The generated ELF files were kept local and excluded from Git version control.

Evidence:

- [02-payload-generation.png](../screenshots/02-payload-generation.png)
- [05-stageless-handler.png](../screenshots/05-stageless-handler.png)

## 4. Reverse Handler

The Metasploit multi/handler was configured for the staged reverse TCP payload with:

```text
LHOST 127.0.0.1
LPORT 4444
```

The handler was started as a background job.

Evidence:

- [03-handler-configuration.png](../screenshots/03-handler-configuration.png)
- [04-reverse-handler-running.png](../screenshots/04-reverse-handler-running.png)

## 5. Reverse Connection Result

Execution of the staged reverse payload caused the handler to receive the Meterpreter stage and report that a Meterpreter session had opened.

The session then terminated with:

```text
Reason: Died
```

The same general result occurred during the stageless reverse TCP test.

The evidence therefore supports the following statement:

> The local reverse TCP path successfully reached the Metasploit handler and created a Meterpreter session, but the session was not stable.

It would be inaccurate to describe this test as a persistent or stable reverse Meterpreter session.

## 6. Bind TCP Payload

The available Linux x64 Meterpreter bind TCP payload was selected:

```text
linux/x64/meterpreter/bind_tcp
```

It was generated with the local bind endpoint:

```text
127.0.0.1:5555
```

Evidence: [06-bind-payload-generation.png](../screenshots/06-bind-payload-generation.png)

## 7. Bind Handler

The Metasploit multi/handler was configured for the bind payload with:

```text
RHOST 127.0.0.1
LPORT 5555
```

Evidence: [07-bind-handler-configuration.png](../screenshots/07-bind-handler-configuration.png)

## 8. Meterpreter Session

The handler connected successfully to the locally running bind payload and established a Meterpreter session.

Evidence: [08-bind-meterpreter-session.png](../screenshots/08-bind-meterpreter-session.png)

The session was then used for limited, read-only verification.

## 9. Post-Exploitation Enumeration

The following Meterpreter commands were used:

```text
getuid
sysinfo
pwd
ls
getpid
ps
```

These commands were selected to demonstrate session identity, system information, working-directory access, directory enumeration, process identification, and process listing without modifying the host.

Evidence: [09-meterpreter-post-exploitation.png](../screenshots/09-meterpreter-post-exploitation.png)

A final session summary was also captured.

Evidence: [10-meterpreter-session-summary.png](../screenshots/10-meterpreter-session-summary.png)

## 10. Evidence Handling

Screenshots are stored in the repository under `screenshots/`.

The generated ELF payloads are excluded through:

```text
payloads/*.elf
```

This keeps executable payload artifacts out of the public repository while preserving the screenshots and written methodology needed to demonstrate the lab work.

## 11. Limitations

- The reverse TCP Meterpreter sessions terminated immediately after creation.
- No stable reverse Meterpreter session was demonstrated.
- The bind TCP test was performed only against the local Kali VM.
- Post-exploitation activity was intentionally limited to read-only enumeration.
- The screenshots document the observed lab results; they should not be interpreted as evidence of behavior on external systems.
