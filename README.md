# quiz

In this assessment, we will be simulating a stripped down version of a claims app.
In this app, there are two components, the `Report` and the `Expense`. Expenses represent the individual
receipts that the user wants to claim. Each expense needs to be added to a report. The structure of the
application has been provided, with basic routing implemented and the MVC structure applied to the file
directory.

Your task is to complete the implementation so that a user can add a new report with a maximum of 3 expenses
to the database. When the report has been added, the table should be updated to reflect the new record.

## Instructions
1. The first step that you will want to take is to implement a new route so that when the form is submitted 
    the data in the POST request can be extracted. 
2. Next, you will need to create a new method within the `ReportController` to take the user input and then insert
    it into the database. 
   1. If the expense is less than or equals to zero, you shouldn't create the expense in the database. 
   2. Think of the creation of the report and expenses as one transaction. Make sure that all database executions are successful before committing to the database. 
3. Once the data has been successfully written to the database you will need to return a view which shows the updated table.
4. You will want to pass an additional param to the view (most likely an array), check if the array is not empty,  and then implement some kind of loop in the view file to render each row accordingly. 
   1. To make it simpler, you can assume that the table only displays reports that have one or more expenses. 
   2. If you're having a hard time getting the `No of expenses` and `Amount` values to return, you can hardcode a "`-`" 
       and just make sure the row is being reflected on the table. 
5. You will only need to modify the `/public/index.php`, `/views/index.php` and the `ReportController`. 
   Please do not modify the other files. 
6. Refer to all of the `todo` comments if you need some direction on where to work on next. 
7. Additionally, please note the following changes to the code:

````
// to create and use a new db connection
$db = App::db();
$db->prepare();

// to return a view and pass params from the controller
return View::make('index', ['foo' => $foo])
````
## Extra Mile
1. Refactor the code so that the database logic is within it's own `Model`:
   1. create a `Report` and `Expense` model with methods for `create` and `fetch`
   2. `create` should write to the database and return the `id` (Both report and expense model).
   3. `fetch` should run a `select` query and return the results (as an array) to the `Controller` method.
2. Format the date column so that it follows the following format : `16, Apr 1983`.

## Setup

1. Once the repository is cloned, change your document root to the `/public` directory.
2. Create a database, and create the following tables via the sql script below: 

````
CREATE TABLE reports (
  id int unsigned PRIMARY KEY AUTO_INCREMENT,
  title varchar(150) NOT NULL,
  status boolean DEFAULT 0 NOT NULL,
  date datetime NOT NULL
);

CREATE TABLE expenses (
  id int unsigned PRIMARY KEY AUTO_INCREMENT,
  amount DECIMAL(10,2),
  report_id int unsigned NOT NULL,
  FOREIGN KEY (report_id) REFERENCES reports (id)
  ON DELETE CASCADE
);
````
3. Create a `.env` file and fill in the Database details.
4. CD to the project directory and run `composer install`.
5. Open up the app on your browser and make sure you're able to see the homepage which should show the `Claim Reports` heading. 

### Example Outcome
![img.png](example.png)