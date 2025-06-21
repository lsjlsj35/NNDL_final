# NeRF
## Colmap & NerfStudio
- Install [colmap](https://github.com/colmap/colmap)
```shell
sudo apt-get install \
    git \
    cmake \
    ninja-build \
    build-essential \
    libboost-program-options-dev \
    libboost-graph-dev \
    libboost-system-dev \
    libeigen3-dev \
    libflann-dev \
    libfreeimage-dev \
    libmetis-dev \
    libgoogle-glog-dev \
    libgtest-dev \
    libgmock-dev \
    libsqlite3-dev \
    libglew-dev \
    qtbase5-dev \
    libqt5opengl5-dev \
    libcgal-dev \
    libceres-dev
sudo apt-get install -y \
    nvidia-cuda-toolkit \
    nvidia-cuda-toolkit-gcc
git clone https://github.com/colmap/colmap.git
cd colmap
mkdir build
cd build
cmake .. -GNinja
ninja
sudo ninja install
```
- Install [Nerfstudio](https://github.com/nerfstudio-project/nerfstudio)
```shell
pip install nerfstudio
```

## Run
- Process data: `ns-process-data video --data ./video --output-dir ./nerf_video`
- Train:
  - NeRF: `CUDA_VISIBLE_DEVICES=0 ns-train vanilla-nerf --vis tensorboard --data nerf_video nerfstudio-data`
  - TensoRF: `CUDA_VISIBLE_DEVICES=1 ns-train tensorf --vis tensorboard --data nerf_video nerfstudio-data`
- Eval:
  - NeRF: `CUDA_VISIBLE_DEVICES=0 ns-render spiral --load-config outputs/nerf_video/vanilla-nerf/xxxxx/config.yml --output-path renders/output_nerf.mp4 --rendered-output-names rgb_fine`
  - TensoRF: `CUDA_VISIBLE_DEVICES=1 ns-render spiral --load-config outputs/nerf_video/tensorf/xxxxx/config.yml --output-path renders/output_tensorf.mp4`

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
