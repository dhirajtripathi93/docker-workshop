# docker-workshop
workshop codespaces

Docker volume mapping command:
 docker run -it --entrypoint=bash -v $(pwd)/test:/app/test  python:3.13.11-slim

install virtual environment in python:
pip install uv
uv init --python 3.13

List all dockers: docker ps -a

Create docker image with Postres SQL :
docker run -it --rm \
  -e POSTGRES_USER="root" \
  -e POSTGRES_PASSWORD="root" \
  -e POSTGRES_DB="ny_taxi" \
  -v ny_taxi_postgres_data:/var/lib/postgresql \
  -p 5432:5432 \
  postgres:18
