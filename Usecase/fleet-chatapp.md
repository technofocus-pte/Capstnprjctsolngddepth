# Next-Gen Fleet Optimization with Azure AI and Machine Learning

**Overview**

In this challenge, you’ll work with a Flask-based conversational
application deployed as an Azure Web App. The solution uses **Azure
OpenAI for LLM-powered insights**, **Azure AI Search for semantic
document retrieval**, and a **PostgreSQL database** to simulate live
vehicle telemetry. Embeddings from PostgreSQL data are generated via
Azure OpenAI and pushed to Azure Search for real-time vector-based
search.

**Objective**

By the end of this hands-on lab, you will:

- Deploy an AI-powered fleet assistant using **Flask**, **Azure
  OpenAI**, and **Azure Search**

- Generate and **push vector embeddings** from PostgreSQL to Azure
  Search

- Create a **chat-based interface** to diagnose and predict vehicle
  issues

**Introduction**

In this challenge, you’ll interact with a fleet chatbot that helps
vehicle operators diagnose real-time issues based on AI-driven insights.
Behind the scenes, the app uses:

- **Azure OpenAI embeddings** for search relevance

- **Azure AI Search** for vector-based retrieval of maintenance logs and
  guidance

- **PostgreSQL** to simulate telemetry and maintenance flags

**Prerequisites**

Participants should have:

- Familiarity with **Python, Flask**, and basic REST APIs

- Basic knowledge of **Azure services**: Azure OpenAI, Azure Search,
  PostgreSQL

- Azure Subscription with OpenAI enabled

- Access to the following tools:

  - Python 3.10+

  - Azure CLI

  - Azure AI Foundry

## Component Overview

[TABLE]

Scenario :

**Contoso Transport Inc., a leading logistics provider, has launched an
AI-powered fleet maintenance assistant to reduce downtime and improve
operational efficiency.**

Drivers interact with the **"Contoso Fleet Guard Chatbot"**, a web app
built using **Flask and Azure App Service**. When a vehicle issue or
query is entered, the message is processed using **Azure OpenAI** for
context understanding.

The system performs **vector-based semantic search** on vehicle
maintenance documents stored in **Azure AI Search**. These embeddings
were generated using Azure OpenAI and indexed alongside metadata
exported from a **PostgreSQL vehicle telemetry database**.

# IMPORTANAT

Open a Notebook from Start menu to save all Azure resources endpoint ,kyes and connection values to use differnet tasks of the labs


## Exercise 1 : Deploy the resources in Azure 

You will deploy Azure Machine learning workspace, create Azure machine
learning compute instance , Azure machine learning compute cluster,
Azure data factory

### Task 1 : Set up environment.

1.  Open a browser and go to +++https://portal.azure.com+++ and sign in
    with your Azure subscription account.

    **Username**: +++@lab.CloudPortalCredential(User1).Username+++

    **Password**: +++@lab.CloudPortalCredential(User1).Password+++

3.  Click on **Cloud Shell** on top navigation menu and select **Bash.**

  ![](./media/image1.png)

3.  Select **No storage account required** and select your **Azure
    subscription** and then click on **Apply** button.

  ![](./media/image2.png)

4.  **Run below command to clone the repo**

  +++https://github.com/technofocus-pte/TFFleetOptmztn.git+++

  ![A screenshot of a computer AI-generated content may be incorrect.](./media/image3.png)

5.  Run below commands to navigate to the folder and provide permission
    to the file to run.

   +++cd TFFleetOptmztn/+++

   +++chmod +x setup.sh+++

   ![A screenshot of a computer screen AI-generated content may be incorrect.](./media/image4.png)

6.  Run below setup script to deploy all the required resources.for the prompt Enter the name of an existing Azure Resource Group: enter
    +++@lab.CloudResourceGroup(ResourceGroup1).Name+++. The deployment take20-30 to deploy all resources.

  +++./setup.sh+++

   ![A screenshot of a computer screen AI-generated content may be  incorrect.](./media/image5.png)

  ![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image6.png)

  ![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image7.png)

  ![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image8.png)

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image9.png)

  ![A screenshot of a computer screen AI-generated content may be
incorrect.](./media/image10.png)

  ![A screenshot of a computer screen AI-generated content may be
incorrect.](./media/image11.png)

  ![A screenshot of a computer screen AI-generated content may be
incorrect.](./media/image12.png)

  ![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image13.png)

  ![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image14.png)

  ![A screenshot of a computer screen AI-generated content may be
incorrect.](./media/image15.png)

  ![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image16.png)

