Categories of Payloads:
#### Singles
- Standalone payloads that perform a specific action without requiring additional components.
- Example: Adding a user to the system (`adduser`) or executing a command.
#### Stagers
- Lightweight payloads designed to establish a connection between the attacker and the target.
- Their purpose is to facilitate the delivery of a larger payload (stage) while using minimal resources.
- Example: reverse_tcp (sets up a reverse connection from the target to the attacker).
#### Stages
- Larger, feature-rich payloads sent after the stager establishes a connection.
- Stages offer advanced capabilities, such as a full interactive session or remote control.
- Example: `meterpreter` (a robust, interactive shell with advanced tools for post-exploitation).
### Reverse Shell Payloads:

The target system initiates a connection back to the attacker's machine.
Useful when the target is behind a firewall or NAT. Example:
`linux/meterpreter/reverse_tcp`