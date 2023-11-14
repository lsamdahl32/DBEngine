# The DBEngine Class

The DBEngine class is a PHP class for managing MySQL databases. It contains the methods necessary to connect to and query a database. It gets the connection parameters from an array stored in a constant which must be set before calling the class. This ensures that the db credentials can be placed in a secrets file outside of a repository and below the root directory of the site.

The DB_SETTINGS array is set like this:

const DB_SETTINGS =  array(
	'dbNickname' => array(
		'username' => "dbuser",
		'password' => "dbpass",
		'db_name' => "dbname",
		'host' => "dbhost",
		'port' => 3306,
        	'ssl'   => false
	),
	// other databases can be added here
);


The class is initialized as follows:

1. Require the class file
2. Initialize the class and variable ($dbe = new DBEngine($dbname, $debug, $errorsOnly, $suppressText)). Parameters are:
  1. $dbname = the standard dbNickname from array in DB_SETTINGS.
  2. $debug, etc. = these are optional Boolean values that are passed to the db functions to display error information. They should be left at their default values for production.

## Properties

The following are properties of this class:

| **Property Settings** | **Scope** | **Description** |
| --- | --- | --- |
| dblink | Public | The dblink value from the db functions. Use this value wherever that value is needed, such as in clean\_param(). |
| setBindtypes | Public | Set the $bindtypes for the query. |
| setBindvalues | Public | Set the $bindvalues array for the query. Must be an array. |
| setDebug | Public | Allows changing the debug setting after initialization. |
| setErrorsOnly | Public | Allows changing the errorsOnly setting after initialization. |
| setSuppressText | Public | Allows changing the suppressText setting after initialization. |
| error\_msg | Public | Returns the error message after an exception. |
| error\_num | Public | Returns the error number after an exception. |

## Methods

The following are callable and private methods of this class:

| **Method** | **Scope** | **Description** |
| --- | --- | --- |
| close | Public | This method releases the mysqli connection. |
| deleteRow | Public | Deletes one or more rows. Parameters: \* @param string $table\* @param string $keyname - field name of primary key\* @param int|string $prikey - the primary key\* @param string $bindtype - s or i The method returns the count of affected records or false if error. |
| execute\_query | Public | Perform the actual query. Parameters: string $query – the SQL query. Returns false if error or no records found or an associative array of results. (i.e. rows[record][field=\>value]) |
| getRowsWhere | Public | Performs a query that can return one or more rows. It expects an array of where clauses that must match bindtypes and bindvalues previously set. Parameters: \* @param string $table - table name\* @param array $select - a flat array of field names for the select to be joined by commas\* @param array $where - a flat array of WHERE clauses to be joined by AND; if empty, all rows returned\* @param array $orderby - a flat array of order by clauses\* @param array $groupby - a flat array of group by clauses\* @param int $limitIt returns false if error or no records found or an associative array of results. (i.e. rows[record][field]) |
| getRowWhere | Public | Get a single table row based on a single field in WHERE. Parameters:\* @param string $table - table name\* @param string $keyField - field name of primary key\* @param int $id - the primary key\* @param string $orderby - the order by clause for SQLIt returns false if error or no records found or an associative array of results. (i.e. row[field=\>value]) |
| insertRow | Public | Insert one record into $table.Parameters:\* @param string $table\* @param array $data - a key=\>value array of new data to insertIt returns the new record id or false. |
| updateRow | Public | Update a single table record.Parameters:\* @param string $table\* @param array $data - a key=\>value array of new data to update\* @param string $keyname - field name of primary key\* @param string $prikey - the primary key\* @param string $bindtype - s or iIt returns the count of affected records (1 or 0 if no change) or -1 if error. |
| detectType | Private | Detect value type to generate bind parameter. |
| processData | Private | Extract "where" array and bindtype from input array. If value is array, then it contains both bindtype and value. Used with the $data array in inserRow() and updateRow(). |
| beginTrans | Public | A wrapper for mysqli\_begin\_transaction(). It will return false if an error occurs. |
| commitTrans | Public | A wrapper for mysqli\_commit(). |
| rollbackTrans | Public | A wrapper for mysqli\_rollback(). |
| isInTransaction | Public | Indicates that a transaction is in progress. |
