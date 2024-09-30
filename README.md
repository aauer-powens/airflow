# Airflow

## Installation
Follow this [documentation](https://airflow.apache.org/docs/apache-airflow/stable/howto/docker-compose/index.html)

### Detailed
Download the `docker-compose.yaml file
```sh
curl -LfO 'https://airflow.apache.org/docs/apache-airflow/2.10.0/docker-compose.yaml'
```

Then create the desired output folder
```sh
mkdir -p ./dags ./logs ./plugins ./config
echo -e "AIRFLOW_UID=$(id -u)" > .env
```

Initialize the database
```sh
docker compose up airflow-init
```
Which should produce this output 
```sh
airflow-init-1  | Database migrating done!
```

Finally run all the containers
```sh
docker compose up
```

The account created has the login `airflow` and the password `airflow` and you can now connect to [localhost:8080](http://localhost:8080).

NB: You can have some limitations while deploying Airflow with your current Operating System. Please refer to the documentation to avoid this limitations on Linux or MacOS.
It could be achieved by running the Docker Desktop application on Linux or by increasing the memory allocated to Docker (4Gb to 8Gb)

### Python
:warning: Version requirements:

- `AIRFLOW_VERSION` - Airflow version (e.g. 2.10.0) or main, 2-0, for latest development version
- `PYTHON_VERSION` Python version e.g. 3.8, 3.9

#### An alternative installation with `conda`

Download Anaconda distribution from [here](https://www.anaconda.com/download/success).
After installation, proceeded as follows

```sh
conda create --name airflow python=3.9
conda activate airflow

conda install git pkg-config
pip install "apache-airflow[celery]==2.10.0" --constraint "https://raw.githubusercontent.com/apache/airflow/constraints-2.10.0/constraints-3.9.txt"
pip install apache-airflow-providers-amazon[s3fs] duckdb pandas


```
Or install from requirements_conda.txt: 
```sh
conda env create --file my_conda_env.ymli
```
You should restart the shell for the changes take into effect.

#### With venv

Basic installation with `pip install apache-airflow` will not work as desired.
To achieve this, please run this command or refer to the [documentation](https://airflow.apache.org/docs/apache-airflow/stable/installation/installing-from-pypi.html)
```sh
python -m venv .venv
source activate .venv
pip install "apache-airflow[celery]==2.10.0" --constraint "https://raw.githubusercontent.com/apache/airflow/constraints-2.10.0/constraints-3.8.txt"
```

If you want to avoid any warning while running some scripts/tasks, please at list install
```sh
pip install virtualenv pandas apache-airflow[cncf.kubernetes]
```
### Uninstall

Cleaning-up the environment

The docker-compose environment we have prepared is a “quick-start” one. It was not designed to be used in production and it has a number of caveats - one of them being that the best way to recover from any problem is to clean it up and restart from scratch.

The best way to do this is to:
```sh
docker compose down --volumes --remove-orphans
```
Remove the entire directory where you downloaded the docker-compose.yaml file 
```sh
rm -rf '<DIRECTORY>'
```

## Tutorial
Then to make some tests with the airflow deployed, you can follow up the [Apache Airflow tutorial](https://airflow.apache.org/docs/apache-airflow/stable/tutorial/fundamentals.html#testing)

### Basic features
- Reuse already created tasks with `@task` decorator [link](https://airflow.apache.org/docs/apache-airflow/stable/tutorial/taskflow.html#reusing-a-decorated-task)
- [TaskFlow API with complex/conflicting Python dependencies](https://airflow.apache.org/docs/apache-airflow/stable/tutorial/taskflow.html#using-the-taskflow-api-with-complex-conflicting-python-dependencies)
    - [Virtualenv created dynamically for each task](https://airflow.apache.org/docs/apache-airflow/stable/tutorial/taskflow.html#virtualenv-created-dynamically-for-each-task)
    - [Using Python environment with pre-installed dependencies](https://airflow.apache.org/docs/apache-airflow/stable/tutorial/taskflow.html#using-python-environment-with-pre-installed-dependencies)
    - [Dependency separation using Docker Operator](https://airflow.apache.org/docs/apache-airflow/stable/tutorial/taskflow.html#dependency-separation-using-docker-operator) or [Kubernetes](https://airflow.apache.org/docs/apache-airflow/stable/tutorial/taskflow.html#dependency-separation-using-kubernetes-pod-operator)




