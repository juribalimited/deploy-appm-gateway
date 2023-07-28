# AppM Gateway - One click deploy

Juriba offer a one click deploy template to create the desktop gateway infrastructure for use with AppM. The desktop gateway is used to facilitate access between the main AppM application and guest VM's by creating a set of resources which allows communication to Guest VM's on a private subnet.
The one click deploy template creates a Container Instance, Azure Web App and all associated networking infrastructure to setup connectivity quickly and easily. However customers are **not required** to use the one click deploy template. If they want to consume the container images individually leveraging a different container engine/runtime the images can be pulled from here:
 - [AppM Gateway](https://hub.docker.com/r/juriba/appmgateway)
 - [Guacd](https://hub.docker.com/r/guacamole/guacd)


## Gateway infrastructure
![image](https://github.com/juribalimited/deploy-appm-gateway/assets/109281915/26cba5f0-a867-4fe1-83d4-19ac0a109d30)

## Usage


1. To use the one click deploy template click the "Deploy to Azure" below:

- [![Deploy To Azure](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.svg?sanitize=true)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjuribalimited%2Fdeploy-appm-gateway%2Fmain%2FmainTemplate.json/createUIDefinitionUri/https%3A%2F%2Fraw.githubusercontent.com%2Fjuribalimited%2Fdeploy-appm-gateway%2Fmain%2FcreateUiDefinition.json)
- [![Visualize](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/visualizebutton.svg?sanitize=true)](http://armviz.io/#/?load=https%3A%2F%2Fraw.githubusercontent.com%2Fjuribalimited%2Fdeploy-appm-gateway%2Fmain%2FmainTemplate.json)

2. Basics

    The initial tab allows you to select which Azure subscription you want to deploy into. You can also select an existing resource group or create a new one.
    - `Region` - This will be the geographical location of the resources. **Any virtual machines** will also need to be deployed into the matching region.
    * Juriba **suggests** creating a new resource group with a relevant name so that there is a clear separation for these Azure resources.

    ![image](https://github.com/juribalimited/deploy-appm-gateway/assets/109281915/f7e2831f-db99-4347-81fe-a25d244044d5)


3. Networking

    The networking tab allows you to select an existing network or **create a new virtual network**. By default a new virtual network and subnets are created, however using the drop down menu you can select an existing network held within your Azure subscription. The virtual network requires three subnets for gateway resources, guest virtual machines and the guacamole container. You can also amend the Network Security Group names on this page if required.
    * Juriba **suggests** creating a new network as the subnet settings <u>**will overwrite**</u> any existing subnets held within the network selected.
    ```diff
    - Any subnets on an existing network will be removed!
    ```

    ![image](https://github.com/juribalimited/deploy-appm-gateway/assets/109281915/21d53c53-387a-4a32-b62a-34483d822dda)

4.  AppM Desktop Gateway

    The third panel askes for configuration relating to the configuration of the gateway.
    - `AppM Website Address` - This will be the web address you use to access AppM. This should not include anything after the first `/` within the url.
    - `AppM Version` - This is the version of AppM being run, currently this will always default to latest.
    - `Gateway WebApp Name` This is the name of the resource craated within azure that you want to use for your desktop gateway. By default this **is** exposed to the internet with the `azurewebsites.net` suffix. Once entered please click out of the entry box, this will start a check to ensure that address is available. If available you will see a green tick, otherwise you will receive an error message (examples below).
    * Juriba **suggests** that the Azure Web Application (Desktop Gateway) is configured with private networking or IP restrictions to further secure the gateway.

    ![image](https://github.com/juribalimited/deploy-appm-gateway/assets/109281915/59ee9fb1-e8f4-4146-ab35-467abc3ea444)

    ![image](https://github.com/juribalimited/deploy-appm-gateway/assets/109281915/c4fbf88b-56a0-44d7-88c9-34ebe54071ed)

    The Encryption Key can be obtained from the main AppM application under System > Integrations > Desktop Gateways.

    Enter the details that you know here and then click update. You will need to append the address with "/vnc/tunnel". So if your Gateway WebApp address is mygatewaywebapp.azurewebsites.net then you will need to enter **https://mygatewaywebapp.azurewebsites.net/vnc/tunnel**. You wil not be able to test connection at this point as we have not completed the desktop gateway deployment. Once you have clicked update
    an encryption hex key will be created for you. Please enter this into the Encryption key fields.

    ![image](https://github.com/juribalimited/deploy-appm-gateway/assets/109281915/fd095519-53f0-4c81-a56b-366f8dbb9028)
    ![image](https://github.com/juribalimited/deploy-appm-gateway/assets/109281915/770ef24c-c60f-4a4d-b665-4a0799df931f)



    The final fields for Username and Password will need to be filled in. These details won't be needed for general AppM usage but may be needed when troubleshooting connectivity issues between AppM and Guest VM's. Please store these details securely should you need them in the future.

    ![image](https://github.com/juribalimited/deploy-appm-gateway/assets/109281915/119e0081-a39a-49b6-99ab-9b92e03a9430)

5. Apache Guacamole

    The fourth panel enables you to set a name for the Guacamole container instance.

    ![image](https://github.com/juribalimited/deploy-appm-gateway/assets/109281915/aa546a13-5193-4bf2-984d-cd4eb513df6e)

6. Review and create

    Once all details have been entered then click Review+create to deploy all objects.

    Once complete then you can view your Web App details from the Overview page.

    ![image](https://github.com/juribalimited/deploy-appm-gateway/assets/109281915/02d00444-f7ea-4902-a744-86ff44e5d11b)

    The web App will only accept connections from your AppM application. This be verified within the CORS section

    ![image](https://github.com/juribalimited/deploy-appm-gateway/assets/109281915/635db2dd-bc96-4807-8dd1-2f71e583afe6)

7. Once created please ensure that the Guacamole IP within your AppM Desktop Gateway Integration is correct. This can be found by checking the Guacamole container and noting it's IP.

    ![image](https://github.com/juribalimited/deploy-appm-gateway/assets/109281915/885df0f7-ea02-4c45-83b2-7a850abca71c)

    Then from within the main AppM application under System > Integrations > Desktop Gateways please update with correct IP and test connectivity. This should come back as successful. If you had to make a change to this then a new hex encryption key will be generated.

    ![image](https://github.com/juribalimited/deploy-appm-gateway/assets/109281915/726d7c4a-7e31-43cd-b367-86a3383aeb8f)

    If you need to change your Encryption Hex key or login details then these can be updated from the Configuration section of of your Web App.

    ![image](https://github.com/juribalimited/deploy-appm-gateway/assets/109281915/440a494f-5047-40e9-b489-7ce7f691f760)










