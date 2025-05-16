> [!IMPORTANT]  
> The conntent and views expressed here are personal and meant for information/education purposes only and not representative of my employer directly or indirectly and not investment advice. Any mention of company names are for illustrative examples only.
> 
# Chat with my Energy Asset Data
Azure Data Manager for Energy brings together the power of the Microsoft Cloud with the open, standards-based OSDU® Data Platform and a growing Partner Ecosystem in a fully-managed PaaS solution that simplifies implementation and speeds time to market. 

In addition to the large structured sets of subsurface data like Wells - well logs, wellbore trajectories, wellbore markers; Seismic, Reservoir data and others ingested on Azure Data Manager for Energy, Energy Companies also have unstructured data in a form of geological, well, seismic interpretation and other reports as PDFs and images, available on other storage systems. End-users are required to bring together structured & unstructured data to unveil insights and properly evaluate their Energy Assets.

This solution accelerator leverages the full power of Agentic AI to showcase the integration of Copilot agents with energy data. Play the video below to watch the "Chat with your Energy Asset" Sizzler.

https://github.com/user-attachments/assets/09fd3735-2097-4fc7-a11f-7535c54f2276

## Before you start
If you would like to get a live demonstration of the solution and some guidance, please feel free to request it here:

[![Button Shield1]][Shield1]
<br>

