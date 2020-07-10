[win]: /note/win/README.md
[url:scoop]: https://scoop.sh/

# Scoop

[返回][win]

> [Scoop官网][url:scoop]

## 安装

确认PowerShell 5以上

```bat
Invoke-Expression (New-Object System.Net.WebClient).DownloadString('https://get.scoop.sh')

# or shorter
iwr -useb get.scoop.sh | iex
```

## 使用

- 下载安装

    默认的安装目录是 C:\Users\当前用户\scoop\apps

    ```bat
    scoop install <name>
    ```
