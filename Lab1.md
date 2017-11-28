## Lab 1:
### Pre-Requisites

An [AWS Account](https://aws.amazon.com/)

**IMPORTANT:  Make sure you are on the 'EU (Ireland)' AWS Region**

A [Developer Portal Account](https://developer.amazon.com/)

--------

### Task 1: Deploy Code to Lambda
  1. In a new browser window go to [console.aws.amazon.com](http://console.aws.amazon.com)
  2. Sign in with your credentials.
  3. Type `Lambda` in the search bar at the top & hit *enter*
  4. Click on the orange 'Create function' button in the top right corner or on the blue 'get started' button
  5. Type `Fact` in the Blueprint search bar & hit *enter*
  6. Click on 'alexa-skill-kit-sdk-factskill'
  7. Type `ALX402` for your function name
  8. From the 'Role' dropdown select 'Create new role from template(s)'
  9. Type `ALX402-role` for your role name
  10. Select 'Simple Microservices Permissions' from the 'Policy templates' dropdown
  11. Scroll to the bottom of the page and click the orange 'Create function' button.
#### Verify:
  12. Click on the 'Test' button on the top right corner
  13. From the 'Event Template' dropdown select 'Alexa Start Session'
  14. Type `StartSession` in the 'event name' field
  15. Click the orange 'Create' button at the bottom of the screen.
  16. Click the 'Test' button again to run your newly created test.
  17. You should see a green box with the legend: 'Execution result: succeeded'
  18. Click on 'Triggers' tab
  19. Click on 'Add trigger'
  20. Click on the dotted line square
  21. From the dropdown select 'Alexa Skills Kit'
  22. Click 'Submit'
  23. Congratulations! You've deployed a serverless backend for an Alexa Skill
  24. From the top right corner copy the Amazon Resource Name (ARN) for this Lambda function (make sure to get everything after the dash)

![screenshot](https://s3-eu-west-1.amazonaws.com/alx402/AwsCopyArn.png)

--------

### Task 2: Build a Voice UI
  1. In a new browser tab go to [developer.amazon.com](https://developer.amazon.com/)
  2. On the top right corner Click on "Developer Console" and sign in
  3. Click on 'Alexa' in the navigation bar
  4. Under 'Alexa Skills Kit' click on the 'Get Started>' button.
  5. Click the 'Add a New Skill' button in the top right corner.
  6. Leave 'Custom Interaction Model' selected as the skill type
  7. Select 'English (US)' from the 'Language' dropdown
  8. Set the 'Name' & 'Invocation Name' to `Dynamic Facts`
  9. Leave all of the 'Global Fields' set to 'No'
  10. Click the 'Save' button at the bottom of the page.
  11. Click the yellow 'Next' button.
  12. Click the large black 'Launch Skill Builder (beta)' button.

  ![screenshot](https://s3-eu-west-1.amazonaws.com/alx402/LaunchSkillBuilder.png)
  13. In the blue 'Intents' navigation pane click 'Add an Intent'

  ![screenshot](https://s3-eu-west-1.amazonaws.com/alx402/SkillBuilderAddIntent.png)

  14. Under 'Create a new custom intent' type `GetNewFactIntent`
  15. Click the 'Create Intent' button
  16. In the 'Sample Utterances' field, type the following sentences, hit *enter* after each
      1. `We want a fact`
      2. `Another fact please`
      3. `One more`

   ![screenshot](https://s3-eu-west-1.amazonaws.com/alx402/AddingUtterances.png) 
   
  17. Click on the 'Build Model' button from the top pane
  18. Wait for the 'model built' confirmation message (this can take up to 90 seconds.)
  19. Click the 'Configuration' button
  20. Select the 'AWS Lambda ARN (Amazon Resource Name)' radio button
  21. Paste the **ARN you copied in Step 1.23** into the 'Default' Field
      1. Leave the checkbox for 'Provide geographical region endpoint' unchecked
  22. Leave 'Account Linking' set to 'no' and all the 'Permissions' checkboxes unchecked
  ![screenshot](https://s3-eu-west-1.amazonaws.com/alx402/FinalDevSkill.png)
  23. Click 'Next'
### Verify:
  24. Under 'Service Simulator' type `Give me a fact`
  25. There will now be a JSON object under the 'Service Response' Field
  26. Congrats again :trophy: You have built a simple Alexa Skill

--------

### [Lab 2](/Lab2.md)

--------

### Extra Credit: Use Echosim.io to test your skill
  1. Open a new browser tab and navigate to (Echosim.io)[https://echosim.io/]
  2. Click the yellow 'Login with Amazon' button
  3. Log in with the same credentials you used in [Developer Portal](https://developer.amazon.com/)
      1. **Allow your browser to access the microphone**
  3. Click and hold the blue 'microphone' button and say `Open Dynamic Facts`

### Extra Credit 2: Use a device with Alexa to test your skill
  1. Any device that is configured with the credentials you used for Developer Portal will have access to your skill.
