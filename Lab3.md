## Lab 3:
### Pre-Requisites

The Voice UI you built in
[Task 2](/Lab1.md#task-2-build-a-voice-ui)

--------

### Task 5: Create IAM role for our API
  1. In a new browser tab click on 'Services' (on the top left)
  2. In the search bar type 'IAM' and hit return
      1. You'll be presented with the [IAM Dashboard](https://console.aws.amazon.com/iam/home?region=eu-west-1#/home)
  3. Click on 'Roles' on the left navigation bar
  4. Click the 'Create role'
  5. Make sure 'AWS service' is selected as the 'type of trusted entity'
  6. Select 'API Gateway' as the service that will use the role
  7. Click 'Next: Permissions'
  8. Click 'Next: Review'
  9. Type 'ALX402API-Role' as the 'Role name'
  10. Click on "Create role"
  11. You'll be presented with the list of roles, click on the one you created ('ALX402API-Role')
  12. Click 'Add inline policy' (on the bottom right)
  13. Make sure 'Policy Generator' is selected and click the 'Select' button
  14. Select the 'Allow' radio button
  15. From the 'AWS Service' dropdown select 'DynamoDB'
  16. In the 'Actions' dropdown check the boxes for:
      1. 'GetItem'
      2. 'PutItem'
      3. 'Query'
  17. Paste the ARN for the 'ALX402Facts' DynamoDB table you created in Task 2
      1. You should have it in a text file, if not you can find it under the 'Overview' tab for your table
  18. Click 'Add Statement'
  19. Review the confirmation
  20. Click 'Next Step'
  21. Policy should look as follows:

  ```javascript
  {
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Stmt1511207534000",
            "Effect": "Allow",
            "Action": [
                "dynamodb:GetItem",
                "dynamodb:PutItem",
                "dynamodb:Query"
            ],
            "Resource": [
                "The ARN for you DynamoDB Table should be here"
            ]
        }
    ]
  }
  ```

  22. Click 'Apply Policy'
  23. You will be presented with a 'Summary' page
#### Verify:
  24. Make sure a policy that has 'policygen' as part of its name is present
      1. You can confirm the JSON object by clicking the arrow next to it
  25. Copy the Role ARN, for this role we will need it in the next task
      1. 

--------

### Task 6: Add an API Gateway to handle writing new facts
  1. Navigate to the ['API Gateway' Dashboard](https://eu-west-1.console.aws.amazon.com/apigateway)
  2. Click on the 'Create API' button or the 'Get Started' button
      1. If presented with a confirmation pop-up click 'OK'
  3. Select the 'New API' radio button
  4. Set 'API name' to 'ALX402FactAPI'
  5. Leave the 'Endpoint Type' set to 'Edge optimized'
  6. Click on the 'Create API' button
  7. Select 'Create Method' from the Actions dropdown
  8. From the new dropdown select 'Post'
      1. Click the checkmark button
  10. From the right hand pane select the 'AWS Service' radio button
  11. Select 'eu-west-1' as the 'AWS Region'
  12. Select 'DynamoDB' as the 'AWS Service'
  13. Leave the 'Subdomain' field blank
  14. Select 'Post' as the 'HTTP method'
  15. Leave the 'Use action name' radio button selected
  16. In the 'Action' field type 'PutItem'
  17. Paste the Role ARN you created in Task 5, into the 'Execution role' bfield
  18. Leave 'Content Handling' set to 'Passthrough'
  19. Click the save button
  20. You'll be presented with a 'Method Execution' page
  21. Click on 'Integration Request'
  22. Expand 'Body Mapping Templates' (at the bottom)
  23. Click on 'Add mapping template'
  24. Type 'application/json' as the 'content type'
  25. Click the gray check mark button
  26. Select 'Yes, secure this integration' from the pop-up that will appear
  27. Click 'Add mapping'
  28. Add the following mapping in the form that appeared at the bottom

  ```javascript
  {
    "TableName": "ALX402FactDB",
    "Item": {
        "FactId": {
            "S": "$context.requestId"
            },
        "locale": {
            "S": "$input.path('$.locale')"
        },
        "speech": {
            "S": "$input.path('$.speech')"
        },
        "text": {
            "S": "$input.path('$.text')"
        }
    }
}
  ```

  29. Click 'Save'
#### Verify:
  30. Scroll back to the top
  31. Click '<- Method Execution'
  32. Click 'Test'
  33. Paste the following objet in 'Request Body'

```javascript
    {
      "locale":"en-US",
      "speech":"Added from API test",
      "text":"Text for Added from API test"
    }
```

  33. Click ':zap: Test' button
  34. On the top right you should see 'Status: 200'
  35. Make sure the 'Response Body' is '{}'
  36. There will be an item in you DynamoDB table with 'test-invoke-request' as it's 'FactId'

--------

### [Lab 4](/Lab4.md)

--------

### Extra Credit 1 : Add a Get method that returns all the facts  
### Extra Credit 2 : Add a method to edit a single fact by providing it's FactId

--------
