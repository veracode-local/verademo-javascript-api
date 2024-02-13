<img src="https://help.veracode.com/internal/api/webapp/header/logo" width="200" /><br>  

test 12348sah
  
# Verademo API  
  
## What is this about  
Verademo API is very simple API for the Verademo Java Application that can be found here: [https://github.com/veracode/verademo-javascript-api](https://github.com/veracode/verademo-javascript-api). It allows you to use almost the same functionality as the web application, only as an API.   
It's used as a demo application to run static code analysis, software composition analysis and dynamic API scanning. There will be findings for all differnt type of scanning technologies.  
Static Findings  
<img src="https://github.com/veracode/verademo-javascript-api/blob/main/pictures/static_findings.png" width="800" />  
  
Dynamic Findings  
<img src="https://github.com/veracode/verademo-javascript-api/blob/main/pictures/dynamic_findings.png" width="800" />  
  
SCA Findings  
<img src="https://github.com/veracode/verademo-javascript-api/blob/main/pictures/sca_findings.png" width="800" />  
  
## How to build and run  
Simply clone this repo and run   
``npm install``   
This will install required node modules for this application.  
``node index.js``  
This will run an express server on port 8000.  
  
Alos this app requires a database that is right now setup to connect on 192.168.178.80:3306. Please refer to the overlaying group of repositorires [https://github.com/veracode/verademo-app-docker](https://github.com/veracode/verademo-app-docker) how you can run the web app, the database and this API in one go using Docker images.  
  
A few little configurations if you wnat to adjust.  
It's configured to run on port 8000, if you want to change please change the code on ``index.js`` accordingly.  
```  
app.listen(8000, () => {
  console.log("Verademo API is ready to listen for requests");
});
```  
The database configuration is found on ``/config/db.config.js``, if you are using a different database or database connection please adjust this part of the code.  
```  
const { createPool } = require("mysql");
const db = createPool({
  port: 3306,
  host: "192.168.178.80",
  user: "blab",
  password: "z2^E6J4$;u;d",
  database: "blab",
  connectionLimit: 10,
});
module.exports = db;
```  
  
If you want to run on a docker your Dockerfile could be similar to this  
```
FROM node:16

WORKDIR /user/src/app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 8000
CMD ["node", "index.js"]
``` 
  
A full documentation how to run with a full data base and the corresponding web app in a dockerized environement can be found here: [https://github.com/veracode/verademo-app-docker](https://github.com/veracode/verademo-app-docker)
  
  
## Functionality  
It's using token based authentication. You are required to send an authentication header with every request to this API.  
``Authentication: Token: YOUR-TOKEN``  
<img src="https://github.com/veracode/verademo-javascript-api/blob/main/pictures/authentication.png" width="800" />  
The token is the md5 hashedpassword of the user. If your are using the default database this is already stored on the `blab` database and the `users` table. Please refer to the full dockerized setup here [https://github.com/veracode/verademo-app-docker](https://github.com/veracode/verademo-app-docker).  
The API provides a few calls that also can reviewed on the automatically generated swagger overview under `http://IP:8000/public/`. It also brings a full swagger file in `/public/swagger.json` that can be used for dynamic API scanning.  
!<img src="https://github.com/veracode/verademo-javascript-api/blob/main/pictures/swagger_overview.png" width="800" />  
If you run a call against this API it requires JSON data to be sent (where applicalble) and it will also return JSON data.  
<img src="https://github.com/veracode/verademo-javascript-api/blob/main/pictures/insomnia_request.png" width="800" />
  
All calls to `admin` can only be do by the admin user, using the corresponding token to authenticate.  

## License
![MIT license](https://img.shields.io/badge/License-MIT-blue.svg)
