# AppSync Cheatsheet

## Mapping (VTL)

### Context

```
{
   "arguments" : { ... },
   "source" : { ... },
   "result" : { ... }, ## Only present in the response
   "identity" : { ... }
}
```

### Examples for reference

<details>
  <summary>List all items</summary>
  <br />
  <p>Request</p>
  <pre>
    {
      "version" : "2017-02-28",
      "operation" : "Scan"
    }
  </pre>
  <p>Response</p>
  <pre>
    $util.toJson($ctx.result.items)
  </pre>
  <hr />
</details>

<details>
  <summary>Getting sub-list or resources for parent type</summary>
  <p>Lets say you have schema:</p>
  <pre>
    type List {
      id: ID,
      items: [Item]
    }
    type Item {
      id: ID,
      listId: ID,
      text: String
    }
  </pre>
  <br />
  <p>In order to map your items to your type list, you'll do the following:</p>
  <p>Request</p>
  <pre>
    {
      "version" : "2017-02-28",
      "operation" : "Scan",
      "filter" : {
          "expression" : "listId = :listId",
          "expressionValues" : {
              ":listId" : { "S" : "${context.source.id}" }
          }
      }
    }
  </pre>
  <p>Response</p>
  <pre>
    $util.toJson($ctx.result.items)
  </pre>
  <hr />
</details>
