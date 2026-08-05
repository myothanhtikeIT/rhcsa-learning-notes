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
## useful hands on commands
```systemctl list-units --type=target```
to list all targets currently active on the machine

---
# What's systemd
## systemd (The Manager)

When a Linux machine turns on, the Linux kernel boots up and spawns a single master process with Process ID 1 (PID 1). 
That process is systemd.
It acts as the general manager of the entire operating system. 
handles:
- Starting background services in parallel during boot so startup is fast
- Monitoring processes and restarting them automatically if they crash
- Managing network connections, mounting drive filesystems, and handling targets
- Windows world equivalent: Fusing the Windows Service Control Manager (services.msc), Task Scheduler, and Device Manager into one engine.
---
# what is systemctl
when systemd runs quietly in the background, the admin cannot interact with it directly. admin would need a command line utility to send instructions to it.
that particular utility is known as systemctl (short for system control). 
Whenever you want to start, stop, or check on a background program, you use systemctl.
Windows equivalent would be typing ```net start or opening services.msc``` to click Start, Stop, or Restart on a service.
### Common hands on commands
```systemctl status ssh```
 check if the SSH service is currently running.
```systemctl start ssh```
 turn on the SSH service immediately.
```systemctl stop ssh```
 turn off the SSH service immediately.
```systemctl enable ssh```
 set SSH to launch automatically every time the system boots (like Windows Startup apps).
```systemctl disable ssh```
 stop SSH from automatically launching on boot.
 
 ---

