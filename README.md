# NodeJs
- https://www.npmjs.com/package/underscore
- https://underscorejs.org
- https://www.npmjs.com/package/node-persist
- https://www.npmjs.com/package/credit-card-type
- https://www.npmjs.com/package/card-validator
- https://www.npmjs.com/package/validator
- https://www.npmjs.com/package/yargs
- https://www.npmjs.com/package/crypto-js
- https://www.npmjs.com/package/jsonwebtoken
- https://www.npmjs.com/package/opossum
- https://www.npmjs.com/package/express-http-context
- https://solidgeargroup.com/express-logging-global-unique-request-identificator-nodejs
- https://www.npmjs.com/package/winston
- https://www.npmjs.com/package/log4js


```
var crypto = require('crypto-js');
var secretMessage = {
	name: 'Andrew',
	secretName: '007'
};
var secretKey = '123abc';
// Encrypt
var encryptedMessage = crypto.AES.encrypt(JSON.stringify(secretMessage), secretKey);
console.log('Encrypted Message: ' + encryptedMessage);
// Decrypt Message
var bytes = crypto.AES.decrypt(encryptedMessage, secretKey);
var decryptedMessage = JSON.parse(bytes.toString(crypto.enc.Utf8));
console.log(decryptedMessage);
console.log(decryptedMessage.secretName);
```
