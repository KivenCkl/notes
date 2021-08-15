# linux_command

## mount

在 wsl 中，挂载 win 中的磁盘，或挂载 U盘，可通过以下方式

```bash
sudo mkdir /mnt/f
sudo mount -t drvfs F: /mnt/f
```
