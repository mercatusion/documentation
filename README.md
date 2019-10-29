# Mercatus Blockchain Client API
> This document describes the Mercatus blockchain client API.

## Client API

### heightPants

#### Description
> heightPants returns the hash and index of the most recent block.

#### Request URL
> http://{host:port}/pants/height

#### Method
> POST

#### Content-Type
> application/json

#### Request Parameters
| Parameter | Required? | Type   | Description         | Sample           |
| --------- | --------- | ----   | -----------         | ------           |
| address   | True      | String | blockchain location | "127.0.0.1:8000" |
|           |           |        |                     |                  |

#### Sample Request
`{
	"address": "127.0.0.1:8000"
}`

#### Sample Response
`{
	"length": 10,
	"hash": "abcd"
}`

### blockPants

#### Description
> blockPants returns a segment of the targeted blockchain.

#### Request URL
> http://{host:port}/pants/block

#### Method
> POST

#### Content-Type
> application/json

#### Request Parameters
| Parameter | Required? | Type        | Description                                                                                        | Sample           |
| --------- | --------- | ----        | -----------                                                                                        | ------           |
| address   | True      | String      | blockchain location                                                                                | "127.0.0.1:8000" |
| start     | False     | Int         | start index                                                                                        | 0                |
| stop      | False     | Int         | stop index                                                                                         | 10               |
| hash      | False     | Bool/String | if true, returns only index and hash, if string, returns specific block, else returns segment data | true             |
| verify    | False     | Bool        | if true and hash is non-nil, checks if the requested block exists                                  | true             |
|           |           |             |                                                                                                    |                  |

#### Sample Request
`{
	"address": "127.0.0.1:8000",
	"start": 0,
	"stop": 10,
	"hash": true
}`

#### Sample Response
`[
    {
        "previous": "dcba",
        "index": 0,
        "hash": "abcd"
    }
]`

### generatePants

#### Description
> generatePants creates a public-private key pair. It returns the id to the created key as a v4 UUID.

#### Request URL
> http://{host:port}/pants/generate

#### Method
> POST

#### Content-Type
> application/json

#### Request Parameters
| Parameter | Required? | Type   | Description     | Sample |
| --------- | --------- | ----   | -----------     | ------ |
| aes       | True      | String | encryption seed | "test" |
|           |           |        |                 |        |

#### Sample Request
`{
    "aes": "test"
}`

#### Sample Response
`{
    "key": "MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAuW/WdwRJnpyd4IejOnAMeJ0hi9SBVoDr86MWhnk5Y6K+M4+Ykvceqn2YDdrUCz2cifofFG7MFlJLHO5MQacn1jJzujUq2ayqv0Hx/qhSY1sgnMD/27e5WwOFiujiXEDNEW6wpCoblWlsGpHVtcoCRXeY0kSdbdvDNT/4dmvj6OtdmKjzQcOcm+YUPz5q3RoaIxEJ1V7e3LzDRcvfwVXIFj9zpg7SUGHho52ASIHZZ6DAbrKOszXIVJX7Wx5Ngb+eXEOo3YtUmgwXyScBg1r6aOmPBNsnsusHnnhJOpjrT2SDaKHhjRLG/amHuJAW2Hq5jFHX9EDPZYRy+X425oXX2QIDAQAB",
    "id": "43A0B870-6F3D-497C-8A7C-2BAD2C6A166D"
}` 

### getPants

#### Description
> getPants returns the public key associated with a v4 UUID at the specified location.

#### Request URL
> http://{host:port}/pants/get

#### Method
> POST

#### Content-Type
> application/json

#### Request Parameters
| Parameter | Required? | Type   | Description              | Sample                                 |
| --------- | --------- | ----   | -----------              | ------                                 |
| id        | True      | String | UUID associated with key | "A1D0101A-9C68-48d7-A974-DADB8C038ED4" |
|           |           |        |                          |                                        |

#### Sample Request
`
{
    "id": "7232E2DC-0143-44DD-9869-8F66F554E812"
}
`

#### Sample Response
`{
    "key": "MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAuW/WdwRJnpyd4IejOnAMeJ0hi9SBVoDr86MWhnk5Y6K+M4+Ykvceqn2YDdrUCz2cifofFG7MFlJLHO5MQacn1jJzujUq2ayqv0Hx/qhSY1sgnMD/27e5WwOFiujiXEDNEW6wpCoblWlsGpHVtcoCRXeY0kSdbdvDNT/4dmvj6OtdmKjzQcOcm+YUPz5q3RoaIxEJ1V7e3LzDRcvfwVXIFj9zpg7SUGHho52ASIHZZ6DAbrKOszXIVJX7Wx5Ngb+eXEOo3YtUmgwXyScBg1r6aOmPBNsnsusHnnhJOpjrT2SDaKHhjRLG/amHuJAW2Hq5jFHX9EDPZYRy+X425oXX2QIDAQAB"
}`

### messagePants

#### Description
> messagePants sends an encrypted message to designated recipients.

#### Request URL
> http://{host:port}/pants/message

#### Method
> POST

#### Content-Type
> application/json

