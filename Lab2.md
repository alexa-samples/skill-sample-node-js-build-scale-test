## Lab 2:
### Pre-Requisites

**IMPORTANT:  Make sure you are on the 'EU (Ireland)' AWS Region**

The back end hosted as a Lambda function
you built in [Task 1](/Lab1.md#task-1-deploy-code-to-lambda)

--------

### Task 3: Create a DynamoDB for Storing facts
  1. In a separate browser tab go to [the Amazon Dynamo DB Dashboard](https://eu-west-1.console.aws.amazon.com/dynamodb)
  2. Click on 'Create table'
  3. In the 'table name' field type `ALX402FactDB`
  4. For 'Partition key' use 'FactId'
      1. Leave the type set to 'String'
  5. Leave 'Use Default Settings' checked
  6. Click the 'Create' button at the bottom of the page. (This can take a couple of minutes.)
  7. On the right hand side click on the 'Indexes' tab
  8. Click on the 'Create Index' button
  9. Type `locale` into the 'Primary key' field
      1. Leave the type set to 'String'
      2. The index name should have auto populated to 'locale-index'
  10. Don't change any of the other settings
  11. Click the 'Create Index' button
      1. This process will take about 5 minutes
      2. The 'Status' will change to 'Active' once it's done
  12. Click on the 'Overview' tab
  13. Copy the 'Amazon Resource Name (ARN)' for the table, paste it to a text file as we will use it later
      1. It is located at the **bottom** of the page
#### Verify: Add an item to the table
  14. Click on the 'Items' tab
  15. Click the 'Create item' button
  16. In the new form that appears, do the following:
      1. Type in `0001` for 'FactId'  
      2. Type in `en-US` for 'locale'
      2. Click the '+' button and append a string.
      3. Type `speech` for the 'Field' and `Manually added fact` for the 'Value'
      4. Click the '+' button and append an additional string.
      5. Type `text` for the 'Field' and `Text for manually added fact` for the 'Value'
  17. Click 'Save' button
  18. You now have a properly formatted item stored in DynamoDB

--------

### Task 4: Modify Lambda to read from the new DynamoDB table
  1. In a new tab, navigate to the [AWS Identity Access Management Service](https://console.aws.amazon.com/iam)
  2. Click on 'Roles' on the left hand side
  3. Click on the **name** 'ALX402-Role'
  4. Click 'Attach Policy' button
  5. Type `DynamoDBReadOnlyAccess` in the search bar
  6. Click on the **checkbox** next to 'AmazonDynamoDBReadOnlyAccess'
  7. Click 'Attach Policy' button
  8. Go back to the Lambda service and select the function called 'ALX402' (We created it in [Task 1](/Lab1.md#task-1-deploy-code-to-lambda))
  9. Replace the 'GetFact' function with the following code:
      1. The function should start on line 115
  ```javascript
  'GetFact': function () {
  let requestLocale = this.event.request.locale; //get locale from request for our DB secondary index
  let AWS = require('aws-sdk'); //instantiate the aws sdk
  let docClient = new AWS.DynamoDB.DocumentClient(); // create a DynamoDB client
  let params = { // Parameters object for a query that will return all facts for a locale
    TableName : "ALX402FactDB",
    IndexName: "locale-index",
    KeyConditionExpression: "locale = :v1",
    ExpressionAttributeValues: {
      ":v1": requestLocale
    }
  };

  let dynamoQuery = docClient.query(params).promise();//create the query and wrap in a promise
  dynamoQuery.then(fulfilled.bind(this), rejected.bind(this));//execute query asynchronously

  function fulfilled(value) {//function for handling a fulfilled promise
    console.log(value);
    const factArr = value.Items;//array form DynamoDB
    const factIndex = Math.floor(Math.random() * factArr.length); //random int bounded by length of array
    const randomFact = factArr[factIndex]; //get the item that matches the random number
    const speechOutput = randomFact.speech; //get the speech value form the item
    const textOutput = randomFact.text; //get the text value form the item
    this.emit(':tellWithCard', speechOutput, 'ALX 402', textOutput);//send a response to the alexa service
  };
  function rejected(reason) {//function for handling a rejected promise
    console.log(reason);
    this.emit(':tell', "An Error occurred, please check the console log");
  };
},
  ```
#### Verify: Test the code in the lambda console
  10. Make sure the 'StartSession' event you created is selected
  11. Click on 'Save and Test' on the top right
  12. Click the arrow next to 'Details' and review the object being sent back
  13. Review the content of the console log

--------

### [Lab 3](/Lab3.md)

--------

### Extra Credit:
  1. Remove all the hardcoded facts, but leave the other constants
