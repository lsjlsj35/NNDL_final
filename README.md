# gaussian-splatting使用教程
参考官方代码库使用步骤如下：
## 创建并激活新环境：
* python -m venv env/gs
* source env/gs/bin/activate
## 拉取官方项目代码并安装相关包：
* git clone https://github.com/graphdeco-inria/gaussian-splatting --recursive
* pip install plyfile tqdm opencv-python joblib
* pip install ./submodules/diff-gaussian-rasterization
* pip install ./submodules/simple-knn
* pip install ./submodules/fused-ssim
* pip install torch==2.4.0 torchvision==0.19.0 torchaudio==2.4.0 -i https://pypi.tuna.tsinghua.edu.cn/simple --extra-index-url https://download.pytorch.org/whl/cu124
## 转化数据格式：
* export QT_QPA_PLATFORM=offscreen
* python /opt/tiger/vtcl/gaussian-splatting/convert.py -s /opt/tiger/vtcl/gaussian-splatting/data --no_gpu
## 训练&评测&可视化：
* CUDA_VISIBLE_DEVICES=5 python /opt/tiger/vtcl/gaussian-splatting/train.py     -s /opt/tiger/vtcl/gaussian-splatting/data     -m /opt/tiger/vtcl/gaussian-splatting/output --test_iterations 1000 2000 5000 7000 10000 15000 20000 25000 30000
* tensorboard --logdir=/opt/tiger/vtcl/gaussian-splatting/output --bind_all
## 测试渲染&指标计算：
* python '/opt/tiger/vtcl/gaussian-splatting/render.py' -m /opt/tiger/vtcl/gaussian-splatting/output
* python '/opt/tiger/vtcl/gaussian-splatting/metrics.py' -m /opt/tiger/vtcl/gaussian-splatting/output
