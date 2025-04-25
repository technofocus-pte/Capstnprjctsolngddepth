# Next-Gen Fleet Optimization with Azure AI and Machine Learning

**Overview**

In this challenge, you’ll work with a Flask-based conversational
application deployed as an Azure Web App. The solution uses **Azure
OpenAI for LLM-powered insights**, **Azure AI Search for semantic
document retrieval**, and a **PostgreSQL database** to simulate live
vehicle telemetry. Embeddings from PostgreSQL data are generated via
Azure OpenAI and pushed to Azure Search for real-time vector-based
search.

If a vehicle fault requires manual escalation, the app triggers a
**Logic App workflow**, notifying a human engineer via email. This
project demonstrates how **AI-powered insights**, **vector search**, and
**human-in-the-loop workflows** can be combined to optimize fleet
performance and uptime.

**Objective**

By the end of this hands-on lab, you will:

- Deploy an AI-powered fleet assistant using **Flask**, **Azure
  OpenAI**, and **Azure Search**

- Generate and **push vector embeddings** from PostgreSQL to Azure
  Search

- Create a **chat-based interface** to diagnose and predict vehicle
  issues

- Use **Logic Apps** to send escalation alerts to human engineers

**Introduction**

In this challenge, you’ll interact with a fleet chatbot that helps
vehicle operators diagnose real-time issues based on AI-driven insights.
Behind the scenes, the app uses:

- **Azure OpenAI embeddings** for search relevance

- **Azure AI Search** for vector-based retrieval of maintenance logs and
  guidance

- **PostgreSQL** to simulate telemetry and maintenance flags

- **Logic Apps** to escalate urgent repair needs to support personnel

**Prerequisites**

Participants should have:

- Familiarity with **Python, Flask**, and basic REST APIs

- Basic knowledge of **Azure services**: Azure OpenAI, Azure Search,
  PostgreSQL, Logic Apps

- Azure Subscription with OpenAI enabled

- Access to the following tools:

  - Python 3.10+

  - Azure CLI or Azure Developer CLI (azd)

  - pgAdmin / Azure Data Studio (for PostgreSQL)

  - Git

## Component Overview

[TABLE]

Scenario :

**Contoso Transport Inc., a leading logistics provider, has launched an
AI-powered fleet maintenance assistant to reduce downtime and improve
operational efficiency.**

Drivers interact with the **"Contoso SmartFleet Chatbot"**, a web app
built using **Flask and Azure App Service**. When a vehicle issue or
query is entered, the message is processed using **Azure OpenAI** for
context understanding.

The system performs **vector-based semantic search** on vehicle
maintenance documents stored in **Azure AI Search**. These embeddings
were generated using Azure OpenAI and indexed alongside metadata
exported from a **PostgreSQL vehicle telemetry database**.

All real-time interactions and responses are managed through a
**FastAPI-based backend**, and system logs or diagnostic decisions are
optionally stored in **Cosmos DB** for traceability. If a driver’s
vehicle needs urgent service, a **Logic App** is triggered to notify the
nearest service center and escalate it to a human maintenance engineer.

This smart architecture ensures proactive alerts, real-time support, and
an intelligent fusion of **AI insights + human judgment**, resulting in
improved fleet uptime, lower operational costs, and happier drivers.

Bottom of Form

## Exercise 1 : Deploy the resources in Azure 

You will deploy Azure Machine learning workspace, create Azure machine
learning compute instance , Azure machine learning compute cluster,
Azure data factory

### Task 1 : Set up environment.

1.  Open a browser and go to **https:\\portal.azure.com** and sign in
    with your Azure subscription account.

2.  Click on **Cloud Shell** on top navigation menu and select **Bash.**

![](./media/image1.png)

3.  Select **No storage account required** and select your **Azure
    subscription** and then click on **Apply** button.

![](./media/image2.png)

4.  **Run below command to clone the repo**

**git clone
<https://github.com/ManjulaChintharla/vehicle-maintaince-prediction-using-AML-ADF-chatapp-using-Azure-OpenAI-.git>**

![A screenshot of a computer screen AI-generated content may be
incorrect.](./media/image3.png)

5.  Run below commands

> pip3 install --upgrade pip
>
> pip install azureml-sdk
>
> ![](./media/image4.png)

6.  Run below commands to navigate to the folder and provide permission
    to the file to run.

