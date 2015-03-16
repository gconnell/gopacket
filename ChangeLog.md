# gopacket Change Log #



## Overview ##

This changelog is meant to provide a history of the high (and low) points of the
gopacket library.  We won't put things into it for every change that's
submitted, but it will hightlight some of the particulary interesting or
largescale changes made along the way.  Also, importantly, every change which
could cause backwards incompatibility will be documented here.

If you're adding a change here, please use the already-established style for
doing so.  Major changes or additions should also be announced at the
gopacket@googlegroups.com mailing list.

## Changes ##

### v1.1.10 20150108 SFlow, random bugfixes, QinQ support ###

Commit:  https://code.google.com/p/gopacket/source/detail?r=9dea2d8fe3605e9b0fc8bca77f708f113b37f76e

Thanks to Brian Green for SFlow support, and to Adrian Tam for QinQ.

### v1.1.9 20141111 Many bug fixes, PCAP additions, PFLog ###

Commit:  https://code.google.com/p/gopacket/source/detail?r=35f76d16d9afcf180032639ea95daf2fe846d536

Mostly a bug-fix release, but added some support for PFLog/LinuxSLL and exposed some PCAP functionality.  Bug fixes include:

  * DNS incorrect decoding
  * DNS infinite recursion
  * lookupnet for pcap files
  * Port endpoint only used 1 byte
  * TCP MoreFragments/Evil flag mixup

### v1.1.8 20140815 DNS optimization, various PCAP extras ###

Commit: https://code.google.com/p/gopacket/source/detail?r=d5007b02aabc80b3a4373ca88f7a17903661e54a

Added support for pcap\_setdirection, pcap\_set\_immediate\_mode, and pcap\_set\_buffer\_size.

Optimized DNS decoding so that repeated DecodeFromBytes calls on the same object converge quickly towards not needing any mallocs.  Thus using DNS with DecodingLayerParser should be much faster than it was before.  I've measured a ~6-7x speedup on DNS packets with a bunch of pointer indirection.

### v1.1.7 20140811 DNS decoding, bugfixes, PCAP offline filters ###

Commit: https://code.google.com/p/gopacket/source/detail?r=40224af0781c3f56273b096666e006e7056d551d

The largest change here is DNS decoding for UDP packets... we've started implementing application-layer datagrams for certain protocols (right now only DNS, but maybe more coming soon).

As well as this, we've added PCAP bindings for offline filters and fixed a small number of bugs.

### v1.1.6 20140729 Yet Another Bugfix Release ###

Commit: https://code.google.com/p/gopacket/source/detail?r=3384f43f003356f559f201b14775a27919c571e1

Another set of bugfixes, including:
  * Fix IPv4/IPv6 fragmentation handling
  * Fix IPv4 flag offsets
  * Added SCTP serialization support
  * Added IPv6Destination decoding/serialization support
  * Handle deep MPLS stacks
  * Fix rfmon for winpcap

### v1.1.5 20140707 Many bug fixes and minor improvements ###

Commit: https://code.google.com/p/gopacket/source/detail?r=70a9543b8f192951452e66a9806bb295b7658c4c

Thanks to many issue reporters, we've fixed a few bugs, including Dot1Q priority decoding/serialization, PacketSource.Packets always returning a consistent channel, and pcap.FindAllDevs returning IPs when it fails to gather netmasks.

We've also made some minor improvements, including the ability to set RFMon on pcap handles.


### v1.1.4 20140625 Radiotap and 802.11 support ###

Commit: https://code.google.com/p/gopacket/source/detail?r=6b5cf45d642f631d1106785f8e2bd436aaa90369

Thanks to the great work of Remco Verhoef, we now have support for 802.11 and Radiotap encapsulations in gopacket/layers.  See dot11.go and radiotap.go in that directory for all the gory details.  Great contribution!

### v1.1.3 20140623 Fix PFRing slice-out-of-bounds error ###

Commit:  https://code.google.com/p/gopacket/source/detail?r=35e689a6f395166300d5c71a1979cc98d09e9981

Fix issue where packets larger than PFRing snaplen would cause slice-out-of-bounds panics.

### v1.1.2 20140620 Exposed bytediff ###

Commit:  https://code.google.com/p/gopacket/source/detail?r=56f03efc9aea94e80d85e64d58a6c13dcd40c3df

I've decided to export the simple, easy to use, probably not very good method we use to display byte diffs in layers/decode\_test so others can use it for simple debugging tasks.  Check out the subpackage gopacket/bytediff.  It's optimized for diffing small slices of bytes like packets, and currently has methods for outputting to bash and HTML.  See examples/bytediff for an example usage:

