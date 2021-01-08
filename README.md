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
  <li>Script Task</li>
</ol>
