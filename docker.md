
# Q&A
## Q: Is is possible to install Docker Desktop and Docker Engine on WSL?
A: 
- I believe it might be technically possible, but not recommended.
- I tried this on 2023-04-07, but didn't finish because, it seemed like I would need to enable Systemd in WSL but it didn't seem recommended: https://askubuntu.com/questions/1379425/system-has-not-been-booted-with-systemd-as-init-system-pid-1-cant-operate

### 2023-04-07 Note
Tried installing Docker Desktop on Ubuntu on WSL instead of directly on Windows
https://docs.docker.com/desktop/install/ubuntu/

```
sudo apt install gnome-terminal
sudo apt-get update
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg
sudo mkdir -m 0755 -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
sudo docker run hello-world
```
results in
```
docker: Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?.
```

```
curl https://desktop.docker.com/linux/main/amd64/docker-desktop-4.18.0-amd64.deb --output docker-desktop-4.18.0-amd64.deb
sudo apt-get install ./docker-desktop-4.18.0-amd64.deb
systemctl --user start docker-desktop
```
resulted in
```
Failed to connect to bus: No such file or directory
```