### Task 2 : Check deployed resources in Azure

1.  On home page of Azure portal, click on Resource group tile.

  ![](./media/image17.png)

2.  Click on your resource group name.

  ![](./media/image18.png)

3.  Make sure below resources are deployed.

  - **Azure Machine learning workspace**
  - **Azure Data factory**
  - **Azure database for PostgreSQL**
  -  **Azure OpenAI**
  - **Azure Search service**

 ![A screenshot of a computer AI-generated content may be incorrect.](./media/image19.png)

  ![A screenshot of a computer AI-generated content may be incorrect.](./media/image20.png)

### Task 3 : Create a tables in Azure PostgreSQL

1.  Click on Azure PostgreSQL server name.

  ![](./media/image21.png)

2.  From left navigation menu, expand **Settings -> Networking**
    .Select **Allow public access from any azure service within Azure
    to this server** check box, click on **Add current IP address((your
    IP address)** and then click on **Save** button

  ![](./media/image22.png)

3.  Wait for the configuration to be successful.

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image23.png)

4.  **Click on Server parameters and seach for extensions. Select vector** and **azure-ai extensions** and then click on **Save**

  ![](./media/image24.png)

5.  Wait for the deployment to complete.

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image25.png)

6.  Click on **Go to resource.**

  ![](./media/image26.png)

7.  Click on **Connect** from left navigation menu .Select
    **flexibleserverdb** as database,expand Connect from browser or
    locallay and copy the command. Click on cloud slice icon on top
    navigation menu.Also, make a note of PG_HOST value to use in AML studio- notebook

  ![](./media/image27.png)

8.  Enter the command then enter password for user citus as :+++Fhtest208+++ and press Enter.

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image28.png)

9.  Run below command to allow access.

  +++GRANT CREATE ON DATABASE flexibleserverdb TO citus;+++

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image29.png)

10. Run the below command to create a **contosofleet** table.

  ```
  CREATE TABLE contosofleet (
  RecordID  serial PRIMARY KEY,
  VehicleID INTEGER,
       MeasurementTimestamp TIMESTAMP,
        FleetID   INTEGER,
      TruckID  INTEGER,
      Region INTEGER,
     Mileage INTEGER,
  EngineHealth float,
  SensorData float,
  VehicleSpeedSensor  INTEGER,
  Vibration DOUBLE PRECISION,    
  EngineLoad float,
  EngineCoolantTemp INTEGER,  
  IntakeManifoldPressure INTEGER,
  EngineRPM INTEGER,   
  SpeedOBD INTEGER,
  IntakeAirTemp INTEGER,     
  MassAirFlowRate float,
  ThrottlePosManifold float,
  VoltageControlModule float,
  AmbientAirTemp INTEGER,      
  AccelPedalPosD float,
  EngineOilTemp INTEGER,      
  SpeedGPS INTEGER,
  GPSLongitude float,
   GPSLatitude float,
  GPSBearing float,
  GPSAltitude INTEGER,      
  TurboBoostAndVcmGauge float,
  TripDistance float,
  LitresPer100kmInst  float,
  AccelSsorTotal float,
  CO2InGPerKmInst float,
  TripTimeJourney INTEGER,
  MaintenanceFlag INTEGER
  );
  ```

  ![A screenshot of a computer screen AI-generated content may be
incorrect.](./media/image30.png)

11. Run below command to grant access to the user.

  +++GRANT ALL ON TABLE contosofleet TO citus;+++

  ![A screenshot of a computer screen AI-generated content may be
incorrect.](./media/image31.png)

