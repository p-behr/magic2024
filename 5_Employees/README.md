# Employees

Now it's time to fetch some data and return results!

<br>➡️ Update `index.php` so that it looks like this:  
```
<?php

include '../db.php';

$dbConn = get_db_conn();

try {
    $sql = "select * from SAMPLE.EMPLOYEE";
    $query = $dbConn->prepare($sql);
    $query->execute(); 
    $rows = $query->fetchAll(PDO::FETCH_ASSOC);
    print_r($rows); 
} catch (PDOException $exception) {
    echo $exception->getMessage();
    exit;
} 
```
After we get our database connection we are going to build an SQL statement and run it.  
We build the SQL statement `$sql = "select * from SAMPLE.EMPLOYEE";`   
Prepare the SQL statement `$query = $dbConn->prepare($sql);`   
Execute the SQL statement `$query->execute();`   
And then fetch the results as an associative array (using field names) `$rows = $query->fetchAll(PDO::FETCH_ASSOC);`   
The results in the variable `$rows` will be a PHP array.  

<br>➡️ Open your browser and go to `http://magic.magic-ug.org:{your_port}`  

You should see something like this:  
![server array](images/array.PNG)  

Yay! We are fetching data from the database and returning it to the browser...that's awesome!

## Separate Files
We now want to make a few changes: let's convert this code into a function, move this code into a separate file, and let's return JSON instead of a PHP array.


<br>➡️ In your /magic2024 directory, create a file named `employee.php`  
❗ make sure this file is in magic2024 and *not* in htdocs.  


Your file structure should look like this:   
![file structure](images/files.PNG)  


<br>➡️ Paste this code into `employee.php` so that it looks like this: 
```
<?php

function get_all_employees() {
    $dbConn = get_db_conn();

    try {
        $sql = "select * from SAMPLE.EMPLOYEE";
        $query = $dbConn->prepare($sql);
        $query->execute(); 
        $rows = $query->fetchAll(PDO::FETCH_ASSOC);
        return $rows; 
    } catch (PDOException $exception) {
        echo $exception->getMessage();
        return NULL;
    } 
}
```
We now have a function we can call to fetch all the employees.
We want to change our `index.php` to call this new function.  



<br>➡️ Update `index.php` so that it looks like this:
```
<?php

include '../db.php';
include '../employee.php';

$employees = get_all_employees();
print(json_encode($employees));
```

First we include the employee file,  
Then we call our new function and get an array of all the employees,  
Then we use the `json_encode` function to convert the PHP array into JSON, and print the results.  


<br>➡️ Open your browser and go to `http://magic.magic-ug.org:{your_port}`  

You should see something like this:  
![server array](images/json.PNG)  

This is the same employee data, now formatted as JSON.


## Matching the spec

If we look at the OpenAPI specification, it tells us that the the return value for the /employees route should be a JSON object that looks like this:  
```
{
  "length": 0,
  "employees": [
    {
      "id": "string",
      "first": "string",
      "last": "string",
      "job": "string",
      "workdept": "str",
      "salary": 1
    }
  ]
}
```

First thing we want to do is match the names for the employee object; we currently have the system field names (i.e. "EMPNO").


<br>➡️ Update `employee.php` so that it looks like this:
```
<?php

function get_all_employees() {
    $dbConn = get_db_conn();

    try {
        $sql = <<<SQL
            select 
                empno as "id",
                firstnme as "first",
                lastname as "last",
                job as "job",
                workdept as "workdept",
                salary as "salary"
            from SAMPLE.EMPLOYEE
        SQL;
        $query = $dbConn->prepare($sql);
        $query->execute(); 
        $rows = $query->fetchAll(PDO::FETCH_ASSOC);
        return $rows; 
    } catch (PDOException $exception) {
        echo $exception->getMessage();
        return NULL;
    } 
}
```
The only thing we have changed is the value of the `$sql` variable and how we set it (using what's called a heredoc).  
This will give us the field names that we want (i.e. "id").  Keep in mind that JSON is case-sensitive, so "ID" is not the same as "id".  


## Return object
We have the employees array and the proper field names so we just need to add the length.  


<br>➡️ Update `index.php` so that it looks like this:
```
<?php

include '../db.php';
include '../employee.php';

$employees = get_all_employees();
$returnObj = array(
    "length" => count($employees),
    "employees" => $employees
);
print(json_encode($returnObj));
```

We call get_all_employees, same as before.  
Then we create a PHP array named `$returnObj` that has the length.  The length of the array is calculated by using the `count()` function.  Then, in the same array, we include the employees.  
Then we encode the $returnObj as JSON and return that back to the client.   


<br>➡️ Open your browser and go to `http://magic.magic-ug.org:{your_port}`  

You should see something like this:  
![server array](images/return.PNG)  



## 🚀 Congratulations!
We are now able to return the employee data and length as JSON!
Now let's do the same thing for the departments.