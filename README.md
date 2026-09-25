# docker-workshop
workshop codespaces

Docker volume mapping command:
 docker run -it --entrypoint=bash -v $(pwd)/test:/app/test  python:3.13.11-slim

install virtual environment in python:
pip install uv

uv init --python 3.13
