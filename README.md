# Use fork.dev and git on WSL with HTTPS repo


### First step, using GCM with git on WSL ([source](https://github.com/git-ecosystem/git-credential-manager/blob/main/docs/wsl.md#configuring-wsl-without-git-for-windows))
- So in WSL execute

```git config --global credential.helper "/mnt/c/Users/<USER>/AppData/Local/Fork/gitInstance/<VERSION>/mingw64/bin/git-credential-manager.exe"```
- In Windows, execute these commands:

```SETX WSLENV %WSLENV%:GIT_EXEC_PATH/wp``` (needs admin)

```C:\Users\<USER>\AppData\Local\Fork\gitInstance\<VERSION>\bin\git.exe config --global http.sslBackend openssl```



### Second step, fetch your company SSL certificates for HTTPS ([source](https://stackoverflow.com/questions/67647067/git-clone-from-gitlab-fails-on-linux-while-working-in-windows-git-bash/67647093#67647093)): 

```openssl s_client -showcerts -servername git.mycompany.com -connect 127.0.0.1:4443 </dev/null 2>/dev/null | sed -n -e '/BEGIN\ CERTIFICATE/,/END\ CERTIFICATE/ p' > git-mycompany-com.pem```


Then add it to your cert lists
- WSL:

```cat git-mycompany-com.pem | sudo tee -a /etc/ssl/certs/ca-certificates.crt```

- Windows:

```cat git-mycompany-com.pem | sudo tee -a /mnt/c/Users/<USER>/AppData/Local/Fork/gitInstance/<VERSION>/mingw64/etc/ssl/certs/ca-bundle.crt```


And there you go.