12. Create another table to export cleaned data –  **Fleet-Clearned_data** table.

  ```
  CREATE TABLE fleet_cleaned_data (
  RecordID  serial PRIMARY KEY,
  VehicleID INTEGER,
  Region INTEGER,
  Mileage INTEGER,
  EngineHealth float,
  SensorData float,
  VehicleSpeedSensor  INTEGER,
  Vibration DOUBLE PRECISION,    
  EngineLoad float,
  EngineCoolantTemp INTEGER,  
  IntakeManifoldPressure INTEGER,
  EngineRPM INTEGER,   
  SpeedOBD INTEGER,
  IntakeAirTemp INTEGER,     
  MassAirFlowRate float,
  ThrottlePosManifold float,
  VoltageControlModule float,
  AmbientAirTemp INTEGER,      
  AccelPedalPosD float,
  EngineOilTemp INTEGER,      
  SpeedGPS INTEGER,
  GPSLongitude float,
   GPSLatitude float,
  GPSBearing float,
  GPSAltitude INTEGER,      
  TurboBoostAndVcmGauge float,
  TripDistance float,
  LitresPer100kmInst  float,
  AccelSsorTotal float,
  CO2InGPerKmInst float,
  TripTimeJourney INTEGER,
  MaintenanceFlag INTEGER
  );
  ```

  ![](./media/image32.png)

13. Run below command to grant access to the user.

  +++GRANT ALL ON TABLE fleet_cleaned_data TO citus;+++

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image33.png)

14. Run below command to Install **vecotor** and **azure_ai extensions**

  +++CREATE EXTENSION azure_ai;+++

  +++CREATE EXTENSION vector;+++

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image34.png)

15. Create a **Table to Store Documents and Embeddings**

You'll need a PostgreSQL table with a column for document text and
another for embeddings.

  ```
  CREATE TABLE maintenance_documents (
      id SERIAL PRIMARY KEY,
      filename TEXT NOT NULL,
      content TEXT NOT NULL,
      embeddings vector(1536)  -- Adjust dimensions based on the embedding model
  );
  ```

  +++GRANT ALL ON TABLE maintenance_documents TO citus;+++

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image35.png)

# Exercise 2 : Use ADF to clean the data and load it into PostgreSQL.

## Task 1 : Create a Azure Postgresql conneciton

1.  Switch back to Resource group and select **Azure Datafactory**
    resource.

  ![](./media/image36.png)

2.  Click on **Launch studio** button.

  ![](./media/image37.png)

3.  Click on **Manage- >Linked service -> New** .

  ![](./media/image38.png)

4.  Search for +++postgresql+++ and select **Azure Database for PostgreSQL** and then click on **Connect**.

  ![](./media/image39.png)

5.  Enter below values and then click on Test connection.

  - Name: +++PostgreSQLLinkedService+++
  - Server Name: Select your PostgreSQL server name (e.g., <yourserver>.postgres.database.azure.com).
  - Version : **1.0**
  - Database Name: **flexibleserverdb**.
  - Authentication: Provide the username and password.(+++citus+++ – +++Fhtest208+++
  - Encryption Method : **SSL**
  - Click **Test Connection** to verify.

   ![](./media/image40.png)

   ![](./media/image41.png)

6.  Click on **Create** now.

  ![](./media/image42.png)

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image43.png)

## Task 2 : Create a Pipeline and Dataflow to Transfer Data

1.  In Azure Data Factory, go to the **Author** tab (pencil icon on the left).Click **Pipeline-> New Pipeline**

  ![](./media/image44.png)

2.  Name it +++IngestBlobToPostgreSQL+++

  ![](./media/image45.png)

3.  Click **Move and transform →** Drag **Copy data** action to the canvas 

  ![](./media/image46.png)

4. Enter **Copy data name**: +++blobtopostgresql+++

  ![](./media/image47.png)

5. Click on **Source** tab and select **New** next to **Source dataset** drop down.

  ![](./media/image48.png)

6. Search for +++blob+++, select **Azure blob storage** and click on **Continue**.

  ![](./media/image49.png)

7. Select **Delimited Text** format and click **Continue**.

![](./media/image50.png)

8. Enter the name as +++blobfleetdata+++, select your blob linked service 
    and then click drop down next to **File path** and select **From
    root.**

  ![](./media/image51.png)

9. Double click on **fleetdata** folder and select **fleet_data.csv** file and then click on **Ok**.

  ![](./media/image52.png)

  ![](./media/image53.png)

10. Click on **Ok**.

  ![](./media/image54.png)

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image55.png)

11. Click on **Sink** tab and then click on **New** next to **Sink dataset** drop down.

  ![](./media/image56.png)

