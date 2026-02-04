# Docker

## Ubuntu

```bash
# 更新套件列表
sudo apt-get update

# 建立 keyrings 資料夾
sudo install -d -m 0755 /etc/apt/keyrings

# 下載 Docker 官方 GPG key
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# 加入 Docker 的 apt repository
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] \
https://download.docker.com/linux/ubuntu \
$(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# 更新套件列表
sudo apt-get update

# 安裝 Docker
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# 將目前使用者加入 docker 群組（重啟後生效）
sudo usermod -aG docker $USER

# 重新啟動系統
sudo reboot
```

## Debian

```bash
# Add Docker's official GPG key:
sudo mkdir -p /etc/apt/keyrings
sudo apt update
sudo apt install -y ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
# Add the repository to Apt sources:
sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/debian
Suites: $(. /etc/os-release && echo "$VERSION_CODENAME")
Components: stable
Signed-By: /etc/apt/keyrings/docker.asc
EOF
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
sudo usermod -aG docker $USER
sudo reboot
```

## Alpine

1. Docker 本體
    
    ```bash
    sudo apk update
    # 安裝 Docker 與 openrc（若你用 OpenRC 作為 service 管理器）
    sudo apk add docker openrc
    sudo adduser username docker
    # 啟用 Docker 在開機時啟動
    sudo rc-update add docker default
    # 啟動 Docker daemon
    sudo service docker start
    ```
    
2. Docker Compose
    
    ```bash
    # 從 GitHub 下載二進制執行檔 (建議去改成最新釋出的版本)
    sudo wget -O /usr/local/bin/docker-compose https://github.com/docker/compose/releases/download/v2.39.4/docker-compose-linux-x86_64
    
    # 賦予執行權限
    sudo chmod +x /usr/local/bin/docker-compose
    
    # (可選) 連接檔案，用於讓指令也能用 docker compose，讓 CI/CD Pipeline 更好寫/讀
    mkdir -p ~/.docker/cli-plugins
    ln -s /usr/local/bin/docker-compose ~/.docker/cli-plugins/docker-compose
    ```
    

# Nvidia GPU Driver

## Ubuntu

```bash
sudo ubuntu-drivers install
```

## Debian

1. 更新系統並安裝 Kernel headers
    
    ```bash
    	sudo apt update
    	sudo apt upgrade -y
    	sudo apt install -y linux-headers-$(uname -r) build-essential
    ```
    
2. 啟用非自由套件來源 (應該是要改 第 3 和 4 行，添加 ***non-free contrib*** )
    
    ```bash
    sudo nano /etc/apt/sources.list
    ```
    
    ```bash
    deb http://deb.debian.org/debian trixie main non-free-firmware non-free contrib
    
    deb http://deb.debian.org/debian trixie-updates main non-free-firmware non-free contrib
    ```
    
3. 更新套件清單
    
    ```bash
    sudo apt update
    
    # 可查詢有哪些驅動
    # sudo apt search nvidia-driver
    ```
    
4. Turing 架構 (含) 以後的 GPU ( RTX20 系列之後)，安裝以下套件
    
    ```bash
    sudo apt install nvidia-open-kernel-dkms nvidia-driver
    ```
    
5. 其餘舊卡，安裝以下套件
    
    ```bash
    sudo apt install nvidia-kernel-dkms nvidia-driver
    ```
    

# [Nvidia Container CUDA Toolkie](https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&Distribution=Ubuntu&target_version=22.04&target_type=deb_network)

## Ubuntu 22.04 LTS

1. 安裝
    
    ```bash
    wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/x86_64/cuda-keyring_1.1-1_all.deb
    sudo dpkg -i cuda-keyring_1.1-1_all.deb
    sudo apt-get update
    sudo apt-get -y install cuda-toolkit-13-1
    sudo reboot
    ```
    
2. [DEBUG] 當遇到 CUDA-toolkit 指令失效，設定執行路徑 (13.1 為例)
    
    ```bash
    echo 'export PATH=/usr/local/cuda-13.1/bin:$PATH' >> ~/.bashrc
    echo 'export LD_LIBRARY_PATH=/usr/local/cuda-13.1/lib64:$LD_LIBRARY_PATH' >> ~/.bashrc
    source ~/.bashrc
    ```
    

## Ubuntu 24.04 LTS

1. 安裝
    
    ```bash
    wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2404/x86_64/cuda-keyring_1.1-1_all.deb
    sudo dpkg -i cuda-keyring_1.1-1_all.deb
    sudo apt-get update
    sudo apt-get -y install cuda-toolkit-13-1 nvidia-docker2
    sudo reboot
    ```
    
2. [DEBUG] 當遇到 CUDA-toolkit 指令失效，設定執行路徑 (13.1 為例)
    
    ```bash
    echo 'export PATH=/usr/local/cuda-13.1/bin:$PATH' >> ~/.bashrc
    echo 'export LD_LIBRARY_PATH=/usr/local/cuda-13.1/lib64:$LD_LIBRARY_PATH' >> ~/.bashrc
    source ~/.bashrc
    ```
    

## Debian 13

```bash
wget https://developer.download.nvidia.com/compute/cuda/repos/debian13/x86_64/cuda-keyring_1.1-1_all.deb
sudo dpkg -i cuda-keyring_1.1-1_all.deb
sudo apt-get update
sudo apt-get -y install cuda-toolkit-13-1

sudo mkdir -p /usr/share/keyrings
curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey \
  | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg

curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list \
  | sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' \
  | sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list

sudo apt update
sudo apt install -y nvidia-container-toolkit

sudo reboot
```

### 測試

```bash
# 驗證 cuda-toolkit
$ /usr/local/cuda-13.1/bin/nvcc --version

nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2025 NVIDIA Corporation
Built on Tue_Dec_16_07:23:41_PM_PST_2025
Cuda compilation tools, release 13.1, V13.1.115
Build cuda_13.1.r13.1/compiler.37061995_0

# 驗證容器 cuda-toolkit
$ sudo docker run --rm --gpus all nvidia/cuda:13.0.1-base-ubuntu22.04 nvidia-smi
Mon Feb  2 15:04:43 2026
+-----------------------------------------------------------------------------------------+
| NVIDIA-SMI 550.163.01             Driver Version: 550.163.01     CUDA Version: 13.0     |
|-----------------------------------------+------------------------+----------------------+
| GPU  Name                 Persistence-M | Bus-Id          Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |           Memory-Usage | GPU-Util  Compute M. |
|                                         |                        |               MIG M. |
|=========================================+========================+======================|
|   0  NVIDIA GeForce GTX 1650        On  |   00000000:01:00.0 Off |                  N/A |
| N/A   36C    P8              2W /   50W |       6MiB /   4096MiB |      0%      Default |
|                                         |                        |                  N/A |
+-----------------------------------------+------------------------+----------------------+

+-----------------------------------------------------------------------------------------+
| Processes:                                                                              |
|  GPU   GI   CI        PID   Type   Process name                              GPU Memory |
|        ID   ID                                                               Usage      |
|=========================================================================================|
+-----------------------------------------------------------------------------------------+
```

## Docker Container 時間與主機同步範例

```yaml
services:
  ubuntu:
    image: ubuntu:latest
    tty: true
    stdin_open: true
    volumes:
      - /etc/localtime:/etc/localtime:ro
      - /etc/timezone:/etc/timezone:ro
```