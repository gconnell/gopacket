# gopacket Wishlist #

This page contains a list of some features, large and small, that would make gopacket better.  If you've got a lot of free time and want to do one, do so!  If you have an idea you'd like to add, go for it!  These are not a product roadmap... they're simply ideas which folks can take on given enough free time.

# Ideas #

## IPv4 Packet Defragmentation ##

We need some way to defragment IPv4 packets.  I'm thinking an interface something like this:

```
type Defragmenter interface {
  // DefragIPv4 takes in an IPv4 packet with a fragment payload.  It modifies the IPv4 layer  
  // in place, returning true if the modified layer is now a full IPv4 payload.  If the passed-in IP
  // layer is NOT fragmented, it will immediately return true without modifying the layer.  If the IPv4
  // layer is a fragment and we don't have all fragments, it will return false and store whatever
  // internal information it needs to eventually defrag the packet.  If the IPv4 layer is the last fragment
  // needed to reconstruct the packet, *ip will be set to the entire defragmented packet, and the function
  // will return true.
  func DefragIPv4(IPv4* ip) (bool, error)
  // DiscardOlderThan discards all packets we haven't seen a fragment for since 't'.  It returns the
  // number of packets discarded in this way.
  func DiscardOlderThan(t time.Time) int
}
```

## IPv6 Defragmentation ##

See IPv4 defragmentation above.

## SCTP Assembly ##

Is anyone ever going to use SCTP?  If so, we might want to allow assembly of streams.  I'm somewhat hesitant, though, since I really haven't seen any widespread use of the protocol.

## Layer 3 Packet Sending ##

Add some mechanisms for easily sending Layer 3 packets without worrying about filling in layer 2.  An example might be exposing a simple socket(AF\_PACKET, SOCK\_DGRAM) socket, or the like.  Not sure how to do this on Windows.

## Packet Layer Code Generation ##

This is pretty pie-in-the-sky, but would be great... it seems like there's a ton of boilerplate we currently need to write when creating a new packet layer.  It would be great if we could instead specify the format of the packet abstractly (first field is a uint32, second field is a uint16 casted to TCPPort, etc...), then ran that description through something that generated all the relevant Go code.  While we're at it, I want a pony.

## Connection-tracking bidirectional TCP assembly ##

The current TCP assembly places a lot of priority on speed, and some design decisions (unidirectional decoding being a big one) are based off of that.  It would be nice to have an alternate implementation which instead placed an emphasis on really modeling the TCP stack.  In this implementation, for example, data would not be delivered to the caller until it was ACK'd by the receiving party, and streams would be presented bidirectionally with a known client vs. server.

At the moment, it's very difficult to correctly parse an HTTP session and correctly match up requests with responses, especially if we start watching that session mid-stream.  This should make that type of parsing much easier.