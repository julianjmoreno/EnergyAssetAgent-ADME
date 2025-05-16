> [!IMPORTANT]  
> The conntent and views expressed here are personal and meant for information/education purposes only and not representative of my employer directly or indirectly and not investment advice. Any mention of company names are for illustrative examples only.
> 
# Sample SQL scripts for your Fabric Data Agent
<br>
These are examples of how a natural language question would become a SQL query for your data. The data agent will automatically select up to three valid examples to share with the model each time it handles a user question. The following instructions assumes the tables / views available on the mirrored data from Azure Data Manager for Energy into Fabric leverading the New Zealand Petroleum data pack.
<br><br>

| Question                                                                          | SQL query |
|:---                                                                               |:---       |
| How many wellbores are currently producing?                                       |  SELECT COUNT(*) AS NumberOfProducingWellbores FROM [dbo].adme_osdu_wks_master_data_Wellbore_view] WHERE extSLB_WellboreStatus like '%producing%'         |
| What is the number of wellbores that are onshore?                                 | SELECT COUNT(*) AS OnshoreWellboreCount  FROM [dbo].[adme_osdu_wks_master_data_Wellbore_view]  WHERE OperatingEnvironmentID like '%Onshore%' |
| How many wellbores does the Placid Oil Company operator have?                     | SELECT COUNT(*) AS OperatorWellboreCount FROM [dbo].[adme_osdu_wks_master_data_Wellbore_view] WHERE CurrentOperatorID like '%PLACID%20OIL%20COMPANY%' |
| Give me the number of wellbores in 38160 license block.                           | SELECT COUNT(*) AS WellboreCount FROM [dbo].[adme_osdu_wks_master_data_Wellbore_view] WHERE GeoPoliticalEntity_LeaseBlock like '%38160%' |
| Give me the number of wellbores for each operator |                               | SELECT COUNT(*) AS WellboreCount, CurrentOperatorID FROM [dbo].[adme_osdu_wks_master_data_Wellbore_view] GROUP BY CurrentOperatorID |
| How many wellbores in Taranaki basin?                                             | SELECT COUNT(*) FROM [dbo].[adme_osdu_wks_master_data_Wellbore_view],
    [dbo].[adme_osdu_wks_master_data_Field_view] WHERE [dbo].[adme_osdu_wks_master_data_Wellbore_view].GeoPoliticalEntity_Field = [dbo].[adme_osdu_wks_master_data_Field_view].FieldID AND [dbo].[adme_osdu_wks_master_data_Wellbore_view].GeoPoliticalEntity_Basin like '%TARANAKI%20BASIN%'|


