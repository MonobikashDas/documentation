### ABIS Vendor guide

#### Steps to integrate ABIS system with MOSIP
The abis system will talk to MOSIP via message queue(Ex - ActiveMQ). MOSIP will send message to a queue address where abis will listen. After consuming message successfully ABIS will send response to another queue address where MOSIP will consume. The messages will be exchanged in json format. The actual biometrics will not be sent via queue. ABIS system has to call a callback URL to get the actual biometrics. The biometrics will be exchanged using CBEFF xml across MOSIP. Refer to [Biometric Data Specification](https://mosipdocs.gitbook.io/platform/functionalities/biometric/biometric-data-specification) to know more about specifications. 
1. First step is to implement the ABIS API specification. MOSIP will send request to ABIS in a specific format and expect response in specific json format. Refer to the [ABIS API SPEC](https://mosipdocs.gitbook.io/platform/functionalities/apis/abis-apis) to consume and send message to MOSIP.
2. Second step is to implement the callback url. As mentioned above, json message will be sent via queue where the callback url will be present to get the actual biometric file. Refer to [bio-dedupe-api](https://mosipdocs.gitbook.io/platform/functionalities/apis/registration-processor-apis#5-bio-dedupe-api) for callback url API spec. This callback URL is authenticated and authorized. Hence ABIS has to send jwt token inside request header COOKIE. Refer to [Authentication and authorization API](https://mosipdocs.gitbook.io/platform/functionalities/apis/authn-and-authz-apis#authenticate-using-clientid-and-secret-key) to get the jwt token. Request json expects clientid, secretkey and appid. Please get in touch with the country SI for same.
3. Third step is to connect to the queue. Use the queue details to connect  and refer to [ABIS Json file in config server](https://github.com/mosip/mosip-config/blob/master/config-templates/RegistrationProcessorAbis-env.json) to know the message consume and send address. The "inboundQueueName" is where mosip will send message to ABIS. The "outboundQueueName" is where ABIS will send response message to MOSIP.
4. After completing above steps test the integration with some messages.