- Config 
```
var AWS = require("aws-sdk");
let awsConfig = {
    "region": "us-east-1",
    "endpoint": "http://dynamodb.us-east-1.amazonaws.com",
    "accessKeyId": "", "secretAccessKey": ""
};
AWS.config.update(awsConfig);

```

- READ

```
let docClient = new AWS.DynamoDB.DocumentClient();
let fetchOneByKey = function () {
    var params = {
        TableName: "Test",
        Key: {
            "user": 1
        }
    };
    docClient.get(params, function (err, data) {
        if (err) {
            console.log("users::fetchOneByKey::error - " + JSON.stringify(err, null, 2));
        }
        else {
            console.log("users::fetchOneByKey::success - " + JSON.stringify(data, null, 2));
        }
    })
}
fetchOneByKey();
```


- WRITE

```
let docClient = new AWS.DynamoDB.DocumentClient();
let save = function () {
    var input = {
        "user": 100, "age": "10", "created_on": new Date().toString(),
        "updated_by": "clientUser", "updated_on": new Date().toString(), "is_deleted": false
    };
    var params = {
        TableName: "Test",
        Item:  input
    };
    docClient.put(params, function (err, data) {
        if (err) {
            console.log("users::save::error - " + JSON.stringify(err, null, 2));                      
        } else {
            console.log("users::save::success" );                      
        }
    });
}
save();
```


- UPDATE

```
AWS.config.update(awsConfig);
let docClient = new AWS.DynamoDB.DocumentClient();
let modify = function () {
    var params = {
        TableName: "Test",
        Key: {
            "user": 1
        },
        UpdateExpression: "set age = :age",
        ExpressionAttributeValues: {
            ":age": "30"
        },
        ReturnValues: "UPDATED_NEW"
    };
    docClient.update(params, function (err, data) {
        if (err) {
            console.log("users::update::error - " + JSON.stringify(err, null, 2));
        } else {
            console.log("users::update::success "+JSON.stringify(data) );
        }
    });
}
modify();
```


- DELETE 
```
AWS.config.update(awsConfig);
let docClient = new AWS.DynamoDB.DocumentClient();
let remove = function () {
    var params = {
        TableName: "Test",
        Key: {
            "user": 2
        }
    };
    docClient.delete(params, function (err, data) {
        if (err) {
            console.log("users::delete::error - " + JSON.stringify(err, null, 2));
        } else {
            console.log("users::delete::success");
        }
    });
}
remove();
```

