![meltano.png](img/meltano.png)

![mongodb.png](img/mongodb.png)

![postgresql.png](img/postgresql.png)

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
source venv/bin/activate
```

### 3. Install Python Dependencies

```sh
pip install -r requirements.txt
```

### 4. Initialize Meltano

```sh
meltano init
```

```sh
cd Meltano_project_name/
```

### 5. Add the Required Meltano Plugins

```sh
meltano add tap tap-mongodb
meltano add target target-postgres
meltano install
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
      strategy: raw
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

Choose the strategy as defined https://github.com/z3z1ma/tap-mongodb

```
Strategy 1 (strategy: raw) merely outputs data as-is with an additonalProperties: true schema. This is ideal for loading to unstructured sources such as blob storage where it may be preferable to keep documents exactly as they are.
Strategy 2 (strategy: envelope) will wrap the document in a document key and output a fixed schema. The schema will use type: object and the target should be able to handle unstructured data, ie. via a VARIANT/JSON column.
Strategy 3 (strategy: infer) infers the schema of each collection from a configurable sample size of records. This allows the tap to work with strongly typed destinations. This is an attractive option. Particularly when we don't expect the documents to vary dramatically.
```

Replace placeholders (YOUR_PROJECT_ID, YOUR_MONGO_USERNAME, YOUR_MONGO_PASSWORD, YOUR_POSTGRES_USER,
YOUR_POSTGRES_PASSWORD) with actual values relevant to your setup.

### 7. Run a Meltano

```sh
meltano run tap-mongodb target-postgres
```

## Conclusion

This setup guide provides the foundational steps for configuring and running a Meltano project to transfer data from
MongoDB to PostgreSQL. However, depending on the specific structure of your MongoDB collections and your PostgreSQL
schema, you may need to adjust the configuration settings in the **meltano.yml** file.

### Considerations for Configuration

- **MongoDB Collection Structure**: Ensure that the MongoDB collections and fields you intend to extract match the
  configurations specified. You may need to customize the `config` section under `tap-mongodb` to accurately reflect
  your collection names and data structure.

- **PostgreSQL Schema**: Verify that the PostgreSQL database schema specified in the configuration aligns with your data
  requirements. Adjust the `dbname`, `schema`, and other related settings as needed to fit your schema.

- **Authentication Details**: Double-check the MongoDB and PostgreSQL credentials and connection details. Ensure
  that `username`, `password`, `host`, and `port` are correctly set for your environment.

### Additional Information

- **Documentation**: For more detailed information on configuring Meltano plugins, refer to
  the [Meltano documentation](https://docs.meltano.com/).

- **Troubleshooting**: If you encounter issues, verify the log files located in the `.meltano/logs` directory for any
  error messages. Adjust configurations as necessary based on the logs.

- **Community Support**: For further assistance or to connect with other users, consider joining
  the [Meltano community](https://meltano.com/community) or seeking help through relevant forums.

By following these guidelines and making the necessary adjustments, you should be able to successfully run the Meltano
pipeline and achieve your data integration goals. If you have any questions or need further assistance, feel free to
open an issue in the GitHub repository or reach out to the project maintainers.