12. Search for +++postgresql+++ ,select **Azure Database for PostgreSQL** and click **Continue**.

  ![](./media/image57.png)

13. Set below properties and then click on OK.

  - **Name** : +++AzurePostgreSqlcontosofleettable+++
  - **Linked Service** : **PostgreSQLLinkedService**
  - **Table name** : Search for +++public.contosofleet+++ and select it.

  ![](./media/image58.png)
  
14. Click on **Mapping** tab.

  ![](./media/image59.png)

15. Click on **Import schema**

  ![](./media/image60.png)![A screenshot of a computer AI-generated
content may be incorrect.](./media/image61.png)

16. Add Data flow to the Copy data action. Select **Data Flow**

  ![](./media/image62.png)

17. Name the data flow - +++Cleandataindataflow+++

  ![](./media/image63.png)

18. Click on **Settings -> New** next to **Data flow**.

  ![](./media/image64.png)

## Task 3 : Create Dataflow.

1.  You are navigate to new Data flow window. Name the data flow as +++Cleandatainpostgresqldataflow+++ and then select Add source action.

  ![](./media/image65.png)

2. Name the output stream – +++datacleanpostsql+++ and select    **AzurePostgreSqlcontosofleettable** dataset

  ![](./media/image66.png)

3. Turn on **Data flow debug** and click Ok.

  ![](./media/image67.png)

4. Select source action and click on + and click on **Select** action.

  ![](./media/image68.png)

5. Select the box and enter **Output steam name** as - +++dropcolumns+++

  ![](./media/image69.png)

6. Scroll down and select **MeasurementTimestamp , fleetid and  truckid** column and then click on **Delete**.

  ![](./media/image70.png)

7. Click on + next to the tile and select **Derived Column** option
    from the list.

  ![](./media/image71.png)

8. Select the Derived Column option and enter Output steam name as – +++repalcenullvalues+++ and click on **Open expression builder** link

  ![](./media/image72.png)

9. Create/Enter column names and their **Expression** as per the below table

  |Column Name|Expression|
  |--|--|
  |+++enginecoolanttemp+++|+++coalesce(toInteger(enginecoolanttemp), 25)+++|
  |+++ambientairtemp+++|+++coalesce(toInteger(ambientairtemp), 25)+++|
  |+++intakeairtemp+++|+++coalesce(toInteger(intakeairtemp), 25)+++|
  |+++enginerpm+++|+++coalesce(toInteger(enginerpm), 800)+++|
  |+++vehiclespeedsensor+++|+++coalesce(toInteger(vehiclespeedsensor), 0)+++|

  ![](./media/image73.png)

  ![A screenshot of a computer AI-generated content may be incorrect.](./media/image74.png)

  ![A screenshot of a computer AI-generated content may be incorrect.](./media/image75.png)

10. then click on **save and finish** button

  ![](./media/image76.png)

  ![A screenshot of a computer AI-generated content may be incorrect.](./media/image77.png)

11. Select **repalcenullvalue** action and add **Filter** under Row modifier section.

  ![](./media/image78.png)

12. Enter the output steam name - +++filterrpm+++ and the expression -+++enginerpm>=500&&enginerpm<=6000+++
    
   ![](./media/image79.png)

13. Add another filter map

  ![](./media/image80.png)

14. Name the filter – +++filtervehiclesensor+++ and add below expression

  +++vehiclespeedsensor >= 0 && vehiclespeedsensor <=200+++

  ![](./media/image81.png)

15. Add **Sink** to the **filtervehiclesensor** map.

  ![](./media/image82.png)

16. Name - +++tofleetcleanedtable+++* , add the description - +++export cleaned
    data to fleet_cleaned_data table+++ and click on **New** next to
    **Dataset** dropdown

  ![](./media/image83.png)

17. Search for +++postgresql+++ and select **Azure Database for PsotgreSQL**
    tile and then click on **Continue**.

  ![](./media/image84.png)

18. Enter below values and click on **OK**.

  - **Name** : +++cleandatatoPostgreSqlTable+++
  - **Linked Service** : +++PostgreSQLLinkedService+++
  - **Table name** : +++public.fleet_cleaned_data+++

  ![](./media/image85.png)

19. Click on **Validate all** and close the notification.

  ![](./media/image86.png)

