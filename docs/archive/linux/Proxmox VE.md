# Proxmox VE

建立時間: 2026年2月3日 下午7:51
類別: Linux
最後更新時間: 2026年2月3日 下午8:43

# Nvidia GPU 直通 - 使用 GPU 顯示畫面卡住是正常的

## 1. 修改 grub

```bash
nano /etc/default/grub
```

### 原本設定

```bash
...
GRUB_CMDLINE_LINUX_DEFAULT="quiet"
...
```

### intel 需要改成

```bash
GRUB_CMDLINE_LINUX_DEFAULT="quiet intel_iommu=on"
```

### AMD 需要改成

```bash
GRUB_CMDLINE_LINUX_DEFAULT="quiet amd_iommu=on"
```

### 比較不省心的 CPU 可能要改成(這會顯示不了畫面)

```bash
GRUB_CMDLINE_LINUX_DEFAULT="quiet intel_iommu=on iommu=pt pcie_acs_override=downstream,multifunction nofb nomodeset video=vesafb:off,efifb:off"
```

### 我 GTX970

```bash
GRUB_CMDLINE_LINUX_DEFAULT="quiet intel_iommu=on iommu=pt"
```

### Ctrl + O 保存後，更新 grub

```bash
update-grub
```

## 2. 編輯 VFIO 模組

```bash
nano /etc/modules
```

### 新增

```
vfio
vfio_iommu_type1
vfio_pci
vfio_virqfd
```

## 3. 中斷 IOMMU 映射

```bash
echo "options vfio_iommu_type1 allow_unsafe_interrupts=1" > /etc/modprobe.d/iommu_unsafe_interrupts.conf
echo "options kvm ignore_msrs=1" > /etc/modprobe.d/kvm.conf
```

## 4. 把一些 Driver 拉入黑名單

```bash
echo "blacklist radeon" >> /etc/modprobe.d/blacklist.conf
echo "blacklist nouveau" >> /etc/modprobe.d/blacklist.conf
echo "blacklist nvidia" >> /etc/modprobe.d/blacklist.conf
```

## 5. 把 GPU 加入 VFIO 中

### 檢查 GPU

```bash
lspci -v
```

### 可能會看到

```
01:00.0 VGA compatible controller: NVIDIA Corporation GM204 [GeForce GTX 970] (rev a1) (prog-if 00 [VGA controller])
...

01:00.1 Audio device: NVIDIA Corporation GM204 High Definition Audio Controller (rev a1)
...
```

### 確認一下 GPU 的詳細編號

```bash
lspci -n -s 01:00
```

### 可能是

```bash
01:00.0 0300: 10de:13c2 (rev a1)
01:00.1 0403: 10de:0fbb (rev a1)
```

### 然後寫入 VFIO

```bash
echo "options vfio-pci ids=10de:13c2,10de:0fbb disable_vga=1"> /etc/modprobe.d/vfio.conf
```

## 6. 更新

```bash
update-initramfs -
```

### 重設

```bash
reset
```

# 關閉訂閱彈窗

```bash
wget https://raw.githubusercontent.com/foundObjects/pve-nag-buster/master/install.sh
chmod +x install.sh && ./install.sh
```

# ISO

## [Ubuntu 官方 ISO - 點此可見各國競像網站](https://launchpad.net/ubuntu/+cdmirrors)

### 22.04.5 Server LTS - Taiwan Digital Streaming Co.

```bash
[https://mirror.twds.com.tw/ubuntu-releases/jammy/ubuntu-22.04.5-live-server-amd64.iso](https://mirror.twds.com.tw/ubuntu-releases/jammy/ubuntu-22.04.5-live-server-amd64.iso)
```

```markdown
bfd1cee02bc4f35db939e69b934ba49a39a378797ce9aee20f6e3e3e728fefbf
```

### 24.04.3 Server LTS - Taiwan Digital Streaming Co.

```bash
[https://mirror.twds.com.tw/ubuntu-releases/noble/ubuntu-24.04.3-live-server-amd64.iso](https://mirror.twds.com.tw/ubuntu-releases/noble/ubuntu-24.04.3-live-server-amd64.iso)
```

```markdown
c3514bf0056180d09376462a7a1b4f213c1d6e8ea67fae5c25099c6fd3d8274b
```

### 24.04.3 Desktop LTS - Taiwan Digital Streaming Co.
⁨⁨

```bash
https://mirror.twds.com.tw/ubuntu-releases/noble/ubuntu-24.04.3-desktop-amd64.iso
```

```markdown
faabcf33ae53976d2b8207a001ff32f4e5daae013505ac7188c9ea63988f8328
```

## [Windows Virtio - 0.1.271](https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/archive-virtio/)

```bash
https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/archive-virtio/virtio-win-0.1.271-1/virtio-win-0.1.271.iso
```

## [IPFire 最新鏡像 (官網取載點)](https://www.ipfire.org/downloads)

# Docker

```
# 安裝必要工具
apt-get update
apt-get install -y ca-certificates curl gnupg2 lsb-release

# 建 keyrings 路徑並下載官方 GPG key
install -d /usr/share/keyrings
curl -fsSL <https://download.docker.com/linux/debian/gpg> \\
  | gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

# 修正權限，讓 apt 可以全局讀取
chmod 0755 /usr/share/keyrings
chmod a+r /usr/share/keyrings/docker-archive-keyring.gpg

# 生成並檢查 codename
CODENAME=$(lsb_release -cs)
echo "deb [arch=$(dpkg --print-architecture) \\
  signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] \\
  <https://download.docker.com/linux/debian> $CODENAME stable" \\
  > /etc/apt/sources.list.d/docker.list

# 更新並下載
apt-get update
apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# 啟用 docker daemon，並測試
systemctl enable --now docker
docker version
docker run --rm hello-world
```