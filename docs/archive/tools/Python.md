# Python

# uv (Python 套件管理工具)

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

# miniconda (Python 套件管理工具)

```bash
wget -qO /tmp/miniconda.sh https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
sudo bash /tmp/miniconda.sh -b -p /opt/conda
echo 'export PATH=/opt/conda/bin:$PATH' | sudo tee /etc/profile.d/conda.sh
sudo chmod +x /etc/profile.d/conda.sh
source /etc/profile.d/conda.sh
conda init
```