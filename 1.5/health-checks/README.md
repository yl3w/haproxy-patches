DESCRIPTION
===========
Health check enhancements to haproxy. These patches are not part
of the haproxy distribution. There is a plan to implement larger
haproxy health check support, possibly via something similar
to PROXY protocol. However, this patch achieve the same for all
modes of haproxy operation (tcp/http) mode

Supported version : 1.5.1

Add ability to externalize health check
---------------------------------------
We add new directives to the http-check keyword

The server option is used to specify an external
health checking server to use for health checks.

Example:
    http-check server <ipv4|ipv6>

The info option is used to specify a http
header to communicate information about the server
being checked to the external health checking server

Example:
    http-check info [header &lt;http-header&gt;]

The default value of the header is 'X-Check-Info'

This option is independent of the server directive. When
used includes a http header in the health check request
of the form

&lt;http-header&gt;: &lt;value&gt;

Finally, we add to the server directive a 'info'
option. This value assigned to this option is passed
to the external health checking server.

Example:
    server id <addr> [info &lt;value&gt;]

Putting it all together

backend bck1
    mode http

    # the host header works but wasn't the intent
    option      httpchk   GET /health HTTP/1.1\r\nHost:\ www.hst1.com

    # specify a server to use for the check
    http-check  server chksrv1.dc.hst1.com

    # request for info about the server using the default host header
    # the following 2 lines are equivalent
    # http-check info
    http-check  info X-Check-Info

    # pass in the info using the default
    # the following 2 lines are equivalent
    # server a1 a1.dc.hst1.com:80 weight 20 maxconn 5 check inter 2s
    server a1 a1.dc.hst1.com:80 weight 20 maxconn 5 check inter 2s info a1

    # this is probably a better alternative
    server a1 a1.dc.hst1.com:80 weight 20 maxconn 5 check inter 2s info a1.dc.hst1.com
---
 doc/configuration.txt     |  41 +++++++++++++--
 include/common/defaults.h |   1 +
 include/types/proxy.h     |   8 ++-
 include/types/server.h    |   3 ++
 src/cfgparse.c            | 128 ++++++++++++++++++++++++++++++++++++----------
 src/checks.c              | 107 +++++++++++++++++++++-----------------
 src/server.c              |  15 +++++-
 7 files changed, 222 insertions(+), 81 deletions(-)
