> [!IMPORTANT]  
> The conntent and views expressed here are personal and meant for information/education purposes only and not representative of my employer directly or indirectly and not investment advice. Any mention of company names are for illustrative examples only.
> 
# Sample AI instructions for your Fabric Data Agent
<br>
These instructions helps the data agent understand your data by writing a detailed system prompt. The folling instructions assumes the tables / views available on the mirrored data from Azure Data Manager for Energy into Fabric leverading the New Zealand Petroleum data pack

```markdown
the osdu_lake is a lakehouse that have structured data about Basins, Fields, Wells, Wellbores and WellboreMarkerSet.

The Basins data is accessible from the view adme_osdu_wks_master_data_Basin_view and defines a natural geographic area covering a single depositional system.

The fields data is accessible from the view adme_osdu_wks_master_data_Field_view and defines A mineral deposit that has been exploited for commercial purposes.

The Wells data is accessible from the view adme_osdu_wks_master_data_Well_view and defines a proposed, permitted or actual drilled hole in the ground designed to exchange (or facilitate the exchange of) fluids between a subsurface reservoir and the surface (or another reservoir), or to enable the detection and measurement of rock properties. A well consists of one or more wellbores, extending from the bottom of each wellbore up to the top of the Xmas tree on the wellhead.

Wells are related to Fields via GeoPoliticalEntity_Field
Well are related to Wellbores via WellID

The wellbores data is accessible from the view adme_osdu_wks_master_data_wellbore_view and defines a unique well path beginning from the top of the outermost casing string and extending to the total drilled depth of an individual hole. A wellbore may have another wellbore as a parent; in this case, the child wellbore includes a common section from the wellhead down to where it exits the parent wellbore and continues to the total depth of the hole

Wellbores are related to Fields via GeoPoliticalEntity_Field

Wellbores are related to Fields via GeoPoliticalEntity_Basin

Wellbores are related to Wells via WellID

WellboreMarkerSet data is accessible from the view adme_osdu_work-product_component_WellboreMarkerSet_view and defines Wellbore Markers identify the depth in a wellbore, measured below a reference elevation, at which a person or an automated process identifies a noteworthy observation, which is usually a change in the rock that intersects that wellbore. Formation Marker data includes attributes/properties that put these depths in context. Formation Markers are sometimes known as picks or formation tops

WellboreMarkerSet are related to Wellbores via WellboreID



```

