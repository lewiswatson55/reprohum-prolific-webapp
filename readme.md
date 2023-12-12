
# Flask Server for Prolific Tasks

> This code is in development and has not been fully tested. Please use with caution.

## Required files:
- data.csv - this is the datafile containing the rows of data for each task
- templates/interface.html - this is the interface for the HIT
- database.db - This file will be created once you run the CreateDatabase.py script with the correct number of tasks (see below). 

## Setup:

1. First, CreateDatabase.py should be run first to create the database and tables. You must first update the NUMBER_OF_TASKS variable (this is the number of rows of the input data, minus headers) and update the COMPLETIONS_PER_TASK variable to the number of completions you want per task. This is the number of times each task will be completed by a worker. The default is 3.
2. Ensure all required files (see above) have been added for the new task.
3. Update main.py's MAX_TIME variable to the maximum time you want the task to run for. The default is 60 minutes (3600 seconds). This should be the same max time entered in Prolific.
4. Add your task interface html file to /templates/ as required (see "Task Interface Changes" below).
5. Update TEMPLATE and DATA variables in main.py to the correct file names. 
5. When ready, you will need to deploy the flask app to a server.


## Task Interface Changes

1. There are a few changes that need to be made to the task interface HTML file to ensure the data is collected correctly. There is also a provided generic.js file. You will need to add this to your task interface HTML file like so:
```HTML
    <script src="{{url_for('static', filename='js/generic.js')}}"></script>
```

2. We must add a submit button that will collate the worker entered data and send it as JSON POST request to the server running the flask app.
   See the example collectData function in generic.js. **Make sure to include the task_id, prolific_pid, and session_id in the JSON data.**
   This can be done by the following code: 
    ```JS 
   var task_id = document.getElementById("task_id").innerHTML;
   var prolific_pid = document.getElementById("prolific_pid").value;
   var session_id = document.getElementById("session_id").value;
   ```

  3. Optional: Depending on your task, you may wish to include an alert for warning the worker when they have run out of time. There is also example code in generic.js for this. See startFailedCountdown(). There is also example code to alert the worker when they have 10 minutes left, etc.
