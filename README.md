# node-red-contrib-conta-azul

![Conta Azul](https://user-images.githubusercontent.com/3945941/230798312-7f8f69a1-859e-4ff1-a371-1073ec0d49eb.svg)


This node enables you to work with the Conta Azul API, giving you the ability to set parameters through the Node-RED-UI and trigger the flow from within your existing flow.

It is based on [ ContaAzul API](https://developers.contaazul.com/).

## Usage

![Conta Azul](https://user-images.githubusercontent.com/3945941/230798311-d885e12e-a260-469d-9cbe-659868fb2cb7.svg)

### 1. Get the API-operations list

![Operations](https://user-images.githubusercontent.com/3945941/230801175-710c8190-39a4-4a2c-ba47-41a7f891e841.png?raw=true "Operations")

## 2. Understanding the API

Hovering on an operations title or a key, you see the respective comments within the mouseover. This allows you to understand what a parameter is meant for. Required parameters are marked with an asterisk.

![parameter description](https://user-images.githubusercontent.com/3945941/230797180-fb64adf4-8a39-4eea-9d3b-90eabe76fbcd.png?raw=true "Parameter description")

For JSON-parameters you can further show the structure by clicking on show keys. Again, the comments can be found within the mouseover.

![json description](https://user-images.githubusercontent.com/3945941/230797266-e5cea84b-4e14-432e-9724-a0874415f56c.png?raw=true "Json description")

## 3. Parameter configuration

Each parameter has an input-field corresponding to its type. You can further define that a parameter shall be read from the incoming message object or define a jsonata expression.

JSON parameters may define a sample structure. You can set this as the value by clicking the corresponding button - either with only the required keys (set required) or with all keys (set default).

## 4. Authentification

If the API requires an authentification token you can log in using the standard `http-request` node of Node-RED. The JWT token you get as a response must then be put into `msg.openApiToken` to be automatically placed in the request-header as bearer authentification.
In case you would like to use a different authentification than bearer, you can use `msg.headers` as you can do with the default http request node of Node-RED.

Both message object will be deleted afterwards if you do not set the option "Keep authentification".

## 5. Error handling

You can choose how to handle a returning server error. The last server response object will be placed in msg.response instead of msg.error. This ensures that all 3 ways react the same.

* `Standard`: The flow moves on normally. You have to handle an server error in your flow.
* `Separate output`: Your flow will take a different way.
* `Throw an exception`: Throws an node.error which can be catched by the standard 'catch' node (usefull for many nodes with the same error handling).

## Sample

![image](https://user-images.githubusercontent.com/3945941/230800038-8c1ca9d1-4d37-4397-86a5-63ab6a85f923.png)

```json
[{"id":"487db9b6715b0e50","type":"tab","label":"Conta Azul API","disabled":false,"info":"","env":[]},{"id":"2acdc00d41d12ba6","type":"group","z":"487db9b6715b0e50","name":"","style":{"fill":"#bfdbef","label":true},"nodes":["b33404daaaf4968b","2e215a803a6fd61d","61265727e704a1b0","cb6b47549e706ea7","a40d99ac033a7eb1","a2487b41484cb7b4","4bba16c5d6702c0a"],"x":34,"y":39,"w":952,"h":162},{"id":"b33404daaaf4968b","type":"oauth2","z":"487db9b6715b0e50","g":"2acdc00d41d12ba6","name":"OAuth2 - Conta Azul","container":"oauth2Response","grant_type":"authorization_code","access_token_url":" https://api.contaazul.com/oauth2/token","authorization_endpoint":"https://api.contaazul.com/auth/authorize","redirect_uri":"/oauth2/redirect_uri","open_authentication":"","username":"","password":"","client_id":"","client_secret":"","scope":"Product","proxy":"","senderr":false,"client_credentials_in_body":false,"rejectUnauthorized":true,"headers":{},"x":340,"y":80,"wires":[["cb6b47549e706ea7"]]},{"id":"2e215a803a6fd61d","type":"inject","z":"487db9b6715b0e50","g":"2acdc00d41d12ba6","name":"","props":[{"p":"payload"},{"p":"topic","vt":"str"}],"repeat":"","crontab":"","once":false,"onceDelay":0.1,"topic":"","payload":"","payloadType":"date","x":140,"y":80,"wires":[["b33404daaaf4968b"]]},{"id":"61265727e704a1b0","type":"debug","z":"487db9b6715b0e50","g":"2acdc00d41d12ba6","name":"debug","active":true,"tosidebar":true,"console":true,"tostatus":true,"complete":"true","targetType":"full","statusVal":"payload","statusType":"auto","x":880,"y":120,"wires":[]},{"id":"cb6b47549e706ea7","type":"function","z":"487db9b6715b0e50","g":"2acdc00d41d12ba6","name":"set ACCESS_TOKEN","func":"global.set('ACCESS_TOKEN', msg.oauth2Response.access_token);\nreturn msg;","outputs":1,"noerr":0,"initialize":"","finalize":"","libs":[],"x":660,"y":80,"wires":[["61265727e704a1b0"]]},{"id":"a40d99ac033a7eb1","type":"inject","z":"487db9b6715b0e50","g":"2acdc00d41d12ba6","name":"","props":[{"p":"payload"},{"p":"topic","vt":"str"}],"repeat":"","crontab":"","once":false,"onceDelay":0.1,"topic":"","payload":"","payloadType":"date","x":140,"y":160,"wires":[["a2487b41484cb7b4"]]},{"id":"a2487b41484cb7b4","type":"function","z":"487db9b6715b0e50","g":"2acdc00d41d12ba6","name":"get ACCESS_TOKEN","func":"msg.openApiToken = global.get('ACCESS_TOKEN');\nreturn msg;","outputs":1,"noerr":0,"initialize":"","finalize":"","libs":[],"x":340,"y":160,"wires":[["4bba16c5d6702c0a"]]},{"id":"4bba16c5d6702c0a","type":"conta-azul","z":"487db9b6715b0e50","g":"2acdc00d41d12ba6","name":"","api":"Product","server":"","keepAuth":false,"alternServer":false,"operation":"list1","operationData":{"tags":["Product"],"summary":"List product categories","description":"","operationId":"list1","produces":["application/json"],"parameters":[{"name":"name","in":"query","description":"The name of the product category","required":false,"type":"string"},{"name":"page","in":"query","description":"The page of the list to be returned","required":false,"type":"string"},{"name":"size","in":"query","description":"The quantity of items in the page to be returned","required":false,"type":"string"}],"responses":{"200":{"description":"Product Categories found with the specified parameters","schema":{"type":"array","items":{"type":"object","properties":{"id":{"type":"string","format":"uuid","example":"c7288c09-829d-48b9-aee2-4f744e380587","description":"The id of the product's category"},"name":{"type":"string","example":"Kitchen utensils","description":"The name of the product's category"}},"description":"A category that can be applied to a product","$$ref":"https://api.contaazul.com/schema#/definitions/ProductCategory"}}},"400":{"description":"Invalid search parameters. The possible reasons are : <ul><li>A invalid search parameter was provided</li><li>No search parameters were provided</li></ul>"},"401":{"description":"Unauthorized token - it may be due to an invalid token or no token provided"}},"__originalOperationId":"list","path":"/v1/product-categories"},"errorHandling":"throw exception","internalErrors":{"readUrl":false},"parameters":[{"id":"namequery","name":"name","in":"query","required":false,"value":"","isActive":false,"type":"str","allowedTypes":["str","json","jsonata","msg","flow","global"],"description":"The name of the product category","schema":null,"keys":null},{"id":"pagequery","name":"page","in":"query","required":false,"value":"","isActive":false,"type":"str","allowedTypes":["str","json","jsonata","msg","flow","global"],"description":"The page of the list to be returned","schema":null,"keys":null},{"id":"sizequery","name":"size","in":"query","required":false,"value":"","isActive":false,"type":"str","allowedTypes":["str","json","jsonata","msg","flow","global"],"description":"The quantity of items in the page to be returned","schema":null,"keys":null}],"requestContentType":"application/json","responseContentType":"","showDescription":true,"devMode":false,"outputs":1,"x":610,"y":160,"wires":[["61265727e704a1b0"]]}]
```

## License
This repository and the code inside it is licensed under the MIT License. Read LICENSE for more information.

## Developers

If you want to modify something inside the conta-azul.html file, I recommend to use [SIR](https://gitlab.com/2WeltenChris/svelte-integration-red).

With help of SIR you can handle the conta-azul.svelte file in which the code is much cleaner and easier to handle.
