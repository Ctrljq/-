从截图中的错误来看，`cuda_runtime_api.h` 文件找不到。这通常与 CUDA 安装或环境配置有关。以下是详细的排查步骤：

---

### **1. 检查 CUDA 安装**
确保 CUDA 已正确安装，并且包含 `cuda_runtime_api.h` 文件：

运行以下命令检查：
```bash
find /usr/local/cuda/ -name "cuda_runtime_api.h"
```

- 如果找到了文件，路径通常是 `/usr/local/cuda/include/cuda_runtime_api.h`。
- 如果找不到，请重新安装 CUDA Toolkit（确保版本与 PyTorch 和驱动程序兼容）。

---

### **2. 配置 CUDA 环境变量**
如果文件存在，但编译器无法找到，可能是环境变量未正确配置。添加以下路径到环境变量中：

编辑 `~/.bashrc`：
```bash
nano ~/.bashrc
```

添加以下内容：
```bash
export PATH=/usr/local/cuda/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH
export CPLUS_INCLUDE_PATH=/usr/local/cuda/include:$CPLUS_INCLUDE_PATH
```

保存并应用更改：
```bash
source ~/.bashrc
```

---

### **3. 检查 `gcc` 和 CUDA 版本兼容性**
确认你的 `gcc` 和 CUDA 版本匹配：

| **CUDA 版本** | **支持的 GCC 版本** |
|---------------|---------------------|
| CUDA 11.x     | GCC 7.x, 8.x, 9.x  |
| CUDA 10.x     | GCC 5.x, 6.x, 7.x  |

运行以下命令查看当前 `gcc` 版本：
```bash
gcc --version
```

如果不兼容，可以安装正确的版本。例如，安装 `gcc-7`：
```bash
sudo apt install gcc-7 g++-7
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-7 1
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-7 1
sudo update-alternatives --config gcc
sudo update-alternatives --config g++
```

---

### **4. 检查 Python 环境中的依赖**
确保 PyTorch、CUDA 等版本兼容。查看当前 PyTorch 是否正确安装并支持 CUDA：
```bash
python -c "import torch; print(torch.cuda.is_available())"
```

如果返回 `False`，说明 CUDA 未正确配置或 PyTorch 安装有问题。尝试安装支持 CUDA 的 PyTorch 版本：
```bash
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu113
```
（请根据你的 CUDA 版本调整 `cu113` 部分。）

---

### **5. 单独安装 `spconv`**
`spconv` 是 mmdetection3d 的核心依赖，其编译可能导致问题。尝试以下方式：

#### 使用 pip 安装：
```bash
pip install spconv-cu113
```
（根据你的 CUDA 版本选择，例如 `cu113` 代表 CUDA 11.3。）

#### 或者从源码安装：
```bash
git clone https://github.com/traveller59/spconv.git
cd spconv
python setup.py bdist_wheel
pip install dist/*.whl
```

---

### **6. 重新运行安装**
在解决上述问题后，回到 `mmdetection3d` 目录，重新运行安装：
```bash
python setup.py install
```

---

### **总结**
如果问题依旧：
1. 请确认你的 CUDA Toolkit 安装完整。
2. 确保 `gcc` 和 CUDA 版本匹配。
3. 检查 `spconv` 的安装是否成功。

如仍有问题，请提供 `find` 命令结果和完整错误日志，我会进一步协助排查！
