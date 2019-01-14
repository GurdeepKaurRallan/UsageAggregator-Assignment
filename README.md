## UsageAggregator

## How the project is developed

The project named as "UsageAggregatorService" is developed in Java and the APIs "reportUsage" and "getAggregatedUsage" are exposed using RestWebService, Jersey, hibernate framework and MySql Database.
 
A table named as usageData gets created in mysql database with following columms:
```
	id INT PrimaryKey
	subscriberId INT
	amount INT
	timestamp VARCHAR
  
```

Description of Java files and configuration files
- UsageData.java - Entitiy class mapped to usagedata table in database using hibernate framework.
- AggregatorService.java - Contains the service logic for database connection, sessionFactory and CRUD operations with MySql database.
- AggregatorWebService.java - Contains the REST APIs.
- Output.java - To return the success JSON response of reportUsage API.
- AggregatedUsage.java - To return the response of getAggregatedUsage API. JSON Response contains day and totalUsage for a given subscriber.

- UsageAggregatorService\WebContent\WEB-INF\lib directory contains all the dependencies like jersey, hibernate, mysql-connector.
- UsageAggregatorService\WebContent\WEB-INF\web.xml - Contains jersey servlet configurations to run REST requests.
- UsageAggregatorService\src\hibernate.cfg.xml - configuration file for datbase related parameters like driver class, connection url, username, password, database table auto creation/updation.

## Assumptions

- subscriberId and amount should be greater than zero.
- timestamp cannot be null.
- Status 204 No Content is set in case of following scenarios while excecuting APIs.
	1) If the JSON payload in not provided for both APIs.
	2) If the subscriberId/amount is set to zero or timestamp is set to null in JSON payload of reportUsage API.
	3) If the subscriberId is set to zero in JSON payload of getAggregatedAPI.
	4) If the resposne is not success, check logs.

## Prerequisites to run the application 

1) JDK 1.7
 
2) Eclipse IDE (if you want to run the project using eclipse)
   Unzip UsageAggregatorService.zip and Import the project "UsageAggregatorService" into workspace

3) Apache Tomcat 7.0
   Install Apache Tomcat, add the server in Eclipse and give the path at which Tomcat directory is present after installation

4) MySql 5.5 
   Install MySql server using mysql-5.5.62-winx64.exe and follow the steps with default values.
   Set password as "root".
   
5) Postman to execute REST requests.


## How to Run the project using Eclipse

1) Right click on UsageAggregatorService in eclipse and Run as "Run on Server". Tomcat configured in eclipse will get started and run the application.

2) Launch Postman

   To Run reportUsage API :
   - Put URL http://localhost:8080/UsageAggregatorService/UsageAggregator/AggregatorWebService/reportUsage
   - Select POST
   - Select "Body" (below URL field) , select "raw" radio button and select JSON(application/json) from drop-down.
   - Enter data as:
    ```
    {
      "subscriberId" : 5420123,
      "amount": 1024000,
      "timestamp": "2018-06-04T20:30:00.000-04:00"
    }
    ```
   - Click Send.
   - Output would be:
   ```
       {
          "status": "Success"  
       }
   ```
   To Run getAggregatedUsage API
   - Put URL http://localhost:8080/UsageAggregatorService/UsageAggregator/AggregatorWebService/getAggregatedUsage
   - Select POST
   - Select "Body" (below URL field) , select "raw" radio button and select JSON(application/json) from drop-down.
   - Enter data as:
   ```
       {
          "subscriberId": "5420123"  
       } 
   ```
   - Click Send.
   - Output would be:
   ```
        {
           "day": "2018-06-04",
           "totalUsage": 1024000
        }
   ```
3) Check eclispe console for errros if the Status in Postman is other than Status 200 Ok.

## How to Run the project without Eclispe

1) Copy UsageAggregatorService.war to Tomcat webapps directory e.g C:\Program Files (x86)\Apache Software Foundation\Tomcat 7.0\webapps\

2) Start Tomcat. Go to C:\Program Files (x86)\Apache Software Foundation\Tomcat 7.0\bin, run Tomcat7W , select Startup Type "Manual" and click start.

3) Launch Postman and follow the above mentioned steps of Postman.

4) Check Tomcat logs for errors if the Status in Postman is other than Status 200 Ok. Path C:\Program Files (x86)\Apache Software Foundation\Tomcat 7.0\logs\tomcat7-stdout.2019-01-14


Database Entries can also be verified using MySql Command Line Client
```
   password : root
   use mysql;
   select * from usagedata;
```






