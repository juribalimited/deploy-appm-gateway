[![Deploy To Azure](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.svg?sanitize=true)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjuribalimited%2Fdeploy-appm-gateway%2Fmain%2FmainTemplate.json/createUIDefinitionUri/https%3A%2F%2Fraw.githubusercontent.com%2Fjuribalimited%2Fdeploy-appm-gateway%2Fmain%2FcreateUiDefinition.json)

[![Visualize](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/visualizebutton.svg?sanitize=true)](http://armviz.io/#/?load=https%3A%2F%2Fraw.githubusercontent.com%2Fjuribalimited%2Fdeploy-appm-gateway%2Fmain%2FmainTemplate.json)

Juriba offer a one click deploy template to create a desktop gateway for use with AppM. The desktop gateway is used to facilitate access between the main AppM application and guest VM's by creating an internet accessible front end web app which allows communication to Guest VM's on a private subnet.

The one click deploy template creates a Guacamole container, Azure Web App and all associated networking infrastructure to setup connectivity quickly and easily. However customers are not required to use the one click deploy template. If they want to build and host their own gateway then they can use the guacamole container images hosted publically here - https://hub.docker.com/?namespace=juriba

To use the one click deploy template click the "Deploy to Azure" button above.

1. The initial panel allows you to select which Azure subscription you want to deploy in to. You can also select an existing resource group or create a new one to house resources.

![image](https://github.com/juribalimited/deploy-appm-gateway/assets/109281915/f7e2831f-db99-4347-81fe-a25d244044d5)


2. The second panel allows you to select an existing network or create a new virtual network for gateway use. By default a new virtual network and subnets are created, however using the drop down menu you can select an existing network held within your Azure subscription. The virtual network requires three subnets for gateway resources, guest VM's and the guacamole container. We would suggest creating a new network for simplicity of use but this is not mandatory. You can also amend the Network Security Group names on this page if required.


![image](https://github.com/juribalimited/deploy-appm-gateway/assets/109281915/21d53c53-387a-4a32-b62a-34483d822dda)

3.  The third panel requires you to enter your main AppM website address. This will be the web address you use to access your AppM environment. 

   The AppM version is the guacamole image that we want to use - currently this will always default to latest.

   The Gateway WebApp Name is the internet accessible address that you want to use for your desktop gateway, azurewebsites.net will automatically be appended to whatever you enter. Once entered please click out of the entry box, this will start a check to ensure that address is available. If available you will see a green tick, otherwise you will receive an erorr message (examples 
   below). 

   ![image](https://github.com/juribalimited/deploy-appm-gateway/assets/109281915/59ee9fb1-e8f4-4146-ab35-467abc3ea444)

   ![image](https://github.com/juribalimited/deploy-appm-gateway/assets/109281915/c4fbf88b-56a0-44d7-88c9-34ebe54071ed)

   The Encryption Key can be obtained from the main AppM application under System > Integrations > Desktop Gateways.
  
   Enter the details that you know here and then click update, You wil not be able to test connection at this point as we have not completed the desktop gateway deployment. Once you have clicked update 
   an encryption hex key will be created for you. Please enter this into the Encryption key fields.

   ![image](https://github.com/juribalimited/deploy-appm-gateway/assets/109281915/fd095519-53f0-4c81-a56b-366f8dbb9028)
   ![image](https://github.com/juribalimited/deploy-appm-gateway/assets/109281915/770ef24c-c60f-4a4d-b665-4a0799df931f)



   The final fields for Username and Password will need to be filled in. These details won't be needed for general AppM usage but may be needed when troubleshooting connectivity issues between AppM and Guest VM's. Please store these details securely should you need them in the future.

   ![image](https://github.com/juribalimited/deploy-appm-gateway/assets/109281915/119e0081-a39a-49b6-99ab-9b92e03a9430)

4. The fourth panel enables you to set a name for the Guacamole container instance.

![image](https://github.com/juribalimited/deploy-appm-gateway/assets/109281915/aa546a13-5193-4bf2-984d-cd4eb513df6e)

5. Once all details have been entered then click Review+create to deploy all objects.

   Once complete then you can view your Web App details from the Overview page.

  ![image](https://github.com/juribalimited/deploy-appm-gateway/assets/109281915/02d00444-f7ea-4902-a744-86ff44e5d11b)

   The web App will only accept connections from your AppM application. This be verified within the CORS section

   ![image](https://github.com/juribalimited/deploy-appm-gateway/assets/109281915/635db2dd-bc96-4807-8dd1-2f71e583afe6)

6. Once created please ensure that the Guacamole IP within your AppM Desktop Gateway Integration is correct. This can be found by checking the Guacamole container and noting it's IP.
   
   ![image](https://github.com/juribalimited/deploy-appm-gateway/assets/109281915/885df0f7-ea02-4c45-83b2-7a850abca71c)

   Then from within the main AppM application under System > Integrations > Desktop Gateways please update with correct IP and test connectivity. This should come back as successful. If you had to make a change to this then a new hex encryption key will be generated.

   ![image](https://github.com/juribalimited/deploy-appm-gateway/assets/109281915/726d7c4a-7e31-43cd-b367-86a3383aeb8f)

   If you need to change your Encryption Hex key or login details then these can be updated from the Configuration section of of your Web App. 

   ![image](https://github.com/juribalimited/deploy-appm-gateway/assets/109281915/440a494f-5047-40e9-b489-7ce7f691f760)










