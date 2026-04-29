1. List all running processes

ps aux | head -10

USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.0  1.4  22164 13520 ?        Ss   17:18   0:01 /sbin/init
root           2  0.0  0.0      0     0 ?        S    17:18   0:00 [kthreadd]
root           3  0.0  0.0      0     0 ?        S    17:18   0:00 [pool_workqueue_release]
root           4  0.0  0.0      0     0 ?        I<   17:18   0:00 [kworker/R-rcu_gp]
root           5  0.0  0.0      0     0 ?        I<   17:18   0:00 [kworker/R-sync_wq]
root           6  0.0  0.0      0     0 ?        I<   17:18   0:00 [kworker/R-kvfree_rcu_reclaim]
root           7  0.0  0.0      0     0 ?        I<   17:18   0:00 [kworker/R-slub_flushwq]
root           8  0.0  0.0      0     0 ?        I<   17:18   0:00 [kworker/R-netns]
root          11  0.0  0.0      0     0 ?        I<   17:18   0:00 [kworker/0:0H-events_highpri]

System processes and user processes are running.
Most of the listed processes are system-level (kernel threads) such as `kthreadd` and `kworker`.  
The process with PID 1 (`/sbin/init`) is the parent process that initializes the system.  
No heavy user-level applications are visible in the top 10 processes is consuming some memory.

ps aux --sort=-%mem | head -5

This shows top memory-consuming processess.
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         875  0.0  8.2 1908928 76684 ?       Ssl  17:18   0:00 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
root         607  0.0  5.1 1794232 47616 ?       Ssl  17:18   0:03 /usr/bin/containerd
root        2144  1.8  4.6 617504 43776 ?        Ssl  18:12   0:02 /usr/libexec/fwupd/fwupd
root         546  0.0  4.1 1850396 39056 ?       Ssl  17:18   0:01 /snap/snapd/current/usr/lib/snapd/snapd

Key findings from your output:
dockerd (PID 875) → using ~8.3% memory
containerd (PID 607) → ~5.4%
fwupd (PID 2144) → ~4.6%
snapd (PID 546) → ~4.1

sudo systemctl status nginx
● nginx.service - A high performance web server and a reverse proxy server
     Loaded: loaded (/usr/lib/systemd/system/nginx.service; enabled; preset: enabled)
     Active: active (running) since Wed 2026-04-29 17:18:31 UTC; 1h 0min ago
       Docs: man:nginx(8)
    Process: 539 ExecStartPre=/usr/sbin/nginx -t -q -g daemon on; master_process on; (code=exited, status=0/SUCCESS)
    Process: 558 ExecStart=/usr/sbin/nginx -g daemon on; master_process on; (code=exited, status=0/SUCCESS)
   Main PID: 581 (nginx)
      Tasks: 3 (limit: 1013)
     Memory: 3.8M (peak: 4.3M)
        CPU: 37ms
     CGroup: /system.slice/nginx.service
             ├─581 "nginx: master process /usr/sbin/nginx -g daemon on; master_process on;"
             ├─582 "nginx: worker process"
             └─598 "nginx: worker process"

Apr 29 17:18:30 ip-172-31-32-201 systemd[1]: Starting nginx.service - A high performance web server and a reverse proxy server...

Key points from your output:
Status: active (running)
Uptime: running for ~1 hour
Main PID: 581
Processes: 1 master + 2 worker processes
Memory usage: ~3.8 MB (very low)
Startup logs: service started successfully (no errors)
**Observation:**
The nginx service is active and running without any errors.  
It has been running continuously for over an hour, indicating stability.  
The service is operating with one master process and multiple worker processes, which is typical for handling concurrent requests efficiently.  
Memory usage is low (~3.8 MB), showing that nginx is lightweight and not consuming excessive resources.

journalctl -u docker | tail -10

Apr 29 17:18:33 ip-172-31-32-201 dockerd[875]: time="2026-04-29T17:18:33.554902661Z" level=warning msg="failed to determine if container is already mounted" container=a02c2519970f3033fb3c5db36fb5d8618764cfb96000696185456690575439c1
Apr 29 17:18:33 ip-172-31-32-201 dockerd[875]: time="2026-04-29T17:18:33.639808378Z" level=info msg="Deleting nftables IPv4 rules" error="exit status 1"
Apr 29 17:18:33 ip-172-31-32-201 dockerd[875]: time="2026-04-29T17:18:33.649005707Z" level=info msg="Deleting nftables IPv6 rules" error="exit status 1"
Apr 29 17:18:34 ip-172-31-32-201 dockerd[875]: time="2026-04-29T17:18:34.247505620Z" level=info msg="Loading containers: done."
Apr 29 17:18:34 ip-172-31-32-201 dockerd[875]: time="2026-04-29T17:18:34.271573783Z" level=info msg="Docker daemon" commit="29.1.3-0ubuntu3~24.04.1" containerd-snapshotter=true storage-driver=overlayfs version=29.1.3
Apr 29 17:18:34 ip-172-31-32-201 dockerd[875]: time="2026-04-29T17:18:34.274719460Z" level=info msg="Initializing buildkit"
Apr 29 17:18:34 ip-172-31-32-201 dockerd[875]: time="2026-04-29T17:18:34.331138828Z" level=info msg="Completed buildkit initialization"
Apr 29 17:18:34 ip-172-31-32-201 dockerd[875]: time="2026-04-29T17:18:34.352892276Z" level=info msg="Daemon has completed initialization"
Apr 29 17:18:34 ip-172-31-32-201 dockerd[875]: time="2026-04-29T17:18:34.352958521Z" level=info msg="API listen on /run/docker.sock"
Apr 29 17:18:34 ip-172-31-32-201 systemd[1]: Started docker.service - Docker Application Container Engine.

This shows the latest docker service logs during startup.
**Observation:**
The Docker service logs show that the daemon initialized successfully and is running properly.  
There are some warnings and minor errors related to container mounting and nftables cleanup, but these are not critical.  
The logs confirm that Docker is active and listening on `/run/docker.sock`, meaning it is ready to handle container operations.


