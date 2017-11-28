## Lab 4:
### Pre-Requisites
The API you built in
[Task 6](/Lab3.md#task-6-add-an-api-gateway-to-handle-writing-new-facts)

The DynamoDB table we created in [Task 3](/Lab2.md#task-3-create-a-dynamodb-for-storing-facts)

[cURL](https://curl.haxx.se/) *and/or* [Postman](https://www.getpostman.com/) for API testing

--------

### Task 7: Deploy API and test externally
  1. With the 'ALX402FactAPI' selected
  3. Click on the 'Actions' dropdown
  4. Click on 'Enable CORS'
      1. Click the blue 'Enable CORS button and replace existing CORS headers'
      2. Click the blue 'Yes confirm existing values' button
  5. Click on the 'Actions' dropdown again 
  5. Click on 'Deploy API'
  6. Select '[New Stage]' from 'Deployment stage'
  7. Type `Test` into the 'Stage name' Field
  8. You can fill the other two fields if you want
  9. Click blue 'Deploy' button
#### Verify:
  10. Copy the 'Invoke URL' from the 'Stage Editor' screen & save it to a text file
  11. Use [Postman](https://www.getpostman.com/) to test (or skip to step 12 and use cURL)
      1. Open Postman
      2. Start with a New tab
      3. Select 'Post' from the dropdown
      4. Paste the 'Invoke URL' you copied into the 'Request URL' field
      5. Click on 'Headers'
      6. Type 'Content-Type' under 'Key' & 'application/json' under 'Value'
      7. Click on 'Body'
      8. Select the 'Raw' radio button
      9. Copy and paste the following object into the field

      ```javascript
      {
        "locale":   "en-US",
        "speech": "From Postman",
        "text":  "Text from Postman"
      }
      ```
      10. Click 'Send'
      11. The status for the response should be '200'
      12. The body for the response should be '{}'
      13. There should be a new item in your DynamoDB table
  12. Use [cURL](https://curl.haxx.se/) to test (not necessary if you tested with potman in step 11)
      1. Open a Terminal or Command Line window
      2. Use the following command
      ```javascript
       curl -d '{"locale":   "en-US", "speech":"From cURL", "text":  "Text from cURL"}' -H 'Content-Type: application/json' https://yourInvocatiourl
      ```
      3. You should get '{}' as a response
      4. There should be a new item in your DynamoDB table

--------

### Task 8: Make a sample Web Page to write new facts
  1. Navigate to the S3 console using the Service search bar
  2. Click the blue 'Create bucket' button
  3. Give your bucket a *unique name*
      1. *All S3* buckets share the same namespace
      2. Cannot contain capital letters
  4. Select 'EU (Ireland)' as the 'Region'
      1. Don't copy the settings from another bucket
  6. Click the white 'Create' button
  7. Click on your recently created bucket name
  8. Click on the 'Properties' tab from the top navigation bar
  9. Click on the 'Static website hosting' box
      1. Select the radio button for 'Use this bucket to host a website'
      2. Type `index.html` & `error.html` in the corresponding fields
      3. Leave 'Redirection rules' blank
      4. Click the blue 'Save' button
  10. You'll see a purple check mark in the 'Static website hosting' box
      1. Scroll the back to the top
  11. Click on the 'Overview' tab
  12. Open a new file in a text editor and paste the following *html/javascript* code
  ```html
      <!doctype html>
      <head>
        <meta charset="utf-8">
        <title>Index</title>
      </head>
      <body>
        <h3>Simple web page for capturing facts</h3>
        <script type="text/javascript">
        let localeInput = prompt("Input a locale for the fact", "en-US");
        let speechInput = prompt("Input fact to be spoken", "<say-as interpret-as='interjection'>abracadabra!</say-as>, a fact added from web page");
        let textInput = prompt("Input text for the fact", "a fact added from web page");
        let data = JSON.stringify({
          "locale": localeInput,
          "speech": speechInput,
          "text": textInput
        });

        let xhr = new XMLHttpRequest();

        xhr.addEventListener("readystatechange", function () {
          if (this.readyState === 4) {
            console.log(this.responseText);
          }
        });

        xhr.open("POST", "https://<YourAPIURLhere>");
        xhr.setRequestHeader("content-type", "application/json");

        xhr.send(data);
        console.log(xhr.responseText);
        </script>
      </body>
  ```
  13. Save the file as `index.html`
      1. **In the file** you just created
      2. **change the URL** to the *invoke URL for your API*, you copied it in Task 7 step 9
  14. Click the blue 'Upload' button
  15. Click the blue 'Add Files' button
  16. Select the index.html file you created in *step 13*
  17. Click 'Open'
  18. Confirm you selected the correct file & click the blue 'Upload' button
  19. Click on the **index.html** on the file that was uploaded
  20. Click the white 'Make public' button
      1. A 'Success' dialog message will be displayed  
  21. At the bottom of the page there will be a link to the file hosted on S3, click on it
  22. You should be presented with the web page we created
  23. Fill the three prompts that will be presented
      1. We pre-filled this for you convenience, but feel free to get creative
#### Verify:
  24. Navigate to the DynamoDB table and make sure a new fact matching your input was stored
  25. Look at the logs for the web page using your browsers developer tools
      1. There should be a single line for the response : *'{}'*

--------

### [Lab 5](/Lab5.md)

--------

### Extra Credit: Secure Your API with Amazon Cognito
