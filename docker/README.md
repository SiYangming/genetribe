# Container recipes

推荐（apt 最小化，无 miniconda）：

| 文件 | 说明 |
|------|------|
| `Dockerfile` | Debian bookworm + apt blast/bedtools + pip jcvi + tag `v1.2.1` |
| `Apptainer.def` | 同上路线的 Singularity/Apptainer 配方 |

```bash
# Docker（在仓库根）
docker build -t genetribe:1.2.1 -f docker/Dockerfile .
docker run --rm -u $(id -u):$(id -g) -v $PWD:/data -w /data genetribe:1.2.1 -h

# Apptainer
apptainer build genetribe-1.2.1.sif docker/Apptainer.def
apptainer run -B $PWD:/data -H /data genetribe-1.2.1.sif -h
```

默认 clone 本仓库 `SiYangming/genetribe` 的 `v1.2.1`。覆盖源：

```bash
docker build --build-arg GENETRIBE_REPO=https://github.com/chenym1/genetribe.git \
  -t genetribe:1.2.1 -f docker/Dockerfile .
```

历史留存（miniconda 重型配方，不推荐新构建）：

- `Dockerfile_en` / `Dockerfile_cn` — continuumio/miniconda3 + conda blast/bedtools + pip jcvi  
  曾强锁 `blast=2.9.0` 易触发 C 库冲突/OOM；已改为不锁版本。仅作国内镜像/旧流程对照。
