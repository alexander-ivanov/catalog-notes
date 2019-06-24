# Catalog notes

Some lab tests need several specimens. Sometimes only several specimens can be selected, sometimes all of them should be used. Our current approach is to use ActivityDefinition resource, but this resource doesn't selection behavior for specimens. We decided to add a new field to ActivityDefinition called selectionBehavior. This issue can be solved without adding a new field by using a PlanDefinition.

PlanDefinition resource supports groups of ActivityDefinitions to be performed and selectionBehaviour for them. Here are some examples:

![AndGroup](/assets/PlanDefinitionForLabOrders-AndGroup.png)

Logical "and" expressed by ```"selectionBehavior": "all"```

```json
{
  "resourceType": "PlanDefinition",
  "identifier": [
    {
      "system": "http://loinc.org",
      "value": "34555-3"
    }
  ],
  "title": "Creatinine 24H renal clearance panel",
  "status": "active",
  "action": [
    {
      "groupingBehavior": "logical-group",
      "selectionBehavior": "all",
      "action": [
        {
          "definitionUrl": "https://example.org/fhir/ActivityDefinition/creatinine-in-serum-or-plasma"
        },
        {
          "definitionUrl": "https://example.org/fhir/ActivityDefinition/creatinine-in-24h-urine"
        }
      ]
    }
  ]
}
```

Logical "or" can be expressed in several ways. Usually we want users to choose "exactly one" specimen - ```"selectionBehavior": "exactly-one"```.

![OrGroup](/assets/PlanDefinitionForLabOrders-OrGroup.png)

```json
{
  "resourceType": "PlanDefinition",
  "identifier": [
    {
      "system": "http://loinc.org",
      "value": "2823-3"
    }
  ],
  "title": "Potassium [Moles/?volume] in Serum or Plasma",
  "status": "active",
  "action": [
    {
      "groupingBehavior": "logical-group",
      "selectionBehavior": "exactly-one",
      "action": [
        {
          "definitionUrl": "https://example.org/fhir/ActivityDefinition/creatinine-in-serum"
        },
        {
          "definitionUrl": "https://example.org/fhir/ActivityDefinition/creatinine-in-plasma"
        }
      ]
    }
  ]
}
```

PlanDefinition support group nesting so here is example:

![OrGroup](/assets/PlanDefinitionForLabOrders-ComplexExpressionGroup.png)

```json
{
  "resourceType": "PlanDefinition",
  "identifier": [
    {
      "system": "http://loinc.org",
      "value": "34555-3"
    }
  ],
  "title": "Creatinine 24H renal clearance panel",
  "status": "active",
  "action": [
    {
      "groupingBehavior": "logical-group",
      "selectionBehavior": "all",
      "action": [
        {
          "groupingBehavior": "logical-group",
          "selectionBehavior": "exactly-one",
          "action": [
            {
              "definitionUrl": "https://example.org/fhir/ActivityDefinition/creatinine-in-serum"
            },
            {
              "definitionUrl": "https://example.org/fhir/ActivityDefinition/creatinine-in-plasma"
            }
          ]
        },
        {
          "definitionUrl": "https://example.org/fhir/ActivityDefinition/creatinine-in-24h-urine"
        }
      ]
    }
  ]
}
```
