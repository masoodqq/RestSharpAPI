<H1> RestSharpAPI </H1>

SSIS stands for SQL Server Integeration Service. This is a ETL tool for creating data piplines provided by Microsoft.

This SSIS package downloads data from external source in JSON format and transforms the data into csv format file.

<img src="/worksummariesAPIgraph.PNG"  style="max-width:100%;">

This package uses 3 script task components.

<ol>
  <li>EmploymentDetails API</li>
  <li>Create Header Record WorkSummaries File</li>
  <li>WorkSummaries API</li>
</ol>

A script task component in SSIS pacakge provides an ability write C# code. The script task component comes in handy when calling external API.

EmploymentDetails API and WorkSummaries API are 2 components in which external API is called.<br>
The API retruns JSON response and in terms of transforming the JSON format to csv format. The transformation process is identical and the code is listed in <a class="js-navigation-open link-gray-dark" title="work_summariesapicode.json" href="https://github.com/masoodqq/RestSharpAPI/blob/main/worksummariesapicode.txt">worksummariesapicode.txt</a>

Create Header Record WorkSummaries File, as the name indicates writes a header record in the
text file, this file is then opened in the WorkSummaries API and writes the data from the API response.

Data extracting starts by running the EmploymentDetails API and writing employees in a text file, it is then loaded in a staging table stg_employment_details. Before inserting the data, the table is truncated.

The next step in control flow is to run Create Header Record WorkSummaries File script task.
Then the flow is moved to selecting all the employee numbers that we extracted earlier.

Select EmployeeNumbers task is executing the follwoing SQL statement <br>
SELECT EmployeeNumbers <br>
FROM EmploymentDetails

The output from the Select EmployeeNumbers task is stored in a variable of type Object, this variable is an input for the Foreach Loop Container and the WorkSummariesAPI is executed inside the container as many times as there are EmployeeNumbers. The parameters for this API are employee number and a date range, explained in the code.

The code for WorkSummaries API is in file <a class="js-navigation-open link-gray-dark" title="work_summariesapicode.json" href="https://github.com/masoodqq/RestSharpAPI/blob/main/worksummariesapicode.txt">worksummariesapicode.txt</a>

The JSON response from the WorkSummaries API is in <a class="js-navigation-open link-gray-dark" title="work_summaries.json" href="https://github.com/masoodqq/RestSharpAPI/blob/main/work_summaries.json">work_summaries.json</a>

After the WorkSummaries API is finished then the stg_work_summaries table is truncated and the data is inserted.
Further data transformation is done by executing stored procedures InsertIntoWorkSummaries and InsertIntoDailyLabor.

<H1> Resources </H1>

<a class="js-navigation-open link-gray-dark" href="https://restsharp.dev/">Restsharp</a> <br>
<a class="js-navigation-open link-gray-dark" href="https://www.newtonsoft.com/json">Newtonsoft</a> <br>
<a class="js-navigation-open link-gray-dark" href="https://docs.microsoft.com/en-us/sql/integration-services/integration-services-programming-overview?view=sql-server-ver15">Integration Services Programming Overview</a>
<br>
<a class="js-navigation-open link-gray-dark" href="https://docs.microsoft.com/en-us/sql/integration-services/control-flow/script-task?view=sql-server-ver15">Script Task</a> <br>
<a class="js-navigation-open link-gray-dark" href="https://docs.microsoft.com/en-us/sql/integration-services/extending-packages-scripting-task-examples/script-task-examples?view=sql-server-ver15">Script Task Examples</a> <br>
