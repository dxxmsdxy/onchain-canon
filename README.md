On-Chain Canon (OC)
===================
A metaprotocol for on-chain IP and attribution

OC is a decentralized metaprotocol for on-chain canons. Use the OC metaprotocol in combination with parent-child inscriptions to represent on-chain IP provenance, attribution, and ownership.

* * *

# Inscribe content
```
{
	"p":"oc",
	"desc":"Null the Cat",
	"content":"A hyper-dimensional being that has…"
}
```

OC content inscriptions typically have `desc` and `content` fields.

`desc` is not unique or required.

`content` can be text, a URI, or an inscription ID.

Markdown is ideal for text content.

* * *

# Inscribe multiple contents
```
{
	"p":"oc",
	"content":[
		{
			"desc":"Null the Cat",
			"content":"A hyper-dimensional being that…"
		},
		{
			"desc":"Cartesian the Dog",
			"content":"A hyper-dimensional being with…"
		}
	]
}
```

Inscribe multiple contents by including them as `content` entries in an array.

`desc` can be included to describe individual content items.

* * *

# Inscribe content with a credit
```
{
	"p":"oc",
	"desc":"Null the Cat",
	"content":"A hyper-dimensional being that has…",
	"credit":"ae09c10232kjf4J3143k2k82378345smxKz"
}
```

With the `credit` field, an OC inscription can be attributed to an identifier, Bitcoin address, or inscription ID.

An OC inscription with no credits assigned is attributed to its parent inscription. If there is no parent, then it is attributed to its original creator’s Bitcoin address.

* * *

# Inscribe content with multiple credits
```
{
	"p":"oc",
	"desc":"Null the Cat",
	"content":"A hyper-dimensional being that has…",
	"credit":[
		{
			"desc":"Alice",
			"credit":"ae09c10232kjf4J3143k2k823783smxKz",
			"weight":1
		},
		{
			"desc":"Brolly",
			"credit":"ae09c10232kjf4J3143k2k823785smxKz",
			"weight":1
		}
    ]
}
```

The `credit` field can be used to credit multiple parties.

`desc` can be included to describe individual credits.

The `weight` value is an integer representing the relative contribution of each party within the scope of that OC inscription.

If no weight is specified, it is assumed to be 1.

* * * 

# Inscribe secret content
```
{
	"p":"oc",
	"content":[
		{"content":"hash"},
		{"content":"hash"},
		{"content":"hash"}
	],
	"credit":[
		{
			"credit":"ae09c10232kjf4J3143k2k8...",
			"weight":50
		},
		{
			"credit":"ae09c10232kjf4J3143k2k8...",
			"weight":100
		}
	]
}
```

An OC inscription can hold secret data while still existing transparently on-chain by storing only a hash of the content, or storing encrypted data directly.

* * * 

# Inscribe attribution data only
```
{
	"p":"oc",
	"credit":[
		{
			"credit":"ae09c102jf4J3132k43k2k8...",
			"weight":5
		},
		{
			"credit":"59d00a8eab549f9d59abd8e...",
			"weight":2
		},
		{
			"credit":"f1gdXf3dx3k43nlNMd5554e...",
			"weight":1
		}
	]
}
```

`credit` lists can be created independently of content.

These credit lists can later be referenced in the credit fields of other OC inscriptions.

Useful for re-using a previous attribution scheme.

* * * 

# Assigning a license
```
{
	"p":"oc",
	"desc":"Null the Cat",
	"content":"A hyper-dimensional being that has…",
	"license":"OCL"
}
```

An OC inscription can include a `license`.

The associated license dictates the terms for the OC inscription’s content.

A license can turn an OC inscription into an IP NFT where ownership of the inscription confers rights.

The license can be identified by name or by pointing to an inscription of the license on-chain.

If no license is specified, all rights are reserved as normal by their rights holders, and are not associated with OC inscription ownership in any way.

* * * 

# Appending new OC inscription data
Additional OC content or credits can be appended by re-inscribing an OC including just the new entries.

Parent-child inscription can be used to validate appended data by requiring a parent.

Replacing OC inscription data
```
{"p":"oc"}
```

Previous OC inscription data can be “primed” by re-inscribing a blank OC message on an OC inscription.

Primed OC inscriptions can be normalized by re-inscribing a second sequential blank OC message.

Re-inscribing OC content and/or credit data on a primed OC inscription replaces its previous content.

Replaced OC data remains immutably on-chain.

Parent-child inscription can be used to validate updates to OC data by requiring a parent.

* * *

# Locking OC inscriptions
OC inscriptions can be “locked” to prevent future changes.

Parent-child inscription is used for OC locking by simply burning the OC inscription’s parent.

Extending locking/unlocking permissions to grand-parent inscriptions allows for multi-tiered and provisional governance of OC inscription data.

* * * 

# Recursive applications
Previous OC inscriptions can be referenced in both the content and credit fields of new OC’s using their inscription ID.

Credit prior work, or create teams and nested attribution schemes by including existing OC inscriptions with credits in the credit field of new OC inscriptions.
Create copies of an OC by including an existing OC inscription with content in the content field of a new OC inscription while using the original OC inscription as its parent.

Version handling
```
{
	"p":"oc",
	"desc":"Null the Cat",
	"content":"A hyper-dimensional being that has…",
	"v":"1.1"
}
```

`version` refers to the version of the OC protocol.

An inscription with no version indicated is handled as version “1.0”.

* * * 

# JSON schema
```
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "On-Chain Canon (OC)",
  "type": "object",
  "required": ["p"],
  "properties": {
    "p": {
      "type": "string",
      "const": "oc",
      "description": "Protocol identifier, constant for all OC inscriptions."
    },
    "desc": {
      "type": "string",
      "description": "Optional description of the OC inscription."
    },
    "content": {
      "oneOf": [
        {
          "type": "string",
          "description": "Text, URI, or a reference to another inscription."
        },
        {
          "type": "array",
          "items": {
            "type": "object",
            "required": ["content"],
            "properties": {
              "desc": {
                "type": "string",
                "description": "Description of the content item."
              },
              "content": {
                "type": "string",
                "description": "Text, URI, or inscription ID."
              }
            }
          },
          "description": "Array of content items."
        }
      ]
    },
    "credit": {
      "oneOf": [
        {
          "type": "string",
          "description": "A single credit, typically a Bitcoin address or inscription ID."
        },
        {
          "type": "array",
          "items": {
            "type": "object",
            "required": ["credit", "weight"],
            "properties": {
              "desc": {
                "type": "string",
                "description": "Description of the credit item."
              },
              "credit": {
                "type": "string",
                "description": "Bitcoin address or inscription ID."
              },
              "weight": {
                "type": "integer",
                "description": "The relative contribution weight of multiple contributors."
              }
            }
          },
          "description": "List of credits for multiple contributors."
        }
      ],
      "description": "Credit attribution to the creator or contributors."
    },
    "license": {
      "type": "string",
      "description": "Optional license identifier or inscription ID for on-chain license."
    },
    "v": {
      "type": "string",
      "description": "Version of the OC metaprotocol this inscription adheres to."
    }
  },
  "additionalProperties": false
}
```