## Task 4 : Test the Pipeline

1.  Switch back to pipeline and click on **Validate** all to validate
    the pipeline. Close the notification .

  ![](./media/image87.png)

2.  Click on **Debug** and wait until its success.

  ![](./media/image88.png)

  ![](./media/image89.png)

3.  Go the Data flow and select Sink ,Click on **Data Preview ->Refresh.** Data gets displayed.

  ![](./media/image90.png)

4.  Go back pipeline and click on **Publish all**

  ![](./media/image91.png)

5.  Click on **Publish** button.

  ![](./media/image92.png)

  ![A screenshot of a computer AI-generated content may be incorrect.](./media/image93.png)

# Exercise 3: Set Up Your Azure Machine Learning Environment

## Task 1 : Connect to AML Studio and run Notebook

1.  Switch back to **Azure portal -> Resource group** and select your  Azure machine learning resource.

  ![](./media/image94.png)

2.  On Overview page, click on **Launch studio.**

  ![](./media/image95.png)

3.  Click on **Compute** from left navigation menu .Select the compute instance and select on **Notebook**.

  ![](./media/image96.png)

4.  Close the what’s new in Notebook window.

  ![A screenshot of a computer AI-generated content may be incorrect.](./media/image97.png)

5.  Click on **Terminal** and run the below command to clone the repo.

  +++https://github.com/technofocus-pte/TFFleetOptmztn.git+++

 ![](./media/image98.png)

3.  Click on Refresh to load the repo.

  ![](./media/image99.png)

4.  Expand the **repo -> notebooks** and select **fleet_aml_training** notebook. click on **Authenticate** .

  ![](./media/image100.png)

5.  It opens a new tab to sign in with your Azure subscription
    account.Sign in with your Azure subscription account.Select **Python
    3.10- AzureML** kernel

   ![](./media/image101.png)

6.  Open new terminal .Run below command and enter **y** when asked to
    proced.

   +++conda update -n base -c conda-forge conda+++
 
   ![A screenshot of a computer AI-generated content may be incorrect.](./media/image102.png)

   ![](./media/image103.png)

7.  Run **requirements.txt** cell to install all dependencies.

  ![A screenshot of a computer AI-generated content may be incorrect.](./media/image104.png)

8.  Update **.env** cell with your Azure resource details and run it

  ![](./media/image105.png)

9.  Run the cell to connect to AML Workspace cell.

   ![A screenshot of a computer AI-generated content may be  incorrect.](./media/image106.png)

10. Run the cell to connect to PostgreSQL and import cleaned data.

  ![A screenshot of a computer AI-generated content may be incorrect.](./media/image107.png)

11. Run the cell to plot the Maintenance flag distribution.

  ![A screenshot of a computer AI-generated content may be incorrect.](./media/image108.png)

  ![A screenshot of a computer AI-generated content may be incorrect.](./media/image109.png)

  ![A screenshot of a computer AI-generated content may be incorrect.](./media/image110.png)

12. Run the script to create environment cell and run it.

 ![A screenshot of a computer AI-generated content may be incorrect.](./media/image111.png)

 ![A screenshot of a computer AI-generated content may be incorrect.](./media/image112.png)

13. Run View registered environments cell .

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image113.png)

14. Run the cell to create training script - **fleet_training.py**

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image114.png)

15. Refresh the root folder and you should see training script file got
    created.

  ![](./media/image115.png)

16. Run the cell to run an experiment on remote compute

17. Run the cell to get the cluster.

  ![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image116.png)

  ![A screenshot of a computer AI-generated content may be incorrect.](./media/image117.png)

18. Run the train cell to train the model.It takes 5-10 minutes to run
    the script completely.

  ![A screenshot of a computer AI-generated content may be incorrect.](./media/image118.png)

  ![A screenshot of a computer program AI-generated content may be incorrect.](./media/image119.png)

  ![A screenshot of a computer AI-generated content may be  incorrect.](./media/image120.png)

19. Run Get logged metrics code cell.

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image121.png)

20. Run the register model code cell to register the model.

  ![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image122.png)

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image123.png)

21. Run cell to Create a folder for the web service files

  ![A screenshot of a computer AI-generated content may be incorrect.](./media/image124.png)

