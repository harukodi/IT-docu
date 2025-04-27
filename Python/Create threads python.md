```python
import threading
def start_threads():
    threads = []
    for server in servers_data["servers"]:
        hostname = server["hostname"]
        username = server["username"]
        sudo_pass = server["sudo_pass"]
        ssh_thread = threading.Thread(target=execute_command, args=(hostname, username, args.command, sudo_pass))
        threads.append(ssh_thread)
        ssh_thread.start()
    for thread in threads:
        thread.join()
```