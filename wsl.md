https://learn.microsoft.com/en-us/windows/wsl/install

# History
## 2023-04-07
Successfully installed WSL by running `wsl --install` on my personal Windows Surface Laptop 4.

Was prompted as below
```
Please create a default UNIX user account. The username does not need to match your Windows username.
For more information visit: https://aka.ms/wslusers
Enter new UNIX username:
```

After entering username and password, below was displayed
```
passwd: password updated successfully
Installation successful!
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

Welcome to Ubuntu 22.04.1 LTS (GNU/Linux 5.15.90.1-microsoft-standard-WSL2 x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage
 ```

Running `wsl -l -v` in the Command Prompt resulted in the below output
```
  NAME      STATE           VERSION
* Ubuntu    Stopped         2
```

# Q&A
What do I do if I forget my Unix user name and password for my WSL?

