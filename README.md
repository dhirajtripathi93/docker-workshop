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


Make sure to use the network command (--network=host ) to ensure the taxi_ingest docker and PostgreSQL docker communicate:

 $ docker run --rm -it \
  --network=host \
  taxi_ingest:v001 \
  --pg-user=root \
  --pg-pass=root \
  --pg-host=localhost \
  --pg-port=5432 \
  --pg-db=ny_taxi \
  --target-table=yellow_taxi_trips \
  --year=2021 \
  --month=1


Alternatively you can use network = pgnetwork in both containers postgreSQL and taxi_ingest. The idea is for them to be on the shared network to communicate.

If the Pg admin tool does not work in windows try changing the hostname to : 172.17.0.1 instead of pgdatabase in the pgadmin tool 