22. Run the cell to write score_fleet.py

  ![A screenshot of a computer program AI-generated content may be incorrect.](./media/image125.png)

23. Now Deploy the model as a web service .Run the cell. Deployment takes 10-15 minutes to complete.

  ![A screenshot of a computer AI-generated content may be incorrect.](./media/image126.png)

  ![A screenshot of a computer AI-generated content may be  incorrect.](./media/image127.png)

24. Run the cell to get endpoint

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image128.png)

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image129.png)

25. Run the cell to enable authentication.

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image130.png)

26. Run the cell to get endpoint and key to use in next cells and tasks.

  ![](./media/image131.png)

27. Update endpoint and key values and run the cell to test predictions.

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image132.png)

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image133.png)

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image134.png)

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image135.png)

# Exercise 4: Create and export embeddings 

## Task 1 : Configure settings in Azure AI search service

1.  Click on Resource group name and then select your Azure Search
    service.

  ![](./media/image136.png)

2.  Select Identity under Settings , ON the status and save it.

  ![](./media/image137.png)

3.  Select **Yes**.

  ![](./media/image138.png)

4.  Select **Keys** form left navigation bar and select both option. Wait for the change to be successful.

  ![](./media/image139.png)

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image140.png)

## Task 2 : Create index

1.  Click on **Search management ->indexes** from left navigation and  select **Add index -> Add index**

  ![](./media/image141.png)

2.  Enter the index name as : +++fleet_index+++ and click on **Add field.**

  ![](./media/image142.png)

3.  **Create fields as per below table**

  |||||||||
  |**Field Name**|**Data Type**|**Searchable**|**Filterable**|**Sortable**|**Facetable**|**Retrievable**|**Dimentions**|
  |**filename**|Edm.String|yes|yes|yes|No|yes||
  |**content**|Edm.String|No|Yes|No|No|No|Yes||
  |**embedding**|Collection(Edm.Single)|Yes|No|No|No|Yes|**1536**|

  ![](./media/image143.png)

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image144.png)

4.  Enter Dimensions as +++1536+++ and then click on Create under **No vector search profile**

  ![](./media/image145.png)

5.  Click on **Create.**

  ![](./media/image146.png)

6.  Keep default values and then click on **Save** under **Vector  algorithm**

  ![](./media/image147.png)

7.  Click on **Save** again.

  ![](./media/image148.png)

8.  Click on **Save** in Index Field section.

  ![](./media/image149.png)

9.  Once all fields and settings are filled in, click **"Create"**.

  ![](./media/image150.png)

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image151.png)

## Task 3 : Deploy embedding model.

1.  Switch back to Resource Group and select **Azure OpenAI resource**.

  ![](./media/image152.png)

2.  Click on **Go to Azure AI Foundry portal**

  ![](./media/image153.png)

3.  Click on **Deployments** from left navigation menu and select **Deploy model -> Deploy base model.**

  ![](./media/image154.png)

4.  Search for +++text-embedding-ada-002+++ model and select it and then click on **Confirm** button.

  ![](./media/image155.png)

5.  Click on **Customize** button.

  ![](./media/image156.png)

6.  Keep all the default values . **Increase Tokens per Minute Rate Limit to max tokens** and then click on **Deploy**.

  ![](./media/image157.png)

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image158.png)

## Task 4 : Deploy embedding model.

1.  Click on **Deployments** from left navigation menu. Select **Deploy model -> Deploy base model.**

  ![](./media/image159.png)

2.  Search for +++gpt+++ model and select **gpt-35-turbo** ( any available chat model in resource region) and then click on **Confirm**.

  ![](./media/image160.png)

3.  Keep all the default values and then click on **Deploy** button.

  ![](./media/image161.png)

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image162.png)

## Task 5 : Export embeddings to Azure AI search.

1.  Switch back to Azure Machine learning studio and select **export_fleet_embeddings.ipynb**

  ![](./media/image163.png)

2.  Run dependencies cell to install dependencies

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image164.png)

3.  Run cell to List of .docx files in the Data folder

  ![A screenshot of a computer program AI-generated content may be incorrect.](./media/image165.png)

4.  Update the environment variables with your azure resource values and then run to write values to .env values.

  ![A screenshot of a computer AI-generated content may be incorrect.](./media/image166.png)

5.  Run cell to assign documents.

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image167.png)

