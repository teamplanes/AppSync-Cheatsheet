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

#### `identity`

If you are using Cognito for authentication your `itentity` object will look like:

```
{
    "sub" : "uuid", ## <- Main user UUID
    "issuer" : "string",
    "username" : "string"
    "claims" : { ... },
    "sourceIp" : "x.x.x.x",
    "defaultAuthStrategy" : "string"
}
```

### Examples for reference

<details>
  <summary><strong>List all items</strong></summary>
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
  <summary><strong>Getting sub-list of resources for a parent type</strong></summary>
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
