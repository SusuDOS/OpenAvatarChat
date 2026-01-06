<h1 style='text-align: center; margin-bottom: 1rem'> Open Avatar Chat </h1>

<p align="center">
<strong>中文 | <a href="readme_en.md">English</a></strong>
</p>


<p align="center">
<strong>模块化的交互数字人对话实现。</strong>
</p>

## step by step install

```bash
# temp proxy for speed!
export http_proxy="http://127.0.0.1:10808"
export https_proxy="http://127.0.0.1:10808"
export all_proxy="http://127.0.0.1:10808"

export no_proxy="localhost,127.0.0.1,::1"

# get a download key for https://huggingface.co/settings/tokens
export HF_TOKEN="hf_NSLhslDYwsbfQq45htizExrjqFSkhyh"

export UV_HTTP_TIMEOUT=1800
export UV_CONNECT_TIMEOUT=120
export UV_READ_TIMEOUT=1800

# mount my backup disk_part
sudo mount /dev/nvme1n1p1 /media/gaoming/BackUpOS


# 1.===安装===
# install all,no recommend...
uv sync --all-packages

# install just for you need.

uv venv --python 3.11.11
uv pip install setuptools pip

uv run install.py --uv --config config/chat_with_openai_compatible_bailian_cosyvoice_musetalk.yaml


# 2.修复进入项目目录
cd /media/gaoming/BackUpOS/OpenAvatarChat

uv run python -m pip uninstall -y mmcv
uv pip install setuptools wheel ninja
uv pip install cmake
sudo apt-get update
sudo apt-get install -y build-essential
MMCV_WITH_OPS=1 MMCV_WITH_CUDA=1 uv pip install "mmcv==2.1.0" \
    --no-binary=mmcv \
    --force-reinstall \
    --no-cache-dir \
    --no-build-isolation

# 验证
uv run python - << 'EOF'
import mmcv
import importlib

print("mmcv version:", mmcv.__version__)
try:
    ext = importlib.import_module('mmcv._ext')
    print("mmcv._ext loaded:", ext)
except Exception as e:
    print("ERROR loading mmcv._ext:", repr(e))
EOF
    
# 3.download weight,maybe huggingface-cli need use single file to download...
bash scripts/download_musetalk_weights.sh

# 4.change a key for alibailian...project root path run it...,you can get it from alibalian...
echo "DASHSCOPE_API_KEY=sk-283e83fafdafdc4ee5a96faa93bf31">.env

# 5.run it for you.
uv run src/demo.py --config config/chat_with_openai_compatible_bailian_cosyvoice_musetalk.yaml


#########################
# do not run docker... because i don't test it.
bash build_cuda128.sh
#########################
```