6.  Run cell to Insert documents into PostgreSQL table

  ![A screenshot of a computer program AI-generated content may be incorrect.](./media/image168.png)

7.  Run cell to Fetch records from the table

  ![A screenshot of a computer program AI-generated content may be incorrect.](./media/image169.png)

  ![A screenshot of a computer AI-generated content may be incorrect.](./media/image170.png)

8.  Run cell to get embeddings.

  ![A screenshot of a computer program AI-generated content may be incorrect.](./media/image171.png)

  ![A screenshot of a computer AI-generated content may be incorrect.](./media/image172.png)

9.  Run cell to export embeddings to Azure AI Search

  ![A screenshot of a computer AI-generated content may be  incorrect.](./media/image173.png)

10. Run cell to Ru cell to create maintenance_documents.jso file

  ![A screenshot of a computer AI-generated content may be incorrect.](./media/image174.png)

11. Run cell to list indexes

  ![A screenshot of a computer AI-generated content may be incorrect.](./media/image175.png)

12. Run cell to upload documents to index **'fleet_index'**

  ![A screenshot of a computer screen AI-generated content may be  incorrect.](./media/image176.png)

  ![A screenshot of a computer AI-generated content may be  incorrect.](./media/image177.png)

13. Run cell to check the response for the prompt

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image178.png)

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image179.png)

14. Update cell with chat model name for engine and run it.

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image180.png)

15. Run last cell to check responses for the prompts.

  ![A screenshot of a computer AI-generated content may be  incorrect.](./media/image181.png)

  ![A screenshot of a computer AI-generated content may be  incorrect.](./media/image182.png)

# Exercise 5 : Build and deploy Azure AI fleet guard chatapp

### Task 1 : Build fleetguard app locally

1.  Create fleetapp folder on your vm desktop.

  ![A screenshot of a computer AI-generated content may be  incorrect.](./media/image183.png)

2.  Open visual Studio code from Dekstop. Click on **File-> Open Folder.**

  ![](./media/image184.png)

3.  Browse Desktop and select the fleetapp folder created on desktop.

  ![](./media/image185.png)

4.  Open new Terminal.

  ![](./media/image186.png)

5.  Run the command to clone the repo.

   +++git clone https://github.com/technofocus-pte/TFFleetOptmztn.git+++

  ![A screenshot of a computer AI-generated content may be  incorrect.](./media/image187.png)

6.  Expand chatapp folder and update .env file with your azure resource values.Save the file

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image188.png)

  ![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image189.png)

7.  Run below command to navigate to the folder.

  +++cd .\TFFleetOptmztn\chatapp\+++

  ![A screenshot of a computer AI-generated content may be  incorrect.](./media/image190.png)

8.  Run below dependencis in terminal

  +++pip install openai==0.28+++

  +++python.exe -m pip install --upgrade pip+++

  +++pip install -r requirements.txt+++

  +++python -m pip install --user -r requirements.txt+++

  +++pip install openai[datalib]++

  ![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image191.png)

  ![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image192.png)

9.  Run below command to run the app locally.Click on **Allow access**
    button on pop up window

   +++python app.py+++

  ![](./media/image193.png)

10. Click on localhost url (ctrl+ click)

  ![](./media/image194.png)

11. App opens in default browser.

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image195.png)

12. Enter the below prompts and click on **Send**.

  +++my vehicle - 98765 has enginehealth - 0.826120+++

  +++Predict maintenance issues for my vehicle with 80,000 km mileage+++

  +++Will my car overheat based on engine temperature 90°C?+++

  +++vehicle 24353 's enginehealth is 0.9 and vehicle sensor speed is 101+++

  +++What should I do if my engine health is below 0.5?+++

  ![A screenshot of a chat AI-generated content may be
incorrect.](./media/image196.png)

  ![A screenshot of a chat AI-generated content may be
incorrect.](./media/image197.png)

  ![A screenshot of a chat AI-generated content may be
incorrect.](./media/image198.png)

  ![A screenshot of a chat AI-generated content may be
incorrect.](./media/image199.png)

  ![A screenshot of a chat AI-generated content may be
incorrect.](./media/image200.png)

### Task 2 : Insall extension in visual Studio code.

