https://cwe.mitre.org/data/definitions/285.html
When the application don't check properly (or simply don't check) whether an actor is authorized to access some resource or perfome some action.

##### Example:
The following function in SQL, that process an arbitrary query
```php
function runEmployeeQuery($dbName, $name){

mysql_select_db($dbName,$globalDbHandle) or die("Could not open Database".$dbName);   
$preparedStatement = $globalDbHandle->prepare('SELECT * FROM employees WHERE name = :name');  
$preparedStatement->execute(array(':name' => $name));  
return $preparedStatement->fetchAll();

}  
  
$employeeRecord = runEmployeeQuery('EmployeeDB',$_GET['EmployeeName']);
```
Is not confirm whether the "EmployeeDB" parameter is authorized to do so. 

### Child list
