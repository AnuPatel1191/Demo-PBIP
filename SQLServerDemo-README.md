# SQL Server Demo - Power BI Project (PBIP)

This is a Power BI Project (PBIP) file using the TMDL (Tabular Model Definition Language) format, demonstrating connection to a SQL Server data source.

## Project Structure

```
SQLServerDemo.pbip                          # Main project file
├── SQLServerDemo.SemanticModel/            # Semantic model (data model) in TMDL format
│   ├── .pbi/                               # Metadata files
│   │   └── datasetReference.json           # Dataset reference configuration
│   ├── .platform                           # Platform configuration
│   └── definition/                         # TMDL definition files
│       ├── model.tmdl                      # Main model definition with SQL Server data source
│       ├── tables/                         # Table definitions
│       │   └── SampleTable.tmdl            # Sample table connecting to SQL Server
│       └── relationships/                  # Table relationships (if any)
└── SQLServerDemo.Report/                   # Report definition
    ├── .pbi/                               # Report metadata
    │   └── datasetReference.json           # Link to semantic model
    ├── .platform                           # Platform configuration
    └── definition.pbir                     # Report layout definition
```

## SQL Server Connection

The project connects to a SQL Server database with the following configuration:

- **Server**: localhost
- **Database**: DemoDB
- **Authentication**: Integrated Security (Windows Authentication)

### Data Source Configuration

The SQL Server data source is defined in `SQLServerDemo.SemanticModel/definition/model.tmdl`:

```tmdl
dataSource 'SQLServerSource' {
    type: structured
    provider: System.Data.SqlClient
    connectionString: "Data Source=localhost;Initial Catalog=DemoDB;Integrated Security=True"
    
    annotation ConnectionType = "sqlServer"
}
```

### Sample Table

The project includes a sample table `SampleTable` that demonstrates how to query data from SQL Server using Power Query (M language):

```m
let
    Source = Sql.Database("localhost", "DemoDB"),
    dbo_SampleTable = Source{[Schema="dbo",Item="SampleTable"]}[Data]
in
    dbo_SampleTable
```

The table includes three columns:
- **SampleID** (Integer): A unique identifier
- **SampleName** (String): A text field
- **SampleValue** (Decimal): A numeric value formatted as currency

## Prerequisites

To use this PBIP file, you need:

1. **Power BI Desktop** (with developer mode enabled)
2. **SQL Server** instance accessible at `localhost` 
3. A database named `DemoDB` with a table `dbo.SampleTable`

### Sample SQL Server Table Schema

You can create the sample table using this SQL script:

```sql
CREATE DATABASE DemoDB;
GO

USE DemoDB;
GO

CREATE TABLE dbo.SampleTable (
    SampleID INT PRIMARY KEY,
    SampleName NVARCHAR(100),
    SampleValue DECIMAL(18, 2)
);

-- Insert sample data
INSERT INTO dbo.SampleTable (SampleID, SampleName, SampleValue)
VALUES 
    (1, 'Sample A', 100.50),
    (2, 'Sample B', 250.75),
    (3, 'Sample C', 350.25);
```

## How to Use

1. **Open in Power BI Desktop**:
   - Enable Developer Mode in Power BI Desktop (File > Options and settings > Options > Preview features > Store dataset using the TMDL format)
   - Open the `SQLServerDemo.pbip` file in Power BI Desktop

2. **Configure Connection**:
   - Update the connection string in `model.tmdl` if your SQL Server instance is not at `localhost`
   - Update the database name if different from `DemoDB`
   - Update authentication method if not using Windows Authentication

3. **Refresh Data**:
   - Click "Refresh" in Power BI Desktop to load data from SQL Server

## Benefits of PBIP/TMDL Format

- **Version Control**: Each component is stored as a separate file, making it Git-friendly
- **Collaboration**: Multiple developers can work on different parts of the project simultaneously
- **Transparency**: All definitions are human-readable text files
- **External Editing**: Can be edited with text editors or external tools like Tabular Editor

## Customization

To connect to your own SQL Server database:

1. Update the connection string in `SQLServerDemo.SemanticModel/definition/model.tmdl`
2. Modify the M query in `SQLServerDemo.SemanticModel/definition/tables/SampleTable.tmdl`
3. Add additional tables by creating new `.tmdl` files in the `definition/tables/` folder

## Notes

- Connection credentials are managed by Power BI Desktop and not stored in the TMDL files for security
- The TMDL format is a preview feature and may evolve with future Power BI updates
- This is a minimal example to demonstrate SQL Server connectivity in PBIP format
