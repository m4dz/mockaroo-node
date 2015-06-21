# mockaroo-node

A nodejs client for generating data using the rest api from http://mockaroo.com

# Installation

    npm install mockaroo

# Documentation

http://mockaroo.com/api/node

# API Overview

The mockaroo client's generate method returns a promise that resolves to an array of records.

You can either generate data using a schema that you've built and saved on mockaroo.com:

```js
var Mockaroo = require('mockaroo');

var client = new Mockaroo.Client({
    apiKey: 'e93db400' // see http://mockaroo.com/api/docs to get your api key
})

client.generate({
    count: 10,
    schema: 'My Saved Schema'
}).then(records) {
    ...
});
```

Or you can specify fields using the API:

```js
client.generate({
    count: 10,
    fields: [{
        name: 'id',
        type: 'Row Number'
    }, {
        name: 'transactionType',
        type: 'Custom List',
        values: ['credit', 'debit']
    }]
}.then(records) {
    ...
});
```

Field types and parameters are documented [here](http://mockaroo.com/api/docs)

# Handling Responses

The Promise returned by client.generate resolves to an array of records when count > 1 and a single object when count == 1.
The keys are the names of the fields in your schema.  For example:

```js
client.generate({
    count: 10,
    fields: [{
        name: 'id',
        type: 'Row Number'
    }, {
        name: 'transactionType',
        type: 'Custom List',
        values: ['credit', 'debit']
    }]
}.then(records) {
    for (var i=0; i<records.length; i++) {
        var record = records[i];
        console.log('record ' + i, 'id:' + record.id + ', transactionType:' + record.transactionType);
    }
});
```

# Errors

This module contains Error classes to help you handle specific error conditions.

```js
client.generate({
    count: 10,
    schema: 'My Saved Schema'
}).then(records) {
    ...
}).catch(error) {
    if (error instanceof Mockaroo.errors.InvalidApiKeyError) {
      console.log('invalid api key');
    } else if (error instanceof Mockaroo.errors.UsageLimitExceededError) {
      console.log('usage limit exceeded');
    } else {
      console.log('unknown error', error);
    }
});
```

# Gulp Tasks

To generate documentation and compile the es6 code to js, use the default gulp task:

    gulp
