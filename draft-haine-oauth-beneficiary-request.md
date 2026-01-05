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


informative:

...

--- abstract

This specification defines a mechanism for including information about
beneficial recipients of the data returned by an OAuth transaction, and optionally and intermediaries, by adding a new request parameter to OAuth 2.0.

--- middle

# Introduction

In some applications of OAuth 2.0, there may be multiple legal entities which have access
to or process data returned by an Authorization Server. In "The OAuth 2.0 Authorization Framework",
a `client_id` represents only a single application, and so the OAuth consent screen
lists just one third party: the OAuth client.

In this situation, in order to comply with various local laws and regulations,
the user needs to be informed by the authorization server of the list of entities
that will have access to their data after authorizing the client.

This specification extends {{RFC6749}} to define a parameter that identifies
the additional parties that receive data provided by an OAuth transaction. There may be multiple parties and there are optional mechanisms to determine metadata and validity of those additional parties

This specification also defines requirements of the OAuth authorization server
to present this information about the additional parties in the OAuth consent screen
during an OAuth transaction.


# Conventions and Definitions

{::boilerplate bcp14-tagged}

# Terminology

TODO Terminology

# Beneficiary Request

TODO Beneficiary Request



# Security Considerations

TODO Security


# IANA Considerations

TODO IANA Considerations


--- back

# Acknowledgments
{:numbered="false"}


The author would like to thank Aaron Parecki, Jan Vereecken, and Joseph Heenan for their initial contributions of the concepts behind this specification. The author would also like to thank XXXXXXXX for their reviews of this specification. Additionally the work of the OAuth Working Group on the referenced and related specifications that this specification builds upon is much appreciated.
