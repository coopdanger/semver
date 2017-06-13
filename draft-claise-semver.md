---
title: "Semantic Versioning and Structure for IETF Specifications"
abbrev: SemVer
docname: draft-claise-semver
category: std
ipr: trust200902

stand_alone: yes
pi: [toc, sortrefs, symrefs]

author:
 -
    ins: B. Claise
    name: Benoit Claise
    org: Cisco
    email: bclaise@cisco.com
 -
    ins: R. Barnes
    name: Richard Barnes
    org: Cisco
    email: rlb@ipv.sx


--- abstract

In the Internet engineering ecosystem, there is increasingly a need for
specifications that evolve over time, and are encoded directly in structured
formats (e.g., YANG models).  Internet-Drafts are a poor fit for working groups
that want to produce structured specifications, and occasionally publishing an evolving specification as an RFC is a suboptimal way to track the specification over time.
This document outlines recommendations for how working groups can provide
semantic versioning and work directly on structured documents while still
fitting within established IETF processes.


--- middle

# Introduction

The pace at which the software that drives the Internet is developed and
deployed today is much faster than it was early in the Internet's history.  In
this environment, maintaining interoperability can be more challenging.

Two of the major mechanisms that have been developed for driving
interoperability in a more dynamic ecosystem are semantic versioning and
structured specifications.  Structured specifications allow much of the work of
implementing a specification to be automated, so that developers can focus on
the parts of a specification that really need human involvement.  Semantic
versioning helps operators know what versions can be deployed without breaking
running systems, so that they can safely deploy updated versions of a protocol
more quickly.

The traditional practices of the IETF interact poorly with these mechanisms.
Each document presented to the IETF for last call and IESG approval must be
formatted as an Internet-Draft, i.e., as unstructured text.  All RFCs across
the IETF share a single, common numbering space, so that RFC numbers have no
useful semantic.  Nonetheless, there is still a need to be able to capture the
consensus of the IETF at critical points in the life-cycle of a specification.

This document describes recommendations for how a WG that wishes to produce
structured specifications with semantic version numbers can interact best with
IETF processes. 


# Managing Semantic Versions

We start from the premise that a working group controls a version-controlled
repository for a structured specification (not formatted as an Internet-Draft), and can "tag" commits in the repository as
having certain version numbers.

The recommended structure for semantic versions follows the widely-used
three-part convention, with an additional field for use in working group
processes:

~~~~~
  MAJOR.MINOR.PATCH-bBETA
~~~~~

The fields in such a structured version have the following semantics (cf.
semver.org):

* MAJOR is incremented when the new version of the specification
  is incompatible with previous versions. 
* MINOR is incremented when new functionality is added in a manner that is
  backward-compatible with previous versions.
* PATCH is incremented when bug fixes are made in a backward-compatible manner.
* BETA is incremented when a new pre-release or testing version is made. 


In IETF terms, this versioning scheme provides functionality analagous to
several parts of the traditional IETF process.

* MAJOR is analogous to an "obsoletes" relation between RFCs
* MINOR is analagous to an "updates" relation between RFCs
* PATCH is analgous to use of the IETF errata process
* BETA is analgous to incrementing an Internet-draft version


The more major a change to the specification, the more consensus is required.
When the WG wants to make a MAJOR change to a structured specification, the specification MUST be converted into Internet-Draft format and run through the typical IETF consensus process. Every commit that
is tagged with a MAJOR version change MUST also have a tag indicating the
number of the RFC describing the change.

For MINOR changes, WG consensus is required. The WG chairs can additionally decide whether IETF consensus is
required. Any change to the structured specification that significantly changes the security considerations for the protocol or requires
additional IANA actions MUST be converted into Internet-Draft format and submitted for IETF consensus.  Changes without
such impacts MAY be approved by consensus of the working group. PATCH-level
changes MAY be made by the editors, with the consent of the WG chairs.

Typically, the working group will want to implement changes in the
specification and discuss them before committing to a version.  While such
changes can be implemented directly in the repository, it can be useful to mark
certain versions as checkpoints, e.g., for reference at a hackathon.  These
interim versions are marked with BETA numbers.  For example, a preliminary
draft of a new feature that would become version 3.2.0 might be labeled
"3.2.0-b2".  Versions submitted for working group or IETF last call must be
tagged with a version of this form.

Work on a new version SHOULD be conducted on a dedicated branch.  Once there is
consensus to update the main specification to that version, the branch should
be merged, and the merge commit tagged with the new version number.


# IETF Consensus for Structured Specifications

While Working Groups may use any format for protocols under development, the
Internet Standards process requires that a document proposed as an RFC must be
submitted in the RFC format, i.e., as unstructured text.  Proposed RFCs are
also required to contain explanatory text not typically contained in structured
specifications, most notably Security Considerations and IANA Considerations.
Thus, a working group that is focused mainly on a structured specification will
have to convert the structured specification to the RFC format and add the
additional explanatory text.

In order to keep the repository as the primary home for the specification, the
working group should keep any required explanatory text in the repository
alongside the structured specification, and use automated tools to generate an
RFC-formatted document from the artifacts in the repository.  As the outputs
are reviewed in the IETF last call and IESG processes, the editors should
reflect their responses in the repository, generating updated versions of the
RFC-formatted document as necessary.


# Example history

~~~~~
* a9d7d29 (tag:2.0.0, tag:RFCXXXX) Merge branch 'v2'
|\
| * df5d437 Responses to WGLC and IETF LC comments
| | 
| * 986ebb6 (tag:2.0.0-b2) Checkpoint for hackathon
| | 
| * d86986e (tag:2.0.0-b1) Some more v2 features
| | 
| * ca02154 Restructure for v2
|/
*   a5f3214 (tag:v1.2.0) Merge branch 'v1.2'
|\     
| * 8fb9cb6 Responses to WGLC comments on feature Y
| | 
| * 39322e9 (tag:v1.2.0-b1) Add feature Y
|/     
*   d1d201d (tag:v1.1.1) Fix validation errors
|
*   6571483 (tag:v1.1.0) Merge branch 'v1.1'
|\     
| * cabb1f6 Responses to WGLC comments on feature X
| | 
| * fbfaa6b (tag:v1.1.0-b1) Responses to comments on feature X
| | 
| * 0630638 Add feature X
|/     
* e3091df (tag:v1.0.0, tag:RFCXXXX) Responses to IESG comments
| 
* 7494725 Responses to IETF LC comments
| 
* 8e2be54 (tag:v1.0.0-b3) Responses to WGLC comments
| 
* 9703a60 (tag:v1.0.0-b2) Responses to comments at IETF meeting
| 
* 2b83977 Responses to J. Smith comments
| 
* 8b75e1e (tag:v1.0.0-b1) Responses to BoF comments
| 
* 1991498 Initial commit
~~~~~
