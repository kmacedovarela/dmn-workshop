Test DMN Solution
=================

In this section, you will test the DMN solution with KIE Server’s Swagger interface.

The Swagger interface provides the description and documentation of the Execution Server’s RESTful API. At the same time, it allows the APIs to be called from the UI. This enables developers and users to quickly test, in this case, a deployed DMN Service.

1.  Navigate to [KIE Server](http://localhost:8080/kie-server/docs)

2.  Locate the **DMN Models** section. The DMN API provides the DMN model as a RESTful resources, which accepts 2 operations:

    1.  `GET`: Retrieves the DMN model.

    2.  `POST`: Evaluates the decisions for a given input.

3.  Expand the `GET` operation by clicking on it.

4.  Click on the **Try it out** button.

5.  Set the **containerId** field to `vacation-days-decisions` and set the **Response content type\* to `application/json` and click on **Execute\*\* ![DMN Swagger Get]({% image_path dmn-swagger-get.png %})

6.  If requested, provide the username and password of your **Business Central** and **KIE-Server** user.

7.  The response will be the model-description of your DMN model.

Next, we will evaluate our model with some input data. We need to provide our model with the **age** of an employee and the number of **years of service**. Let’s try a number of different values to test our deicions.

1.  Expand the `POST` operation and click on the **Try it out** button

2.  Set the **containerId** field to `vacation-days-decisions`. Set the **Parameter content type** and **Response content type** fields to `application/json`.

3. Pass the following request to lookup the number of vacation days for an employee of 16 years old with 1 year of service (note that the namespace of your model is probably different as it is generated. You can lookup the namespace of your model in the response/result of the `GET` operation you executed ealier, which returned the model description). 

   ````
        { "dmn-context":{ "Age":16, "Years of Service":1 } }
   ````

4. Click on **Execute**. The result value of the `Total Vacation Days` should be 27.

5. Test the service with a number of other values. See the following table for some sample values and expected output.

<table><colgroup><col style="width: 33%" /><col style="width: 33%" /><col style="width: 33%" /></colgroup><tbody><tr class="odd"><td><p>Age</p></td><td><p>Years of Service</p></td><td><p>Total Vacation Days</p></td></tr><tr class="even"><td><p>16</p></td><td><p>1</p></td><td><p>27</p></td></tr><tr class="odd"><td><p>25</p></td><td><p>5</p></td><td><p>22</p></td></tr><tr class="even"><td><p>44</p></td><td><p>20</p></td><td><p>24</p></td></tr><tr class="odd"><td><p>44</p></td><td><p>30</p></td><td><p>30</p></td></tr><tr class="even"><td><p>50</p></td><td><p>20</p></td><td><p>24</p></td></tr><tr class="odd"><td><p>50</p></td><td><p>30</p></td><td><p>30</p></td></tr><tr class="even"><td><p>60</p></td><td><p>20</p></td><td><p>30</p></td></tr></tbody></table>

Using the KIE-Server Client
===========================

Red Hat Decision Manager provides a KIE-Server Client API that allows the user to interact with the KIE-Server from a Java client using a higher level API. It abstracts the data marshalling and unmarshalling and the creation and execution of the RESTful commands from the developer, allowing him/her to focus on developing business logic.

In this section we will create a simple Java client for our DMN model.

1.  Create a new Maven Java JAR project in your favourite IDE (e.g. IntelliJ, Eclipse, Visual Studio Code).

2. Add the following dependency to your project: 

   ````
	   <dependency> 
	     <groupId>org.kie.server</groupId> 
	     <artifactId>kie-server-client</artifactId> 
	     <version>7.18.0.Final</version> 
	     <scope>compile</scope> 
	   </dependency>
   ````

3. Create a Java package in your `src/main/java` folder with the name `org.kie.dmn.lab`.

4. In the package you’ve just created, create a Java class called `Main.java`.

5. Add a `public static void main(String[] args)` method to your main class.

6. Before we implement our method, we first define a number of constants that we will need when implementing our method (note that the values of your constants can be different depending on your environment, model namespace, etc.): 

   ````java
   private static final String KIE_SERVER_URL = "http://localhost:8080/kie-server/services/rest/server"; 
   private static final String CONTAINER_ID = "vacation-days-decisions_1.0.0"; 
   private static final String USERNAME = "pamAdmin"; 
   private static final String PASSWORD = "redhatpam1!"; 
   private static final String DMN_MODEL_NAMESPACE = "https://github.com/kiegroup/drools/kie-dmn/_D0E62587-C08C-42F3-970B-8595EA48BEEE";
   private static final String DMN_MODEL_NAME = "vacation-days";
   ````

7. KIE-Server client API classes can mostly be retrieved from the `KieServicesFactory` class. We first need to create a `KieServicesConfiguration` instance that will hold our credentials and defines how we want our client to communicate with the server: 

   ```java
   KieServicesConfiguration kieServicesConfig = KieServicesFactory.newRestConfiguration(KIE_SERVER_URL, new EnteredCredentialsProvider(USERNAME, PASSWORD)); 
   ```

8. Next, we create the `KieServicesClient`: 

   ```java
   KieServicesClient kieServicesClient = KieServicesFactory.newKieServicesClient(kieServicesConfig); 
   ```

9. From this client we retrieve our DMNServicesClient: 

   ```java
   DMNServicesClient dmnServicesClient = kieServicesClient.getServicesClient(DMNServicesClient.class); 
   ```

10. To pass the input values to our model to the Execution Server, we need to create a `DMNContext`: \`\`\` 

    ```java
    DMNContext dmnContext = dmnServicesClient.newContext();
    dmnContext.set("Age", 16); dmnContext.set("Years of Service", 1);
    ```

    

11. We now have defined all the required instances needed to send a DMN evaluation request to the server: 

    ```java
    ServiceResponse<DMNResult> dmnResultResponse = dmnServicesClient.evaluateAll(CONTAINER_ID, DMN_MODEL_NAMESPACE, DMN_MODEL_NAME, dmnContext);
    ```

12. Finally we can retrieve the DMN evaluation result and print it in the console:

    ```java
    DMNDecisionResult decisionResult = dmnResultResponse.getResult().getDecisionResultByName("Total Vacation Days"); System.out.println("Total vacation days: " + decisionResult.getResult()); 
    ```

    

13. Compile your project and run it. Observe the output in the console, which should say: **Total vacation days: 27**

    

The complete project can be found here: <https://github.com/DuncanDoyle/vacation-days-dmn-lab-client>