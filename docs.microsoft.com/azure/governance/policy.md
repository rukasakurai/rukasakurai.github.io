Sample
```
{
  "mode": "Indexed",
  "policyRule": {
    "if": {
      "allOf": [
        {
          "not": {
            "field": "location",
            "in": "[parameters('allowedLocations')]"
          }
        },
        {
          "field": "type",
          "equals": "Microsoft.DocumentDB/databaseAccounts"
        }
      ]
    },
    "then": {
      "effect": "audit"
    }
  },
  "parameters": {
    "allowedLocations": {
      "type": "Array",
      "metadata": {
        "displayName": "Allowed locations",
        "description": "The list of locations that can be specified when deploying resources",
        "strongType": "location"
      },
      "defaultValue": [
        "japaneast"
      ]
    }
  }
}
```

Q: Is it possible to send alerts when non-compliance is detected?
A: Not easily, but below is one solution
https://blog.tyang.org/2021/12/06/monitoring-azure-policy-compliance-states-2021-edition/