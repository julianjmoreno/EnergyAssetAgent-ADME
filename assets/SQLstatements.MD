# SQL statements to flatten JSON structures as Views
Flattening JSON-based structures is a crucial step in preparing data for analytics because it transforms complex, nested data into a more accessible, tabular format. Most analytical tools and platforms are optimized for flat, table-like structures where each row represents a single record and each column represents a distinct attribute. Nested JSON, with its hierarchical layers and embedded arrays or objects, can be difficult to query directly and often requires additional processing to extract meaningful insights. By flattening the data, analysts can simplify querying, improve performance, and ensure compatibility with data warehouses and business intelligence tools. This transformation also aids in data modeling, making relationships between variables more transparent and enabling more efficient feature engineering for machine learning. Furthermore, flat structures are easier to validate and debug, which enhances data quality and reliability. Overall, flattening JSON structures streamlines the analytical workflow and unlocks the full potential of the data.
<br>
The following SQL statements will help you flatten your JSON structures on the bronze level mirrored data from Azure Data Manager for Energy into a Fabric
<br>

## Basin OSDU WKS Master

```markdown
CREATE VIEW [dbo].[adme_osdu_wks_master_data_Basin_view]
AS SELECT
    CONCAT(id,':') AS BasinID,
    kind AS Kind,
    JSON_VALUE([data],'$.BasinName') AS BasinName,
    JSON_VALUE([data],'$.BasinDescription') AS BasinDescription
FROM [osdu_lake].[dbo].[storage_records_bronze]
WHERE [kind] = 'osdu:wks:master-data--Basin:1.2.0'
```

<br>

## Field WKS Master

```markdown
CREATE VIEW [dbo].[adme_osdu_wks_master_data_Field_view] AS
SELECT
    CONCAT(id,':') AS FieldID,
    kind AS Kind,
    TRY_CONVERT(DATE, JSON_VALUE([data], '$.EffectiveDate'),127) AS EffectiveDate,
    JSON_VALUE([data], '$.FieldName') AS FieldName,
    JSON_VALUE([data], '$.Source') AS Source,
    JSON_VALUE([data], '$.ExtensionProperties.slb.GeologicalAge') AS extSLB_GeologicalAge,
    JSON_VALUE([data], '$.ExtensionProperties.slb.ProducingFormation') AS extSLB_ProducingFormation
FROM [osdu_lake].[dbo].[storage_records_bronze]
WHERE [kind] = 'osdu:wks:master-data--Field:1.1.0'
```

<br>

## Well WKS Master

