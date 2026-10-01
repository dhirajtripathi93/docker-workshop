# NYC Taxi Data Ingestion Pipeline

This repository contains an end-to-end, containerized data pipeline built with Docker. It automatically provisions a PostgreSQL database, a pgAdmin web interface, and a custom Python ingestion script that downloads NYC Taxi trip data from the internet and loads it into the database.

## Architecture & Components

This project is orchestrated using **Docker Compose**, which manages three distinct services:

1. **`pgdatabase` (PostgreSQL 18):** The core database where the taxi data is stored. It runs on a custom bridge network and maps to port `5432`. Data is persisted using Docker volumes so it survives container restarts.
2. **`pgadmin` (pgAdmin 4):** A web-based GUI for interacting with the PostgreSQL database. It runs on port `8085`.
3. **`taxi_ingest` (Python Ingestion Script):** A custom, lightweight Python application. It uses Pandas to download chunked `.csv.gz` files from GitHub and streams them directly into the PostgreSQL database. 

## Order of Execution (Quick Start)

You do not need to manually create or run any individual Docker containers. Docker Compose handles the entire setup in a single command. 

**1. Clone the repository:**
```bash
git clone [https://github.com/dhirajtripathi93/docker-workshop.git](https://github.com/dhirajtripathi93/docker-workshop.git)
```

**2. Navigate to the pipeline directory:**
```bash
cd docker-workshop/pipeline
```

**3. Build and run the pipeline:**
```bash
docker compose up --build
```
*(Note: Add the `-d` flag at the end if you want to run the containers in the background).*

## How it Works Under the Hood

If you are new to Docker, here is how this architecture handles images and execution without requiring complex manual setup:

* **Public vs. Custom Images:** 
  * The `pgdatabase` and `pgadmin` services rely on official, pre-compiled software. When you run the compose command, Docker automatically reaches out to Docker Hub (the global registry) and pulls these heavy images for you.
  * The `taxi_ingest` service is a custom application. Because of the `build: .` instruction in the `docker-compose.yaml`, Docker reads the local `Dockerfile` and dynamically compiles the Python environment on your machine before running it.
* **Orchestration:** The `depends_on` configuration ensures that the database is actively booting up before the ingestion script attempts to connect and write data.
* **Networking Context:** The Python script utilizes `network_mode: "host"` and connects via `localhost:5432` to bypass strict outbound DNS limitations found in certain cloud environments (like GitHub Codespaces), ensuring it can successfully download external datasets.

## Accessing Your Data

Once the `docker compose up` command finishes running the Python ingestion script, you can view your populated database:

1. Open a web browser and go to `http://localhost:8085` (or your Codespace forwarded port).
2. **Log in to pgAdmin:**
   * **Email:** `admin@admin.com`
   * **Password:** `root`
3. **Register the Server:**
   * **Name:** `Local Postgres`
   * **Host name/address:** `pgdatabase` 
   * **Port:** `5432`
   * **Maintenance database:** `ny_taxi`
   * **Username:** `root`
   * **Password:** `root`
4. **View the Tables:** Navigate in the left sidebar to `ny_taxi` -> `Schemas` -> `public` -> `Tables` to view the successfully ingested `yellow_taxi_trips` data.

### Notes:
If the pgAdmin tool does not work in Windows or GitHub Codespaces, try changing the Host name/address to **`172.17.0.1`** instead of `pgdatabase` when registering the server.
