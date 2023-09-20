## Hands-On Exercise 1 - DQL Optimization

With the introduction of Grail and DQL, it's important to pay attention to the quality of the queries you and your customer write.  <u>Customers, and you as their trusted advisor, should be consious of how DQL queries affect their consumption. </u>

To help with this, Dynatrace stores data on query executions in Grail, accessible via the **dt.system.query_executions** data object.

In this hands-on exercise, you will practice writing a cost-tracking query you can use in your own environments and with your customers.

<br>

### Step 1: Fetch a Sample of Query Execution Data

To understand what fields are available in a query_executions record, you can start by querying a sample of the data.  

_**Note**: A limit should be used as query\_executions is a large dataset and we are only looking for a sample._

**Run the following query and review the results:**
```
fetch dt.system.query_executions
| limit 10
```

**Questions to think about:**
- What fields tell me about the cost of this query?  
- Is there more than one way to think about query cost?
- What fields could I use to find where this query was run?


**Expected Results:**

![Query Executions Data Sample](../../assets/images/Query_Executions_Sample.png)

<br>
<br>

### Step 2: Write a Query to Provide Practical Results

Now that you know what fields are included in the query_executions data, you can write specific queries to find costly queries, monitor the consumption of popular dashboards, and more.

In this exercise, you will write a query to fetch **your own** queries run in notebooks in this environment and their relevant cost metrics: execution duration and scanned GB.

<b>
Paste the following query into your notebook and make the following changes:

- Enter your email to filter to your queries
- Choose whether to sort results by scanned GB or execution duration
- Choose the table visualization
</b>


```
fetch dt.system.query_executions
| filter user == "[YOUR EMAIL HERE]" and client.application_context == "dynatrace.notebooks"
| fieldsAdd scanned_gb = scanned_bytes / 1000000000.0
| fields timestamp, execution_duration_ms, scanned_gb, query_string, user
| sort [INSERT METRIC HERE] desc
```

**Review the results**

<br>


_**Note:** query\_executions does not provide a scanned GB metric, but you can return this value by converting from bytes using a mathematical operation._

<br>


**Expected Results:**

![Query Executions Filtered Table](../../assets/images/Query_Executions_Table.png)




