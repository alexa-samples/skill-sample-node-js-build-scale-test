## Lab 5: Scaling and proactive monitoring

### Pre-Requisites
  The Skill we created in Tasks [1](/Lab1.md#task-1-deploy-code-to-lambda) & [2](/Lab1.md#task-2-build-a-voice-ui)

  The DynamoDB table we created in [Task 3](/Lab2.md#task-3-create-a-dynamodb-for-storing-facts)

--------
### Task 9: DynamoDB Scaling
  1. Open the [DynamoDB Dashboard](https://eu-west-1.console.aws.amazon.com/dynamodb/home?region=eu-west-1)
  2. Click on 'Tables' in the left navigation pane
  3. Click on the name of the table you created in **task 3**
  4. Click on the 'Capacity' Tab in the right hand pane
  5. Check the boxes for 'Read capacity' & 'Write capacity' under the 'Auto Scaling' header
  6. The default settings should suffice
      1. Be mindful of the high 'Maximum provisioned capacity'
  7. You will require a new **IAM** role to **authorize** capacity scaling
      1. Select the radio button labeled 'New Role: DynamoDBAutoscaleRole'
  8. Click the blue 'Save' button
#### Verify:
  9. The radio button for 'Existing role with pre-defined policies' should be selected
  10. The field 'Role Name' Should read 'DynamoDBAutoscaleRole'
  11. The rest of the settings should have remained as you selected them
--------

### Task 10: Create an SNS topic to notify us when an alarm triggers
  1. Open the [SNS Dashboard](https://eu-west-1.console.aws.amazon.com/sns) in a new browser tab
  2. Click on 'Create a topic'
  3. Type `ALX402-Alarm` as the 'Topic Name' &  `ALX402` as the 'Display Name'
  4. Click the blue 'Create Topic' button
  5. Copy the **ARN** for your Topic
  6. Click the grey 'Create Subscription' button
  7. Confirm the the 'Topic ARN' matches the one you copied
  8. Click the grey 'Create Subscription' button
  9. Chose 'SMS' or 'Email' from the 'Protocol' dropdown
  10. Type your phone number or email in the 'Endpoint' field
      1. Don't forget the **Country Code** if using SMS
  11. Click the blue 'Create subscription' button
#### Verify:
  12. A green confirmation dialog will briefly appear
  13. Create the blue 'Publish to topic' button
  14. Fill out the title and message fields & leave everything else set to the default values
  15. Click the blue 'Publish message' button
  16. You should get an SMS or Email after a couple of seconds

--------

### Task 11: Set alarms for DynamoDB and Lambda
  1. Open the [DynamoDB Dashboard](https://eu-west-1.console.aws.amazon.com/dynamodb/)
  2. Click on 'Tables' from the left navigation bar
  3. Click on the **name** of the table you created in Task 3
  4. Click on the 'Alarms' Tab on the right pane
  5. Click the blue 'Create alarm' button
  6. Make sure the 'Send a Notification to SNS topic' checkbox is selected
  7. Select the 'ALX402-Alarm' topic you created in **Task 10** from the 'SNS Topic' dropdown
  8. From the 'Whenever' dropdown select 'Consumed write capacity'
  9. Select *'>='* from the 'Is' dropdown
  10. Type `5` in the 'Average Unit/Second' field
  11. Leave the 'For at least' field set to '1' and the 'consecutive periods' set to '5 minutes'
  12. Type a descriptive name in the 'Name of Alarm' field
  13. Click the blue 'Create alarm' button
#### Verify:
  14. Your Alarm should be listed under the 'Alarms' tab for that table

--------

### Extra Credit 1: Create an artificially low alarm and write a script to trigger it

### Extra Credit 2: Add Lambda Alarms through CloudWatch

### Extra Credit 3: Use X-Ray to map your infrastructure
