---
abbrev: OAuth Bene
area: "Security"
author:
  - email: mark.haine@selectid.co.uk
    ins: M. Haine
    name: Mark Haine
    organization: Select ID Ltd
category: info
docname: draft-haine-oauth-beneficiary-request-latest
ipr: trust200902
keyword:
 - beneficiary
 - intermediary
 - request
stand_alone: 'yes'
submissiontype: IETF
workgroup: "Web Authorization Protocol"
title: OAuth 2.0 Beneficiary Request Parameter
number:
date: 2026-01-05
normative:
  RFC6749:
  RFC7591:
  Client-ID-Scheme:
    title: "OAuth 2.0 Client ID Scheme"
    date: 2025-08-11
    target: https://www.ietf.org/archive/id/draft-parecki-oauth-client-id-scheme-01.txt
    author:
      - name: Aaron Parecki
        org: Okta
informative:
  OpenID.Core:
    author:
    - ins: N. Sakimura
      name: Nat Sakimura
    - ins: J. Bradley
      name: John Bradley
    - ins: M. Jones
      name: Michael B. Jones
    - ins: B. de Medeiros
      name: Breno de Medeiros
    - ins: C. Mortimore
      name: Chuck Mortimore
    date: December 2023
    target: https://openid.net/specs/openid-connect-core-1_0.html
    title: OpenID Connect Core 1.0 incorporating errata set 2

--- abstract

An Authorization Server authorizes a Client to access data and may also pass claims about the end user via OpenID Connect.  This data is provided to the Client application.  In many implementations this Client application will not be operated by the legal entity that will ultimately receive and process that data for the benefit of the end-user and/or the eventual data recipient
This specification describes a mechanism for including information about intermediate data oprocessors and beneficial recipients of the data authorized by an OAuth transaction (or presented via any additional mechanism such as OpenID Connect) by adding a new request parameter to OAuth 2.0.

--- middle

# Introduction {#introduction}

In some applications of OAuth 2.0, there may be multiple legal entities that have access to or process data returned by an Authorization Server. In "The OAuth 2.0 Authorization Framework" [RFC6749], a `client_id` represents only a single application, and so the OAuth consent screen lists just one third party - the name of the OAuth client name usually derived from some clinet metadata.

In this situation, either to be open with the end user or to comply with various local laws and regulations, it may be appropriate for the authorization server to inform the end-user of some or all of the of the list of entities that will have access to their data after the client has received authorization.

This specification extends [RFC6749] to define a parameter that describes
the additional (potentially multiple) parties that will have data shared with them as a result of authorizing the OAuth transaction. Optional mechanisms are provided to determine metadata and the validity of those additional parties.

This specification also defines the requirements that the Authorization server should meet to present this information to the end user to facilitate informed consent.

## Target Audience {#target-audience}

 The intended audiences of this document are:

 * Implementers of OAuth 2.0 Authorization servers and clients,

 * Solution architects of business solutions that need to convey the intermediaries and eventual beneficial recipients of data accessible as a result of an OAuth 2.0 authorization.


# Conventions and Definitions

{::boilerplate bcp14-tagged}

# Terminology

__TODO Terminology__

* Beneficiary

* Intermediary

* Client




# Beneficiary ID

Each beneficiary shall be uniquely identified in the ecosystem by a unique identifier `beneficiary_id`. This identifier shall be unique within the ecosystem and should be unique globally.

`beneficiary_id` should follow the client ID scheme as described in [Client-ID-Scheme]. 



# Beneficiaries Request {#beneficiaries}

TODO Beneficiaries Request

## Example 1. OpenID Federation beneficiary ecosystem.
In this example, `beneficiary_id` points to the entity configuration that the Authorization Server can resolve.

Note: Each ecosystem defines its own entity resolution and trust resolution rules, which are out of scope for this specification.

```
{
    "beneficiaries": [
        {
            "beneficiary_id": "openid_federation:https://www.trust_anchor.org/entity/1234",
            "discovery_method": "OIDFed"
        }
   ]
}
```

## Example 2
In this example there are two "beneficiaries" the first of which is an intermediary which in turn has one beneficiary "1234" that it passes data to.  In this case the intermediary entity metadata can be discovered and trused via an OpenID Federation endpoint and the end beneficiary ("1234") uses an "embedded metadata" mechanism.

```
{
    "beneficiaries": [
        {
            "beneficiary_id": "https://intermediary-one.co.uk",
            "discovery_method": "OIDFed",
            "uri": "https://intermediary-one.co.uk/federationendpoint"
            "beneficiaries": [
              {
                "beneficiary_id": "1234",
                "discovery_method": "embedded",
                "name": "TheBestController",
                "description": "Hello World",
                "purpose": "marketing",
                "uri": "https://thebestcontroller.com",
                "logo_uri": "https://thebestcontroller.com/tbclogo.svg",
                "contacts": ["dpo@thebestcontroller.com"],
                "GDPR_type": "processor|controller"
              }
            ]
        }


    ]
}
```

# Beneficiary metadata

The following metadata can be defined for each beneficiary. 

Metadata can be transmitted during the request or fetched outside it, depending on the requirements of each particular ecosystem.

| # | Metadata field name    | Mandatory | Description                                                                                                  |
| - | ---------------------- | --------- | ------------------------------------------------------------------------------------------------------------ |
| 1 | beneficiary_id         | Y         | Unique identifier as defined in [Client-ID-Scheme].                                                       |
| 2 | beneficiary_name       | N         | Name of the beneficiary as defined for client_name in [RFC7591].                                             | 
| 3 | beneficiary_uri        | N         | URL string of a web page providing information about the beneficiary as defined for client_uri in [RFC7591]. | 
| 4 | beneficiary_logo_uri   | N         | URL string that references a logo for the beneficiary as defined for logo_uri in [RFC7591].                  | 
| 5 | beneficiary_tos_uri    | N         | URL string that points to a human-readable terms of service as defined for tos_uri in [RFC7591].             | 
| 6 | beneficiary_policy_uri | N         | URL string that points to a human-readable privacy policy document as defined for policy_uri in [RFC7591].   | 
| 7 | beneficiary_contacts   | N         | Array of strings representing ways to contact people responsible for this beneficiary as defined for contacts in [RFC7591].    |   
| 8 | beneficiary_role       | N         | Array of strings representing different roles beneficiaries could play in an ecosystem (e.g.: "GDRP_CONTROLLER"). This will be defined at an ecosystem level. |   

# Privacy Considerations

TODO Privacy

# Security Considerations

TODO Security


# IANA Considerations {#IANA}

 This section registers one value, as listed in the subsection
   below, in the IANA "OAuth Parameters" registry established by RFC
   6749 [RFC6749].

## "beneficiaries" Parameter registration

  * Name: beneficiaries
  * Parameter Usage Location: client-rs request
  * Change Controller: IETF
  * Specification Document {{beneficiaries}} of this specification

--- back

# Acknowledgments

The author would like to thank 
Aaron Parecki, 
Jan Vereecken, 
and 
Joseph Heenan 
for their initial contributions of the concepts behind this specification. The author would also like to thank XXXXXXXX for their reviews of this specification. Additionally the work of the OAuth Working Group on the referenced and related specifications that this specification builds upon is much appreciated.


# Document History

[[Note to RFC Editor: please remove before publication.]]

## draft-haine-oauth-beneficiary-request-latest-00

* Initial version