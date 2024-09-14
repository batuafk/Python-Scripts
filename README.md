# Wait for internet connection
```python
import socket
import time
def wait_internet_connection():
    print("Waiting for an internet connection...")
    while True:
        try:
            socket.create_connection(("8.8.8.8", 53), timeout=10)
            print("Internet connection established.")
            return True
        except:
            time.sleep(20)
```

# Check if proccess running
```python
import psutil
def is_process_running(process_name):
    for process in psutil.process_iter(["pid", "name"]):
        if process.info["name"] == process_name:
            return True
    return False
```

# Terminate proccess
```python
import psutil
def terminate_process(process_name):
    for process in psutil.process_iter(attrs=["pid", "name"]):
        if process.info["name"] == process_name:
            pid = process.info["pid"]
            try:
                p = psutil.Process(pid)
                p.terminate()
            except psutil.NoSuchProcess:
                pass
```

# Duration of a code
```python
def duration():
    end_time = time.time()
    elapsed_time = end_time - start_time

    hours = int(elapsed_time // 3600)
    minutes = int((elapsed_time % 3600) // 60)
    seconds = int(elapsed_time % 60)

    return f"[{hours:02d}:{minutes:02d}:{seconds:02d}]"

start_time = time.time()
print(f"[{duration()}] Hello, World!")
```

# Set console title
```python
def set_title(title):
    try:
        if os.name == "nt":
            os.system(f"title {title}")
        else:
            os.system(f'echo -ne "\\033]0;{title}\\007"')
    except Exception as e:
        print(f"Error setting title: {e}")
```

# Clear console
```python
def clear():
    os.system('cls' if os.name == 'nt' else 'clear')
```

# Fast open ports scan
```python
import threading
import socket

ip = "127.0.0.1"
timeout = 5  # 3=fast, 5=slow


def scan_port(ip, port):
    sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    sock.settimeout(timeout)

    try:
        sock.connect((ip, port))
        print(f"\033[32m[+]\033[0m {ip}:{port}")
    except socket.error:
        pass
    finally:
        sock.close()

for port in range(1, 65535):
    thread = threading.Thread(target=scan_port, args=(ip, port))
    thread.start()
```

# Tor Proxies (needs update)
```python
class tor:
    def configure():
        try:
            path = "tor\\torrc"
            ports = range(9000, 10000)

            with open(path, "w") as torrc_file:
                for port in ports:
                    line = f"SocksPort {port}\n"
                    torrc_file.write(line)
            return True
        except Exception as e:
            return e

    def start():
        if is_process_running("tor.exe"):
            terminate_process("tor.exe")
            
        if os.path.exists("tor\\torrc"):
            os.system("start tor\\tor.exe -f tor\\torrc")
        else:
            os.system("start tor\\tor.exe")

        time.sleep(5)

    def stop():
        os.system("taskkill /F /IM tor.exe")

    def enable_system_wide(port):
        os.system('reg add "HKCU\\Software\\Microsoft\\Windows\\CurrentVersion\\Internet Settings" /v ProxyEnable /t REG_DWORD /d 1 /f')
        os.system(f'reg add "HKCU\\Software\\Microsoft\\Windows\\CurrentVersion\\Internet Settings" /v ProxyServer /t REG_SZ /d "socks=127.0.0.1:{port}" /f')
        os.system('reg add "HKCU\\Software\\Microsoft\\Windows\\CurrentVersion\\Internet Settings" /v ProxyOverride /t REG_SZ /d "<local>" /f')

    def disable_system_wide():
        os.system('reg add "HKCU\\Software\\Microsoft\\Windows\\CurrentVersion\\Internet Settings" /v ProxyEnable /t REG_DWORD /d 0 /f')
```
