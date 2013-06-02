Table-Definition Configuration Schema
The following example shows the configuration schema for tables defined using the .hdbtable syntax. Each of the entries in the table-definition configuration schema is explained in more detail in a dedicated section below:
struct TableDefinition {
string SchemaName;
optional bool temporary;
optional TableType tableType;
optional bool public;
optional TableLoggingType loggingType; list<ColumnDefinition> columns;
optional list<IndexDefinition> indexes; optional PrimaryKeyDefinition primaryKey; optional string description
};
Schema Name
To use the .hdbtable syntax to specify the name of the schema that contains the table you are defining, use the schemaName keyword. In the table definition, the schemaName keyword must adhere to the syntax shown in the following example.
 table.schemaName = "MYSCHEMA";
P U B L I C SAP HANA Developer Guide 88 © 2013 SAP AG or an SAP affiliate company. All rights reserved. Setting Up the Persistence Model
￼Temporary
To use the .hdbtable syntax to specify that the table you define is temporary, use the boolean temporary keyword. Since data in a temporary table is session-specific, only the owner session of the temporary table is allowed to INSERT/READ/TRUNCATE the data. Temporary tables exist for the duration of the session, and data from the local temporary table is automatically dropped when the session is terminated. In the table definition, the temporary keyword must adhere to the syntax shown in the following example.
 table.temporary = true;
Table Type
To specify the table type using the .hdbtable syntax, use the tableType keyword. In the table definition, the TableType keyword must adhere to the syntax shown in the following example.
 table.tableType = COLUMNSTORE;
The following configuration schema illustrates the parameters you can specify with the tableType keyword:
Table Logging Type
To enable logging in a table definition using the .hdbtable syntax, use the tableLoggingType keyword. In the table definition, the tableLoggingType keyword must adhere to the syntax shown in the following example.
 table.tableLoggingType = LOGGING;
The following configuration schema illustrates the parameters you can specify with the tableLoggingType keyword:
Table Column Definition
To define the column structure and type in a table definition using the .hdbtable syntax, use the columns keyword. In the table definition, the columns keyword must adhere to the syntax shown in the following example.
SAP HANA Developer Guide P U B L I C
Setting Up the Persistence Model © 2013 SAP AG or an SAP affiliate company. All rights reserved. 89
￼￼￼enum TableType {
    COLUMNSTORE;
ROWSTORE; };
￼￼enum TableLoggingType {
    LOGGING;
    NOLOGGING;
};
￼table.columns = [
    {name = "Col1"; sqlType = VARCHAR; nullable = false; length = 20; comment =

￼"dummy comment";},
    {name = "Col2"; sqlType = INTEGER; nullable = false;},
    {name = "Col3"; sqlType = NVARCHAR; nullable = true; length = 20; defaultValue
= "Defaultvalue";},
    {name = "Col4"; sqlType = DECIMAL; nullable = false; precision = 2; scale =
3;}];
￼The following configuration schema illustrates the parameters you can specify with the columns keyword:
￼struct ColumnDefinition {
    string name;
    SqlDataType sqlType;
    optional bool nullable;
    optional bool unique;
    optional int32 length;
    optional int32 scale;
    optional int32 precision;
    optional string defaultValue;
    optional string comment;
};
SQL Data Type
To define the SQL data type for a column in a table using the .hdbtable syntax, use the sqlType keyword. In the table definition, the sqlType keyword must adhere to the syntax shown in the following example.
The following configuration schema illustrates the data types you can specify with the sqlType keyword:
Table Order
To define the table order type using the .hdbtable syntax, use the order keyword. In the table definition, the order keyword must adhere to the syntax shown in the following example.
 table.order = ASC;
The following configuration schema illustrates the parameters you can specify with the order keyword:
P U B L I C SAP HANA Developer Guide 90 © 2013 SAP AG or an SAP affiliate company. All rights reserved. Setting Up the Persistence Model
￼table.columns = [
    {name = "Col1"; sqlType = VARCHAR; nullable = false; length = 20; comment =
"dummy comment";},
    ...
];
￼enum SqlDataType {
    DATE; TIME; TIMESTAMP; SECONDDATE; INTEGER; TINYINT;
    SMALLINT; BIGINT; REAL; DOUBLE; FLOAT; SMALLDECIMAL;
    DECIMAL; VARCHAR; NVARCHAR; CHAR; NCHAR;CLOB; NCLOB;
    ALPHANUM; TEXT; SHORTTEXT; BLOB; VARBINARY;
};
￼￼enum Order {
    ASC;
DSC; };

