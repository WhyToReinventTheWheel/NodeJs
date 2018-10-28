- Java SQS: 
    https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/java/example_code/sqs/src/main/java/aws/example/sqs

- Install AWS SDK
```
    "aws-sdk": ""
```
- Setup Config config.json
```
{ 
    "accessKeyId": "",
    "secretAccessKey": "",
    "region": "us-east-1"
}
```

- SQS CURD app.js
```
// Require objects.
var express  = require('express');
var app      = express();
var aws      = require('aws-sdk');
const queueUrl = "";
var receipt  = "";
var requestId = "";

// Load your AWS credentials and try to instantiate the object.
aws.config.loadFromPath(__dirname + '/config.json');

// Instantiate SQS.
var sqs = new aws.SQS();

// Creating a queue.
app.get('/create', function (req, res) {
    var params = {
        QueueName: "MyFirstQueue"
    };
    
    sqs.createQueue(params, function(err, data) {
        if(err) {
            res.send(err);
        } 
        else {
            requestId = data.ResponseMetadata.RequestId;
            queueUrl=QueueUrl;
            res.send(data);
        } 
    });
});

// Listing our queues.
app.get('/list', function (req, res) {
    console.log("list");
    sqs.listQueues(function(err, data) {
        if(err) {
            res.send(err);
        } 
        else {
            console.log(data);
            res.send(data);
        } 
    });
});

app.get('/send', function (req, res) {
    console.log(new Date().toISOString());
    for(let i=0; i<10 ;i++){
        var params = {
            MessageBody: 'Hello world!',
            QueueUrl: queueUrl,
            DelaySeconds: 0
        };

        sqs.sendMessage(params, function(err, data) {
            if(err) {
                res.send(err);
                console.log(i);
            } 
            else {
                console.log(i+"+"+new Date().toISOString());
                //res.send(data);
            } 
        });
    }
   
    res.send("time");
    
});

// Receive a message.
// NOTE: This is a great long polling example. You would want to perform
// this action on some sort of job server so that you can process these
// records. In this example I'm just showing you how to make the call.
// It will then put the message "in flight" and I won't be able to 
// reach that message again until that visibility timeout is done.
app.get('/receive', function (req, res) {
    var params = {
        QueueUrl: queueUrl,
        VisibilityTimeout: 10 // 10 Sec wait time for anyone else to process.
    };
    
    sqs.receiveMessage(params, function(err, data) {
        if (err) {
          console.log("Receive Error", err);
          res.json(err);
        } else if (data.Messages) {
          var deleteParams = {
            QueueUrl: queueUrl,
            ReceiptHandle: data.Messages[0].ReceiptHandle
          };
          console.log(data);
          //https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-general-identifiers.html
          sqs.deleteMessage(deleteParams, function(err, data) {
            if (err) {
              console.log("Delete Error", err);
              res.json(err);
            } else {
              console.log("Message Deleted", data);
              res.json(data);
            }
          });
        }
    });
});


// Purging the entire queue.
app.get('/purge', function (req, res) {
    var params = {
        QueueUrl: queueUrl
    };
    
    sqs.purgeQueue(params, function(err, data) {
        if(err) {
            res.send(err);
        } 
        else {
            res.send(data);
        } 
    });
});

// Start server.
var server = app.listen(3000, function () {
    console.log('AWS SQS example app listening at 3000');
});


```

