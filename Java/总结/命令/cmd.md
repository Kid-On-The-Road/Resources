# 端口号

## 查看端口号

```bash
netstat -ano|findstr 1099
```

## 查看对应应用

```bash
tasklist|findstr 3728
```

## 结束对应应用程序

```bash
taskkill /pid 20856 /f
```

# 修复系统文件

```bash
按 “Windows 徽标键+X”，启动 “Windows PowerShell（管理员）”，依次执行以下命令：
Dism /Online /Cleanup-Image /ScanHealth
Dism /Online /Cleanup-Image /CheckHealth
DISM /Online /Cleanup-image /RestoreHealth
sfc /SCANNOW
```

