# ufw 規則參考

```bash
# 停用防火牆（非強制，但較安全）
sudo ufw disable

# 重設所有規則（包含允許/封鎖的規則、預設策略）
sudo ufw reset

# 允許所有網路出去
sudo ufw default allow outgoing
# 阻擋未授權的 IP 連線
sudo ufw default deny incoming

# 新增規則

## Cloudflare CDN IPv4 授權
for ip in $(curl -s <https://www.cloudflare.com/ips-v4>); do
    sudo ufw allow from $ip to any port 443 proto tcp
done
## Cloudflare CDN IPv6 授權
for ip in $(curl -s <https://www.cloudflare.com/ips-v6>); do
    sudo ufw allow from $ip to any port 443 proto tcp
done

## 特定 IP 開通全部服務

### Docker 網路
sudo ufw allow from 172.16.0.0/12

### 宜蘭大學校園網路
sudo ufw allow from 120.101.0.0/19
sudo ufw allow from 120.101.32.0/20

# 啟用防火牆
sudo ufw enable
```