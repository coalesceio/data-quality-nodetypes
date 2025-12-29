# Data Quality Nodetypes

The Coalesce Data Quality Nodetypes Package includes:

* [DMF](#DMF)
* [Data Profiling](#data-profiling)
* [Code](#code)

---

### DMF

The Coalesce DMF node allows you to to monitor the state and integrity of your data. You can use DMFs to measure key metrics, such as, but not limited to, freshness and counts that measure duplicates, NULLs, rows, and unique values.This is based on Data Metric functions concept in Snowflake.For more information on roles and privileges refer [docs](https://docs.snowflake.com/en/user-guide/data-quality-intro#about-data-quality-and-dmfs).

You can set a DMF on the following kinds of table objects:
* Dynamic table
* Event table
* External table
* Apache Iceberg™ table
* Materialized view
* Table (Including temporary and transient tables)
* View

### Limitations:
* You cannot set a DMF on a hybrid table or a stream object.
* You can only have 10,000 total associations of DMFs on objects per account. Each instance of setting a DMF on a table or view counts as one association.
* You cannot grant privileges on a DMF to share or set a DMF on a shared table or view.
* Setting a DMF on an object tag is not supported.
* You cannot set a DMF on objects in a reader account.
* Trial accounts do not support this feature.
* In coalesce,this node cannot be used as part of a pipeline.This node can be added at the end of the pipeline to infer Data Metrics
* Package DMF settings are static once defined and deployed. Redeployment behavior is driven only by universal enable or disable toggles, not by changes to package configuration values.
  
### DMF Node Configuration

The Work node type has seven configuration groups:

* [Node Properties](#node-properties)
* [Scheduling Options](#scheduling-options)
* [Universal DMFs](#universal-dmfs)
* [Object Level DMFs](#object-level-dmfs)
* [Column Level DMFs](#column-level-dmfs)
* [Custom DMFs](#custom-DMFs)
* [Alerting Options](#alerting-options)

<img width="472" height="387" alt="config" src="https://github.com/user-attachments/assets/0bf52813-c27a-4c75-baa8-ad76163eaeee" />


#### Node Properties

| **Property** | **Description** |
|----------|-------------|
| **Storage Location** | Storage Location where the WORK will be created |
| **Node Type** | Name of template used to create node objects |
| **Deploy Enabled** | If TRUE the node will be deployed / redeployed when changes are detected<br/> If FALSE the node will not be deployed or will be dropped during redeployment |

#### Scheduling Options

You can create DMF run schedule to automate the data quality measurements on a table, as:

| **Option** | **Description** |
|------------|----------------|
| **Schedule Options** | Choose schedule type:<br/>- **Minutes** - Specify interval in minutes. Enter a whole number from 1 to 11520 which represents the number of minutes between DMF runs.<br/>- **Cron** - Uses [Cron expressions](https://docs.coalesce.io/docs/reference/cron-reference/). Specifies a cron expression and time zone for periodically running the DMF. Supports a subset of standard cron utility syntax.<br/>- **TRIGGER_ON_CHANGES** - Set the data metric function to run when a general DML operation, such as inserting a new row, modifies the table |

#### Universal DMFs

Package Config-driven defaults that apply common DMF patterns across all DMFS nodes, with per-node toggle controls covering all Snowflake system DMFs suitable for universal application

When the universal toggle and the object-level universal toggle are enabled, package DMF settings are applied at the **table level.**

When the universal toggle and the column-level universal toggle are enabled, package DMF settings are applied to the corresponding **columns**.

**Package Config Settings:**

<img width="592" height="271" alt="PackageConfig" src="https://github.com/user-attachments/assets/1f804857-cf9d-48ac-9906-c5898ef95389" />

<img width="597" height="492" alt="universalDMF" src="https://github.com/user-attachments/assets/813d6edc-40c8-4804-9916-bb5c594fcf11" />

| **Property** | **Description** |
|----------|-------------|
| **Enable Universal DMFs** | Master toggle for Universal DMFs section. `Default: true` |
| **Apply Universal Object Level DMFs** | Enable object-level DMFs. `Default: true` |
| **Object Level DMFs** | TextBox for DMFs to apply at table level. Defaults from Package Config universalObjectDmfs. Visible when toggle is true |
| **Apply Null Count DMF** | Enable NULL_COUNT on specified columns. `Default: true` |
| **Null Count Columns** | Comma-separated column names. Defaults from Package Config. Visible when toggle is true |
| **Apply Null Percent DMF** | Enable NULL_PERCENT on specified columns. `Default: false` |
| **Null Percent Columns** | Comma-separated column names. Defaults from Package Config. Visible when toggle is true |
| **Apply Blank Count DMF** | Enable BLANK_COUNT on specified columns. `Default: false` |
| **Blank Count Columns** | Comma-separated column names. Defaults from Package Config. Visible when toggle is true |
| **Apply Blank Percent DMF** | Enable BLANK_PERCENT on specified columns. `Default: false` |
| **Blank Percent Columns** | Comma-separated column names. Defaults from Package Config. Visible when toggle is true |
| **Apply Duplicate Count DMF** | Enable DUPLICATE_COUNT on specified columns. `Default: true` |
| **Duplicate Count Columns** | Comma-separated column names. Defaults from Package Config. Visible when toggle is true |
| **Apply Unique Count DMF** | Enable UNIQUE_COUNT on specified columns. `Default: false` |
| **Unique Count Columns** | Comma-separated column names. Defaults from Package Config. Visible when toggle is true |
| **Apply Freshness DMF** | Enable FRESHNESS on specified columns. `Default: true` |
| **Freshness Columns** | Comma-separated column names. Defaults from Package Config. Visible when toggle is true |


#### Object Level DMFs

<img width="486" alt="Screenshot 2025-03-31 at 1 49 14 PM" src="https://github.com/user-attachments/assets/5f69b19d-8f4b-4ce0-bea7-b4174a4beea2" />


| **Property** | **Description** |
|----------|-------------|
| **Add Object Level DMF** | Enable this to add object level DMF |
| **DMF Function** | Select the DMF function to use for object |

#### Column Level DMFs

<img width="486" alt="Screenshot 2025-03-31 at 1 52 48 PM" src="https://github.com/user-attachments/assets/699b4765-4362-456b-8188-5ab5fb42e149" />


| **Property** | **Description** |
|----------|-------------|
| **Add Column Level DMF** | Enable this to add object level DMF |
| **DMF Function** | Select the Column name and DMF function to use |

#### Custom DMFs

Custom DMFs in Snowflake allow you to define your own data quality checks using SQL logic and apply them to tables or columns based on your requirements. For more info on **Custom DMFs** [please refer this documentation](https://docs.snowflake.com/en/user-guide/data-quality-custom-dmfs)

**Edit the placeholders shown here to reflect your own table and column references.**

<img width="727" height="456" alt="custom" src="https://github.com/user-attachments/assets/2a4aaf65-b049-4b6c-a98f-ce51abe25116" />

**Example input for only base table columns**

<img width="722" height="377" alt="1 table" src="https://github.com/user-attachments/assets/086e2024-1086-4aa1-9841-a41f68666069" />

**Example input for reference table columns**

<img width="726" height="296" alt="ex - 2 tables" src="https://github.com/user-attachments/assets/51942a6c-5ee0-4267-9d68-f1c4e0b637d9" />

#### Alerting Options

Optional Snowflake ALERT creation that sends notifications when data quality issues are detected. More abouit Alerting [please refer this documentation.](https://docs.snowflake.com/en/guides-overview-alerts)

| **Property** | **Description** |
|----------|-------------|
| **Enable Email Alerts** | Master toggle for alerting. `Default: false` |
| **Override Global Parameters** | If enabled, use node config values instead of deployment parameters. `Default: false`. Visible when Enable Email Alerts is true |
| **Notification Integration Name** | Snowflake notification integration name. Defaults to parameter DMF_ALERT_INTEGRATION. Visible when Enable Email Alerts is true |
| **Alert Email Address** | Email recipient for alerts. Defaults to parameter DMF_ALERT_EMAIL. Visible when Enable Email Alerts is true |
| **Warehouse for Alert** | Warehouse to execute alert checks. Defaults to parameter DMF_ALERT_WAREHOUSE. Visible when Enable Email Alerts is true |
| **Alert Check Frequency (Minutes)** | How often to check for issues. `Default: 60`. Defaults to parameter DMF_ALERT_SCHEDULE. Visible when Enable Email Alerts is true |
| **Trigger on Null Count** | Alert when NULL_COUNT > 0. `Default: true`. Visible when Enable Email Alerts is true |
| **Trigger on Null Percent** | Alert when NULL_PERCENT exceeds threshold. `Default: false`. Visible when Enable Email Alerts is true 
| **Null Percent Threshold (%)** | Alert if null percentage exceeds this value. `Default: 5`. Visible when Trigger on Null Percent is true |
| **Trigger on Blank Count** | Alert when BLANK_COUNT > 0. `Default: false`. Visible when Enable Email Alerts is true |
| **Trigger on Blank Percent** | Alert when BLANK_PERCENT exceeds threshold. `Default: false`. Visible when Enable Email Alerts is true |
| **Blank Percent Threshold (%)** | Alert if blank percentage exceeds this value. `Default: 5`. Visible when Trigger on Blank Percent is true |
| **Trigger on Duplicates** | Alert when DUPLICATE_COUNT > 0. `Default: true`. Visible when Enable Email Alerts is true |
| **Trigger on Freshness** | Alert when FRESHNESS exceeds threshold. `Default: true`. Visible when Enable Email Alerts is true |
| **Freshness Threshold (Hours)** | Alert if data older than this many hours. `Default: 24`. Visible when Trigger on Freshness is true |

**Email Alerting Options**

<img width="596" height="600" alt="Alerting" src="https://github.com/user-attachments/assets/c7e11ec9-3e06-4723-b8ad-362cc46affa8" />

**Override Options**

<img width="727" height="516" alt="Overrdide" src="https://github.com/user-attachments/assets/4cd57613-a663-4986-b3db-05c4fb61bdd8" />

**Override Parameters**
<img width="1087" height="257" alt="override-parameter" src="https://github.com/user-attachments/assets/75c37421-2ed4-46f4-901d-42ad5a8add9a" />

### DMF Deployment

#### DMF Initial Deployment

When deployed for the first time into an environment the DMF node will update the metadata with DMFs.

| **Stage** | **Description** |
|-----------|----------------|
| **Create ** | This will execute a Alter statement and create a DMF entries in metadata |

#### DMF Redeployment

The subsequent deployment of DMF node with changes in DMFs definition, adding/dropping DMFs results in updating metadata with latest changes.

#### DMF Undeployment

If a DMF Node is deleted from a Workspace, that Workspace is committed to Git and that commit deployed to a higher level environment then the all the DMFs will be dropped from metadata.


---

### Data Profiling

The Data Profiling node type creates a data profiling table containing details such as the object name, column name, metric function applied, and the corresponding profiling result. It supports operations like count, min, max, avg, and other statistical measures on source table columns. Additionally, this node type allows scheduling, enabling automated and periodic data profiling to maintain data quality and integrity.
The Data Profiling node type will create the Data Profiling table with the following fixed column structure: OBJECT_NAME, COLUMN_NAME, FUNCTION_APPLIED, and RESULT.
The Coalesce Mapping grid will display the source column list; however, the Data Profiling table created in Snowflake will maintain a fixed column structure.

The node name in Coalesce will be used as the name of the Snowflake Data Profiling table.

### Data Profiling Node Configuration

The Data Profiling node type has four configuration groups:

* [Node Properties](#node-properties)
* [Options](#options)
* [Counts](#counts)
* [Min/Max/Avg](#min-max-avg)
* [Column Details](#column-details)
* [Scheduling Options](#data-profiling-scheduling-options)


<img width="232" alt="image" src="https://github.com/user-attachments/assets/2998538c-a676-4b63-8d69-efebd7bd7e76" />

#### Node Properties

| **Option** | **Description** |
|----------|-------------|
| **Storage Location** | Storage Location where the Data Profiling table will be created |
| **Node Type** | Name of template used to create node objects |
| **Description** | A description of the node's purpose |
| **Deploy Enabled** | If TRUE the node will be deployed / redeployed when changes are detected<br/> If FALSE the node will not be deployed or will be dropped during redeployment |

#### Options

| **Option** | **Description** |
|---------|-------------|
| **Schedule Task** | True / False toggle that determines whether a task will be created or if the SQL to be used in the task will execute as DML as a Run action.<br/>**True** - The insert data SQL will wrap in a task with specified Scheduling Options. When Run is executed, a message appears prompting the user to wait or suggesting a manual run. <br/>**False** - A table will be created and SQL will execute as a Run action|
| **Truncate Before** | Toggle: True/False<br/>This determines whether a table will be overwritten each time a task executes. **True**: Uses INSERT OVERWRITE<br/>**False**: Uses INSERT to append data |
| **Enable tests** | Toggle: True/False<br/>Determines if tests are enabled |
| **Sample Mode** | Options: Sample/Full Table<br/>**Sample**: Options: Percent/Fixed Number of Rows <br/>**Full Table**: Performs Data Profiling on all records of source table|

#### Counts

| **Option** | **Description** |
|---------|-------------|
| **Row Count** | True / False toggle that determines weather to add a record for the row count of the source table.<br/>**True** - Add a record for the row count of the source table. <br/>**False** - A record for the row count of the source table is not added.|
| **Distinct Count** | Add column/columns to get the count of distinct values of the selected column|
| **Null Count** | Add column/columns to get the count of null values of the selected column |
| **Not Null Count** | Add column/columns to get the count of not null values of the selected column |

#### Min / Max / Avg

| **Option** | **Description** |
|---------|-------------|
| **MAX Value** | Add column/columns to get the Maximum value of the selected column|
| **MIN Value** | Add column/columns to get the Minimum value of the selected column|
| **Average Value** | Add column/columns to get the Average value of the selected column|


#### Column Details

| **Option** | **Description** |
|---------|-------------|
| **Top 100 Value Distribution** | Add column/ columns to get the JSON array that contains the top 100 distinct values of the column along with their occurrence counts|
| **Max Data Length** | Add column/columns to get the maximum length of non-null values in the selected column|
| **Min Data Length** | Add column/columns to get the minimum length of non-null values in the selected column|
| **Standard Deviation** | Add column/columns to get  the standard deviation of the lengths of non-null selected column values|

#### Data Profiling Scheduling Options

![Task Scheduling Options](https://github.com/user-attachments/assets/2f8bc6c3-117c-4130-9619-45ee50feebef)

| **Option** | **Description** |
|------------|----------------|
| **Scheduling Mode** | Choose compute type:<br/>- **Warehouse Task**: User managed warehouse executes tasks<br/>- **Serverless Task**: Uses serverless compute |
| **When Source Stream has Data Flag** | True/False toggle to check for stream data<br/>**True** - Only run task if source stream has capture change data<br/>**False** -  Run task on schedule regardless of whether the source stream has data. If the source is not a stream should set this to false. |
| **Multiple Stream has Data Logic** | AND/OR logic when multiple streams (visible if Stream has Data Flag is true)<br/>**AND** - If there are multiple streams task will run if all streams have data<br/>**OR** -  If there are multiple streams task will run if one or more streams has data |
| **Select Warehouse** | Visible if Scheduling Mode is set to Warehouse Task. Enter the name of the warehouse you want the task to run on without quotes.|
| **Select initial serverless size** | Visible when Scheduling Mode is set to Serverless Task.<br/> Select the initial compute size on which to run the task. Snowflake will adjust size from there based on target schedule and task run times. |
| **Task Schedule** | Choose schedule type:<br/>- **Minutes** - Specify interval in minutes. Enter a whole number from 1 to 11520 which represents the number of minutes between task runs.<br/>- **Cron** - Uses [Cron expressions](https://docs.coalesce.io/docs/reference/cron-reference/). Specifies a cron expression and time zone for periodically running the task. Supports a subset of standard cron utility syntax.<br/>- **Predecessor** - Specify dependent tasks |
| **Enter predecessor tasks separated by a comma**| Visible when Task Schedule is set to Predecessor. <br/>One or more task names that precede the task being created in the current node. Task names are case sensitive, should not be quoted and must exist in the same schema in which the current task is being created. If there are multiple predecessor task separate the task names using a comma and no spaces.|
| **Root task name** | Visible when Task Schedule is set to Predecessor.<br/> Name of the root task that controls scheduling for the DAG of tasks. Task names are case sensitive, should not be quoted and must exist in the same schema in which the current task is being created. If there are multiple predecessor task separate the task names using a comma and no spaces.|

#### Example of Serverless Task with Multiple Predecessors and Root Task

![Example_of_Serverless_Task](https://github.com/coalesceio/Streams-and-Task-Nodes/assets/7216836/0af0c04a-5e88-42d1-85de-fd71a792436d)

#### Example of Warehouse Task With 60 Minute Task Schedule

![Example_of_Warehouse_Task](https://github.com/coalesceio/Streams-and-Task-Nodes/assets/7216836/04afc6b2-aebc-4346-b1fe-33340e7df0bf)

#### Example of Warehouse Task With Cron Schedule Not Using a Stream

![Example_of_Warehouse_Task_with_Cron](https://github.com/coalesceio/Streams-and-Task-Nodes/assets/7216836/89c85551-59af-44e6-b383-a9bdaacf7cce)


### Data Profiling Deployment

#### Deployment Parameters

The Data Profiling with Task includes an environment parameter that allows you to specify a different warehouse used to run a task in different environments.

The parameter name is `targetTaskWarehouse` and the default value is `DEV ENVIRONMENT`.

When set to `DEV ENVIRONMENT` the value entered in the Scheduling Options config Select Warehouse on which to run the task will be used when creating the task.

```json
{
    "targetTaskWarehouse": "DEV ENVIRONMENT"
}
```

When set to any value other than `DEV ENVIRONMENT` the node will attempt to create the task using a Snowflake warehouse with the specified value.

For example, with the below setting for the parameter in a QA environment, the task will execute using a warehouse named `compute_wh`.

```json
{
    "targetTaskWarehouse": "compute_wh"
}
```
The parameter is to be added in the Workspace settings and Environment settings for deployment

![image](https://github.com/user-attachments/assets/1a360cec-2517-4748-9a81-650d06f96850)

#### Data Profiling  With Task Initial Deployment


When deployed for the first time into an environment the Data Profiling  With Task node will execute the following stages:

For tasks without predecessors:

| **Stage** | **Description** |
|-----------|----------------|
| **Create Data Profiling Table** | Creates a Data Profiling table that will be loaded by the task.Note that the structure of the data profiling table is defined in the create template and will not be same as the columns in the mapping grid of Coalesce node |
| **Create Task** | Creates task that will load the Data Profiling table on schedule |
| **Resume Task** | Resumes the task so it runs on schedule |

For tasks with predecessors:

| **Stage** | **Description** |
|-----------|----------------|
| **Create Target Table** | Creates table that will be loaded by the task |
| **Suspend Root Task** | Suspends root task for DAG modification |
| **Create Task** | Creates task that will load the target table on schedule |

If a task is part of a DAG of tasks, the DAG needs to include a node type called "Task DAG Resume Root." This node will resume the root node once all the dependent tasks have been created as part of a deployment.

The task node has no ALTER capabilities. All task-enabled nodes are CREATE OR REPLACE only, though this is subject to change.

#### Data Profiling With Task Redeployment

After initial deployment, changes in task schedule, warehouse, or scheduling options will result in a CREATE or RESUME

For tasks without predecessors:

| **Stage** | **Description** |
|-----------|----------------|
| **Create Task** | Recreates task with new schedule |
| **Resume Task** | Resumes task with new schedule |

For tasks with predecessors:

| **Stage** | **Description** |
|-----------|----------------|
| **Suspend Root Task** | Suspends root task for DAG modification |
| **Create Task** | Recreates task with new schedule |

#### Data Profiling With Task Altering Tables

The structure of the data profiling table is defined int he create template and cannot be changed so altering the table is not supported in redeployment.

### Data Profiling With Task Undeployment

If a Data Profiling with Task node is deleted from a Workspace, that Workspace is committed to Git and that commit deployed to a higher level environment then all objects created by the node in the target environment will be dropped.

For tasks without predecessors:

| **Stage** | **Description** |
|-----------|----------------|
| **Drop Table** |  Drop the data profiling table originally created to be loaded by the task. |
| **Drop Current Task** | Drop the task |

For tasks with predecessors:

| **Stage** | **Description** |
|-----------|----------------|
| **Drop Table** | Drop the data profiling table |
| **Suspend Root Task** | Drop a task from a DAG of task the root task needs to be put into a suspended state. |
| **Drop Task** | Drops the task |


## Code

### DMF Code

* [Node definition](https://github.com/coalesceio/data-quality-nodetypes/blob/main/nodeTypes/DMFS-448/definition.yml)
* [Create Template](https://github.com/coalesceio/data-quality-nodetypes/blob/main/nodeTypes/DMFS-448/create.sql.j2)
* [Macros](https://github.com/coalesceio/data-quality-nodetypes/blob/main/macros/macro-1.yml)

### Data Profiling Code

* [Node definition](https://github.com/coalesceio/data-quality-nodetypes/blob/main/nodeTypes/DataProfiling-416/definition.yml)
* [Create Template](https://github.com/coalesceio/data-quality-nodetypes/blob/main/nodeTypes/DataProfiling-416/create.sql.j2)
* [Run Template](https://github.com/coalesceio/data-quality-nodetypes/blob/main/nodeTypes/DataProfiling-416/run.sql.j2)
* [Macros](https://github.com/coalesceio/data-quality-nodetypes/blob/main/macros/macro-1.yml)
