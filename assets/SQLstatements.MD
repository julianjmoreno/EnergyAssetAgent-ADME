# SQL statements to flatten JSON structures as Views
Flattening JSON-based structures is a crucial step in preparing data for analytics because it transforms complex, nested data into a more accessible, tabular format. Most analytical tools and platforms are optimized for flat, table-like structures where each row represents a single record and each column represents a distinct attribute. Nested JSON, with its hierarchical layers and embedded arrays or objects, can be difficult to query directly and often requires additional processing to extract meaningful insights. By flattening the data, analysts can simplify querying, improve performance, and ensure compatibility with data warehouses and business intelligence tools. This transformation also aids in data modeling, making relationships between variables more transparent and enabling more efficient feature engineering for machine learning. Furthermore, flat structures are easier to validate and debug, which enhances data quality and reliability. Overall, flattening JSON structures streamlines the analytical workflow and unlocks the full potential of the data.
<br>

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