```markdown
CREATE VIEW [dbo].[adme_osdu_wks_master_data_Well_view] AS
SELECT
    CONCAT(id,':') AS WellID,
    kind AS Kind,
    JSON_VALUE([data], '$.FacilityName') AS FacilityName,
    JSON_VALUE([data], '$.FacilityTypeID') AS FacilityTypeID,
    JSON_VALUE([data], '$.DefaultVerticalMeasurementID') AS DefaultVerticalMeasurementID,

    -- Facility Operator Information
    JSON_VALUE([data], '$.FacilityOperators[0].FacilityOperatorID') AS FacilityOperatorID,
    JSON_VALUE([data], '$.FacilityOperators[0].FacilityOperatorOrganisationID') AS FacilityOperatorOrganisationID,

    -- Well Information - SLB Extensions
    JSON_VALUE([data], '$.ExtensionProperties.slb.kind') AS extSLB_Kind,
    JSON_VALUE([data], '$.ExtensionProperties.slb.basinName') AS extSLB_BasinName,
    JSON_VALUE([data], '$.ExtensionProperties.slb.directionWell') AS extSLB_DirectionWell,
    JSON_VALUE([data], '$.ExtensionProperties.slb.fluidWell') AS extSLB_FluidWell,
    JSON_VALUE([data], '$.ExtensionProperties.slb.wellPurpose') AS extSLB_WellPurpose,
    JSON_VALUE([data], '$.ExtensionProperties.slb.wellStatus') AS extSLB_WellStatus,
    JSON_VALUE([data], '$.ExtensionProperties.slb.wellType') AS extSLB_WellType,

    -- Spatial Location
    JSON_VALUE([data], '$.ExtensionProperties.slb.locationWGS84.features[0].geometry.type') AS GeometryType,
    TRY_CONVERT(FLOAT, JSON_VALUE([data], '$.ExtensionProperties.slb.locationWGS84.features[0].geometry.coordinates[0]')) AS Longitude,
    TRY_CONVERT(FLOAT, JSON_VALUE([data], '$.ExtensionProperties.slb.locationWGS84.features[0].geometry.coordinates[1]')) AS Latitude,
    
    TRY_CONVERT(FLOAT, JSON_VALUE([data], '$.SpatialLocation.Wgs84Coordinates.features[0].geometry.coordinates[0]')) AS SpatialLongitude,
    TRY_CONVERT(FLOAT, JSON_VALUE([data], '$.SpatialLocation.Wgs84Coordinates.features[0].geometry.coordinates[1]')) AS SpatialLatitude,

    -- Facility Event
    TRY_CONVERT(DATE, JSON_VALUE([data], '$.FacilityEvents[0].EffectiveDateTime'), 127) AS FacilityEventDate_Spud,
    JSON_VALUE([data], '$.FacilityEvents[0].FacilityEventTypeID') AS FacilityEventType_Spud,

    -- Geo Contexts
    JSON_VALUE([data], '$.GeoContexts[0].GeoPoliticalEntityID') AS GeoPoliticalEntity_Country,
        CASE 
        WHEN JSON_VALUE([data], '$.GeoContexts[1].FieldID') IS NULL THEN JSON_VALUE([data], '$.GeoContexts[1].GeoPoliticalEntityID') 
        ELSE NULL
    END AS GeoPoliticalEntity_LeaseBlock,
    CASE 
        WHEN JSON_VALUE([data], '$.GeoContexts[1].GeoTypeID') = 'Field' THEN JSON_VALUE([data], '$.GeoContexts[1].FieldID')
        ELSE JSON_VALUE([data], '$.GeoContexts[2].FieldID')
    END AS GeoPoliticalEntity_Field,

    -- Vertical Measurements
    TRY_CONVERT(FLOAT, JSON_VALUE([data], '$.VerticalMeasurements[0].VerticalMeasurement')) AS VerticalMeasurement_REF,
    JSON_VALUE([data], '$.VerticalMeasurements[0].VerticalMeasurementID') AS VerticalMeasurementID_REF,
    TRY_CONVERT(FLOAT, JSON_VALUE([data], '$.VerticalMeasurements[1].VerticalMeasurement')) AS VerticalMeasurement_WH_ELEV,
    JSON_VALUE([data], '$.VerticalMeasurements[1].VerticalMeasurementID') AS VerticalMeasurementID_ELEV,

    -- Metadata
    JSON_VALUE([data], '$.OperatingEnvironmentID') AS OperatingEnvironmentID,
    JSON_VALUE([data], '$.ResourceSecurityClassification') AS ResourceSecurityClassification,
    JSON_VALUE([data], '$.ExistenceKind') AS ExistenceKind,
    JSON_VALUE([data], '$.Source') AS Source
FROM [osdu_lake].[dbo].[storage_records_bronze]
WHERE [kind] = 'osdu:wks:master-data--Well:1.0.0'
```

<br>

## Wellbore WKS Master

