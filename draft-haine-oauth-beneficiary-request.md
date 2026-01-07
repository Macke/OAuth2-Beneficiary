---
title: "OAuth 2.0 Beneficiary Request"
category: info

docname: draft-haine-oauth-beneficiary-request
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
number:
date: 2026-01-05
area: OAuth
keyword:
 - beneficiary
 - intermediary

author:
 -
    fullname: Mark Haine
    organization: Select ID Ltd
    email: mark.haine@selectid.co.uk

normative:
  RFC6749:

  CLIENT_ID_SCHEME: https://www.ietf.org/archive/id/draft-parecki-oauth-client-id-scheme-01.txt

  RFC7591: OAuth 2.0 Dynamic Client Registration Protocol https://datatracker.ietf.org/doc/html/rfc7591



informative:

...

--- abstract

This specification defines a mechanism for including information about
beneficial recipients of the data returned by an OAuth transaction, and optionally intermediaries, by adding a new request parameter to OAuth 2.0.

--- middle

# Introduction

In some applications of OAuth 2.0, there may be multiple legal entities that have access
to or process data returned by an Authorization Server. In "The OAuth 2.0 Authorization Framework",
a `client_id` represents only a single application, and so the OAuth consent screen
lists just one third party: the OAuth client.

In this situation, to comply with various local laws and regulations, the user needs to be informed by the authorization server of the list of entities
that will have access to their data after the client authorizes it.

This specification extends {{RFC6749}} to define a parameter that identifies
the additional parties that receive data provided by an OAuth transaction. There may be multiple parties, and optional mechanisms exist to determine metadata and the validity of those additional parties.

This specification also defines the requirements that the Authorization server should meet to present this information to the end user to facilitate informed consent.


# Conventions and Definitions

{::boilerplate bcp14-tagged}

# Terminology

TODO Terminology

# Beneficiary ID

Each beneficiary shall be uniquely identified in the ecosystem by a unique identifier (beneficiary_id). This identifier shall be unique within the ecosystem and should be unique globally.

beneficiary_id should follow the client ID scheme as described in [CLIENT_ID_SCHEME]. 

# Beneficiary metadata

The following metadata can be defined for each beneficiary. 

Metadata can be transmitted during the request or fetched outside it, depending on the requirements of each particular ecosystem.

| # | Metadata field name    | Mandatory | Description                                                                                                  |
| 1 | beneficiary_id         | Y         | Unique identifier as defined in [CLIENT_ID_SCHEME].                                                          |
| 2 | beneficiary_name       | N         | Name of the beneficiary as defined for client_name in [RFC7591].                                             | 
| 3 | beneficiary_uri        | N         | URL string of a web page providing information about the beneficiary as defined for client_uri in [RFC7591]. | 
| 4 | beneficiary_logo_uri   | N         | URL string that references a logo for the beneficiary as defined for logo_uri in [RFC7591].                  | 
| 5 | beneficiary_tos_uri    | N         | URL string that points to a human-readable terms of service as defined for tos_uri in [RFC7591].             | 
| 6 | beneficiary_policy_uri | N         | URL string that points to a human-readable privacy policy document as defined for policy_uri in [RFC7591].   | 
| 7 | beneficiary_contacts   | N         | Array of strings representing ways to contact people responsible for this beneficiary as defined for contacts in [RFC7591].    |   
| 8 | beneficiary_role       | N         | Array of strings representing different roles beneficiaries could play in an ecosystem (e.g.: "GDRP_CONTROLLER"). This will be defined at an ecosystem level. |   

# Beneficiary Request

TODO Beneficiary Request

## Example 1. OpenID Federation beneficiary ecosystem.
In this example, beneficiary_id points to the entity configuration that the Authorization Server can resolve.

Note: Each ecosystem defines its own entity resolution and trust resolution rules, which are out of scope for this specification.

```
{
    "beneficiaries": [
        {
            "beneficiary_id": "openid_federation:https://www.trust_anchor.org/entity/1234"
        }
   ]
}
```

## Example 2

```
{
    "dataRecipients": [
        {
            "RecipientID": "1234",
            "discovery_method": "embedded",
            "name": "TheBestController",
            "description": "Hello World",
            "purpose": "marketing",
            "uri": "https://thebestcontroller.com",
            "logo_uri": "https://thebestcontroller.com/tbclogo.svg",
            "contacts": ["dpo@thebestcontroller.com"],
            "GDPR_type": "processor|controller"
        },
        {
            "RecipientID": "https://intermediary-one.co.uk",
            "discovery_method": "OIDFed",
            "uri": "https://intermediary-one.co.uk/federationendpoint"
        }
    ]
}
```



# Security Considerations

TODO Security


# IANA Considerations

TODO IANA Considerations


--- back

# Acknowledgments
{:numbered="false"}


The author would like to thank Aaron Parecki, Jan Vereecken, and Joseph Heenan for their initial contributions of the concepts behind this specification. The author would also like to thank XXXXXXXX for their reviews of this specification. Additionally the work of the OAuth Working Group on the referenced and related specifications that this specification builds upon is much appreciated.