> cd
> vehicle-maintaince-prediction-using-AML-ADF-chatapp-using-Azure-OpenAI-/Scripts/
>
> chmod +x setup.sh
>
> ![A screenshot of a computer screen AI-generated content may be
> incorrect.](./media/image5.png)

7.  **Run below setup script to deploy all the required resources.Copy
    the code and clik on the link to login**

> **./** setup.sh
>
> ![](./media/image6.png)

8.  It opens new tab .Etner the code and click on NExt and sing in with
    your Azure subscription account.

![](./media/image7.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.png)

9.  Switch back Azure cloudshell tab and select your subscription.

![](./media/image9.png)

> ![](./media/image10.png)
>
> ![](./media/image10.png)
>
> ![A screenshot of a computer screen AI-generated content may be
> incorrect.](./media/image11.png)
>
> ![A screenshot of a computer screen AI-generated content may be
> incorrect.](./media/image12.png)
>
> ![A screenshot of a computer program AI-generated content may be
> incorrect.](./media/image13.png)
>
> ![A screenshot of a computer screen AI-generated content may be
> incorrect.](./media/image14.png)
>
> ![A screenshot of a computer program AI-generated content may be
> incorrect.](./media/image15.png)
>
> ![](./media/image16.png)

### Task 2 : Check deployed resources in Azure

1.  On home page of Azure portal, click on Resource group tile.

![](./media/image17.png)

2.  Click on your resource group name.

> ![](./media/image18.png)

3.  Make sure below resources are deployed.

> **Azure Machine learning workspace**
>
> **Azure Data factory**
>
> **Azure database for PostgreSQL**
>
> **Azure OpenAI**
>
> **Azure Search srvice**
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image19.png)

### Task 3 : Create a tables in Azure PostgreSQL

1.  Click on Azure PostgreSQL server name.

![](./media/image20.png)

2.  From left navigation menu, expand **Settings -\> Networking**
    .Select **Allow public access from any /azure service within Azure
    to this server** check box, click on **Add current IP address((your
    IP address)** and then click on **Save** button

![](./media/image21.png)

3.  Wait for the configuration to be successful.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image22.png)

4.  **Set vector and azure-ai extensions.**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image23.png)

5.  Wait for the deployment to complete.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image24.png)

6.  Click on **Databases** and click on **Connect** next to
    **flexibleserverdb**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image25.png)

7.  Enter the password for user citus as :
    [Fhtest208](mailto:P@55w.rd12345) and press Enter.

![](./media/image26.png)

8.  Run below command to allow access.

> GRANT CREATE ON DATABASE flexibleserverdb TO citus;

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image27.png)

9.  Run the below command to create a table.

> CREATE TABLE contosofleet1 (
>
> RecordID serial PRIMARY KEY,
>
> VehicleID INTEGER,
>
> MeasurementTimestamp TIMESTAMP,
>
> FleetID INTEGER,
>
> TruckID INTEGER,
>
> Region INTEGER,
>
> Mileage INTEGER,
>
> EngineHealth float,
>
> SensorData float,
>
> VehicleSpeedSensor INTEGER,
>
> Vibration DOUBLE PRECISION,
>
> EngineLoad float,
>
> EngineCoolantTemp INTEGER,
>
> IntakeManifoldPressure INTEGER,
>
> EngineRPM INTEGER,
>
> SpeedOBD INTEGER,
>
> IntakeAirTemp INTEGER,
>
> MassAirFlowRate float,
>
> ThrottlePosManifold float,
>
> VoltageControlModule float,
>
> AmbientAirTemp INTEGER,
>
> AccelPedalPosD float,
>
> EngineOilTemp INTEGER,
>
> SpeedGPS INTEGER,
>
> GPSLongitude float,
>
> GPSLatitude float,
>
> GPSBearing float,
>
> GPSAltitude INTEGER,
>
> TurboBoostAndVcmGauge float,
>
> TripDistance float,
>
> LitresPer100kmInst float,
>
> AccelSsorTotal float,
>
> CO2InGPerKmInst float,
>
> TripTimeJourney INTEGER,
>
> MaintenanceFlag INTEGER
>
> );

![A screenshot of a computer screen AI-generated content may be
incorrect.](./media/image28.png)

10. Run below command to grant access to the user.

GRANT ALL ON TABLE contosofleet1 TO citus;

![A screenshot of a computer screen AI-generated content may be
incorrect.](./media/image29.png)

11. Create another table to export cleaned data – Fleet-Clearned_data
    table.

> CREATE TABLE fleet_cleaned_data1 (
>
> RecordID serial PRIMARY KEY,
>
> VehicleID INTEGER,
>
> Region INTEGER,
>
> Mileage INTEGER,
>
> EngineHealth float,
>
> SensorData float,
>
> VehicleSpeedSensor INTEGER,
>
> Vibration DOUBLE PRECISION,
>
> EngineLoad float,
>
> EngineCoolantTemp INTEGER,
>
> IntakeManifoldPressure INTEGER,
>
> EngineRPM INTEGER,
>
> SpeedOBD INTEGER,
>
> IntakeAirTemp INTEGER,
>
> MassAirFlowRate float,
>
> ThrottlePosManifold float,
>
> VoltageControlModule float,
>
> AmbientAirTemp INTEGER,
>
> AccelPedalPosD float,
>
> EngineOilTemp INTEGER,
>
> SpeedGPS INTEGER,
>
> GPSLongitude float,
>
> GPSLatitude float,
>
> GPSBearing float,
>
> GPSAltitude INTEGER,
>
> TurboBoostAndVcmGauge float,
>
> TripDistance float,
>
> LitresPer100kmInst float,
>
> AccelSsorTotal float,
>
> CO2InGPerKmInst float,
>
> TripTimeJourney INTEGER,
>
> MaintenanceFlag INTEGER
>
> );

![](./media/image30.png)

12. Run below command to grant access to the user.

GRANT ALL ON TABLE fleet_cleaned_data1 TO citus;

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image31.png)

13. **Install vecotor and azure_ai extensions**

**CREATE EXTENSION azure_ai;**

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image32.png)

**8. Create a Table to Store Documents and Embeddings**

You'll need a PostgreSQL table with a column for document text and
another for embeddings.

CREATE TABLE maintenance_documents (

id SERIAL PRIMARY KEY,

filename TEXT NOT NULL,

content TEXT NOT NULL,

embeddings vector(1536) -- Adjust dimensions based on the embedding
model

);

);

GRANT ALL ON TABLE maintenance_documents TO citus;

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image33.png)

10\. Run below command to create **maintenance_tips** table

CREATE TABLE IF NOT EXISTS maintenance_tips (

id SERIAL PRIMARY KEY,

maintenanceflag INT NOT NULL,

tip TEXT NOT NULL,

cost_estimation TEXT NOT NULL,

tip_embedding VECTOR(1536)

);

GRANT ALL ON TABLE maintenance_tips TO citus;

![A computer screen with white text AI-generated content may be
incorrect.](./media/image34.png)

1.  Run the following SQL query to insert sample maintenance tips .Run
    below query to Insert sample maintenance tips

INSERT INTO maintenance_tips (maintenanceflag, tip, cost_estimation)
VALUES

(0, 'No immediate maintenance required. Regular check-ups recommended.',
'$0 - Preventive Check-up'),

(1, 'Minor maintenance required. Inspect engine and cooling system.',
'$50 - $150'),

(2, 'Moderate maintenance required. Check engine health and fluid
levels.', '$150 - $300'),

(3, 'Severe maintenance required. Engine repair and component
replacement needed.', '$500 - $1000'),

(4, 'Critical maintenance required! Immediate attention needed to avoid
breakdown.', '$1000+ - Major Overhaul');

![A screen shot of a computer code AI-generated content may be
incorrect.](./media/image35.png)

2.  Run below code to Create predictions table

CREATE TABLE predictions (

id SERIAL PRIMARY KEY,

vehicleid INT UNIQUE NOT NULL,

enginehealth FLOAT NOT NULL,

vehiclespeedsensor INT NOT NULL,

enginecoolanttemp INT NOT NULL,

enginerpm INT NOT NULL,

massairflowrate FLOAT NOT NULL,

speedgps INT NOT NULL,

litresper100kminst FLOAT NOT NULL,

co2ingperkminst FLOAT NOT NULL,

triptimejourney INT NOT NULL,

maintenanceflag INT NOT NULL,

prediction_time TIMESTAMP DEFAULT CURRENT_TIMESTAMP

);

![A screenshot of a computer screen AI-generated content may be
incorrect.](./media/image36.png)

3.  Run below command to grant permissions

GRANT ALL ON TABLE predictions TO citus;

![A screenshot of a computer screen AI-generated content may be
incorrect.](./media/image37.png)

# Exercise 2 : Use ADF to clean the data and load it into PostgreSQL.

## Task 1 : Create a Azure Postgresql conneciton

1.  Switch back to Azure portal home page- \> Resource group -\> Click
    on Azure data factory resource.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image38.png)

2.  Click on **Launch studio** button.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image39.png)

3.  Click on **Manage- \>Linked service -\> New** .Search for
    **postgresql** and select Azure Database for PostgreSQL and then
    click on **Connect**.

![](./media/image40.png)

4.  Enter below values and then click on Test connection.

> **Name**: **PostgreSQLLinkedService**.
>
> **Server Name**: Select your PostgreSQL server name (e.g.,
> \<yourserver\>.postgres.database.azure.com).
>
> Version : 1.0
>
> **Database Name**: **flexibleserverdb**.
>
> **Authentication**: Provide the username and password.(**citus** –
> Fhtest208
>
> **Encryption Method** : **SSL**
>
> Click **Test Connection** to verify.
>
> ![](./media/image41.png)

5.  Click on **Create** now.

![](./media/image42.png)

## Task 2 : Create a Pipeline and Dataflow to Transfer Data

6.  In Azure Data Factory, go to the **Author** tab (pencil icon on the
    left).Click **Pipelin-\> New Pipeline**

![](./media/image43.png)

7.  Name it **IngestBlobToPostgreSQL**.

![](./media/image44.png)

8.  Click **Move and transform →** Drag **Copy data** action to the
    canvas 

![](./media/image45.png)

9.  Enter **Copy data name**: **blobtopostgresql .**

![](./media/image46.png)

10. Click on **Source** tab and select **New** next to **Source
    dataset** drop down.

![](./media/image47.png)

11. Search for **blob**, select **Azure blob storage** and click on
    **Continue**.

![](./media/image48.png)

12. Select **Delimited Text** format and click **Continue**.

![](./media/image49.png)

13. Enter the name as **blobfleetdata**, select your blob linked service
    and then click drop down next to **File path** and select **From
    root.**

![](./media/image50.png)

14. Double click on **fleetdata** folder and select **fleet_data.csv**
    file and then click on **Ok**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image51.png)

![](./media/image52.png)

15. Click on **Ok**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image53.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image54.png)

16. Click on **Sink** tab and then click on **New** next to **Sink
    dataset** drop down.

![](./media/image55.png)

17. Search for **postgresql** ,select **Azure Database for PostgreSQL**
    and click **Continue**.

![](./media/image56.png)

18. Set below properties and then click on OK.

> Name : **AzurePostgreSqlcontosofleettable**
>
> Linked Service : **PostgreSQLLinkedService**
>
> Table name : Search for **public.contosofleet** and select it.
>
> ![](./media/image57.png)

19. Click on **Mapping** tab.

![](./media/image58.png)

20. Click on **Import schema**

![](./media/image59.png)![A screenshot of a computer AI-generated
content may be incorrect.](./media/image60.png)

21. Add Data flow to the Copy data action. Select **Data Flow**
    ![](./media/image61.png)

22. Name the data flow - Cleandataindataflow

![](./media/image62.png)

23. Click on **Settings -\> New** next to **Data flow**.

![](./media/image63.png)

## Task 3 : Create Dataflow.

1.  You are navigate to new Data flow window. Name the data flow as **–
    Cleandatainpostgresqldataflow** and then select Add source action.

![](./media/image64.png)

24. Name the output stem – **datacleanpostsql** and select
    **postgresql** dataset

![](./media/image65.png)

25. Turn on **Data flow debug** and click Ok.

![](./media/image66.png)

26. Select source action and click on + and click on **Select** action.

![](./media/image67.png)

27. Select the box and enter **Output steam name** as - **dropcolumns**.

![](./media/image68.png)

28. Scroll down and **select** **MeasurementTimestamp , fleeted,
    truckid** column and and then click on **Delete**.

![](./media/image69.png)

29. Click on + next to the tile and select **Derived Column** option
    from the list.

![](./media/image70.png)

30. Select the Derived Column option and enter Output steam name as –
    **repalcenullvalues** and click on **Open expression builder** link

![](./media/image71.png)

31. Create/Enter column names and their **Expression** as per the below
    table

[TABLE]

![](./media/image72.png)

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image73.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image74.png)

32. then click on **save and finish** button

> ![](./media/image75.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image76.png)

33. Select **repalcenullvalue** action and add **Filter** under Row
    modifier section.

![](./media/image77.png)

34. Enter the output steam name - filterrpm and the expression -
    enginerpm\>=500&&enginerpm\<=6000

> ![](./media/image78.png)

35. Add another filter map

> ![](./media/image79.png)

36. Name the filter – **filtervehiclesensor** and add below expression

vehiclespeedsensor \>= 0 && vehiclespeedsensor \<=200

![](./media/image80.png)

37. Add **Sink** to the **filtervehiclesensor** map**.**

![](./media/image81.png)

38. Name -**tofleetcleanedtable** , add the description - export cleaned
    data to fleet_cleaned_data table and click on **New** next to
    **Dataset** dropdown

![](./media/image82.png)

39. Search for postgresql and select **Azure Database for PsotgreSQL**
    tile and then click on **Continue**.

![](./media/image83.png)

40. Enter below values and click on **OK**.

> Name : **cleandatatoPostgreSqlTable**
>
> Linked Service **: PostgreSQLLinkedService**
>
> Table name : **public.fleet_cleaned_data**

![](./media/image84.png)

41. Click on **Validate all** and close the notification.

![](./media/image85.png)

## Task 4 : Test the Pipeline

1.  Switch back to pipeline and click on **Validate** all to validate
    the pipeline. Close the notification .

![](./media/image86.png)

2.  Click on **Debug** and wait until its success.

![](./media/image87.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image88.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image89.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image90.png)

3.  Go the Data flow and select Sink ,Click on **Data Preview -\>
    Refresh.** Data gets displayed.

![](./media/image91.png)

4.  Go back pipeline and click on **Publish all**

![](./media/image92.png)

5.  Click on **Publish** button.

![](./media/image93.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image94.png)

# Exercise 3: Set Up Your Azure Machine Learning Environment

## Task 1 : Connect to AML Studio and run Notebook

1.  Switch back to Azure portal -\> Resource group and select your Azure
    machine learning resource.

![](./media/image95.png)

2.  On Overview page, click on **Launch studio.**

![](./media/image96.png)

3.  Select the compute instance and click on Applications-\>
    **Notebook**.

![](./media/image97.png)

1.  Close the what’s new in Notebook window.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image98.png)

2.  Click on **Terminal** and run the below command to clone the repo.

> git clone
> <https://github.com/ManjulaChintharla/azure-e2e-ai-fleet-maintenance-platform.git>
>
> ![](./media/image99.png)

3.  Click on Refresh to load the repo.

![](./media/image100.png)

4.  Expand the **repo -\> notebooks** and select **fleet_aml_training**
    notebook. click on **Authenticate** .

![](./media/image101.png)

5.  It opens a new tab to sign in with your Azure subscription
    account.Sign in with your Azure subscription account.Select **Python
    3.8- AzureML** kernel

> ![](./media/image102.png)

6.  Open new terminal .Run below command and enter **y** when asked to
    proced.

> conda update -n base -c conda-forge conda
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image103.png)
>
> ![](./media/image104.png)

7.  Run requirements.txt cell to install all dependencies.

![](./media/image105.png)

8.  Update .env cell with your Azure resource details and run it

> ![](./media/image106.png)

9.  Run the cell to connect to AML Workspace cell.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image107.png)

10. Run the cell to connect to PostgreSQL and import cleaned data.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image108.png)

11. Run the cell to plot the Maintenance flag distribution.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image109.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image110.png)

12. Run the script to create environment cell and run it.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image111.png)

13. Run View registered environments cell .

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image112.png)

14. Run the cell to create training script.

15. Refresh the root folder and you should see training script file got
    created.

![](./media/image113.png)

16. Run the cell to run an experiment on remote compute

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image114.png)

17. Run the cell to get the cluster.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image115.png)

18. Run the train cell to train the model.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image116.png)

19. Run Get logged metrics code cell.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image117.png)

20. Run the register model code cell to register the model.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image118.png)

21. Now Deploy the model as a web service .Run the cell.

> ![](./media/image119.png)

22. Create fleet_service folder to save scoring script.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image120.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image121.png)

23. Run the cell to load the input data, get the model from the
    workspace, and generate and return predictions. We'll save this code
    in an entry script .

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image121.png)

24. Run the deployment code to deploy the model.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image122.png)

25. Runn the cell to retrieve endpoint.Save the endpoint to a notepad

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image123.png)

26. Run the code to retrieve endpoint with authe key

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image124.png)

27. Eun the code to Enable authentication

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image125.png)

28. Run the code cell to get the auth key and endpoint values.

![](./media/image126.png)

29. Update endpoint and key values and run the cell to test predictions.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image127.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image128.png)

# Exercise 4: Create and export embeddings 

## Task 1 : Configure settins in Azure AI search service

1.  Click on Resource group name and then select your Search service.

2.  Select Identity under Settings , ON the status and save it.

![](./media/image129.png)

3.  Select **Yes**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image130.png)

4.  Select Keys form left navigation bar and select both option. Wait
    for the change to be successful.

![](./media/image131.png)

### Task 2 : Create index

1.  **Click on index form left navigation menu**

![](./media/image132.png)

[TABLE]

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image133.png)

![](./media/image134.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image135.png)

![](./media/image136.png)

![](./media/image137.png)

1.  Click **“+ Add profile”**

    - Name: my-vector-profile

    - Algorithm config: my-vector-config

![](./media/image138.png)

![](./media/image139.png)

![](./media/image140.png)

Once all fields and settings are filled in, click **"Create"**.

![](./media/image141.png)

## Task 3 : Deploy chat completion model.

![](./media/image142.png)

![](./media/image143.png)

![](./media/image144.png)

![](./media/image145.png)

![](./media/image146.png)

Select **gpt-35-turbo-16k and confirm**

![](./media/image147.png)

![](./media/image148.png)

![](./media/image149.png)

## Task 4 : Export embeddings to Azure AI search.

1.  Open export_fleet_embeddings notebook and run cells.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image150.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image151.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image152.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image153.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image154.png)

![](./media/image155.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image156.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image157.png)

![](./media/image158.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image159.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image160.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image161.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image162.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image163.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image164.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image165.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image166.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image167.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image168.png)

# Exercise 5 : Build and deploy Azure AI fleet guard chatapp

### Task 1s : Deploy the app:

1.  Install Azure Tool kit in visual studio

&nbsp;

1.  Clone the repo

2.  Run below dependencis in terminal

pip install openai==0.28

python.exe -m pip install --upgrade pip

pip install -r requirements.txt

python -m pip install --user -r requirements.txt

pip install openai\[datalib\]

![A computer screen with text AI-generated content may be
incorrect.](./media/image169.png)

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image170.png)

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image171.png)

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image172.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image173.png)

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image174.png)

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image175.png)

### Task 2 : Deploy the app:

2.  Install Azure Tool kit in visual studio

3.  Sign in with your Azure subscirotion

4.  Create a web app

> ![](./media/image176.png)

5.  Enter the name and select Python 10.0 as environment

6.  Once app service got created , right click on the folder and select
    Deploy

7.  ![](./media/image177.png)

8.  Select the app created and deploy

9.  Wait for the deployment to complete.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image178.png)

### Task 3 : Access the app from Azure.

1.  Open Azure portal, search App service and open it.

> ![](./media/image179.png)

2.  Select your app name

> ![](./media/image180.png)

3.  Select Environment variables and click on Add

![](./media/image181.png)

4.  Add below variables with their values

> ![](./media/image182.png)

5.  Add FLASK_APP and value as app.py

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image183.png)

6.  Add WEBSITES_PORT and value as 8000

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image184.png)

7.  Click on Configuration and add startup command as - python -m flask
    run --host=0.0.0.0 --port=8000

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image185.png)

8.  Add Microsoft as identity provider

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image186.png)

9.  On identity

10. ![A screenshot of a chat AI-generated content may be
    incorrect.](./media/image187.png)

11. Enable public network access

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image188.png)

12. Click on /Overview and select Default domain link.

> ![](./media/image189.png)

13. ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image190.png)

14. ![A screenshot of a chat AI-generated content may be
    incorrect.](./media/image191.png)

15. ![A screenshot of a chat AI-generated content may be
    incorrect.](./media/image192.png)

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image193.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image194.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image195.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image196.png)

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image174.png)
