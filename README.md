node-red-axios-wrapper
=====================

AxiosWrapper

## Install

Run the following command in your Node-RED user directory - typically `~/.node-red`

        npm install @phygineer/node-red-axios-wrapper

## Information


# Node-RED Axios Wrapper

## Overview

This provides a generic wrapper function for Axios, enabling you to make HTTP requests within Node-RED with configurations similar to those used in Postman. The function follows the subscribe pattern and is designed to be easily integrated into your Node-RED workflows.

## Parameters

- `config` (Object): The request configuration object that should include properties like `url`, `method`, `headers`, `params`, and `data`.

## Returns

A Promise that resolves with the response data as a JSON object.

## Usage in Node-RED Subflow

The function is executed using the subscribe pattern, which means it will process incoming messages (`msg.payload`) containing request configurations:

```javascript
return axiosWrapper(msg.payload)
  .then(responseData => {
    msg.payload = responseData;
    return msg;
  })
  .catch(error => {
    msg.payload = { status: 'error', message: error };
    return msg;
  });
```

## Functionality

1. **Header Configuration**:
   - Merges any headers provided in the `config` with a default `'Content-Type'` of `'application/json'`.

2. **Request Configuration**:
   - Constructs the request configuration using properties from the input config (`url`, `method`, `headers`, `params`, and `data`).

3. **Debug Logging** (if enabled):
   - Logs the request configuration to the Node-RED console if debugging is enabled globally.

4. **HTTP Request Execution**:
   - Uses Axios to make the HTTP request.
   - Resolves with response data on success.
   - Rejects with an error message if the request fails.

## Example Input Message (`msg.payload`)

```json
{
  "url": "https://api.example.com/data",
  "method": "POST",
  "headers": {
    "Authorization": "Bearer <token>"
  },
  "data": {
    "key1": "value1"
  }
}
```

## Example Output Message (`msg.payload`)

- **Success**: Contains the response data from the API.
- **Error**: Contains a status of 'error' and the error message.

## Installation

1. Clone this repository or download the subflow file directly.
2. Import the subflow into your Node-RED instance via the "Import" feature in the Node-RED UI.

## Dependencies

- Axios: Ensure you have Axios installed in your Node-RED environment to use this wrapper function.

```bash
npm install axios
```

## Configuration

- The `config` object should contain at least a `url` and a `method`. Other properties like `headers`, `params`, and `data` are optional but commonly used.

## Debugging

Set the global debug setting in Node-RED to enable logging of request configurations:

```javascript
global.set('debug', true);
```

This will print out the request configuration being sent via Axios to help with debugging issues.

---

This subflow function is designed to simplify making HTTP requests within Node-RED by leveraging Axios while maintaining flexibility similar to Postman configurations.

