### 四、完整执行示例（以 Conda 环境为例）
#### 1. 编写 Dockerfile（保存到 `my-docker-project/Dockerfile`）
```dockerfile
FROM continuumio/miniconda3:latest
WORKDIR /app
# 创建 Conda 环境
RUN conda create -n myenv python=3.8 -y
# 激活环境
RUN echo "conda activate myenv" >> ~/.bashrc
# 保持容器运行
CMD ["tail", "-f", "/dev/null"]
```

#### 2. 构建镜像
```bash
cd my-docker-project
docker build -t conda-myenv:3.8 .
```
构建过程中会输出每一步的日志（如 `Step 1/5 : FROM ...`），无报错则构建成功。

#### 3. 运行容器并验证
```bash
# 创建容器
docker run -d --name conda-container conda-myenv:3.8
# 进入容器验证环境
docker exec -it conda-container bash
# 容器内执行
conda info --envs  # 能看到 myenv 环境
```