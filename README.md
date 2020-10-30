# Edegscan-AzureSentinel-Integration

## Functionality
This package contains three separate logic apps:
* **edgescan_vulnerabilities**
* **edgescan_assets**
* **edgescan_hosts**


The logic apps are set to run daily and to ingest records created in Edgescan over the past two days. 
  
**Note** In the case of hosts, since no created date field appears to exist, the field indicating the last update is used instead.
  
The logic apps will scan the entries created within the last 7 days in the **custom logs** in Azure Sentinel for IDs duplicate IDs before adding a new entry to the corresponding log.


Entries will be stored in Azure Sentinel **custom logs** with the following table names:
* **edgescan_vulnerabilities_CL**
* **edgescan_assets_CL**
* **edgescan_hosts_CL**

### Viewing Custom Logs
* From your home page, navigate to the Azure Sentinel service
* There, select the workspace your deployed logic apps reference
* There, click on Logs in the left-hand menu and expand Custom Logs

![customlogs](Images/customlogs.png)


## Deploy the Logic Apps
###edgescan_vulnerabilities

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FArbala-Security%2FEdegscan-AzureSentinel-Integration%2Fmain%2Fazuredeploy1.json" target="_blank">
    <img src="https://aka.ms/deploytoazurebutton""/>
</a>
      
      
add the following condition to the URI after deployment: 

**?c[date_opened_after]=@{formatDateTime(addDays(utcNow(),-2),'yy-MM-dd')}**



###edgescan_assets

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FArbala-Security%2FEdegscan-AzureSentinel-Integration%2Fmain%2Fazuredeploy2.json" target="_blank">
    <img src="https://aka.ms/deploytoazurebutton""/>
</a>

      
Optionally, add the following condition to the URI after deployment: 

**?c[created_at]=@{formatDateTime(addDays(utcNow(),-2),'yy-MM-dd')}**


###edgescan_hosts

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FArbala-Security%2FEdegscan-AzureSentinel-Integration%2Fmain%2Fazuredeploy3.json" target="_blank">
    <img src="https://aka.ms/deploytoazurebutton""/>
</a>

Optionally, add the following condition to the URI after deployment: 

**?c[updated_at]=@{formatDateTime(addDays(utcNow(),-2),'yy-MM-dd')}**


## Usage
For initial runs, the URI in the GET request should be modified, removing every character after the question mark.
Be sure to while first a copy of the original URI.

![uri](Images/URI.png)

By running the logic app without the conditional constraints, all existing vulnerability, host, and asset records in Edgescan will be integrated into Azure Sentinel.
To do this, enable each logic app and run their triggers.

![enable](Images/enable.png)

![trigger](Images/trigger​.png)

Once this is done, restore the URIs in each logic app to their original text.

If you do not wish to have constant polling of assets and hosts, these logic apps may be disabled again.

