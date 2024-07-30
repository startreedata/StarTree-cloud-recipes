# StarTree Cloud Recipes

This repository contains recipes intended to be used with Startree's free-forever Pinot cluster. To get started, you will need to create a `.env` file to hold the credentials needed for secure connectivity.

## Create .env file

1. Create a `.env` file in the parent directory with the properties in this [example](#sample-env-file) below.
2. Create a token in StarTree's Cloud cluster by going to [this](https://apps.celpxu.cp.s7e.startree.cloud/pinot-api-tokens) page and saving in the `.env` file. 
3. Obtain your workspace id from the top of the StarTree's web application and set it in the `.env` file.


### Get the Broker and Controller URL

The broker hostname and port will not be readily available until you create a table. To get this, create a table using one of the sample data sources provided. Once the table is created in StarTree Cloud, a broker URL will be provided to you.

The controller url will be the same as the broker url without the `broker.` subdomain. 

### Sample .env File
```
ST_TOKEN=st-your-token
ST_WORKSPACE=ws_yourworkspace
ST_BROKER=https://broker.pinot.xxxxxxxx.startree.cloud:443
ST_CONTROLLER=https://pinot.xxxxxxxx.startree.cloud:443
```

## Recipes

| Recipe | Description |
|-----------|-----------|
| [Clickstream](./click-stream-dashboard/)   | Streamlit application that connects to Apache Pinot hosted on StarTree Cloud. The dashboard visualizes key metrics such as Event Counts by Type and Average Duration by Event Type. |

