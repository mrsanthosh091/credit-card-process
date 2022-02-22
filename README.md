# credit-card-process

Steps to start the application:

1. Unzip the credit-card-process.zip
2. Import it into eclipse
3. Run the pom.xml file with goals (clean install)
4. Right click on the Application.java and run as java application.
5. Wait until the server starts up and you see the below line:
Tomcat started on port(s): 8080
6. Open up a browser window and type localhost:8080 in address bar
7. You will be granted with Camunda login screen
8. Login with default username and password (demo, demo)
9. Click on Tasklist.

Steps to test the API's:

1. API to return list of products:
	a. Open a command prompt and enter the below line of code
		curl -X GET http://localhost:8080/CreditCard/getProducts

	b. The webservice returns the output as shown below:
		["Bronze-CreditCard","Gold-CreditCard","Platinum-CreditCard"]

2. API to capture the user selected input:

	a. Open a command prompt and enter the below line of code
	
		curl -H "Content-Type: application/json" -X POST -d {\"firstName\":\"Santhosh\",\"lastName\":\"Kumar\",\"email\":\"m_mrsanthosh@yahoo.com\",\"address\":\"Titaniumstraat10\",\"pincode\":\"7335CC\",\"city\":\"Apeldoorn\",\"country\":\"Netherlands\",\"selectedProduct\":\"Gold-CreditCard\"} http://localhost:8080/CreditCard/selectProduct

	b. The webservice returns the output as shown below:
		
		Thank you for your order! Kindly note the transaction ID: c8e22774-936f-11ec-af05-005056c00008 for future references. You will receive an email once the product is dispatched.
		
	c. The transaction id returned is the process instance id in camunda. This API triggers a process in Camunda to process the credit card for the user.
	

3. Navigate to the Camunda Takslist with the url http://localhost:8080/camunda/app/tasklist/default/#

4. There would be a process instance to Validate the request.

5. Claim the task and select Approve in status.
	If you select Reject in the status, the process is terminated.

6. After approval, the task moves to "Submit for printing" step. Claim the task and complete.

7. After printing, the task moves to "Submit for Delivery" step. Claim the task and complete.

8. After delivery, the task moves to "Check Order Status" step. Claim and validate the order status and complete the task.

9. The process is complete.


Architectural Decisions:

1. Used Camunda spring boot starter to download the intial configuration of the application from the below site. This would make sure all the dependencies are in place.
https://start.camunda.com/

2. Used springboot rest services to implement the GET and POST API's.

3. Created the camunda process using bpm modeler.

4. The webservice that selects the card, invokes a Camunda process instance. This invocation is done by using startProcessInstanceByKey method. This would make sure the lastes version of process is invoked at all times.

5. The API's to registerCard and createCard is developed as Java Delegate. This is called directly from the process definition.

6. Created the application with minimal logic. So nothing is saved in database as its a assignment application.
