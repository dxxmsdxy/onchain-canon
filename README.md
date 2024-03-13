![oc-gray](https://github.com/dxxmsdxy/On-Chain-Canon/assets/154453317/216369ae-6f6a-4c0b-bb8d-737aafd9aeab)

On-Chain Canon (OC)
===================
***A metaprotocol for on-chain IP and attribution on Bitcoin***

On-Chain Canon (OC) is a decentralized metaprotocol for on-chain IP. Use the OC metaprotocol in combination with parent-child inscriptions to represent on-chain IP provenance, attribution, and ownership.

</br>

* * *
# Usage
## Inscribe content
```json
{
	"p":"oc",
	"desc":"Null the Cat",
	"content":"A hyper-dimensional fugitive that has taken the form of a cat."
}
```

OC content inscriptions typically have `desc` and `content` fields.

`desc` is not unique or required.

`content` can be text, a URI, or an inscription ID.

Markdown can be used for rich text content.

</br>

## Inscribe multiple contents

```json
{
	"p":"oc",
	"content":[
		{
			"desc":"Null the Cat",
			"content":"A hyper-dimensional fugitive that has taken the form of a cat."
		},
		{
			"desc":"Cartesian the Dog",
			"content":"A hyper-dimensional detective that has taken the form of a dog."
		}
	]
}
```

Inscribe multiple contents by including them as `content` entries in an array.

`desc` can be included to describe individual content items.

</br>

## Inscribe content with a credit
```json
{
	"p":"oc",
	"desc":"Null the Cat",
	"content":"A hyper-dimensional fugitive that has taken the form of a cat.",
	"credit":"f5ac3eb2ba6dec087a4170735d8865fc0019bff5263d431e0496fe4695651b5ai302"
}
```

With the `credit` field, an OC inscription can be attributed to an identifier, Bitcoin address, or inscription ID.

An OC inscription with no credits assigned is attributed to its parent inscription. If there is no parent, then it is attributed to its original creator’s Bitcoin address.

</br>

## Inscribe content with multiple credits
```json
{
	"p":"oc",
	"desc":"Null the Cat",
	"content":"A hyper-dimensional fugitive that has taken the form of a cat.",
	"credit":[
		{
			"desc":"Albus",
			"credit":"f5ac3eb2ba6dec087a4170735d8865fc0019bff5263d431e0496fe4695651b5ai302",
			"weight":2
		},
		{
			"desc":"Brolly",
			"credit":"bc1q0z5x3rsqjqt59mzs42vrwra83a2upprtm7lkc0",
			"weight":1
		}
    ]
}
```

The `credit` field can be used to credit multiple parties.

`desc` can be included to describe the credited entity.

The `weight` value is an integer representing the relative contribution of each party within the scope of the same credit envelope.

If no weight is specified, it is assumed to be 1.

</br>

## Inscribe secret content
```json
{
	"p":"oc",
	"content":[
		{"content":"hash"},
		{"content":"hash"},
		{"content":"hash"}
	],
	"credit":[
		{
			"credit":"f5ac3eb2ba6dec087a4170735d8865fc0019bff5263d431e0496fe4695651b5ai302",
			"weight":50
		},
		{
			"credit":"bc1q0z5x3rsqjqt59mzs42vrwra83a2upprtm7lkc0",
			"weight":100
		}
	]
}
```

An OC inscription can hold secret data while still existing transparently on-chain by storing only a hash of the content, or storing encrypted data directly.

</br>

## Inscribe attribution data only
```json
{
	"p":"oc",
	"credit":[
		{
			"credit":"f5ac3eb2ba6dec087a4170735d8865fc0019bff5263d431e0496fe4695651b5ai302",
			"weight":5
		},
		{
			"credit":"bc1q0z5x3rsqjqt59mzs42vrwra83a2upprtm7lkc0",
			"weight":2
		},
		{
			"credit":"bc1qrhlw047ju02vlt5sk9e9cctn04x8hes5rlc5zl",
			"weight":1
		}
	]
}
```

`credit` lists can be created independently of content.

These credit lists can later be referenced in the credit fields of other OC inscriptions.

Useful for re-using a previous attribution scheme.

</br>

## Assigning a license
```json
{
	"p":"oc",
	"desc":"Null the Cat",
	"content":"A hyper-dimensional fugitive that has taken the form of a cat.",
	"license":"OCL"
}
```

An OC inscription can include a `license`.

The associated license dictates the terms for the OC inscription’s content.

A license can turn an OC inscription into an IP NFT where ownership of the inscription confers rights.

The license can be identified by name or by pointing to an inscription of the license on-chain.

If no license is specified, all rights are reserved as normal by their rights holders, and are not associated with OC inscription ownership in any way.

</br>

## Appending new OC inscription data
Additional OC content or credits can be appended by re-inscribing an OC including just the new entries.

Provenance requirements such as creator address or parent-child can be used to validate appended data.

</br>

## Replacing OC inscription data
```json
{"p":"oc"}
```

Previous OC inscription data can be “primed” by inscribing a blank OC message on an existing OC inscription.

Primed OC inscriptions are ignored.

Primed OC inscriptions can be normalized by re-inscribing a second sequential blank OC message.

Inscribing OC content and/or credit data on a primed OC inscription 'replaces' its previous content.

Replaced OC data remains immutably on-chain.

Provenance requirements such as a creator address or parent-child can be used to validate changes to OC data.

</br>

## Locking OC inscriptions
OC inscriptions can be “locked” to prevent future changes.

Parent-child inscription can be used to 'lock' OC's by simply burning the relevant parent.

Extending locking/unlocking permissions to grand-parent inscriptions allows for multi-tiered and provisional governance of OC inscription data.

</br>

## Nested contents and credits
```json
{
	"p":"oc",
	"content":[
		{
			"desc":"Null the Cat",
			"content":"A hyper-dimensional fugitive that has taken the form of a cat.",
			"credit":[
				{
					"desc":"Albus",
					"content":"Creator, Illustrator"
					"credit":"bc1q0z5x3rsqjqt59mzs42vrwra83a2upprtm7lkc0"
				}
			],
		},
		{
			"desc":"Cartesian the Dog",
			"content":"A hyper-dimensional detective that has taken the form of a dog."
			"credit":[
				{
					"desc":"Albus",
					"content":"Creator"
					"credit":"bc1q0z5x3rsqjqt59mzs42vrwra83a2upprtm7lkc0",
					"weight":3
				},
				{
					"desc":"Brolly",
					"content":"Illustrator"
					"credit":"bc1qrhlw047ju02vlt5sk9e9cctn04x8hes5rlc5zl"
				}
			]
		}
	]
}
```

If credit is associated with specific content, it can be included with the content.

`content` can be included with a credit to indicate what they are credited for.

</br>

## Recursive applications
Previous OC inscriptions can be referenced in both the content and credit fields of new OC’s using their inscription ID.

* Credit prior work, or create teams and nested attribution schemes by including existing OC inscriptions with credits in the credit field of new OC inscriptions.
* Create copies of an OC by including an existing OC inscription with content in the content field of a new OC inscription while using the original OC inscription as its parent.

</br>

## Version handling
```json
{
	"p":"oc",
	"desc":"Null the Cat",
	"content":"A hyper-dimensional fugitive that has taken the form of a cat.",
	"v":"1.1"
}
```

`version` refers to the version of the OC protocol.

An inscription with no version indicated is handled as version “1.0”.

* * * 

</br>

# JSON schema
```json
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


