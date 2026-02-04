# Debian CPU 微碼問題

建立時間: 2026年2月3日 下午7:51
類別: Linux
最後更新時間: 2026年2月3日 下午7:51

# 修改庫源

```bash
nano /etc/apt/sources.list
```

## 新增庫源 ( ***contrib non-free-firmware*** )

```bash
deb http://deb.debian.org/debian bookworm main contrib non-free-firmware
deb http://deb.debian.org/debian bookworm-updates main contrib non-free-firmware
deb http://deb.debian.org/debian-security bookworm-security main contrib non-free-firmware
```

## Intel 安裝微碼

```bash
apt install intel-microcode
```

## AMD 安裝微碼

```bash
apt install amd64-microcode
```