1.  Switch back to Visual Studio code and click on **Extensions** from
    left navigation menu, search for Azure Developer CLI and install it.

  ![](./media/image201.png)

2.  Click on **Azure** icon from left navigation menu and select **Sign
    in to Azure**.

  ![](./media/image202.png)

3.  Click on **Allow**.Sign in with your assigned account.

  ![](./media/image203.png)

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image204.png)

4.  Click on **Extensions** and search for **Azure App Service** and **install** it.

  ![](./media/image205.png)

### Task 3 : Create a web app 

1.  Click on **Azure** icon from left navigation and right click on
    **App service** and select **Create New Web App**

  ![](./media/image206.png)

2.  Select a any region to deploy your app

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image207.png)

3.  Enter unique name (+++fleetguardXXXXX+++- XXXXX can be unique number)for your app and press Enter.

  ![](./media/image208.png)

4.  Select **Python 3.10**

  ![](./media/image209.png)

5.  Select Basic price tier.

  ![](./media/image210.png)

6.  Wait for the app to create.

  ![](./media/image211.png)

### Task 4 : Deploy the App

1.  Go to the **chatapp** folder , right click on the folder and select **Deploy to web app**.

  ![](./media/image212.png)

2.  Select the webapp you created above.

  ![](./media/image213.png)

3.  Click on **Deploy** now.

  ![](./media/image214.png)

4.  Wait for the deployment to complete.

  ![](./media/image215.png)

### Task 5 : Configure the fleet app.

1.  Open Azure portal, search **App service** and open it.

  ![](./media/image216.png)

2.  Select your app name

  ![](./media/image217.png)

3.  Select **Environment variables under Settings** from left navigation menu and click on **Add**

  ![](./media/image218.png)

4.  Add below variables with their values

  ![](./media/image219.png)

5.  Add +++FLASK_APP+++ and value as +++app.py+++

  ![](./media/image220.png)

6.  Add +++WEBSITES_PORT+++ and value as +++8000+++

  ![](./media/image221.png)

7.  Click on **Apply** -> **Confirm**

  ![](./media/image222.png)

  ![](./media/image223.png)

8.  Click on **Configuration** and add startup command as - +++python -m flask run --host=0.0.0.0 --port=8000+++

   ![](./media/image224.png)

9.  Add Microsoft as identity provider

  ![A screenshot of a computer AI-generated content may be incorrect.](./media/image225.png)

10. Click on **Continue**.

  ![](./media/image226.png)

11. Click on identity from left navigation menu, On the status and  Save-> Yes

   ![](./media/image227.png)

12. Click on **Authentication** form left navigation bar and select  **Add identity provider.**

  ![](./media/image228.png)

13. Select below values and keep all default values and then click on **Next: Permissions.**

  - **Identity provider** : **Microsoft**

  - **Client secret expiration** : **90 dayes**

  ![](./media/image229.png)

14. Click on Add permission. Expand Applicaions , select **Application.ReadWrite.All Read and write all applications** check box
    and then click on Update permissions.

  ![](./media/image230.png)

15. Click on Add now.

  ![](./media/image231.png)

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image232.png)

### **Task 6 : Access the Fleetguard app**

1.  Click on **Overview** and select **Default domain** link.

  ![](./media/image233.png)

2.  App opens in new tab. Wait and click on Accetp.

  ![](./media/image234.png)

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image235.png)

3.  Enter below prompts and check responses.

  - +++my vehicle - 98765 has enginehealth - 0.826120+++
  - +++Predict maintenance issues for my vehicle with 80,000 km mileage+++
  - +++Will my car overheat based on engine temperature 90°C?+++
  - +++vehicle 24353 's enginehealth is 0.9 and vehicle sensor speed is 101+++
  - +++What should I do if my engine health is below 0.5?+++
  - +++There's a burning smell and smoke from the exhaust+++

  ![A screenshot of a chat AI-generated content may be incorrect.](./media/image236.png)

  ![A screenshot of a chat AI-generated content may be  incorrect.](./media/image237.png)

  ![A screenshot of a chat AI-generated content may be
incorrect.](./media/image238.png)

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image239.png)

  ![A screenshot of a chat AI-generated content may be
incorrect.](./media/image240.png)

  ![A screenshot of a chat AI-generated content may be
incorrect.](./media/image241.png)
