# wping- windowed ping

Authored by John Hood, cgull@glup.org

This adds a sliding window feature to an ancient version of ping(8)
from FreeBSD, which allows multiple outstanding ICMP request/responses
at once.  Instead of waiting for an ICMP RESPONSE to the last REQUEST,
it will wait for a RESPONSE from *any* REQUEST within the window.
This has two nice benefits: 1) it allows ping to make many queries
without being limited by the RTT on the link, and 2) if the window
size is smaller than the input queue on the bandwidth-limiting link,
but large enough to produce enough traffic for the round-trip's
bandwidth \* delay product, it will nicely saturate the limited link
without causing queue drops, so is reasonably friendly to other
traffic.

Note that the "window" here is not the same as TCP's sliding window.
If packets are lost or reordered, it is quite possible for more
packets to be in flight than the nominal window size would suggest.
With severe reordering, it's possible for the number of packets in
flight to exceed the window size by many times.

I originally coded this around 2000 to work out what was wrong with a
10Mb/s (!) laser-based Ethernet bridge between two buildings.  As I
suspected, it was an Ethernet full-duplex/half-duplex mismatch on a
local wired link.  So it's handy as a traffic generator to test
physical network layers, too.

The Makefile here is the original FreeBSD Makefile and will only work
there and possibly on other BSDs.  This code builds on Mac OS and
Linux too, use `build.sh` there.
