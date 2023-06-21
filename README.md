
# CS-Node Rules

Node-rules is a light weight forward chaining Rule Engine, written in JavaScript for both browser and node.js environments.

#### Installation

    npm install node-rules

![Sample Screencast]https://github.com/fahadkbhatti

#### Try This Out!

You can see this in action in a node.js environment using [this RunKit example]https://github.com/fahadkbhatti. If you are interested to use it in the browser, use [this JSFiddle](https://jsfiddle.net/fahadkbhatti) for a quick start.

#### Overview

Css-Node-rules takes rules written in JSON friendly format as input. Once the rule engine is running with rules registered on it, you can feed it facts and the rules will be applied one by one to generate an outcome.

###### 1. Defining a Rule

A rule will consist of a condition and its corresponding consequence. You can find the explanation for various mandatory and optional parameters of a rule in [this wiki]https://github.com/fahadkbhatti.

```js
{
    "condition" : (R, fact) => {
        R.when(fact.transactionTotal < 500);
    },
    "consequence" : (R, fact) => {
        fact.result = false;
        R.stop();
    }
}
```

Here priority is an optional parameter which will be used to specify priority of a rule over other rules when there are multiple rules running. In the above rule `R.when` evaluates the condition expression and `R.stop` used to stop further processing of the fact as we have arrived at a result.

The functions `R.stop`, `R.when`, `R.next`, `R.restart` are part of the Flow Control API which allows user to control the Engine Flow. Read more about [Flow Controls]https://github.com/fahadkbhatti in [wiki]https://github.com/fahadkbhatti.

###### 2. Defining a Fact

Facts are those input json values on which the rule engine applies its rule to obtain results. A fact can have multiple attributes as you decide.

A sample Fact may look like

```json
{
  "name": "user4",
  "application": "MOB2",
  "transactionTotal": 400,
  "cardType": "Credit Card"
}
```

###### 3. Using the Rule Engine

The example below shows how to use the rule engine to apply a sample rule on a specific fact. Rules can be fed into the rule engine as Array of rules or as an individual rule object.

```js
const { RuleEngine } = require("node-rules");

/* Creating Rule Engine instance */
const R = new RuleEngine();

/* Add a rule */
const rule = {
  condition: (R, fact) => {
    R.when(fact.transactionTotal < 500);
  },
  consequence: (R, fact) => {
    fact.result = false;
    fact.reason = "The transaction was blocked as it was less than 500";
    R.stop();
  },
};

/* Register Rule */
R.register(rule);

/* Add a Fact with less than 500 as transaction, and this should be blocked */
let fact = {
  name: "user4",
  application: "MOB2",
  transactionTotal: 400,
  cardType: "Credit Card",
};

/* Check if the engine blocks it! */
R.execute(fact, (data) => {
  if (data.result !== false) {
    console.log("Valid transaction");
  } else {
    console.log("Blocked Reason:" + data.reason);
  }
});
```

To read more about storing rules running on the engine to an external DB, refer this [wiki article]https://github.com/fahadkbhatti


#### Issues

Got issues with the implementation?. Feel free to open an issue [here]https://github.com/fahadkbhatti.

#### Licence

Node rules is distributed under the MIT License.


