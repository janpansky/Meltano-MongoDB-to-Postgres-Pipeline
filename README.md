# Meltano MongoDB to PostgreSQL Pipeline

This repository contains a Meltano project for extracting data from MongoDB and loading it into PostgreSQL.

## Getting Started

Follow these steps to set up and run the Meltano project on your local machine.

### 1. Clone the Git Repository

```sh
git clone git@github.com:janpansky/Meltano-MongoDB-to-Postgres-Pipeline.git
cd Meltano-MongoDB-to-Postgres-Pipeline
```

### 2. Set Up a Python Virtual Environment

```sh
python3 -m venv venv
source venv/bin/activate  # On Windows use `venv\Scripts\activate`
```

### 3. Install Python Dependencies

```sh
pip install -r requirements.txt
```

### 4. Initialize Meltano

```sh
meltano init
```

### 5. Add the Required Meltano Plugins

```sh
meltano add tap tap-mongodb
meltano add target target-postgres
```

### 6. Configure Meltano

Create or edit the **meltano.yml** file in the project root directory with the following content:

```
version: 1
default_environment: dev
project_id: YOUR_PROJECT_ID
environments:
- name: dev
- name: staging
- name: prod
plugins:
  extractors:
  - name: tap-mongodb
    variant: z3z1ma
    pip_url: git+https://github.com/z3z1ma/tap-mongodb.git
    config:
      mongo:
        host: localhost
        port: 27017
        username: YOUR_MONGO_USERNAME
        password: YOUR_MONGO_PASSWORD
  loaders:
    - name: target-postgres
      variant: transferwise
      pip_url: git+https://github.com/transferwise/pipelinewise-target-postgres.git
      config:
        host: localhost
        port: 5432
        user: YOUR_POSTGRES_USER
        password: YOUR_POSTGRES_PASSWORD
        dbname: postgres
        schema: public
```
Replace placeholders (YOUR_PROJECT_ID, YOUR_MONGO_USERNAME, YOUR_MONGO_PASSWORD, YOUR_POSTGRES_USER, YOUR_POSTGRES_PASSWORD) with actual values relevant to your setup.

### 7. Run a Meltano

```sh
meltano run tap-mongodb target-postgres
```