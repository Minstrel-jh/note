#### PowerShell
```
Install-Module <module name>
    -Scope CurrentUser
Get-Module
Get-InstalledModule                         Gets a list of modules on the computer that were installed by PowerShellGet. 目录 C:\Users\当前用户\文档\WindowsPowerShell\Modules
```

#### Winscp
```
winscp /console /command "option batch continue" "option confirm off" "open ftp://minstrel:pwd@10.228.19.77" "option transfer binary" "put apiConnect.jar /home/minstrel/" "exit" /log=log.txt
```

#### CRLF->LF
```
find . -type f | xargs dos2unix
```