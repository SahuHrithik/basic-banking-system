# basic-banking-system
This is a basic banking system created using Node.js and MongoDB database.
new project
## Mongo DB Installation and Setup Instructions

Use below command to deploy a MongoDB Container

docker run -d --name mongo -p 27017:27017 -e MONGO_INITDB_ROOT_USERNAME=mongoadmin -e MONGO_INITDB_ROOT_PASSWORD=App123Password -e MONGO_INITDB_DATABASE=bankDB mongo

Accessing Mongo DB using below command:

docker run -it --link mongo:mongo --rm mongo mongosh --host mongo -u mongoadmin -p App123Password --authenticationDatabase admin bankDB

Use below command to give mongoadmin user access to bankDB

use admin

db.grantRolesToUser('mongoadmin', [{ role: 'readWrite', db: 'bankDB' }]);

## Installation and Setup Instructions

#### Example:  

Clone down this repository. You will need `node` and `npm` installed globally on your machine.  

Installation:

`npm install`    

To Start Server:

`npm start`  

To Visit App:

`localhost:3000`  