<!---------------------------------------------------------------------------->
[Button Shield1]: https://img.shields.io/badge/Request_Your_Personal_Demo_Here-37a779?style=for-the-badge
[License]: LICENSE
[Shield1]: https://forms.office.com/r/He2aHtAVMf
[KBD]: Types/KBD.md
[#]: #

## Important Reading
Before you deep dive, we strongly recommend you get familiar with the following subjects so you better understand the reasons behind and the value proposition of this solution accelerator:

|  Please get familiar with the following subjects                                  |
|:---                                                                               |
| **Azure Data Manager for Energy**                                                 |
| https://azure.microsoft.com/en-us/products/data-manager-for-energy                |
| **Microsoft Copilot**                                                             |
| https://azure.microsoft.com/en-us/products/data-manager-for-energy/               |
| **Microsoft Copilot Studio**                                                      |
| https://www.microsoft.com/en-us/microsoft-copilot/microsoft-copilot-studio/       |

## Understanding the Solution
The following diagram illustrates the main components on this solution accelerator
<br> <br>


![Solution Accelerator Diagram](https://github.com/julianjmoreno/EnergyAssetAgent-ADME/blob/main/assets/images/EnergyAssetDataChat_solDiagram.png "Solution Accelerator Diagram")

**A. The data landscape:** represented by structured data sources (like Azure Data Manager for Energy), including metadata and bulk data, and corresponding unstructured data sources (such as SharePoint or OneDrive, which contains data in the form of PDFs, Excel files, PowerPoint presentations, and other documents).

**B. The consumption zone:** powered by OneLake under Microsoft Fabric, provides a data workspace for data mirrored from Azure Data Manager for Energy to all AI-driven Fabric workloads including but not limited to Fabric Data Agents, to facilitates the delivery of data to the Copilot Agent.

**C. The Copilot Agent:** Specialized set of AI tools built to handle specific processes or solve business challenges, in this case to provide access to data insights from structured data store in ADME, and unstructured data store on corresponding SharePoint site. 
They also automate tasks, analyze data, make decisions, and adapt to new challenges.
To access the data, the solution uses “Knowledge Sources” that points to:
<ol>
  <li>a Fabric Data Agent connector (advanced) that data mirrored from Azure Data Manager for Energy</li>
  <li>a SharePoint connector (featured) to the corresponding SharePoint site with New Zealand data</li>
</ol>


## Getting Started
### Prerequisites 

<ol>
  <li> <p> <strong>Get your dataset ready</strong> <br>
    For this solution, we have selected the New Zealand Petroleum Exploration Data Pack as dataset, which features a large selection of open-file seismic and well data, interpretation projects, reports and studies. To understand more about the New Zealand Petroleum Exploration Data Pack here:<br>
    https://www.nzpam.govt.nz/maps-geoscience/petroleum-datapack<br>
  <strong>Note:</strong> You could also use your own datasets.</p>
  </li>
  <li> <p> <strong>Make available your unstructured data in SharePoint</strong> <br>
   Make sure that unstructured data in a form of geological, well, seismic interpretation and other reports as PDFs and images, are available on a SharePoint site / folder of your preferences. Please remember that the Agent will respect the entitlement / secutiry configuration in place.<br>
    </p>
  </li>
</ol>
 
## QuickStart
### Create your Copilot Studio agent
For our solution we are going to use Copilot Studio, so navigate to here using the following URL:<br><br>
<a href="https://copilotstudio.microsoft.com/">https://copilotstudio.microsoft.com/</a>

![alt text](https://github.com/julianjmoreno/EnergyAssetAgent-ADME/blob/main/assets/images/CopilotStudio_Home.png)

Make sure that the environment is the one you have created in previous steps.

![alt text](https://github.com/julianjmoreno/EnergyAssetAgent-ADME/blob/main/assets/images/CopilotStudio_Env.png)

Select the “Create” option on the left-hand side of the screen. You will see a “Create Assistant” that would allow you to follow an specific “Template” or “start from scratch”

![alt text](https://github.com/julianjmoreno/EnergyAssetAgent-ADME/blob/main/assets/images/CopilotStudio_CreateNew.png)

Let’s start “from scratch”, to do that click the “New Agent” button.<br><br>

You will land into the Copilot Agent “creation screen” by conversational assistance, lets skip and go to configure.

![alt text](https://github.com/julianjmoreno/EnergyAssetAgent-ADME/blob/main/assets/images/CopilotStudio_CreateFirstScreen.png)

Once conversational creation experience is skipped, you will see an screen to provide Agent name, description, instructions, connect knowledge sources, and optionally, define starter prompts to guide user interactions. 

![alt text](https://github.com/julianjmoreno/EnergyAssetAgent-ADME/blob/main/assets/images/CopilotStudio_CreateFirstDetails.png)

Start by providing a meaningful name, description… as well as an icon !!! We will now go setting up some instructions.

***Copilot Agent Instructions***<br>
Similar to the Data Agent AI Instructions, Copilot Agent Instructions will direct the behavior of the agent, including its tasks and how it completes them. Here is a sample set of instructions that we have used to set up the solution.<br><br>

[{SAMPLE COPILOT AGENT INSTRUCTIONS}](https://github.com/julianjmoreno/EnergyAssetAgent-ADME/blob/main/assets/CopilotAgent-Instructions.MD)<br><br>

***Adding knowledge sources to your Copilot Agent***<br>
Now you would be able to add data, files, and other resources that your agent will use to learn. These sources form the basis for your agent's responses. Let’s click “Add Knowledge” and add our sources.

![alt text](https://github.com/julianjmoreno/EnergyAssetAgent-ADME/blob/main/assets/images/CopilotStudio_KnowSouce.png)

As mentioned from the beginning, the idea is to bring structured and unstructured data together to provide responses around Energy Assets. We selected:<BR>
	<ul>
  <li> <strong>SharePoint to host all unstructured data</strong> coming from the dataset provided by the New Zealand Petroleum data pack</li>
  <li> <strong>Structured data mirrored from Azure Data Manager for Energy</strong> into Fabric Lakehouse coming from the same dataset provided by the New Zealand Petroleum data pack</li>
</ul>

For SharePoint, click on the corresponding option on the “Add knowledge” screen to connect to SharePoint as knowledge source

![alt text](https://github.com/julianjmoreno/EnergyAssetAgent-ADME/blob/main/assets/images/CopilotStudio_KsSharePoint.png)

> [!NOTE]
> Content from SharePoint will only be available to authenticated end-users on your Company’s Tenant.

<br>
For the data mirrored from Azure Data Manager for Energy into Fabric Lakehouse, go to “Advanced” options

![alt text](https://github.com/julianjmoreno/EnergyAssetAgent-ADME/blob/main/assets/images/CopilotStudio_KsFabric.png)

Select the “Microsoft Fabric” option and then your “Data Agent” would appear in the list of options.<br><br>

With the “Knowledge sources” already associated, you could click on “Create”.<br><br>

***Testing your Copilot Agent***
Once your Agent in create you could start your tests and validate responses.

![alt text](https://github.com/julianjmoreno/EnergyAssetAgent-ADME/blob/main/assets/images/CopilotStudio_AgentTest.png)

On the right-hand side, red square, is where your prompts would be enters and responses would appear.<br><br>

In the middle, blue square, the whole processing would appear, and it would show details of the sources crawled. <br><br>

Once you are satisfied with the responses, you could proceed to “Publish” your Agent

![alt text](https://github.com/julianjmoreno/EnergyAssetAgent-ADME/blob/main/assets/images/CopilotStudio_Publish.png)

Once your Agent is published, you will be able to see it in your Copilot Chat UI.<br><br>

Go to https://copilot.cloud.microsoft/ or https://m365.cloud.microsoft/chat and select the option Get Agents

![alt text](https://github.com/julianjmoreno/EnergyAssetAgent-ADME/blob/main/assets/images/EnergyAssetDataChat_getAgent.png)

Find your Agent by name, using the search capabilities, or looking under the category “Built for your org” and click on it

![alt text](https://github.com/julianjmoreno/EnergyAssetAgent-ADME/blob/main/assets/images/EnergyAssetDataChat_install.png)

Now you could add your Agent to your environment and validate the agent from the Copilot Chat.

![alt text](https://github.com/julianjmoreno/EnergyAssetAgent-ADME/blob/main/assets/images/EnergyAssetDataChat_ChatUI.png)

You will see know the Copilot Agent available in Copilot Chat, Team and M365 apps. 

## Sample Prompts
Here are some sample promts for this dataset<br>
https://github.com/julianjmoreno/EnergyAssetAgent-ADME/blob/main/assets/chatSamplePrompts.MD<br><br>