```markdown
create view adme_osdu_wks_master_data_Wellbore_view as
SELECT
   -- Facility Information
   JSON_VALUE([data], '$.FacilityName') AS FacilityName,
   JSON_VALUE([data], '$.FacilityTypeID') AS FacilityTypeID,
   JSON_VALUE([data], '$.CurrentOperatorID') AS CurrentOperatorID,
   JSON_VALUE([data], '$.DefaultVerticalMeasurementID') AS DefaultVerticalMeasurementID,
   -- Facility Information - SLB Extensions
   JSON_VALUE([data], '$.ExtensionProperties.slb.kind') AS extSLB_Kind,
   JSON_VALUE([data], '$.ExtensionProperties.slb.wellborePurpose') AS extSLB_WellborePurpose,
   JSON_VALUE([data], '$.ExtensionProperties.slb.wellboreStatus') AS extSLB_WellboreStatus,
   JSON_VALUE([data], '$.ExtensionProperties.slb.wellboreType') AS extSLB_WellboreType,
   JSON_VALUE([data], '$.ExtensionProperties.slb.locationWGS84.features[0].geometry.type') AS extSLB_GeometryType,
   TRY_CONVERT(FLOAT, JSON_VALUE([data], '$.ExtensionProperties.slb.locationWGS84.features[0].geometry.coordinates[0]')) AS extSLB_Longitude,
   TRY_CONVERT(FLOAT, JSON_VALUE([data], '$.ExtensionProperties.slb.locationWGS84.features[0].geometry.coordinates[1]')) AS extSLB_Latitude,
   -- Extract Well ID and Name Aliases
   JSON_VALUE([data], '$.WellID') AS WellID,
   JSON_VALUE([data], '$.NameAliases[0].AliasName') AS AliasName,
   -- Extract Vertical Measurements
   TRY_CONVERT(FLOAT, JSON_VALUE([data], '$.VerticalMeasurements[0].VerticalMeasurement')) AS VerticalMeasurement_REF,
   JSON_VALUE([data], '$.VerticalMeasurements[0].VerticalMeasurementID') AS VerticalMeasurementID1,
   TRY_CONVERT(FLOAT,JSON_VALUE([data], '$.VerticalMeasurements[1].VerticalMeasurement')) AS VerticalMeasurement_TotalDepth,
   JSON_VALUE([data], '$.VerticalMeasurements[1].VerticalMeasurementID') AS VerticalMeasurementID2,
 
   -- Extract Facility Events (First Two Events)
   TRY_CONVERT(DATE, JSON_VALUE([data], '$.FacilityEvents[0].EffectiveDateTime'), 127) AS FacilityEventDate_Spud,
   JSON_VALUE([data], '$.FacilityEvents[0].FacilityEventTypeID') AS FacilityEvent1Type,
   TRY_CONVERT(DATE, JSON_VALUE([data], '$.FacilityEvents[1].EffectiveDateTime'), 127) AS FacilityEventDate_Completion,
   JSON_VALUE([data], '$.FacilityEvents[1].FacilityEventTypeID') AS FacilityEvent2Type,
 
   -- Extract GeoPolitical Entities and GeoContexts
   JSON_VALUE([data], '$.GeoContexts[0].GeoPoliticalEntityID') AS GeoPoliticalEntity_Country,
   JSON_VALUE([data], '$.GeoContexts[1].GeoPoliticalEntityID') AS GeoPoliticalEntity_LeaseBlock,
   CASE 
       WHEN JSON_VALUE([data], '$.GeoContexts[2].GeoTypeID') = 'Field' THEN JSON_VALUE([data], '$.GeoContexts[2].FieldID')
       ELSE NULL
   END AS GeoPoliticalEntity_Field,
   CASE 
       WHEN JSON_VALUE([data], '$.GeoContexts[3].BasinID') IS NULL THEN JSON_VALUE([data], '$.GeoContexts[2].BasinID')
       ELSE JSON_VALUE([data], '$.GeoContexts[3].BasinID')
   END AS GeoPoliticalEntity_Basin,
 
   -- Extract Spatial Location (WGS84 Coordinates)
   TRY_CONVERT(FLOAT, JSON_VALUE([data], '$.SpatialLocation.Wgs84Coordinates.features[0].geometry.coordinates[0]')) AS SpatialLongitude,
   TRY_CONVERT(FLOAT, JSON_VALUE([data], '$.SpatialLocation.Wgs84Coordinates.features[0].geometry.coordinates[1]')) AS SpatialLatitude,
 
   -- Extract Metadata
   id AS WellboreID,
   kind AS Kind,
   JSON_VALUE([data], '$.Source') AS Source,
   JSON_VALUE([data], '$.OperatingEnvironmentID') AS OperatingEnvironmentID,
   JSON_VALUE([data], '$.BusinessIntentionID') AS BusinessIntentionID
FROM [OSDU_LAKE].[dbo].[storage_records_bronze]
WHERE [kind] = 'osdu:wks:master-data--Wellbore:1.5.0'
```

<br>

## WellboreMarketSet Work Product Component

```markdown
CREATE VIEW [dbo].[adme_osdu_work-product_component_WellboreMarkerSet_view]
AS SELECT
    CONCAT(id,':') AS MarkerID,
    kind AS Kind,
    JSON_VALUE([data], '$.WellboreID') AS WellboreID,
    JSON_VALUE(m.value, '$.MarkerName') AS MarkerName,
    TRY_CONVERT(FLOAT, JSON_VALUE(m.value, '$.MarkerMeasuredDepth')) AS MarkerMeasuredDepth,
    JSON_VALUE(m.value, '$.MarkerTypeID') AS MarkerTypeID,
    JSON_VALUE(m.value, '$.FeatureTypeID') AS FeatureTypeID
FROM [osdu_lake].[dbo].[storage_records_bronze]
CROSS APPLY OPENJSON([data], '$.Markers') AS m
WHERE [kind] = 'osdu:wks:work-product-component--WellboreMarkerSet:1.0.0'
AND ISJSON([data]) = 1;
```

