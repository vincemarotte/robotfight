# robotfight
RobotFight is an AI resumé writing assistant powered by OpenAI
DEMO: The Google Sheet demo is located here: https://docs.google.com/spreadsheets/d/1zg7ChgtnYtKEWU2OBiRTsHz7Mvp--42XEj9ZmAl1ux8/edit#gid=0
INSTRUCTIONS AND INFO: https://www.robotfight.net

## The Basics
* RobotFight requires an OpenAI API key to function. You can change the model to one that you think serves your needs. 
* The function "OPENAIPROMPT" will call the function from your Google Sheet. 

# Full Documentation

## Overview
The `OPENAIPROMPT` function is a Google Apps Script designed to interact with the OpenAI API. It retrieves a user prompt from a Google Sheet, sends this prompt to the OpenAI API, and returns a generated text response. This script is useful for integrating AI-generated text directly into your Google Sheets workflows.

## Script Details

### Function: `OPENAIPROMPT(userPrompt)`

#### Parameters
- `userPrompt`: A string containing the text prompt provided by the user. This prompt will be sent to the OpenAI API to generate a response.

#### Process Overview
1. **Access the Spreadsheet**
   - The script accesses the active Google Spreadsheet and selects the sheet named 'roles'.

2. **Retrieve the API Key**
   - It retrieves the OpenAI API key stored in cell `G2` of the 'roles' sheet.

3. **Define API Endpoint**
   - The script defines the OpenAI API endpoint for chat completions: `https://api.openai.com/v1/chat/completions`.

4. **Prepare Payload**
   - A JSON payload is created with the following properties:
     - `model`: Specifies the model to be used, default is "gpt-4-turbo".
     - `messages`: An array containing a single message object with `role` set to "user" and `content` set to the `userPrompt`.
     - `max_tokens`: Sets the maximum number of tokens in the response, default is 150.
     - `temperature`: Controls the creativity of the response, default is 0.7.

5. **Set Request Options**
   - The script configures the request options for the `UrlFetchApp.fetch` method:
     - `method`: HTTP method, set to 'post'.
     - `contentType`: Specifies the content type as 'application/json'.
     - `payload`: The JSON payload created in the previous step.
     - `headers`: Includes the Authorization header with the Bearer token from the API key.
     - `muteHttpExceptions`: Set to true to handle HTTP exceptions gracefully.

6. **Send Request and Handle Response**
   - The script sends the request to the OpenAI API.
   - Parses the JSON response and checks the HTTP response code:
     - If the response code is 200, it returns the generated text content.
     - If the response code is not 200, it returns an error message indicating the failure reason.

### Example Usage
To use the `OPENAIPROMPT` function in your Google Sheet:
1. Ensure you have a sheet named 'roles' in your active Google Spreadsheet.
2. Store your OpenAI API key in cell `G2` of the 'roles' sheet.
3. Call the `OPENAIPROMPT` function with a text prompt as an argument.

#### Sample Call
```javascript
function testOpenAIPrompt() {
  const userPrompt = "What is the capital of France?";
  const response = OPENAIPROMPT(userPrompt);
  Logger.log(response);
}
```

## Error Handling
If the API request fails, the function returns a message indicating the failure. The error message includes the specific error returned by the API, if available, or a generic "Unknown error" message.

## Notes
- Ensure the OpenAI API key is kept secure and not exposed in the script or shared documents.
- Adjust the `max_tokens` and `temperature` parameters based on your specific needs for response length and creativity.
