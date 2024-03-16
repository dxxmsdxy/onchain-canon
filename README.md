![oc-gray](https://github.com/dxxmsdxy/On-Chain-Canon/assets/154453317/216369ae-6f6a-4c0b-bb8d-737aafd9aeab)

On-chain Canon (OC)
===================
***A standard for on-chain IP and attribution on Bitcoin.***

On-chain Canon (OC) is an experimental standard for decentralized on-chain IP. Use the OC standard in combination with parent-child inscriptions to represent on-chain IP provenance, attribution, and ownership.

An OC inscription is a composable, recursive IP primitive tailored to the Bitcoin and Ordinal protocols.

OC data can be included in the ordinal protocol's content field as JSON or in the metadata field in CBOR format.

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

OC data inscriptions typically have `desc` and `content` fields.

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

The `content` field can be used recursively to include multiple contents.

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

With the `credit` field, an OC inscription's content can be credited to an inscription ID, Bitcoin address, or other identifier.

OC inscription content with no credits assigned is credited to its parent inscription. If there is no parent (or the parent is burned), then it is credited to its origin Bitcoin address.

</br>

## Inscribe content with multiple credits
```json
{
	"p":"oc",
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
```

The `credit` field can be used recursively to credit multiple parties.

`desc` can be included to describe the credited entity.

Text representing the associated work being credited can be included in the `content` field.

The `weight` value is an integer representing the relative contribution of each credited party within the same scope.

If no weight value is specified, it is assumed to be `1`.

</br>

## Inscribe secret content
```json
{
	"p":"oc",
	"content":[
		{"content":"hash"},
		{"content":"hash"},
		{"content":"hash"}
	]
}
```

An OC inscription can hold secret data while still existing transparently on-chain by storing only a hash of the content, or storing encrypted data directly.

This is also useful for referencing content that is too large to store directly on-chain.

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

## Re-inscribing OC data on existing inscriptions
Existing standard inscriptions, such as images or code, can be re-inscribed with OC data that references that inscription's ID in the `content` field.

For example, owning an art inscription re-inscribed with OC data by the rights-holder could confer additional rights to the inscription holder by including a particular license.

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

The license can be identified by name or by pointing to the inscription ID of the license on-chain.

If no license is specified, all rights are reserved as normal by their rights holders, and are not associated with OC inscription ownership in any way.

</br>

## Appending new OC inscription data
Additional OC content or credits can be appended by re-inscribing on an existing OC including just the new entries.

Provenance requirements such as origin address or parent-child can be used to validate appended data.

</br>

## Replacing OC inscription data
```json
{"p":"oc"}
```

Existing OC inscriptions can be “primed” by inscribing a blank OC message on an existing OC inscription.

Primed OC inscriptions are ignored.

Primed OC inscriptions can be normalized by re-inscribing a second sequential blank OC message.

Inscribing OC content and/or credit data on a primed OC inscription 'replaces' its previous content.

Replaced OC data remains immutably on-chain, but is ignored.

Provenance requirements such as origin address or parent-child can be used to validate changes to OC data.

</br>

## Locking OC inscriptions
Parent-child inscription can be used to 'lock' OC's by simply burning the relevant parent, preventing future updates with that provenance.

Considering grand-parent and ancestor inscriptions allows for multi-tiered and provisional governance of OC inscription data.

For example, An IP owner could use their own inscription as a parent to create a child inscription that will be used by an IP manager. That inscription is then used as a parent to create and manage an OC inscription, and then is burnt. The IP manager can no longer make valid updates to the OC, however the IP owner still could.

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
			]
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
	],
	"credit":[
		{
			"desc":"Carmen",
			"content":"Producer"
			"credit":"bc1qrhlw047ju02vlt5sk9e9cctn04x8hes5rlc5zl"
		}
	]
}
```

If a credit is associated with specific content, it can be included with the content.

In the above example, Albus and Brolly are credited for their creative work, while Carmen is credited for producing. The task of producing, the creation of "Null the Cat", and the creation of "Cartesian the Dog" are all weighted equally with the default weight of 1. Albus and Brolly's credits are weighted 3:1 on "Cartesian the Dog".

Therefore, in this example, Albus is credited the most, Carmen second, and Brolly third.

</br>

## Recursive applications
Existing OC inscriptions can be referenced in both the content and credit fields of new OC’s using their inscription ID.

* Reduces the creation of redundant records.
* Credit prior work by including existing OC inscriptions in the `credit` field.
* Reuse previous teams, or create nested attribution schemes by including existing OC inscriptions with multiple credits in the `credit` field.
* Create a copy of OC content by including an existing OC inscription with content in the `content` field.
* Create bundles of IP by including multiple existing OC inscriptions with content in the `content` field.

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

An inscription with no version indicated will be interpreted with the latest version of the OC standard.

* * * 

</br>

# JSON schema
```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "On-chain Canon (OC)",
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
      "description": "Optional license for attached content, referenced by inscription ID or other identifier."
    },
    "v": {
      "type": "string",
      "description": "Optional specified version of the OC standard this inscription adheres to."
    }
  },
  "additionalProperties": false
}
```


