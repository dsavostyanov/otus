# ДЗ: Работа с процессами
```
ps_ax1.py:

#!/usr/bin/env python3
import os

def list_processes():
    pids = []
    states = []
    cmds = []

    for pid in os.listdir("/proc"):
        if pid.isdigit():
            pids.append(pid)
            try:
                with open(f"/proc/{pid}/stat", "r") as f:
                    stat_content = f.read().split()
                    states.append(stat_content[2])  # Process state
            except Exception:
                states.append("N/A")
            
            try:
                with open(f"/proc/{pid}/cmdline", "r") as f:
                    cmd_content = f.read().replace('\x00', ' ').strip()
                    cmds.append(cmd_content or "N/A")
            except Exception:
                cmds.append("N/A")

    return list(zip(pids, states, cmds))


# Print column headers
print(f"{'PID':<8} {'State':<6} Command")
print("-" * 50)

# Print process details in aligned columns
for pid, state, cmd in list_processes():
    print(f"{pid:<8} {state:<6} {cmd}")
```
