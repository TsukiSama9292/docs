# SSH

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