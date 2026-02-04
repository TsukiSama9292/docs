# SSH

建立時間: 2026年2月3日 下午7:51
類別: 工具使用
最後更新時間: 2026年2月3日 下午7:51

# 遺忘 SSH 金鑰範例

```bash
ssh-keygen -R [127.0.0.1]:20022

```

# SSH 連線範例

- `p`: 可以指定 Port

```bash
ssh user@192.168.0.100 -p 22

```

- `i`: 可以指定Private Key

```bash
ssh [username]@[IP_addr_or_Domain] -i [private_key_path]

```