#### Request Parameters
| Parameter | Required? | Type   | Description                   | Sample                                 |
| --------- | --------- | ----   | -----------                   | ------                                 |
| id      | True      | String | UUID associated with key      | "A1D0101A-9C68-48d7-A974-DADB8C038ED4" |
| aes       | True      | String | encryption seed               | "test"                                 |
| message   | True      | Json   | message body for approval     | [see subsection [Mail](#mail)          |
| address   | True      | String | node for transaction approval | "127.0.0.1:1234"                       |
|           |           |        |                               |                                        |

#### Sample Request
`{
    "id": "7232E2DC-0143-44DD-9869-8F66F554E812",
    "aes": "test",
    "message": {"sender": "MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAuW/WdwRJnpyd4IejOnAMeJ0hi9SBVoDr86MWhnk5Y6K+M4+Ykvceqn2YDdrUCz2cifofFG7MFlJLHO5MQacn1jJzujUq2ayqv0Hx/qhSY1sgnMD/27e5WwOFiujiXEDNEW6wpCoblWlsGpHVtcoCRXeY0kSdbdvDNT/4dmvj6OtdmKjzQcOcm+YUPz5q3RoaIxEJ1V7e3LzDRcvfwVXIFj9zpg7SUGHho52ASIHZZ6DAbrKOszXIVJX7Wx5Ngb+eXEOo3YtUmgwXyScBg1r6aOmPBNsnsusHnnhJOpjrT2SDaKHhjRLG/amHuJAW2Hq5jFHX9EDPZYRy+X425oXX2QIDAQAB", "recipients": ["MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA48H9tCMj4ULz8V6llfiWHzA/6EbryBjNdCoGdGdihOhkTdiCo6HtukW2Z1kAClbtqIQGmbJPf1fMQRgiGLMpxaaXvRxqQw+GMy8VgCQ8hg4RVbeXPcPpABeHchFjvC2WL4W2jq5y3N8RWYq7N62E+eFZ5TdqfyAwhHutqNlKiajc45maLKHGsHZeB88qvLrFl9+1w2G4Q8zyP1b9A+1hj4MPWEGYzYxAYhEZLagCoLch9Y5OKxa9TiGkOA+Qq0lESKK8+R5frHlRT3wLPjDPgHz6QRG4BH2MdEIgC+3b8xNqYiGVNpzjGNV1RDrjeM6UxHWa20t+/CXye8RiPZDdywIDAQAB"], "topic": "Announcements", "message": {"id": "Dururu test.", "content": "Dururu 4eva!"}},
    "address": "127.0.0.1:5001"
}
`

#### Sample Response
`{
  "SIGNATURES": [
    {
      "HASH": "jazwAbH5nRccs1AyGwtebZVKmhok4KAZUFwKZaM4dRQiL5AHwVUcvYH8RIkTI9ZNmz/bLbkRZhx3VuLBwk+pylanI4Ygx3Deir7J+1Y2Wm7fSOzgOmLu1GhUtkyzJ8OFPAPIMQoH44wXK3jQNzJ3aCezkzbWPAl041RXaIJgoWlpTrIFr9EW2XOkohLTvKce0j/s/0zy+lu5uv3uAo1wgat6zdzxokhVn/vPgz3hy1duD9bY0tNQRfaPWTdKTJJU1N5p4wVrL4ZZQmWDfpd0Rh+pbM3TK1GXs5npUhMsSzbq9vMF3Ai1ZmN3VHKUGcjEBG9ObYJllZlBXzjJWkYF9w==",
      "KEY": "MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAuW/WdwRJnpyd4IejOnAMeJ0hi9SBVoDr86MWhnk5Y6K+M4+Ykvceqn2YDdrUCz2cifofFG7MFlJLHO5MQacn1jJzujUq2ayqv0Hx/qhSY1sgnMD/27e5WwOFiujiXEDNEW6wpCoblWlsGpHVtcoCRXeY0kSdbdvDNT/4dmvj6OtdmKjzQcOcm+YUPz5q3RoaIxEJ1V7e3LzDRcvfwVXIFj9zpg7SUGHho52ASIHZZ6DAbrKOszXIVJX7Wx5Ngb+eXEOo3YtUmgwXyScBg1r6aOmPBNsnsusHnnhJOpjrT2SDaKHhjRLG/amHuJAW2Hq5jFHX9EDPZYRy+X425oXX2QIDAQAB"
    }
  ],
  "BODY": {
    "AUTOMATE": true,
    "HASH": "f3ofabefHRAsixI+Q6a9Ig==",
    "CONTENT": [],
    "SENDER": "MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAuW/WdwRJnpyd4IejOnAMeJ0hi9SBVoDr86MWhnk5Y6K+M4+Ykvceqn2YDdrUCz2cifofFG7MFlJLHO5MQacn1jJzujUq2ayqv0Hx/qhSY1sgnMD/27e5WwOFiujiXEDNEW6wpCoblWlsGpHVtcoCRXeY0kSdbdvDNT/4dmvj6OtdmKjzQcOcm+YUPz5q3RoaIxEJ1V7e3LzDRcvfwVXIFj9zpg7SUGHho52ASIHZZ6DAbrKOszXIVJX7Wx5Ngb+eXEOo3YtUmgwXyScBg1r6aOmPBNsnsusHnnhJOpjrT2SDaKHhjRLG/amHuJAW2Hq5jFHX9EDPZYRy+X425oXX2QIDAQAB",
    "RECIPIENT": "Announcements",
    "ASSETS": [
      {
        "ADDRESS": [],
        "TIMESTAMP": "2019-06-25T19:47:27.102259+08:00",
        "REFERENCE": [],
        "ID": "Dururu test.",
        "HASH": [],
        "CONTENT": {
          "BELTS": [
            "XHeSyhDZTyelb21VNutZezs5c81jnhoXezNH1anCv3dWsijKebM2cmHKNc2dX4KNsaO8yNQA53TIlickqJOa7TporbUWirDwVEI2TAOpm9J90hJHzhAhfxPeQ3Tj1WmXiZNE0Wbq1wugBV8gEzEO2r+QtKMO88N9OYqrVeuGa/OnrsU/c8xgq4cxmSEjSgFWANfUCTkmT3Kp7WRDOgxD/y4ekbpwRS8mcIFvaEKyMNoA0jelzIKjUbdz1jVqzltfZU674E3aJ2UOqM7T8E+PJ9/z6VvDn4zyWHfH6q7JK+kwxQsW+W1vM7YmfvrZjQ1JM1gYb+pbREJN55ValueElg==",
            "1XQvRRbL+VIH/H0wM86vpJQpK39v+wUaHepNjUZk0ch4zN6QMDYE9NwfQcIcIFNwbba8tIalJwJAxrczIbiYkU9dJJrzdGoDx264mwby52lpqKj059b5A+KuFQdQTgDwTTNNMRpkg2QsX3Jms7f4LjTipecOvcHyUzv6t948a6VdWyTKPjrBsWi31WIYzMhr9bBulKDsfHPlV6u2DYfXYpQgDqeqRm8QhRTBf5HqW6vValWtjjqwBbSDbPqSerPhu5tFILwLwmoCxhy6szYH4dFlEk1Yqsvske4aUO3B1bpYdiDVoOqq7h54gaTjIp3ZBgGUt/9E2PGilt0CAdmt/g=="
          ],
          "BRIEFS": "RHVydXJ1IDRldmEh"
        },
        "FILTER": []
      }
    ],
    "OUTPUTS": [],
    "RETURNS": []
  }
}`

### trialPants

##### Description
> trialPants packages a given transaction request and passes the request to the given address. The node simply provides a preview of the transaction output. The request is NOT recorded on the blockchain and does not affect any runtime states. The return value is simply the outputs of the intended transaction.

#### Request URL
> http://{host:port}/pants/trial

#### Method
> POST

#### Content-Type
> application/json

#### Request Parameters
| Parameter | Required? | Type   | Description                   | Sample                         |
| --------- | --------- | ----   | -----------                   | ------                         |
| message   | True      | Json   | transaction body for approval | [see subsection [Body](#body)] |
| address   | True      | String | node for transaction approval | "127.0.0.1:1234"               |
|           |           |        |                               |                                |

#### Sample Request
`{
    "address": "127.0.0.1:5001",
    "message": {
        "assets": [
            {
                "id": "foo",
                "content": 1,
                "hash": null,
                "address": null,
                "reference": null
            },
            {
                "id": "bar",
                "content": 2,
                "hash": null,
                "address": null,
                "reference": null
            }
        ],
        "automate": true,
        "content": {
            "reference": null,
            "address": null,
            "hash": null,
            "content": {
                "def": {
                },
                "ret": {
                    "token": 1
                },
                "val": {
                    "value": "(+ \"foo\" \"bar\")"
                }
            },
            "id": "contract"
        },
        "recipient": "MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAuW/WdwRJnpyd4IejOnAMeJ0hi9SBVoDr86MWhnk5Y6K+M4+Ykvceqn2YDdrUCz2cifofFG7MFlJLHO5MQacn1jJzujUq2ayqv0Hx/qhSY1sgnMD/27e5WwOFiujiXEDNEW6wpCoblWlsGpHVtcoCRXeY0kSdbdvDNT/4dmvj6OtdmKjzQcOcm+YUPz5q3RoaIxEJ1V7e3LzDRcvfwVXIFj9zpg7SUGHho52ASIHZZ6DAbrKOszXIVJX7Wx5Ngb+eXEOo3YtUmgwXyScBg1r6aOmPBNsnsusHnnhJOpjrT2SDaKHhjRLG/amHuJAW2Hq5jFHX9EDPZYRy+X425oXX2QIDAQAB",
        "sender": "MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAuW/WdwRJnpyd4IejOnAMeJ0hi9SBVoDr86MWhnk5Y6K+M4+Ykvceqn2YDdrUCz2cifofFG7MFlJLHO5MQacn1jJzujUq2ayqv0Hx/qhSY1sgnMD/27e5WwOFiujiXEDNEW6wpCoblWlsGpHVtcoCRXeY0kSdbdvDNT/4dmvj6OtdmKjzQcOcm+YUPz5q3RoaIxEJ1V7e3LzDRcvfwVXIFj9zpg7SUGHho52ASIHZZ6DAbrKOszXIVJX7Wx5Ngb+eXEOo3YtUmgwXyScBg1r6aOmPBNsnsusHnnhJOpjrT2SDaKHhjRLG/amHuJAW2Hq5jFHX9EDPZYRy+X425oXX2QIDAQAB"
    }
}`

#### Sample Response
`{
    "ret": [
        {
            "FILTER": [],
            "CONTENT": 1,
            "HASH": "eQ7P7iFV78c5xQbWpKCsow==",
            "ID": "token",
            "REFERENCE": "pATcNXlg7hxIWswIU44VCA==",
            "TIMESTAMP": "2019-06-24T14:29:38.959835+08:00",
            "ADDRESS": []
        }
    ],
    "val": [
        {
            "FILTER": [],
            "CONTENT": 3,
            "HASH": "eQ7P7iFV78c5xQbWpKCsow==",
            "ID": "value",
            "REFERENCE": "vedWqxFuab36XlMoE0ig2Q==",
            "TIMESTAMP": "2019-06-24T14:29:38.959797+08:00",
            "ADDRESS": []
        }
    ]
}`

### transactPants

#### Description
> transactPants packages a given transaction request and passes the request to the given address. On failure, it returns an empty message. On success, it returns the approved transaction.

#### Request URL
> http://{host:port}/pants/transact

#### Method
> POST

#### Content-Type
> application/json

#### Request Parameters
| Parameter | Required? | Type   | Description                   | Sample                                 |
| --------- | --------- | ----   | -----------                   | ------                                 |
| id      | True      | String | UUID associated with key      | "A1D0101A-9C68-48d7-A974-DADB8C038ED4" |
| aes       | True      | String | encryption seed               | "test"                                 |
| message   | True      | Json   | transaction body for approval | [see subsection [Body](#body)]         |
| address   | True      | String | node for transaction approval | "127.0.0.1:1234"                       |
|           |           |        |                               |                                        |

#### Sample Request
`{
    "address": "127.0.0.1:5001",
    "message": {
        "assets": [
            {
                "id": "foo",
                "content": 1,
                "hash": null,
                "address": null,
                "reference": null
            },
            {
                "id": "bar",
                "content": 2,
                "hash": null,
                "address": null,
                "reference": null
            }
        ],
        "automate": true,
        "content": {
            "reference": null,
            "address": null,
            "hash": null,
            "content": {
                "def": {
                },
                "ret": {
                    "token": 1
                },
                "val": {
                    "value": "(+ \"foo\" \"bar\")"
                }
            },
            "id": "contract"
        },
        "recipient": "MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAuW/WdwRJnpyd4IejOnAMeJ0hi9SBVoDr86MWhnk5Y6K+M4+Ykvceqn2YDdrUCz2cifofFG7MFlJLHO5MQacn1jJzujUq2ayqv0Hx/qhSY1sgnMD/27e5WwOFiujiXEDNEW6wpCoblWlsGpHVtcoCRXeY0kSdbdvDNT/4dmvj6OtdmKjzQcOcm+YUPz5q3RoaIxEJ1V7e3LzDRcvfwVXIFj9zpg7SUGHho52ASIHZZ6DAbrKOszXIVJX7Wx5Ngb+eXEOo3YtUmgwXyScBg1r6aOmPBNsnsusHnnhJOpjrT2SDaKHhjRLG/amHuJAW2Hq5jFHX9EDPZYRy+X425oXX2QIDAQAB",
        "sender": "MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAuW/WdwRJnpyd4IejOnAMeJ0hi9SBVoDr86MWhnk5Y6K+M4+Ykvceqn2YDdrUCz2cifofFG7MFlJLHO5MQacn1jJzujUq2ayqv0Hx/qhSY1sgnMD/27e5WwOFiujiXEDNEW6wpCoblWlsGpHVtcoCRXeY0kSdbdvDNT/4dmvj6OtdmKjzQcOcm+YUPz5q3RoaIxEJ1V7e3LzDRcvfwVXIFj9zpg7SUGHho52ASIHZZ6DAbrKOszXIVJX7Wx5Ngb+eXEOo3YtUmgwXyScBg1r6aOmPBNsnsusHnnhJOpjrT2SDaKHhjRLG/amHuJAW2Hq5jFHX9EDPZYRy+X425oXX2QIDAQAB"
    },
    "aes": "test",
    "id": "7232E2DC-0143-44DD-9869-8F66F554E812"
}`

#### Sample Response
`{
  "SIGNATURES": [
    {
      "HASH": "WEkUJ+piSNuouCKpiej5xGsO3jlRRt4O7mwGmR4LeVgdIGYHnjGoXXBV0T1rrlSu4aAPvO/isrpqR/JbDRpCRTxgvlYzxv80oLeUBimffRMe0fzhfpM1GySDCBvgnlMTCwt3+ARf82G65CABHSwd30UbTJ/ddOG0otPoC+D36RqofjO14vri8YUB01YtgZIT785AE0w8YWfBDrVEpQI57VquHo9U/GQcZjIhibXI4lsLT2NiAXwUGJSlI70HCNgN+FoIE7nIe2ezm/zo2ziEK4xMR90wM6CvloevOn4r/fWUXs/i5pf7EJXaptZqyb8sA6BhuD5w455+IPhy2O9LUA==",
      "KEY": "MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAuW/WdwRJnpyd4IejOnAMeJ0hi9SBVoDr86MWhnk5Y6K+M4+Ykvceqn2YDdrUCz2cifofFG7MFlJLHO5MQacn1jJzujUq2ayqv0Hx/qhSY1sgnMD/27e5WwOFiujiXEDNEW6wpCoblWlsGpHVtcoCRXeY0kSdbdvDNT/4dmvj6OtdmKjzQcOcm+YUPz5q3RoaIxEJ1V7e3LzDRcvfwVXIFj9zpg7SUGHho52ASIHZZ6DAbrKOszXIVJX7Wx5Ngb+eXEOo3YtUmgwXyScBg1r6aOmPBNsnsusHnnhJOpjrT2SDaKHhjRLG/amHuJAW2Hq5jFHX9EDPZYRy+X425oXX2QIDAQAB"
    }
  ],
  "BODY": {
    "AUTOMATE": true,
    "HASH": "eQ7P7iFV78c5xQbWpKCsow==",
    "CONTENT": {
      "ADDRESS": [],
      "TIMESTAMP": "2019-06-24T14:29:38.880623+08:00",
      "REFERENCE": [],
      "ID": "contract",
      "HASH": [],
      "CONTENT": {
        "VAL": {
          "VALUE": "(+ \"foo\" \"bar\")"
        },
        "RET": {
          "TOKEN": 1
        },
        "DEF": []
      },
      "FILTER": []
    },
    "SENDER": "MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAuW/WdwRJnpyd4IejOnAMeJ0hi9SBVoDr86MWhnk5Y6K+M4+Ykvceqn2YDdrUCz2cifofFG7MFlJLHO5MQacn1jJzujUq2ayqv0Hx/qhSY1sgnMD/27e5WwOFiujiXEDNEW6wpCoblWlsGpHVtcoCRXeY0kSdbdvDNT/4dmvj6OtdmKjzQcOcm+YUPz5q3RoaIxEJ1V7e3LzDRcvfwVXIFj9zpg7SUGHho52ASIHZZ6DAbrKOszXIVJX7Wx5Ngb+eXEOo3YtUmgwXyScBg1r6aOmPBNsnsusHnnhJOpjrT2SDaKHhjRLG/amHuJAW2Hq5jFHX9EDPZYRy+X425oXX2QIDAQAB",
    "RECIPIENT": "MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAuW/WdwRJnpyd4IejOnAMeJ0hi9SBVoDr86MWhnk5Y6K+M4+Ykvceqn2YDdrUCz2cifofFG7MFlJLHO5MQacn1jJzujUq2ayqv0Hx/qhSY1sgnMD/27e5WwOFiujiXEDNEW6wpCoblWlsGpHVtcoCRXeY0kSdbdvDNT/4dmvj6OtdmKjzQcOcm+YUPz5q3RoaIxEJ1V7e3LzDRcvfwVXIFj9zpg7SUGHho52ASIHZZ6DAbrKOszXIVJX7Wx5Ngb+eXEOo3YtUmgwXyScBg1r6aOmPBNsnsusHnnhJOpjrT2SDaKHhjRLG/amHuJAW2Hq5jFHX9EDPZYRy+X425oXX2QIDAQAB",
    "ASSETS": [
      {
        "ADDRESS": [],
        "TIMESTAMP": "2019-06-24T14:29:38.880454+08:00",
        "REFERENCE": [],
        "ID": "foo",
        "HASH": [],
        "CONTENT": 1,
        "FILTER": []
      },
      {
        "ADDRESS": [],
        "TIMESTAMP": "2019-06-24T14:29:38.880525+08:00",
        "REFERENCE": [],
        "ID": "bar",
        "HASH": [],
        "CONTENT": 2,
        "FILTER": []
      }
    ],
    "OUTPUTS": [
      {
        "ADDRESS": [],
        "TIMESTAMP": "2019-06-24T14:29:38.959797+08:00",
        "REFERENCE": "vedWqxFuab36XlMoE0ig2Q==",
        "ID": "value",
        "HASH": "eQ7P7iFV78c5xQbWpKCsow==",
        "CONTENT": 3,
        "FILTER": []
      }
    ],
    "RETURNS": [
      {
        "ADDRESS": [],
        "TIMESTAMP": "2019-06-24T14:29:38.959835+08:00",
        "REFERENCE": "pATcNXlg7hxIWswIU44VCA==",
        "ID": "token",
        "HASH": "eQ7P7iFV78c5xQbWpKCsow==",
        "CONTENT": 1,
        "FILTER": []
      }
    ]
  }
}`

### pendingPants

#### Description
> pendingPants returns all pending manual blocks associated with the user on a given subchain.

#### Request URL
> http://{host:port}/pants/pending

#### Method
> POST

#### Content-Type
> application/json

#### Request Parameters
| Parameter | Required? | Type   | Description                   | Sample                                 |
| --------- | --------- | ----   | -----------                   | ------                                 |
| id      | True      | String | UUID associated with key      | "A1D0101A-9C68-48d7-A974-DADB8C038ED4" |
| address   | True      | String | node for transaction approval | "127.0.0.1:1234"                       |
|           |           |        |                               |                                        |

#### Sample Request
`{
    "id": "7232E2DC-0143-44DD-9869-8F66F554E812",
    "address": "127.0.0.1:5001"
}`

#### Sample Response
`[
  {
    "signatures": [
      {
        "hash": "jI+0/OaYg8UrNCeVUdRyhrPIKxN04xfcBPuSZLwMote5fMhMpTXaG9igcGAGaB+6qRXX8toj4yzjfCqSL1yQg+IPDCHC2sWafFQfBeK4ePWv/e7eV749JF7PwpPV5Bur9OZvmRleU8SKVGg2uSZ5CgTpT5sytnec+cRTgUj3UC5mOyLrIuYeD/mWG4dUQ8szwHANGBfKE4/sJGAv/IveVsBbjjGAM3kgyq8+JBz1qpwoLhCDn04q9D1tJ+mtxyLjzraM/410dIfg6XKavd/glNWHm7UWugiFDde6sf09tXnHiL52DajvO6m6x3YHBZFqOgw6jRFh4uuVb2odLnTi4w==",
        "key": "MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA0TIYjE7o7gNtIwRghlTgJIpbgIbIQaUzqao7jwnvJNZjticU2LGMi/nzU3RtddnATLT4rHP7sGAH6oT0O8CHXt8yvwohlgDW7BdRRCpl84VeTcuq9Q+lk2FYQ70sXNojrVCgcnh1wXMMlkIeIHAnVZMzcrDw1I2zqVASeym9DwpXJ3/tggqh9rxV+W2V6YzHtttu3UQrFG9MC0dMjyA749jlgdcX6vhOc+iVub0PQmf/ybRxTCQNx+WetitZsdpnusv9pZTE9UMIDesze9pAVVuo5VH2F2AdgP2YYqWYySlaholZmebiUjLMW2/p2bdMkloR9uWJl9YRGh15i+5FWQIDAQAB"
      }
    ],
    "body": {
      "transactions": [
        {
          "signatures": [
            {
              "hash": "UfFa2OALf5+Ggp4u560zZAptJB7KwQThnsLSJQfnoT8BNwCEGqFo/Mp256XGUUKobFhPoGuEfz6g0+UsfeCeY1M38Bbq7yuyfOCBNA6VGaoa8fvP4sIdBbRgbjScCLZVvf0cGEqoMsCXGm8LOWd5X6ukR+BiT+qGUm4IPc93GQFoHAvZRSBQTK2hkxT+nIMB07UOGWRgpgzXFn0dbzpmLHOywnmsiPPSccn+LE6QJ1GF/mY0zeMGEBH7K3DqYhFHHrlbRly0cXiylX2SEj+gbrk/gYjxgfz8uFCkMCDgr/ZXmyx+ShK3Ii5zFWMscVvq4SkWJ+Wyzi2N3gesTVz0Ng==",
              "key": "MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAuW/WdwRJnpyd4IejOnAMeJ0hi9SBVoDr86MWhnk5Y6K+M4+Ykvceqn2YDdrUCz2cifofFG7MFlJLHO5MQacn1jJzujUq2ayqv0Hx/qhSY1sgnMD/27e5WwOFiujiXEDNEW6wpCoblWlsGpHVtcoCRXeY0kSdbdvDNT/4dmvj6OtdmKjzQcOcm+YUPz5q3RoaIxEJ1V7e3LzDRcvfwVXIFj9zpg7SUGHho52ASIHZZ6DAbrKOszXIVJX7Wx5Ngb+eXEOo3YtUmgwXyScBg1r6aOmPBNsnsusHnnhJOpjrT2SDaKHhjRLG/amHuJAW2Hq5jFHX9EDPZYRy+X425oXX2QIDAQAB"
            }
          ],
          "body": {
            "automate": [
              "MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAuW/WdwRJnpyd4IejOnAMeJ0hi9SBVoDr86MWhnk5Y6K+M4+Ykvceqn2YDdrUCz2cifofFG7MFlJLHO5MQacn1jJzujUq2ayqv0Hx/qhSY1sgnMD/27e5WwOFiujiXEDNEW6wpCoblWlsGpHVtcoCRXeY0kSdbdvDNT/4dmvj6OtdmKjzQcOcm+YUPz5q3RoaIxEJ1V7e3LzDRcvfwVXIFj9zpg7SUGHho52ASIHZZ6DAbrKOszXIVJX7Wx5Ngb+eXEOo3YtUmgwXyScBg1r6aOmPBNsnsusHnnhJOpjrT2SDaKHhjRLG/amHuJAW2Hq5jFHX9EDPZYRy+X425oXX2QIDAQAB"
            ],
            "hash": "SRo7hSVlf/bT6Rln5Tsb6g==",
            "content": {
              "address": [],
              "timestamp": "2019-06-25T19:31:24.216869+08:00",
              "reference": [],
              "id": "contract",
              "hash": [],
              "content": {
                "val": {
                  "value": "(+ \"foo\" \"bar\")"
                },
                "ret": {
                  "token": 1
                },
                "def": []
              },
              "filter": []
            },
            "sender": "MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAuW/WdwRJnpyd4IejOnAMeJ0hi9SBVoDr86MWhnk5Y6K+M4+Ykvceqn2YDdrUCz2cifofFG7MFlJLHO5MQacn1jJzujUq2ayqv0Hx/qhSY1sgnMD/27e5WwOFiujiXEDNEW6wpCoblWlsGpHVtcoCRXeY0kSdbdvDNT/4dmvj6OtdmKjzQcOcm+YUPz5q3RoaIxEJ1V7e3LzDRcvfwVXIFj9zpg7SUGHho52ASIHZZ6DAbrKOszXIVJX7Wx5Ngb+eXEOo3YtUmgwXyScBg1r6aOmPBNsnsusHnnhJOpjrT2SDaKHhjRLG/amHuJAW2Hq5jFHX9EDPZYRy+X425oXX2QIDAQAB",
            "recipient": "MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAuW/WdwRJnpyd4IejOnAMeJ0hi9SBVoDr86MWhnk5Y6K+M4+Ykvceqn2YDdrUCz2cifofFG7MFlJLHO5MQacn1jJzujUq2ayqv0Hx/qhSY1sgnMD/27e5WwOFiujiXEDNEW6wpCoblWlsGpHVtcoCRXeY0kSdbdvDNT/4dmvj6OtdmKjzQcOcm+YUPz5q3RoaIxEJ1V7e3LzDRcvfwVXIFj9zpg7SUGHho52ASIHZZ6DAbrKOszXIVJX7Wx5Ngb+eXEOo3YtUmgwXyScBg1r6aOmPBNsnsusHnnhJOpjrT2SDaKHhjRLG/amHuJAW2Hq5jFHX9EDPZYRy+X425oXX2QIDAQAB",
            "assets": [
              {
                "address": [],
                "timestamp": "2019-06-25T19:31:24.167467+08:00",
                "reference": [],
                "id": "foo",
                "hash": [],
                "content": 1,
                "filter": []
              },
              {
                "address": [],
                "timestamp": "2019-06-25T19:31:24.177879+08:00",
                "reference": [],
                "id": "bar",
                "hash": [],
                "content": 2,
                "filter": []
              }
            ],
            "outputs": [
              {
                "address": [],
                "timestamp": "2019-06-25T19:31:24.368331+08:00",
                "reference": "jBPl73Pvd2P+ZNpvy+ukLQ==",
                "id": "value",
                "hash": "SRo7hSVlf/bT6Rln5Tsb6g==",
                "content": 3,
                "filter": []
              }
            ],
            "returns": [
              {
                "address": [],
                "timestamp": "2019-06-25T19:31:24.368374+08:00",
                "reference": "Cizs1utHKkHB95SlwOs8pA==",
                "id": "token",
                "hash": "SRo7hSVlf/bT6Rln5Tsb6g==",
                "content": 1,
                "filter": []
              }
            ]
          }
        }
      ],
      "timestamp": "2019-06-25T19:31:25.757717+08:00",
      "vote": true,
      "automate": [
        "MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAuW/WdwRJnpyd4IejOnAMeJ0hi9SBVoDr86MWhnk5Y6K+M4+Ykvceqn2YDdrUCz2cifofFG7MFlJLHO5MQacn1jJzujUq2ayqv0Hx/qhSY1sgnMD/27e5WwOFiujiXEDNEW6wpCoblWlsGpHVtcoCRXeY0kSdbdvDNT/4dmvj6OtdmKjzQcOcm+YUPz5q3RoaIxEJ1V7e3LzDRcvfwVXIFj9zpg7SUGHho52ASIHZZ6DAbrKOszXIVJX7Wx5Ngb+eXEOo3YtUmgwXyScBg1r6aOmPBNsnsusHnnhJOpjrT2SDaKHhjRLG/amHuJAW2Hq5jFHX9EDPZYRy+X425oXX2QIDAQAB"
      ],
      "previousHash": "gJ4JDxfBW37RN5+bzqKRRcKwL6tqtmgIA43Vii+Ql+8=",
      "index": 34,
      "hash": "zSbhjuz4fIrpz0tIQKkEbg==",
      "redirectIndex": []
    }
  }
]`

### votePants

#### Description
> votePants approves a transaction given a block hash returned by pendingPants.

#### Request URL
> http://{host:port}/pants/vote

#### Method
> POST

#### Content-Type
> application/json

#### Request Parameters
| Parameter | Required? | Type   | Description                               | Sample                                         |
| --------- | --------- | ----   | -----------                               | ------                                         |
| id      | True      | String | UUID associated with key                  | "A1D0101A-9C68-48d7-A974-DADB8C038ED4"         |
| aes       | True      | String | encryption seed                           | "test"                                         |
| address   | True      | String | node for transaction approval             | "127.0.0.1:1234"                               |
| vote      | True      | Bool   | whether to approve the transaction or not | true                                           |
| hash      | True      | String | a block hash returned by pendingPants     | "o8Iq0TlpBQsorixmqV4UVVwnU3Kvup2qUzqcYG1W8as=" |
|           |           |        |                                           |                                                |

#### Sample Request
`{
    "id": "7232E2DC-0143-44DD-9869-8F66F554E812",
    "aes": "test",
    "vote": true,
    "address": "127.0.0.1:5001",
    "hash": "zSbhjuz4fIrpz0tIQKkEbg=="
}`

#### Sample Response
`{
  "signatures": [
    {
      "hash": "jI+0/OaYg8UrNCeVUdRyhrPIKxN04xfcBPuSZLwMote5fMhMpTXaG9igcGAGaB+6qRXX8toj4yzjfCqSL1yQg+IPDCHC2sWafFQfBeK4ePWv/e7eV749JF7PwpPV5Bur9OZvmRleU8SKVGg2uSZ5CgTpT5sytnec+cRTgUj3UC5mOyLrIuYeD/mWG4dUQ8szwHANGBfKE4/sJGAv/IveVsBbjjGAM3kgyq8+JBz1qpwoLhCDn04q9D1tJ+mtxyLjzraM/410dIfg6XKavd/glNWHm7UWugiFDde6sf09tXnHiL52DajvO6m6x3YHBZFqOgw6jRFh4uuVb2odLnTi4w==",
      "key": "MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA0TIYjE7o7gNtIwRghlTgJIpbgIbIQaUzqao7jwnvJNZjticU2LGMi/nzU3RtddnATLT4rHP7sGAH6oT0O8CHXt8yvwohlgDW7BdRRCpl84VeTcuq9Q+lk2FYQ70sXNojrVCgcnh1wXMMlkIeIHAnVZMzcrDw1I2zqVASeym9DwpXJ3/tggqh9rxV+W2V6YzHtttu3UQrFG9MC0dMjyA749jlgdcX6vhOc+iVub0PQmf/ybRxTCQNx+WetitZsdpnusv9pZTE9UMIDesze9pAVVuo5VH2F2AdgP2YYqWYySlaholZmebiUjLMW2/p2bdMkloR9uWJl9YRGh15i+5FWQIDAQAB"
    }
  ],
  "body": {
    "transactions": [
      {
        "signatures": [
          {
            "hash": "UfFa2OALf5+Ggp4u560zZAptJB7KwQThnsLSJQfnoT8BNwCEGqFo/Mp256XGUUKobFhPoGuEfz6g0+UsfeCeY1M38Bbq7yuyfOCBNA6VGaoa8fvP4sIdBbRgbjScCLZVvf0cGEqoMsCXGm8LOWd5X6ukR+BiT+qGUm4IPc93GQFoHAvZRSBQTK2hkxT+nIMB07UOGWRgpgzXFn0dbzpmLHOywnmsiPPSccn+LE6QJ1GF/mY0zeMGEBH7K3DqYhFHHrlbRly0cXiylX2SEj+gbrk/gYjxgfz8uFCkMCDgr/ZXmyx+ShK3Ii5zFWMscVvq4SkWJ+Wyzi2N3gesTVz0Ng==",
            "key": "MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAuW/WdwRJnpyd4IejOnAMeJ0hi9SBVoDr86MWhnk5Y6K+M4+Ykvceqn2YDdrUCz2cifofFG7MFlJLHO5MQacn1jJzujUq2ayqv0Hx/qhSY1sgnMD/27e5WwOFiujiXEDNEW6wpCoblWlsGpHVtcoCRXeY0kSdbdvDNT/4dmvj6OtdmKjzQcOcm+YUPz5q3RoaIxEJ1V7e3LzDRcvfwVXIFj9zpg7SUGHho52ASIHZZ6DAbrKOszXIVJX7Wx5Ngb+eXEOo3YtUmgwXyScBg1r6aOmPBNsnsusHnnhJOpjrT2SDaKHhjRLG/amHuJAW2Hq5jFHX9EDPZYRy+X425oXX2QIDAQAB"
          }
        ],
        "body": {
          "automate": [
            "MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAuW/WdwRJnpyd4IejOnAMeJ0hi9SBVoDr86MWhnk5Y6K+M4+Ykvceqn2YDdrUCz2cifofFG7MFlJLHO5MQacn1jJzujUq2ayqv0Hx/qhSY1sgnMD/27e5WwOFiujiXEDNEW6wpCoblWlsGpHVtcoCRXeY0kSdbdvDNT/4dmvj6OtdmKjzQcOcm+YUPz5q3RoaIxEJ1V7e3LzDRcvfwVXIFj9zpg7SUGHho52ASIHZZ6DAbrKOszXIVJX7Wx5Ngb+eXEOo3YtUmgwXyScBg1r6aOmPBNsnsusHnnhJOpjrT2SDaKHhjRLG/amHuJAW2Hq5jFHX9EDPZYRy+X425oXX2QIDAQAB"
          ],
          "hash": "SRo7hSVlf/bT6Rln5Tsb6g==",
          "content": {
            "address": [],
            "timestamp": "2019-06-25T19:31:24.216869+08:00",
            "reference": [],
            "id": "contract",
            "hash": [],
            "content": {
              "val": {
                "value": "(+ \"foo\" \"bar\")"
              },
              "ret": {
                "token": 1
              },
              "def": []
            },
            "filter": []
          },
          "sender": "MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAuW/WdwRJnpyd4IejOnAMeJ0hi9SBVoDr86MWhnk5Y6K+M4+Ykvceqn2YDdrUCz2cifofFG7MFlJLHO5MQacn1jJzujUq2ayqv0Hx/qhSY1sgnMD/27e5WwOFiujiXEDNEW6wpCoblWlsGpHVtcoCRXeY0kSdbdvDNT/4dmvj6OtdmKjzQcOcm+YUPz5q3RoaIxEJ1V7e3LzDRcvfwVXIFj9zpg7SUGHho52ASIHZZ6DAbrKOszXIVJX7Wx5Ngb+eXEOo3YtUmgwXyScBg1r6aOmPBNsnsusHnnhJOpjrT2SDaKHhjRLG/amHuJAW2Hq5jFHX9EDPZYRy+X425oXX2QIDAQAB",
          "recipient": "MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAuW/WdwRJnpyd4IejOnAMeJ0hi9SBVoDr86MWhnk5Y6K+M4+Ykvceqn2YDdrUCz2cifofFG7MFlJLHO5MQacn1jJzujUq2ayqv0Hx/qhSY1sgnMD/27e5WwOFiujiXEDNEW6wpCoblWlsGpHVtcoCRXeY0kSdbdvDNT/4dmvj6OtdmKjzQcOcm+YUPz5q3RoaIxEJ1V7e3LzDRcvfwVXIFj9zpg7SUGHho52ASIHZZ6DAbrKOszXIVJX7Wx5Ngb+eXEOo3YtUmgwXyScBg1r6aOmPBNsnsusHnnhJOpjrT2SDaKHhjRLG/amHuJAW2Hq5jFHX9EDPZYRy+X425oXX2QIDAQAB",
          "assets": [
            {
              "address": [],
              "timestamp": "2019-06-25T19:31:24.167467+08:00",
              "reference": [],
              "id": "foo",
              "hash": [],
              "content": 1,
              "filter": []
            },
            {
              "address": [],
              "timestamp": "2019-06-25T19:31:24.177879+08:00",
              "reference": [],
              "id": "bar",
              "hash": [],
              "content": 2,
              "filter": []
            }
          ],
          "outputs": [
            {
              "address": [],
              "timestamp": "2019-06-25T19:31:24.368331+08:00",
              "reference": "jBPl73Pvd2P+ZNpvy+ukLQ==",
              "id": "value",
              "hash": "SRo7hSVlf/bT6Rln5Tsb6g==",
              "content": 3,
              "filter": []
            }
          ],
          "returns": [
            {
              "address": [],
              "timestamp": "2019-06-25T19:31:24.368374+08:00",
              "reference": "Cizs1utHKkHB95SlwOs8pA==",
              "id": "token",
              "hash": "SRo7hSVlf/bT6Rln5Tsb6g==",
              "content": 1,
              "filter": []
            }
          ]
        }
      }
    ],
    "timestamp": "2019-06-25T19:31:25.757717+08:00",
    "vote": true,
    "automate": [
      "MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAuW/WdwRJnpyd4IejOnAMeJ0hi9SBVoDr86MWhnk5Y6K+M4+Ykvceqn2YDdrUCz2cifofFG7MFlJLHO5MQacn1jJzujUq2ayqv0Hx/qhSY1sgnMD/27e5WwOFiujiXEDNEW6wpCoblWlsGpHVtcoCRXeY0kSdbdvDNT/4dmvj6OtdmKjzQcOcm+YUPz5q3RoaIxEJ1V7e3LzDRcvfwVXIFj9zpg7SUGHho52ASIHZZ6DAbrKOszXIVJX7Wx5Ngb+eXEOo3YtUmgwXyScBg1r6aOmPBNsnsusHnnhJOpjrT2SDaKHhjRLG/amHuJAW2Hq5jFHX9EDPZYRy+X425oXX2QIDAQAB"
    ],
    "previousHash": "gJ4JDxfBW37RN5+bzqKRRcKwL6tqtmgIA43Vii+Ql+8=",
    "index": 34,
    "hash": "zSbhjuz4fIrpz0tIQKkEbg==",
    "redirectIndex": []
  }
}`

### queryPants

#### Description
> queryPants queries a given subchain for all of a user's assets, or alternatively, a filtered list.
> does not return messages.

#### Request URL
> http://{host:port}/pants/query

#### Method
> POST

#### Content-Type
> application/json

#### Request Parameters
| Parameter | Required? | Type          | Description                                                                      | Sample                                 |
| --------- | --------- | ----          | -----------                                                                      | ------                                 |
| id        | False     | String        | if provided, returns assets of UUID associated with key; else returns all assets | "A1D0101A-9C68-48d7-A974-DADB8C038ED4" |
| address   | True      | String        | node for transaction approval                                                    | "127.0.0.1:1234"                       |
| filters   | False     | array(String) | a list of String filters on the id of assets                                     | ["token"]                              |
| restrict  | False     | Bool          | whether results should be restricted                                             | true                                   |
| makers    | False     | array(Json)   | a list of remotes which filters outputs to contain only those generated from it  | [see subsection [Remote](#remote)]     |
| timepoint | False     | Timestamp     | return only outputs which were created after this point in time                  | "2019-01-01T00:00:00.000000+08:00"     |
| private   | False     | Bool          | if true, returns only private contracts                                          | true                                   |
| hash      | False     | String        | hash of specific assets, if provided, returns all assets associated with hash    | "abcd"                                 |
| reference | False     | String        | if provided with hash, returns specific asset                                    | "abcd"                                 |
|           |           |               |                                                                                  |                                        |

#### Sample Request
`{
    "id": "A30A8A5A-83BB-41A3-8D6D-A5E1F7AB5CA7",
    "aes": "test",
    "address": "127.0.0.1:5001",
    "restrict": false,
    "filters": [],
    "makers": [],
    "timepoint": false
}`

#### Sample Response
`[
  {
    "address": [],
    "timestamp": "2019-06-25T19:31:24.368374+08:00",
    "reference": "Cizs1utHKkHB95SlwOs8pA==",
    "id": "token",
    "hash": "SRo7hSVlf/bT6Rln5Tsb6g==",
    "content": 1,
    "filter": []
  }
]`

### historyPants

#### Description
> historyPants recursively returns the given history of any list of assets.

#### Request URL
> http://{host:port}/pants/history

#### Method
> POST

#### Content-Type
> application/json

#### Request Parameters
[see subsection [Asset](#asset)]

#### Sample Request
`[
    {
        "timestamp": "2019-06-25T19:31:24.368374+08:00",
		"description": "",
		"history": []
		"filter": [],
        "content": 1,
        "id": "token",
        "hash": "SRo7hSVlf/bT6Rln5Tsb6g==",
		"reference": "Cizs1utHKkHB95SlwOs8pA==",
        "address": [],
		"private": []
    }
]`

#### Sample Response
`[
    {
        "Cizs1utHKkHB95SlwOs8pA==": [
            {
                "filter": [],
                "content": 1,
                "hash": "SRo7hSVlf/bT6Rln5Tsb6g==",
                "id": "token",
                "reference": "Cizs1utHKkHB95SlwOs8pA==",
                "timestamp": "2019-06-25T19:31:24.368374+08:00",
                "address": []
            }
        ]
    }
]`

### inboxPants

#### Description
> inboxPants returns all messages associated with the receiving key, or a filtered list of these messages.
> the **SENDER** slot in the response body will either be boolean true, in which case it was sent by the queried key, or the key of the sender.
> only returns messages.

#### Request URL
> http://{host:port}/pants/inbox

#### Method
> POST

#### Content-Type
> application/json

#### Request Parameters
| Parameter | Required? | Type          | Description                                                        | Sample                                 |
| --------- | --------- | ----          | -----------                                                        | ------                                 |
| id        | True      | String        | UUID associated with key                                           | "A1D0101A-9C68-48d7-A974-DADB8C038ED4" |
| aes       | True      | String        | encryption seed                                                    | "test"                                 |
| address   | True      | String        | node for transaction approval                                      | "127.0.0.1:1234"                       |
| filters   | False     | array(String) | a list of String filters on the id of assets                       | ["token"]                              |
| restrict  | False     | Bool          | whether results should be restricted                               | true                                   |
| makers    | False     | array(String) | if not empty, only returns messages which has a topic in this list | "Trade War"                            |
| timepoint | False     | Timestamp     | return only outputs which were created after this point in time    | "2019-01-01T00:00:00.000000+08:00"     |
|           |           |               |                                                                    |                                        |

#### Sample Request
`{
    "id": "A30A8A5A-83BB-41A3-8D6D-A5E1F7AB5CA7",
    "aes": "test",
    "address": "127.0.0.1:5001",
    "restrict": false,
    "filters": [],
    "makers": [],
    "timepoint": false
}`

#### Sample Response
`[
  {
    "timestamp": "2019-01-01T00:00:00.000000+08:00",
    "reference": "3qo2QnkdeDok/6PimP6oWQ==",
    "id": "foo",
    "hash": "0EeRyMkiQUe4ulB4TIJtZPN/BnqtbxUbCSnZbtKrnt8=",
    "content": 1,
    "address": "127.0.0.1:1234"
  }
]`

## Subsections

> These sections detail the components of transaction requests.

### Body
| Parameter | Required? | Type               | Description                                                     | Sample                                                                  |
| --------- | --------- | ----               | -----------                                                     | ------                                                                  |
| sender    | True      | String             | a public key for sending assets                                 | "MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAoGWgveLabr1HL..."          |
| recipient | True      | String             | a public key for receiving assets                               | "MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAoGWgveLabr1HL..."          |
| content   | True      | Json               | a contract object for execution                                 | [see subsection [Contract](#contract)]                                  |
| automate  | True      | Bool/array(String) | a toggle for deciding if contract is automated                  | true / ["MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAoGWgveLabr1HL..."] |
| assets    | True      | array(Json)        | a list of assets for transacting                                | [see subsection [Asset](#asset)]                                        |
| terminate | False     | Bool               | if true, the transaction will terminate specified assets and gc | true                                                                    |
|           |           |                    |                                                                 |                                                                         |

### Mail
| Parameter  | Required? | Type          | Description                                 | Sample                                                           |
| ---------  | --------- | ----          | -----------                                 | ------                                                           |
| sender     | True      | String        | a public key for sending messages           | "MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAoGWgveLabr1HL..."   |
| recipients | True      | array(String) | a list of public keys for receiving message | ["MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAoGWgveLabr1HL..."] |
| topic      | True      | String        | topic where the message will be sent        | "Announcements"                                                  |
| message    | True      | Json          | a message object for sending                | [see subsection [Message](#message)]                             |


### Asset
| Parameter   | Required? | Type          | Description                                                                               | Sample                                                |
| ---------   | --------- | ----          | -----------                                                                               | ------                                                |
| id          | True      | null / String | human-readable asset identifier; if null, one will be generated                           | null / "foo"                                          |
| content     | True      | null / any    | value associated with identifier; can be null when requesting off-chain assets            | null / 1 / [see subsection [Contract](#contract)]     |
| hash        | True      | null / String | null for once-only assets; identifies a cross-chain transaction otherwise                 | null / "g+PgJVk43YlKmhCApjBhR9LNet0ZorUKPmCk3RFc+/k=" |
| address     | True      | null / String | http location of the blockchain containing the asset                                      | null / "http://127.0.0.1:80"                          |
| reference   | True      | null / String | null for once-only assets; identifies a cross-chain asset otherwise                       | null / "B0sIfk0BDnBqkkuQ9U+lFQ=="                     |
| timestamp   | True      | Timestamp     | a user-generated timestamp for verification purposes only; **NOT** used in actual request | "2019-01-01T00:00:00.000000+08:00"                    |
| description | False     | String        | human-readable description for asset                                                      | "This is a contract"                                  |
| history     | False     | Json          | a remote of the previous version of this asset                                            | [see subsection [Remote](#remote)]                    |
| private     | False     | Bool          | specifies whether this asset is available to all users                                    | true                                                  |
| filter      | False     | Bool          | if true, asset will not show up in queries                                                | false                                                 |
| tags        | False     | array(String) | a list of searchable tags for the asset                                                   | ["blah"]                                              |
|             |           |               |                                                                                           |                                                       |

### Contract
> A contract follows the same format as [Asset](#asset), except that the **CONTENT** parameter is specified as follows:

| Parameter | Required? | Type          | Description                                                                     | Sample                                     |
| --------- | --------- | ----          | -----------                                                                     | ------                                     |
| def       | True      | array(Json)   | instruction set for smart contract                                              | [see subsection [Definition](#definition)] |
| val       | True      | array(Json)   | output points for contract                                                      | [see subsection [Value](#value)]           |
| ret       | True      | array(Json)   | return points for contract                                                      | [see subsection [Return](#return)]         |
| lang      | False     | String        | contract execution language                                                     | ".py"                                      |
| imp       | False     | array(String) | list of language-specific packages to import                                    | "numpy"                                    |
| lcns      | False     | String        | the specified license for the contract                                          | "mit"                                      |
| prms      | False     | array(Json)   | a list of parameters for the contract                                           | [see subsection [Param](#param)]           |
| srvc      | False     | String        | if specified, opens a service using the specified string on all receiving nodes | "foobar"                                   |
| gc        | False     | Bool          | if true, gcs all data related to the contract execution                         | true                                       |
|           |           |               |                                                                                 |                                            |

### Param
> A param tells the executor what to do in the event that a contract parameter is not satisfied

| Parameter | Required? | Type                   | Description                        | Sample |
|-----------|-----------|------------------------|------------------------------------|--------|
| id        | True      | String                 | the name of the parameter          | "foo"  |
| ini       | True      | String / Json / Number | the initial value of the parameter | "10"   |
| typ       | True      | String                 | the type of the parameter          | "int"  |
|           |           |                        |                                    |        |

### Remote
> A remote is a reduced contract, containing only the time and location of an asset.

| hash      | True | String    | identifies a cross-chain transaction                                                      | "g+PgJVk43YlKmhCApjBhR9LNet0ZorUKPmCk3RFc+/k=" |
| address   | True | String    | http location of the blockchain containing the asset                                      | "http://127.0.0.1:80"                          |
| reference | True | String    | identifies a cross-chain asset                                                            | "B0sIfk0BDnBqkkuQ9U+lFQ=="                     |
|           |      |           |                                                                                           |                                                |

### Message
> A contract follows the same format as [Asset](#asset), except that the **CONTENT** parameter is specified as follows:

| Parameter | Required? | Type   | Description         | Sample                   |
|-----------|-----------|--------|---------------------|--------------------------|
| id        | True      | String | message description | "Click for ads."         |
| content   | True      | String | message body        | "You have won 1 tokens!" |
|           |           |        |                     |                          |


### Definition
> A Key-Value pair.
> A definition is an instruction that may point to an asset enclosed in the transaction or locally within the same definition set.

| *         | Required? | Type   | Description                                                   | Sample                |
| --------- | --------- | ----   | -----------                                                   | ------                |
| Key       | True      | String | a locally-unique identifier for the corresponding instruction | "G96"                 |
| Value     | True      | String | an instruction String                                         | "(+ \"foo\" \"bar\")" |
|           |           |        |                                                               |                       |

### Value
> A Key-Value pair.
> A value is an instruction that serves as the value output point to a transaction. 
> The instruction may point to an asset enclosed in the transaction or locally within the enclosed definition set.
> Value outputs belong to the recipient.

| *         | Required? | Type   | Description                                                          | Sample                |
| --------- | --------- | ----   | -----------                                                          | ------                |
| Key       | True      | String | a locally-unique identifier for the corresponding output instruction | "fooBarSum"           |
| Value     | True      | String | an instruction String                                                | "(/ \"G96\" \"boo\")" |
|           |           |        |                                                                      |                       |

### Return
> A Key-Value pair.
> A return is an instruction that serves as the return output point to a transaction. 
> The instruction may point to an asset enclosed in the transaction or locally within the enclosed definition set.
> Return outputs belong to the sender.

| *         | Required? | Type   | Description                                                          | Sample                |
| --------- | --------- | ----   | -----------                                                          | ------                |
| Key       | True      | String | a locally-unique identifier for the corresponding output instruction | "G69"                 |
| Value     | True      | String | an instruction String                                                | "(- \"foo\" \"boo\")" |
|           |           |        |                                                                      |                       |
