# Big Data Specialization

Hands-on exercises, datasets, notebooks, configuration files, and project
artifacts for the [Coursera Big Data Specialization](https://www.coursera.org/specializations/bigdata).

The repository follows the main topics in the specialization:

1. Introduction to Big Data
2. Big Data Modeling and Management Systems
3. Big Data Integration and Processing
4. Machine Learning with Big Data
5. Graph Analytics for Big Data
6. Capstone project

The materials are a learning workspace rather than a single application. Some
examples were created for older course virtual machines and library versions,
so individual exercises may need small compatibility adjustments on a modern
system.

## Repository map

### [1-intro-to-big-data](./1-intro-to-big-data/)

Introductory Hadoop material, including:

- `hadoop/alice.txt` and `hadoop/words.txt`, sample text inputs
- `hadoop/docker-compose.yaml`, a small HDFS/YARN cluster definition
- `hadoop/config` and `hadoop/mapred-site.xml`, Hadoop configuration

The Compose file starts a NameNode, DataNode, ResourceManager, and NodeManager.
The NameNode UI is exposed on port `9870`; the ResourceManager UI is exposed on
port `8088`.

### [2-big-data-modeling-and-management-systems](./2-big-data-modeling-and-management-systems/)

Examples of common data representations and management techniques:

- `csv/`, census data
- `graph/`, graph-shaped disease data
- `image/`, image metadata and pixel-processing examples
- `json/`, JSON schema and tweet-processing examples
- `sensor/`, sensor-streaming and plotting examples
- `vector/`, Lucene query and TF-IDF shell scripts
- `requirements.txt`, pinned Python dependencies for the examples

The checked-in `big-data-2-env` directory is a local Python virtual
environment. It is not required if you create a new environment from
`requirements.txt`.

### [3-big-data-integration-and-processing](./3-big-data-integration-and-processing/)

Integration examples using Splunk and PostgreSQL:

- `Splunk/census.csv`, sample data for Splunk exercises
- `postgresSQL/docker-compose.yml`, PostgreSQL and pgAdmin services
- `postgresSQL/init.sql`, table definitions and CSV imports
- `postgresSQL/data/`, the CSV files consumed by the database initialization

### [4-machine-learning-with-big-data](./4-machine-learning-with-big-data/)

Machine-learning exercises and sample weather data:

- Jupyter notebooks for exploration, missing values, classification,
  clustering, and model evaluation
- KNIME workflows in `workflows/`
- Java libraries in `lib/` used by the course tooling
- `knime/daily_weather.csv` and `knime/minute_weather.csv`

### [5-graph-analytics-for-big-data](./5-graph-analytics-for-big-data/)

Graph analytics resources:

- GraphX GraphML inputs and generated visualizations in `graphx/`
- Neo4j CSV datasets and supplementary query/reference PDFs in `neo4j/`

### [big-data-capstone](./big-data-capstone/)

Capstone datasets, Spark clustering code, a Neo4j import script, and a
notebook. The capstone includes compressed datasets, so extract the required
archive before running an exercise that expects its files on disk.

## Prerequisites

Install only the tools needed for the section you are studying:

- Git
- Docker Desktop with Docker Compose support, for Hadoop and PostgreSQL
- Python 3.12 or a compatible version for the pinned Python dependencies
- Jupyter Notebook or JupyterLab, for the notebooks
- Java and Apache Spark, for Spark examples
- KNIME Analytics Platform, for `.knwf` workflows
- Neo4j, for Cypher and graph-database exercises
- Splunk, for the Splunk examples

The repository includes course-era JAR files and datasets, but it does not
install system-level applications such as Docker, Spark, KNIME, Neo4j, or
Splunk.

## Python setup

Create a fresh environment for the Python examples instead of relying on the
machine-specific `big-data-2-env` directory:

```powershell
cd "2-big-data-modeling-and-management-systems"
py -3.12 -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

For notebooks, start Jupyter from the repository root or from the directory
containing the notebook:

```powershell
jupyter notebook
```

Examples that open local files accept the input path as a command-line
argument. For example:

```powershell
python .\2-big-data-modeling-and-management-systems\image\dimensions.py `
  .\2-big-data-modeling-and-management-systems\image\Australia.jpg

python .\2-big-data-modeling-and-management-systems\json\json_schema.py `
  .\2-big-data-modeling-and-management-systems\json\twitter.json
```

The live sensor and live tweet examples depend on external services and
network access. They are not required for the local data-processing examples.

## Running the Docker examples

### Hadoop

From the Hadoop directory:

```powershell
cd "1-intro-to-big-data\hadoop"
docker compose up -d
docker compose ps
```

Open these local dashboards when the containers are running:

- NameNode: <http://localhost:9870>
- YARN ResourceManager: <http://localhost:8088>

Stop the cluster when finished:

```powershell
docker compose down
```

### PostgreSQL and pgAdmin

From the PostgreSQL directory:

```powershell
cd "3-big-data-integration-and-processing\postgresSQL"
docker compose up -d
docker compose ps
```

The Compose configuration exposes:

- PostgreSQL on `localhost:5432`
- pgAdmin on <http://localhost:8080>

The sample database is named `sampledb`, with user `admin`. The credentials in
the Compose file are intended for local coursework only; change them before
using this configuration outside a learning environment. The initialization
script imports `buy-clicks.csv`, `game-clicks.csv`, and `ad-clicks.csv` from
the mounted `data` directory.

To stop the services:

```powershell
docker compose down
```

## Spark and capstone notes

The capstone Spark script reads `ad-clicks.csv` and `buy-clicks.csv`, trains a
two-cluster K-Means model, and writes cluster centers to
`clusterCenters.txt`. The script contains course-environment paths and
assumptions; update the input locations and ensure PySpark, NumPy, and pandas
are installed before running it:

```powershell
spark-submit .\big-data-capstone\sparkMLlibClustering.py
```

The accompanying notebook documents the clustering exercise in more detail.
Use the Neo4j Cypher file with the matching capstone data after starting a
local Neo4j instance.

## Working with the data

- Keep generated files such as Spark output, extracted archives, notebook
  checkpoints, and local database volumes out of commits unless they are part
  of an exercise.
- Check each script's expected working directory and input filenames before
  running it; several course examples use relative paths.
- Do not publish API credentials or tokens. The `json/auth` path is a local
  authentication placeholder and should be treated as sensitive.
- Large archives and datasets may take time to extract and may require more
  disk space than their compressed size suggests.

## Suggested learning path

1. Start with the Hadoop configuration and sample text files.
2. Work through CSV, JSON, graph, image, and sensor examples.
3. Run the PostgreSQL integration example and inspect the imported tables.
4. Open the machine-learning notebooks in order: exploration, data cleaning,
   modeling, and evaluation.
5. Explore the GraphX and Neo4j datasets and queries.
6. Finish with the capstone notebook, Spark clustering script, and Neo4j
   import workflow.

## Disclaimer

These materials are for educational use and are not an official release of
the Coursera specialization. Course content, external services, dependency
versions, and container images may change over time.
