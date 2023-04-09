# node-red-contrib-conta-azul

![Conta Azul](https://user-images.githubusercontent.com/3945941/230798312-7f8f69a1-859e-4ff1-a371-1073ec0d49eb.svg)


This node enables you to work with the Conta Azul API, giving you the ability to set parameters through the Node-RED-UI and trigger the flow from within your existing flow.

It is based on [ ContaAzul API](https://developers.contaazul.com/).

## Usage

![Conta Azul](https://user-images.githubusercontent.com/3945941/230798311-d885e12e-a260-469d-9cbe-659868fb2cb7.svg)

### 1. Get the API-operations list

![Operations](https://user-images.githubusercontent.com/3945941/230797375-979bea99-f21e-47d9-9bda-fadfd0f22e3f.png?raw=true "Operations")

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


## License
This repository and the code inside it is licensed under the MIT License. Read LICENSE for more information.

## Developers

If you want to modify something inside the conta-azul.html file, I recommend to use [SIR](https://gitlab.com/2WeltenChris/svelte-integration-red).

With help of SIR you can handle the conta-azul.svelte file in which the code is much cleaner and easier to handle.
