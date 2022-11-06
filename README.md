
## AzureHole

Based on [Pi-hole](https://pi-hole.net/)



**Pi-hole** is a Linux network-level advertisement and Internet tracker blocking application which acts as a DNS sinkhole, intended for use on a private network.

**AzureHole** is the arm template to create and run Pi-hole in Azure Container Instance (ACI), having the settings in a storage account.

### Start the Deployment Process

**Create resource group**

    az group create --location westeurope --name pihole-rg

**Create Storage Account**

    az storage account create --name piholestrgacc --resource-group pihole-rg

**Create shares**

    az storage share create --account-name piholestrgacc --name pihole-etc-pihole
    az storage share create --account-name piholestrgacc --name pihole-etc-dnsmasq

**Get the security keys for the Storage Account**

    az storage account keys list --account-name piholestrgacc
**Example:**

    [
	    {
		    "keyName": "key1",
		    "permissions": "Full",
		    "value": "xxwZ4erV3tTnUq1.......................nWcmAlMiFLypllj2fsCQw=="
		},
		{
			"keyName": "key2",
			"permissions": "Full",
			"value": "xxWTrxt7eCO7Wf.......................Bsi5bHffilHe1aYzejsDcA=="
		}
	]


**note the Azure Storage Account name and one of the keys to use one of them in the parameter file (_azurehole.parameter.json_)**
_remeber to update the **azurehole.parameter.json** file with the desired values_

**Run the deplyment**

    az group deployment create --resource-group pihole-rg --template-file azurehole.template.json --parameters azurehole.parameter.json



**Or hitting in the link below (remember the Azure Storage Account name and one of the keys)**

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fsarmadjari%2FAzureHole%2Fmaster%2Fazurehole.template.json" target="_blank">
    <img src="https://raw.githubusercontent.com/sarmadjari/AzureHole/master/images/deploytoazure.png"/>
</a>

### Usefull commands

**to access the container and run command**

    az container exec --resource-group pihole-rg --name pihole --exec-command "/bin/bash"



**to change the password from withing the container**

    pihole -a -p testqwerty123456



**Get the IP address for the Container Group**

    az container show --resource-group pihole-rg --name pihole --query ipAddress.ip --output tsv