![https://gopacket.googlecode.com/git/examples/bytediff/bytediff.png](https://gopacket.googlecode.com/git/examples/bytediff/bytediff.png)

### v1.1.1 20140616 Minor Bugfixes ###

Commit:  https://code.google.com/p/gopacket/source/detail?r=fffe2456bd6b22f27da51b6aea94bbaeaf42f63d

Numerous small fixes.

### v1.1.0 20140529 Timeout refactoring ###

Commit:  https://code.google.com/p/gopacket/source/detail?r=01c3d077bd9a98fac6954a18089ad30795d1d230

A nasty bug reported by users turned out to be a rather unexpected behavior related to how Mac OSX handles pcap\_open\_live's to\_ms (timeout) argument.  This change modifies the behavior of BlockForever to work as users expect it should while still maintaining backwards compatibility.  See the doc.go changes in the above commit for more details.

Commit:  https://code.google.com/p/gopacket/source/detail?r=c9be950d1eb93ba5422d713c4dc0064bed2fb0f0

Restructure code to better expose new pcap functionality that requires pcap\_create, some other function calls, then pcap\_activate.  This change is BACKWARDS INCOMPATIBLE:  Users of SetTimestampSource will sadly be broken by it, hence the incrementing of the minor version number.  The vast majority of users, though, should be unaffected.  See the doc.go changes in the above commit for more details on this feature.

### v1.0.12 20140428 Capture pcap\_activate errors ###

Commit:  https://code.google.com/p/gopacket/source/detail?r=e4ba99ebf55394c2e8401eeb5564463e65defe09

Also updated MACs/ports

### v1.0.11 20140425 Add race detection to gc, add PCAP file writing ###

Commit:  https://code.google.com/p/gopacket/source/detail?r=fb194d02b5b45356352c30ae681d41b91d839b6e

Added pcapgo subpackage for (possible, eventual) pure-Go PCAP implementation, started by adding PCAP file writer.
Added race detection (-race) to ./gc Git commit script.

### v1.0.10 20140330 Fix assembly stream stripping ###

Commit:  https://code.google.com/p/gopacket/source/detail?r=5eeda521b2fc1f48ed0e20b1d0ecf813f8018fcb

Fix https://code.google.com/p/gopacket/issues/detail?id=6

### v1.0.9 20140305 Fix SetBPF bug introdued by pcap activation ###

Commit:  https://code.google.com/p/gopacket/source/detail?r=53b8a197e5d943d860d4bec428ede4e0cd829224

A previous CL introduced activating PCAP handles, but wasn't aware that such handles had to be activated BEFORE setting BPF filters.  This fixes that.

### v1.0.8 20140213 PCAP timestamp sources ###

Commit:  https://code.google.com/p/gopacket/source/detail?r=1c4d022c0e9149bbc706a13da0a84660bbb9de8d

Allows users to set the timestamp source for PCAP handles (pcap\_set\_tstamp\_type).

### v1.0.7 20140213 TCP Assembly timestamps ###

Commit: https://code.google.com/p/gopacket/source/detail?r=ee0baca11af6dc90d587d0e6fa89936a03ecf352

Allows users to specify packet timestamps when assembling TCP streams, so they can reassemble PCAP files and other persistently stored data where the current timestamp isn't the correct one.

### v1.0.6 20140122 TCP Assembly debugging/bugfixes ###

Commit: https://code.google.com/p/gopacket/source/detail?r=94474fda2852e447a2a131f66df4750a96024148

Fixed some incorrect handling of long-delayed TCP SYN packets and multiple RSTs.

Also added debug logging flag to make future debugging of TCP assembly easier.

### v1.0.5 20140121 Fix pcap/pfring timestamps. ###

Commit: https://code.google.com/p/gopacket/source/detail?r=ee6c29c6a8c6ff137405d98ed04a7009a285b5d3

We were incorrectly treating timestamps coming from pfring and pcap as nanoseconds, when they were in fact microseconds.  Thanks to Peter Membrey for pointing this out.

### v1.0.4 20131220 SYN scanning example, simple routing table. ###

Commit:
https://code.google.com/p/gopacket/source/detail?r=181aef30c2f5c0488c81cdfbab97fed70e7625ab

Added a SYN scanner.  Due to needing to send data off the local network, I also had to do
some routing, so I created a very simplistic router using the routing table provided by
netlink.  This router is very untested so far, but it should allow simple network
utilities to send packets pretty easily.
### v1.0.3 20131220 Write/Fix/Move example code ###

Commit:
https://code.google.com/p/gopacket/source/detail?r=26e14693245d21d9dfb69381803f373a44dcbab6

Wrote arp scanning example to show basic packet generation and sending.  Fixed tcp assembly
example to not hang forever.  Moved all example binaries into examples subdirectory.

### v1.0.2 20131202 Serialization bugfixes for IPv4/TCP ###

Commit:
https://code.google.com/p/gopacket/source/detail?r=e28112142a483f757bbc2c13e22cbbd7bd29fa13

Fixes FixLengths in gopacket to correctly set tcp.DataOffset and ip4.IHL.

### v1.0.1 20131004 More serialization, MPLS bugfix ###

Commit:
https://code.google.com/p/gopacket/source/detail?r=96888560c968dd5b5b3b1e2212147792e25769bf

Adds serialization for ARP, EAP, ICMPv4, ICMPv6, MPLS, PPP, and PPPoE.
Fixes bug in MPLS where wrong bit was being used for bottom-of-stack.

### v1.0.0 20131004 Semantic Versioning ###

Added semantic versioning (http://semver.org/) to gopacket, for easier go
dependency management.  Starting at v1.0.0 since the current API is
relatively stable and usable, and I don't want anyone scared away by
a v0 version number.

### 20131003 Packet serialization. ###

Commit:  https://code.google.com/p/gopacket/source/detail?r=63984d308e5f90225599c32dffd885c5c3d49aba

Adds the ability to create packet data from layer structs.

### 20130930 Add DecodingLayer implementation to numerous layers ###

Commit:  https://code.google.com/p/gopacket/source/detail?r=c3135e34b82b6439303aa44b0f92d047f877fb70

Add DecodingLayer capabilities to ARP, Dot1Q, EAP, EAPOL, EtherIP, GRE, ICMPv6,
and IGMP.

### 20130930 zero-copy capabilities for **pcap** ###

Commit:  https://code.google.com/p/gopacket/source/detail?r=18af9281fcf4d396d4fe3767ad1413f28c242d9e

Adds the ability to use **pcap** subpackage without copying packet data from C to
Go, saving a full copy of all data.  Benchmarks against live traffic have shown
a 2x increase in the throughput processable by a single core.

### 20130917 IGMP support ###

Commit:  https://code.google.com/p/gopacket/source/detail?r=1c9fcb5b211d2b721ace744a410103c4eef69233

### 20130821 AF\_PACKET support ###

Commit:  https://code.google.com/p/gopacket/source/detail?r=3a810335f15ad55ba4b6cdf76181c5bbbade5df3

### 20130722 BACKWARDS INCOMPATIBLE Moved valid mac prefixes into subpackage ###

Commit:  https://code.google.com/p/gopacket/source/detail?r=27312f15fcf8cd261a3bd576263ec587d4a19a20
Commit:  https://code.google.com/p/gopacket/source/detail?r=43cd11eec5c6f690e73420d534e3605c2076f7d7

In order to speed up compiles, the map of valid MAC addresses was moved into a
subpackage.

Adds support for linux AF\_PACKET into the **afpacket** subpackage.

### 20130711 Fix build breakage on Windows ###

Commit:  https://code.google.com/p/gopacket/source/detail?r=6a82289fba56b8628388b8a57398da587859e3e9

Issue:  https://code.google.com/p/gopacket/issues/detail?id=2

### 20130618 TCP Reassembly ###

Commit:  https://code.google.com/p/gopacket/source/detail?r=119d4fcbfd8aa12f449ac44ec3e54c92b0ede64b

Added the **tcpassembly** subpackage to quickly reassemble TCP streams for
sniffing applications.  Along with reassembling streams, this new subpackage
also has some logic to provide those streams as an io.Reader interface, allowing
callers to make use of existing standard library implementations (net/http) for
stream processing.

### 20130611 Decoding Known Packet Stacks ###

Commit:  https://code.google.com/p/gopacket/source/detail?r=1688462e35ceda1b5914434bd1add89190c0526e

Allowed users to decode a known layer stack from packet data with zero
memory allocations, creating a much faster packet processing framework.
10x speed up for simple packet stacks (eth/ip/tcp/payload).

### 20130522 Lots of work on LLDP, NortelDiscovery, and CDP contributed by bdaglish@restorepoint.com ###

Lots of commits for this one, but everything was fully committed as of https://code.google.com/p/gopacket/source/detail?r=45d548af72767a6401fa3f928704c236461fffcd

### 20130128 Fixes to support winpcap ###

Commit:  https://code.google.com/p/gopacket/source/detail?r=a9bd872c6cba98a3180a37a21839d14de2f19f6d

### 20130123 GoPacket initially released ###

Commit:  https://code.google.com/p/gopacket/source/detail?r=7445adfdcf570bd7227fd26c95734b7b21b619f0

Reddit announcement:  http://www.reddit.com/r/golang/comments/174s3e/go_library_for_decoding_packet_data/