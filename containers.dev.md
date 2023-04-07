# Tutorials
- https://learn.microsoft.com/en-us/training/modules/use-docker-container-dev-env-vs-code
- https://code.visualstudio.com/docs/devcontainers/tutorial

# Supporting tools and services
https://containers.dev/supporting

# About Dev Containers
- VS Code doc: https://code.visualstudio.com/docs/devcontainers/containers
- https://github.com/devcontainers/spec
- https://containers.dev/overview
- Also "Remote Development" which I believe might be an umbrella concept/technology including Dev Containers: https://code.visualstudio.com/docs/remote/remote-overview

# Sample images
- https://github.com/devcontainers/images

# Other
- https://github.com/devcontainers/spec/subscription

# "Remote-Containers: xxx" versus "Dev Containers: xxx"
- I believe it used to be "Remote-Containers: xxx" and they renamed it to "Dev Containers: xxx" around 2022/2023. 

# Q&A
Do I need to install Docker Desktop?

## History
### 2023-04-07
Installed Docker Desktop 4.18.0 on my personal Surface Laptop 4 by following the instructions below:
https://docs.docker.com/desktop/install/windows-install/

`docker version` showed the below:
```
Client:
 Cloud integration: v1.0.31
 Version:           20.10.24
 API version:       1.41
 Go version:        go1.19.7
 Git commit:        297e128
 Built:             Tue Apr  4 18:28:08 2023
 OS/Arch:           windows/amd64
 Context:           default
 Experimental:      true

Server: Docker Desktop 4.18.0 (104112)
 Engine:
  Version:          20.10.24
  API version:      1.41 (minimum version 1.12)
  Go version:       go1.19.7
  Git commit:       5d6db84
  Built:            Tue Apr  4 18:18:42 2023
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.6.18
  GitCommit:        2456e983eb9e37e47538f59ea18f2043c9a73640
 runc:
  Version:          1.1.4
  GitCommit:        v1.1.4-0-g5fd4c4d
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
```
