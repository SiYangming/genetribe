# Docker builds

- `Dockerfile_en` — English comments; conda-forge/bioconda deps + GeneTribe
- `Dockerfile_cn` — Chinese comments; same stack

Example:

```bash
docker build -f Dockerfile_en -t genetribe:1.2.1 .
```
