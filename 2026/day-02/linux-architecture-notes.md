Day 02 – Linux Architecture, Processes & systemd

Goal: Understand how Linux works under the hood to troubleshoot servers confidently as a DevOps engineer.

1️⃣ Core Components of Linux
🔹 Kernel

Heart of the OS

Manages CPU, memory, disk, network

Handles system calls from applications

Runs in kernel space (direct hardware access)

🔹 User Space

Where users and applications run

Includes shell, commands, apps, libraries

Cannot directly access hardware

🔹 init / systemd

First process started by the kernel (PID 1)

Starts, stops, monitors services

Handles boot sequence and service dependencies

2️⃣ Processes in Linux
🔹 What is a Process?

A running instance of a program

Identified by PID

Created using fork() and exec()

🔹 Process States (Very Important)

Running (R)
→ Actively using CPU

Sleeping (S / D)
→ Waiting for I/O, disk, or network

Stopped (T)
→ Paused manually (Ctrl+Z or signal)

Zombie (Z)
→ Finished execution but not cleaned by parent
→ Indicates bad process handling

📌 Zombies don’t use CPU but waste PID space

3️⃣ systemd (Service Manager)
🔹 What systemd Does

Starts services at boot

Restarts failed services

Manages dependencies

Stores logs via journalctl

🔹 Common systemd Commands
systemctl status nginx
systemctl start nginx
systemctl restart nginx
systemctl enable nginx
journalctl -u nginx

4️⃣ 5 Linux Commands You’ll Use Daily

ps -ef → View running processes

top / htop → Monitor CPU & memory

systemctl → Manage services

journalctl → Check logs

kill → Stop misbehaving processes

5️⃣ Real Troubleshooting Scenarios
❌ Service Not Starting After Reboot

Steps:

systemctl status service-name

journalctl -xe

Check missing dependencies

❌ High CPU Usage

Steps:

top

Identify PID

ps -fp <PID>

kill or nice

❌ Application Crashing Repeatedly

Steps:

systemctl status app

journalctl -u app

Check config or memory limits

❌ Zombie Processes Increasing

Steps:

ps aux | grep Z

Identify parent PID