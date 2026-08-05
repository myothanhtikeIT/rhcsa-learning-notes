# What's Target/Targets
- A target is a grouping mechanism. Instead of forcing you to start 50 individual services one by one, a target tells systemd: "We want to reach this specific operating state, so load everything needed to get us here."
A plain text file. Zero executable code inside it,  no ExecStart= line the way a .service unit has. Just a list of dependencies. A checkpoint, not a task.
## What They Actually Do
- Think of targets like modes on a smartphone:
- Airplane Mode: Loads low-level OS tasks, disables wireless hardware.
- Safe Mode: Loads basic operating system features only.
- Normal Mode: Loads display drivers, Wi-Fi, background apps, and everything else.
When Linux OS boots, it aims for a single default target. That main target then pulls in all the services linked to it.
For example:  
- graphical.target depends on multi-user.target. 
- multi-user.target depends on network.target. 
- network.target depends on basic hardware setup. 
- Systemd resolves this whole chain automatically.

# How Many Targets are There?
A standard Linux installation comes with dozens of targets behind the scenes (often 40 to 100+). Most are tiny background milestones like bluetooth.target or sound.target. 
However, for most troubleshoting knowledge, only need to care about the main boot targets:
- graphical.target: Full GUI desktop environment with networking and user logins.
- multi-user.target: Text-only command line with networking (standard for servers).
- rescue.target: Minimal single-user repair mode with essential filesystems mounted (networking disabled).
- emergency.target: Bare-minimum root shell used when the system fails to boot cleanly.
- reboot.target: Orchestrates the shutdown and restart process.
- poweroff.target: Orchestrates complete system shutdown.

# What's systemd
