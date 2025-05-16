# SQL statements to flatten JSON structures as Views
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
