从错误信息中可以看出，目前环境中出现了两个主要问题：

1. **`distutils-precedence.pth` 文件中的 `distutils_hack` 模块缺失**：
   - 这与 `setuptools` 安装相关。
2. **无法连接到 pip 的镜像源**：
   - 当前使用的镜像源（http://mirrors.blockelite.cn）无法正常访问，导致无法下载 `setuptools`。

以下是详细的解决方法：

---

### **1. 修复 `distutils-precedence.pth` 问题**
`distutils_hack` 是 `setuptools` 的一部分。这个问题通常是由于 `setuptools` 未正确安装或损坏导致。

- **删除 `distutils-precedence.pth` 文件**：
   ```bash
   rm /home/vipuser/miniconda3/envs/open-mmlab/lib/python3.8/site-packages/distutils-precedence.pth
   ```
   删除后，重新安装 `setuptools`。

---

### **2. 更换镜像源**
当前镜像源不可用，建议切换到更稳定的源（如清华源）。

- **更换 pip 的镜像源**：
   编辑或创建 `~/.pip/pip.conf` 文件，加入以下内容：
   ```ini
   [global]
   index-url = https://pypi.tuna.tsinghua.edu.cn/simple
   ```
   如果使用 Windows，则编辑 `C:\Users\<你的用户名>\pip\pip.ini`。

- **验证镜像源**：
   ```bash
   pip install --upgrade pip
   ```

---

### **3. 安装或修复 `setuptools`**
执行以下命令重新安装 `setuptools`：
```bash
pip install --force-reinstall setuptools
```

---

### **4. 使用 Conda 修复环境**
如果上述步骤仍然无效，可以直接使用 Conda 管理工具重新安装 `setuptools`：
```bash
conda install -c anaconda setuptools
```

---

### **5. 检查和验证**
完成安装后，验证 `setuptools` 是否正确安装：
```bash
python -c "import setuptools; print(setuptools.__version__)"
python -c "import pkg_resources; print(pkg_resources.__file__)"
```

---

### **6. 检查网络连接**
如果仍然无法连接镜像源，请确保网络通畅，或者尝试使用代理。

- **测试网络连接**：
   ```bash
   ping pypi.org
   ```
   如果网络正常，但仍然无法访问镜像源，可以尝试临时通过代理安装：
   ```bash
   pip install setuptools --proxy=http://<代理地址>
   ```

完成这些步骤后，再次尝试运行：
```bash
python setup.py install
```

如果问题仍未解决，请提供最新的错误信息。
