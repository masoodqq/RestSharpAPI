# RestSharpAPI

This is the SSIS package that downloads data from external source in JSON format and transforms the data into csv format file.

<img src="/worksummariesAPIgraph.PNG"  style="max-width:100%;">

EmploymentDetails API is a script task component. A script task component in SSIS pacakge
provides an ability write C# code. The script task component comes in hand when calling
external API.

In this package, there are 3 script task component used.

<ol>
  <li>EmploymentDetails API</li>
  <li>Create Header Record WorkSummaries File</li>
  <li>WorkSummaries API</li>
</ol>

There is no difference between EmploymentDetails API and WorkSummaries API
in terms of transforming the JSON format to csv format. The transformation process is identical and the code is listed in worksummariesapicode.txt

Create Header Record WorkSummaries File, as the name indicates writes a header record in the
text file, this file is then opened in the WorkSummaries API and writes the data from the API.

Data extracting starts by downloading all the employees by running the
EmploymentDetails API in a text file, it is then loaded in a staging table stg_employment_details. Before inserting the data the table is truncated.

The next step in control flow is to run Create Header Record WorkSummaries File script task.
Then the flow is moved to selecting all the employee numbers that we extracted earlier.

Select EmployeeNumbers box is executing the follwoing SQL statement <br>
SELECT EmployeeNumbers
FROM EmploymentDetails

The output from the Select EmployeeNumbers task is written in a variable of type Object, this variable is an input for the Foreach Loop Container and the WorkSummariesAPI is called
as many times as there EmployeeNumbers.

The code for WorkSummaries API is in file worksummariesapicode.txt

The JSON response from the WorkSummaries API is in <a class="js-navigation-open link-gray-dark" title="work_summaries.json" href="https://github.com/masoodqq/RestSharpAPI/blob/main/work_summaries.json">work_summaries.json</a>
