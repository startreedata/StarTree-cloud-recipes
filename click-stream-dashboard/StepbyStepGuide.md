# Real-time Clickstream Analytics with Streamlit and Apache Pinot (StarTree Cloud - Dedicated and Serverless)

## Business Need
An e-commerce company seeks to enhance user engagement, streamline the customer journey, and ultimately drive higher conversion rates. To achieve this, they require real-time visibility into user behavior patterns and the effectiveness of their website's design and content.

## Solution
Apache Pinot, a real-time OLAP data store, will be employed to capture and analyze clickstream data. StarTree Cloud, a managed SaaS for Pinot, will ensure seamless deployment and scalability. A Streamlit app will be developed to transform this raw data into actionable insights, empowering the company to make data-driven decisions.

## Insights Provided by the Dashboard
- **Event Counts by Type**
- **Average Duration by Event Type**

## Decision-Making Impact
- **Content Optimization:** Identify underperforming pages or sections and optimize them based on user engagement metrics.
- **Conversion Funnel Enhancement:** Analyze the user flow through the purchase funnel and pinpoint areas with high abandonment rates.
- **Personalization:** Tailor website content and recommendations based on real-time user behavior patterns.
- **A/B Testing:** Evaluate the impact of website changes by comparing clickstream data before and after the modifications.
- **Marketing Campaign Effectiveness:** Assess the success of marketing campaigns by tracking the clickstream data of users who originated from those campaigns.

## Steps for Creating a Table in Interstellar

### In Scope
1. **Connection:** Securely connect to the StarTree Cloud instance using the provided credentials.
2. **Prepare Data:** Use sample data to try the recipe.
3. **Data Ingestion:** Ingest data to Pinot.
4. **Querying Pinot:** Leverage the Pinot Python SDK to execute SQL queries and extract relevant clickstream data.
5. **Visualization:** Utilize Streamlit's interactive components (charts, tables, filters) to create a dynamic dashboard.

### Sample Data
- Sample data can be found [here](data/clickstream_sample.csv)

### Create .env file`

1. Create a `.env` file with the properties above.
2. Create a token in StarTree's Interstellar cluster by going to [this](https://apps.celpxu.cp.s7e.startree.cloud/pinot-api-tokens) page and saving in the `.env` file. 
3. Obtain your workspace id from the top of the Interstellar web application and set it in the `.env` file.
4. Run the following command below to create a schema and table in Interstellar called `clickstream`.

```bash
make table
```

### Get the Broker and Controller URL

Once the table is created in Interstellar, a broker URL will be provided to you. Click on `clickstream` in the Datasets page and copy the broker URL at the center of the page.

The controller url will be the same as the broker url without the `broker.` subdomain. 

#### For example:
```
ST_TOKEN=st-your-token
ST_WORKSPACE=ws_yourworkspace
ST_BROKER=https://broker.pinot.xxxxxxxx.startree.cloud:443
ST_CONTROLLER=https://pinot.xxxxxxxx.startree.cloud:443
```

### Upload Data
Run the command below. This will execute a ingestion job that will create segments from the CSV file described above and upload them into Interstellar. You can see the [job specification](config/ingestionJobSpec.yaml) for more details.

```bash
make upload
```

## Steps to Install Streamlit on Laptop

The Streamlit application will leverage the `.env` file you created to access authorization information.

1. Create a Python environment by running these commands.

```bash
python -m venv .venv
source .venv/bin/activate
```

2. Install the Python modules needed to run the Streamlit app.

```bash
pip install -r requirements.txt
```

3. Run the Streamlit app.

The Streamlit app should automatically open a tab in your browser to http://localhost:8501/.