￼You can choose to filter the table contents either by ascending (ASC) or descending (DSC) order.
Primary Key Definition
To define the primary key for the specified table using the .hdbtable syntax, use the primaryKey and pkcolumns keywords. In the table definition, the primaryKey and pkcolumns keywords must adhere to the syntax shown in the following example.
 table.primaryKey.pkcolumns = ["Col1", "Col2"];
The following configuration schema illustrates the parameters you can specify with the primaryKey keyword:
Table Index Definition
To define the index type for the specified table using the .hdbtable syntax, use the indexes keyword. In the table definition, the indexes keyword must adhere to the syntax shown in the following example.
The following configuration schema illustrates the parameters you can specify with the indexes keyword:
Table Index Type
To define the index type for the specified table using the .hdbtable syntax, use the indexType keyword. In the table definition, the indexType keyword must adhere to the syntax shown in the following example.
 table.indexType = B_TREE;
The following configuration schema illustrates the parameters you can specify with the indexType keyword:
SAP HANA Developer Guide P U B L I C
Setting Up the Persistence Model © 2013 SAP AG or an SAP affiliate company. All rights reserved. 91
￼￼struct PrimaryKeyDefinition { list<string> pkcolumns;
    optional IndexType indexType;
};
￼table.indexes =  [
    {name = "MYINDEX1"; unique = true; indexColumns = ["Col2"];},
    {name = "MYINDEX2"; unique = true; indexColumns = ["Col1", "Col4"];}];
￼struct IndexDefinition {
    string name;
bool unique;
optional Order order; optional IndexType indexType; list<string> indexColumns;
};
￼￼enum IndexType {
    B_TREE;

￼CPB_TREE; };
￼B_TREE specifies an index tree of type B+, which maintains sorted data that performs the insertion, deletion, and search of records. CPB_TREE stands for “Compressed Prefix B_TREE” and specifies an index tree of type CPB+, which is based on pkB-tree. CPB_TREE is a very small index that uses a “partial key”, that is; a key that is only part of a full key in index nodes.
￼￼Note
If neither the B_TREE nor the CPB_TREE is specified in the table-definition file, SAP HANA chooses the appropriate index type based on the column data type, as follows:
● CPB_TREE
Character string types, binary string types, decimal types, when the constraint is a composite key or a non- unique constraint
● B_TREE
All column data types other than those specified for CPB_TREE
Complete Table-Definition Configuration Schema
The following example shows the complete configuration schema for tables defined using the .hdbtable syntax.
￼enum TableType {
    COLUMNSTORE; ROWSTORE;
};
enum TableLoggingType {
    LOGGING; NOLOGGING;
};
enum IndexType {
    B_TREE; CPB_TREE;
};
enum Order {
ASC; DSC; };
enum SqlDataType {
    DATE; TIME; TIMESTAMP; SECONDDATE;
    INTEGER; TINYINT; SMALLINT; BIGINT;
    REAL; DOUBLE; FLOAT; SMALLDECIMAL; DECIMAL;
    VARCHAR; NVARCHAR; CHAR; NCHAR; CLOB; NCLOB;
    ALPHANUM; TEXT; SHORTTEXT; BLOB; VARBINARY;
};
struct PrimaryKeyDefinition {
list<string> pkcolumns;
    optional IndexType indexType;
};
struct IndexDefinition {
    string name;
bool unique;
optional Order order; optional IndexType indexType; list<string> indexColumns;
};
struct ColumnDefinition {
    string name;
    SqlDataType sqlType;
    optional bool nullable;
    optional bool unique;
P U B L I C SAP HANA Developer Guide 92 © 2013 SAP AG or an SAP affiliate company. All rights reserved. Setting Up the Persistence Model

￼    optional int32 length;
    optional int32 scale;
    optional int32 precision;
    optional string defaultValue;
    optional string comment;
};
struct TableDefinition {
string schemaName;
optional bool temporary;
optional TableType tableType;
optional bool public;
optional TableLoggingType loggingType; list<ColumnDefinition> columns;
optional list<IndexDefinition> indexes; optional PrimaryKeyDefinition primaryKey; optional string description;
};
TableDefinition table;