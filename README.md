# Data-Quality-Nodetypes

The Coalesce Data Quality Nodetypes Package includes:

* [DMF](#DMF)
* [Code](#Code)

---

### DMF

The Coalesce DMF node allows you to to monitor the state and integrity of your data. You can use DMFs to measure key metrics, such as, but not limited to, freshness and counts that measure duplicates, NULLs, rows, and unique values.

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


### DMF Node Configuration

The Work node type has four configuration groups:

* [Node Properties](#node-properties)
* [Scheduling Options](#scheduling-options)
* [Object Level DMFs](#object-level-dmfs)
* [Column Level DMFs](#column-level-dmfs)

<img width="456" alt="Screenshot 2025-03-31 at 1 30 49 PM" src="https://github.com/user-attachments/assets/b3a0a8b9-d185-45b3-86b4-e7546505b7ac" />

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


## Code

### DMF Code

* [Node definition](https://github.com/coalesceio/data-quality-nodetypes/blob/main/nodeTypes/DMFS-448/definition.yml)
* [Create Template](https://github.com/coalesceio/data-quality-nodetypes/blob/main/nodeTypes/DMFS-448/create.sql.j2)
* [Macros](https://github.com/coalesceio/data-quality-nodetypes/blob/main/macros/macro-1.yml)
