
# Setup Linux Environment on Windows and MacOS

There are multiple ways to set up a Linux environment on Windows or Mac machines, such as cloud VMs, WSL2, VirtualBox, or Hyperkit. However, for simplicity and portability, I recommend using a container with Docker. It avoids cost, connectivity issues, and hardware overhead.

Just install Docker Desktop, create a data folder, and run the commands below to spin up a persistent Ubuntu container.

## Prerequisites
- Install `Docker Desktop` for your OS (Windows/Mac).
- Ensure virtualization is enabled in your BIOS/UEFI.
- Create a folder for persistent data: `C:\Users\YourUsername\Downloads\ubuntu-data` on Windows or `/tmp/ubuntu-data` on Mac/Linux.

## Docker Command for Windows Host (Persistent & Long-Term)
Run this in PowerShell (replace `YourUsername` with your actual username):
```bash
docker run -dit `
  --name ubuntu-container `
  --hostname ubuntu-dev `
  --restart unless-stopped `
  --cpus="2" `
  --memory="4g" `
  --mount type=bind,source="C:/Users/YourUsername/Downloads/ubuntu-data",target=/data `
  -v /var/run/docker.sock:/var/run/docker.sock `
  -p 2222:22 `
  -p 8080:80 `
  --env TZ=Asia/Kolkata `
  --env LANG=en_US.UTF-8 `
  ubuntu:latest /bin/bash
```

## Docker Command for Mac or Linux Host (Persistent & Long-Term)
Run this in your terminal:
```bash
docker run -dit \
  --name ubuntu-container \
  --hostname ubuntu-dev \
  --restart unless-stopped \
  --cpus="2" \
  --memory="4g" \
  --mount type=bind,source=/tmp/ubuntu-data,target=/data \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -p 2222:22 \
  -p 8080:80 \
  --env TZ=Asia/Kolkata \
  --env LANG=en_US.UTF-8 \
  ubuntu:latest /bin/bash
```

## Post-Setup Steps
- **Access the container**: `docker exec -it ubuntu-container /bin/bash`.
- **Install essentials**: `apt update && apt install -y openssh-server vim` (for SSH and editing).
- **Set up SSH**: Configure `/etc/ssh/sshd_config` and start the service (`service ssh start`).
- **Connect via SSH**: From host, use `ssh -p 2222 root@localhost` (default password is none; set one with `passwd`).

## Explanation of Each Parameter
| Parameter | Description |
|-----------|-------------|
| `-dit` | Runs the container in **detached (-d)**, **interactive (-i)**, and **terminal (-t)** mode. |
| `--name ubuntu-container` | Assigns a name to the container for easy management. |
| `--hostname ubuntu-dev` | Sets the container’s hostname. |
| `--restart unless-stopped` | Ensures the container restarts automatically unless manually stopped. |
| `--cpus="2"` | Limits the container to **2 CPU cores**. |
| `--memory="4g"` | Allocates **4GB RAM** to the container. |
| `--mount type=bind,source=...` | **Mounts a folder** from the host into the container to persist data. |
| `-v /var/run/docker.sock:/var/run/docker.sock` | Allows running Docker commands inside the container (optional, for advanced use). |
| `-p 2222:22` | Maps port **2222** on the host to **22** (SSH) inside the container. |
| `-p 8080:80` | Maps port **8080** on the host to **80** (for web services). |
| `--env TZ=Asia/Kolkata` | Sets the **timezone** (modify based on your location). |
| `--env LANG=en_US.UTF-8` | Sets the **language** settings inside the container. |
| `ubuntu:latest /bin/bash` | Uses the latest **Ubuntu** image and runs Bash shell. |

## Notes
- **Security**: Exposed ports (e.g., SSH) should use strong authentication. Avoid running as root in production.
- **Customization**: Adjust CPU/memory based on your hardware. For other distros, replace `ubuntu:latest` with `debian:latest` or `alpine:latest`.
- **Troubleshooting**: If ports conflict, change them (e.g., `-p 2223:22`). Check Docker logs with `docker logs ubuntu-container`. For mount errors, run PowerShell/Terminal as admin.
- **Management**: Stop the container with `docker stop ubuntu-container`; remove with `docker rm ubuntu-container`.

---

