**Important:** Read CONTRIBUTING.md before submitting feedback or contributing
```




Network Working Group                                          W. Kumari
Internet-Draft                                                    Google
Intended status: Informational                          January 30, 2014
Expires: August 3, 2014


                          Stretching DNS TTLs
                 draft-wkumari-dnsop-ttl-stretching-00

Abstract

   The TTL of a DNS Resource Record expresses how long a record may be
   cached before it should be discarded.  This document discusses the
   possibility of "stretching TTLS" (using them past their expiration)
   if they cannot be refreshed.  This works on the assumption that stale
   data may be better than no data.

   PLEASE NOTE: This document is a strawman to drive discussion.  It may
   or may not be a good idea; this document documents the idea so that
   there is something concrete to throw tomatoes at.

   [ Ed note: Text inside square brackets ([]) is additional background
   information, answers to frequently asked questions, general musings,
   etc.  They will be removed before publication.  This document is
   being collaborated on in Github at: https://github.com/wkumari/draft-
   wkumari-dnsop-ttl-stretching.  The most recent version of the
   document, open issues, etc should all be available here.  The authors
   (gratefully) accept pull requests ]

Status of This Memo

   This Internet-Draft is submitted in full conformance with the
   provisions of BCP 78 and BCP 79.

   Internet-Drafts are working documents of the Internet Engineering
   Task Force (IETF).  Note that other groups may also distribute
   working documents as Internet-Drafts.  The list of current Internet-
   Drafts is at http://datatracker.ietf.org/drafts/current/.

   Internet-Drafts are draft documents valid for a maximum of six months
   and may be updated, replaced, or obsoleted by other documents at any
   time.  It is inappropriate to use Internet-Drafts as reference
   material or to cite them other than as "work in progress."

   This Internet-Draft will expire on August 3, 2014.






Kumari                   Expires August 3, 2014                 [Page 1]

Internet-Draft                TTL Stretchng                 January 2014


Copyright Notice

   Copyright (c) 2014 IETF Trust and the persons identified as the
   document authors.  All rights reserved.

   This document is subject to BCP 78 and the IETF Trust's Legal
   Provisions Relating to IETF Documents
   (http://trustee.ietf.org/license-info) in effect on the date of
   publication of this document.  Please review these documents
   carefully, as they describe your rights and restrictions with respect
   to this document.  Code Components extracted from this document must
   include Simplified BSD License text as described in Section 4.e of
   the Trust Legal Provisions and are provided without warranty as
   described in the Simplified BSD License.

Table of Contents

   1.  Introduction  . . . . . . . . . . . . . . . . . . . . . . . .   2
     1.1.  Requirements notation . . . . . . . . . . . . . . . . . .   3
   2.  Proposal  . . . . . . . . . . . . . . . . . . . . . . . . . .   3
   3.  IANA Considerations . . . . . . . . . . . . . . . . . . . . .   3
   4.  Security Considerations . . . . . . . . . . . . . . . . . . .   3
   5.  Acknowledgements  . . . . . . . . . . . . . . . . . . . . . .   3
   6.  References  . . . . . . . . . . . . . . . . . . . . . . . . .   3
     6.1.  Normative References  . . . . . . . . . . . . . . . . . .   3
     6.2.  Informative References  . . . . . . . . . . . . . . . . .   4
   Appendix A.  Changes / Author Notes.  . . . . . . . . . . . . . .   4
   Author's Address  . . . . . . . . . . . . . . . . . . . . . . . .   4

1.  Introduction

   DNS Resource Records (RR) have an associated TTL.  This is how long
   the record may be cached before it should be expired and new
   information fetched.  This is based upon the assumption that the
   authoritative servers will be reachable when they are needed, and
   that records expire and are immediately evicted from the cache.

   There are a number of reasons why an authoritative server may become
   unreachable, including, unfortunately, Denial of Service (DoS)
   attacks.  Recent proposals, for example "Highly Automated Method for
   Maintaining Expiring Records" ([I-D.wkumari-dnsop-hammer]) propose
   refreshing records in the cache before they expire and are evicted.
   This means that the recursive server still has information in its
   cache when it attempts to contact the authoritative server.

   This document suggests that, if the recursive server is unable to
   contact the authoritative server, it simply extends the existing




Kumari                   Expires August 3, 2014                 [Page 2]

Internet-Draft                TTL Stretchng                 January 2014


   records TTL, on the assumption that "stale bread if better than no
   bread".

   [Ed: This is the primary point of the document / question -- if you
   cannot reach the authoritative nameservers (perhaps they being DoS-
   ed, perhaps they were unplugged, you cannot really tell) it is better
   to use the last known (and perhaps outdated) information, or is it
   better for the domain to go dark?  I think the former, but this is a
   significant change to the meaning / semantics of TTLs).

1.1.  Requirements notation

   The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT",
   "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this
   document are to be interpreted as described in [RFC2119].

2.  Proposal

   If a recursive nameserver is unable to contact any of the
   authoritative nameservers for a zone, and it still has the resource
   record cached, it MAY "stretch" the TTL by simply increasing it it by
   the original TTL.  It may do this N times, where N should be
   configurable.

   [ Ed: I was going to say "by doubling the TTL", but then if we allow
   implementations to do this e.g 3 times, is that 4 times the original
   TTL, or is it 2^3 the original TTL].

3.  IANA Considerations

   This document contains no IANA considerations.Template: Fill this in!

4.  Security Considerations

   TODO: Fill this out!

5.  Acknowledgements

   The authors wish to thank some folk.

6.  References

6.1.  Normative References

   [IANA.AS_Numbers]
              IANA, "Autonomous System (AS) Numbers",
              <http://www.iana.org/assignments/as-numbers>.




Kumari                   Expires August 3, 2014                 [Page 3]

Internet-Draft                TTL Stretchng                 January 2014


   [RFC2119]  Bradner, S., "Key words for use in RFCs to Indicate
              Requirement Levels", BCP 14, RFC 2119, DOI 10.17487/
              RFC2119, March 1997,
              <http://www.rfc-editor.org/info/rfc2119>.

6.2.  Informative References

   [I-D.ietf-sidr-iana-objects]
              Manderson, T., Vegoda, L., and S. Kent, "RPKI Objects
              issued by IANA", draft-ietf-sidr-iana-objects-03 (work in
              progress), May 2011.

   [I-D.wkumari-dnsop-hammer]
              Kumari, W., Arends, R., and S. Woolf, "Highly Automated
              Method for Maintaining Expiring Records", draft-wkumari-
              dnsop-hammer-00 (work in progress), July 2013.

Appendix A.  Changes / Author Notes.

   [RFC Editor: Please remove this section before publication ]

   From -00 to -01

   o  Nothing changed in the template!

Author's Address

   Warren Kumari
   Google
   1600 Amphitheatre Parkway
   Mountain View, CA  94043
   US

   Email: warren@kumari.net

















Kumari                   Expires August 3, 2014                 [Page 4]
```
