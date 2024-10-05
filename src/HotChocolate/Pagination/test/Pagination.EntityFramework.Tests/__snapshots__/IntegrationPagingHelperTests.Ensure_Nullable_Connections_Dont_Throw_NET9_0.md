# Ensure_Nullable_Connections_Dont_Throw

## SQL 0

```sql
-- @__p_0='11'
SELECT f0."Id", f0."Name", b."Id" IS NULL, b."Id", b."Description"
FROM (
    SELECT f."Id", f."BarId", f."Name"
    FROM "Foos" AS f
    ORDER BY f."Name", f."Id"
    LIMIT @__p_0
) AS f0
LEFT JOIN "Bars" AS b ON f0."BarId" = b."Id"
ORDER BY f0."Name", f0."Id"
```

## Expression 0

```text
[Microsoft.EntityFrameworkCore.Query.EntityQueryRootExpression].OrderBy(t => t.Name).ThenBy(t => t.Id).Select(root => new Foo() {Id = root.Id, Name = root.Name, Bar = IIF((root.Bar == null), null, new Bar() {Id = root.Bar.Id, Description = root.Bar.Description})}).Take(11)
```

## Result

```json
{
  "data": {
    "foos": {
      "edges": [
        {
          "cursor": "Rm9vIDE6MQ=="
        },
        {
          "cursor": "Rm9vIDI6Mg=="
        }
      ],
      "nodes": [
        {
          "id": 1,
          "name": "Foo 1",
          "bar": null
        },
        {
          "id": 2,
          "name": "Foo 2",
          "bar": {
            "id": 1,
            "description": "Bar 1"
          }
        }
      ],
      "pageInfo": {
        "hasNextPage": false,
        "hasPreviousPage": false,
        "startCursor": "Rm9vIDE6MQ==",
        "endCursor": "Rm9vIDI6Mg=="
      }
    }
  }
}
```
