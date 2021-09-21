# File path length limits for Windows

[Microsoft Docs](https://docs.microsoft.com/en-us/windows/win32/fileio/maximum-file-path-limitation?fbclid=IwAR0r0zmRJQ7bqLkn1sPk47k-lTgwjrwnmAv_013cj8KCHeuCYCRVnrYq_Xk&tabs=cmd)

If you run into this issue with your git client try running this command in powershell as admin

```
New-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\FileSystem" `
-Name "LongPathsEnabled" -Value 1 -PropertyType DWORD -Force
```

Reboot computer.

If the problem persists it may be because the git client doesn't support this feature. Try using git-bash CLI to see if it is solved. If so, then you may need to use another git client which is 64bit or another 32bit app which supports the limit override.