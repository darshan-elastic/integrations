# Network Packet Capture Integration

This integration sniffs network packets on a host and dissects
known protocols.

Monitoring your network traffic is critical to gaining observability and
securing your environment — ensuring high levels of performance and security.
The Network Packet Capture integration captures the network traffic between
your application servers, decodes common application layer protocols and
records the interesting fields for each transaction.

## Supported Protocols

Currently, Network Packet Capture supports the following protocols:

-   ICMP (v4 and v6)
-   DHCP (v4)
-   DNS
-   HTTP
-   AMQP 0.9.1
-   Cassandra
-   Mysql
-   PostgreSQL
-   Redis
-   Thrift-RPC
-   MongoDB
-   Memcache
-   NFS
-   TLS
-   SIP/SDP (beta)

### Common protocol options

The following options are available for all protocols:

#### `enabled`

The enabled setting is a boolean setting to enable or disable protocols
without having to comment out configuration sections. If set to false,
the protocol is disabled.

The default value is true.

#### `ports`

Exception: For ICMP the option `enabled` has to be used instead.

The ports where Network Packet Capture will look to capture traffic for specific
protocols. Network Packet Capture installs a
[BPF](https://en.wikipedia.org/wiki/Berkeley_Packet_Filter) filter based
on the ports specified in this section. If a packet doesn’t match the
filter, very little CPU is required to discard the packet. Network Packet Capture
also uses the ports specified here to determine which parser to use for
each packet.

#### `monitor_processes`

If this option is enabled then network traffic events will be enriched
with information about the process associated with the events.

The default value is false.

#### `send_request`

If this option is enabled, the raw message of the request (`request`
field) is sent to Elasticsearch. The default is false. This option is
useful when you want to index the whole request. Note that for HTTP, the
body is not included by default, only the HTTP headers.

#### `send_response`

If this option is enabled, the raw message of the response (`response`
field) is sent to Elasticsearch. The default is false. This option is
useful when you want to index the whole response. Note that for HTTP,
the body is not included by default, only the HTTP headers.

#### `transaction_timeout`

The per protocol transaction timeout. Expired transactions will no
longer be correlated to incoming responses, but sent to Elasticsearch
immediately.

#### `tags`

A list of tags that will be sent with the transaction event. This
setting is optional.

#### `processors`

A list of processors to apply to the data generated by the protocol.

#### `keep_null`

If this option is set to true, fields with `null` values will be
published in the output document. By default, `keep_null` is set to
`false`.


## Network Flows

Overall flow information about the network connections on a
host.

You can configure Network Packet Capture to collect and report statistics
on network flows. A *flow* is a group of packets sent over the same time
period that share common properties, such as the same source and destination
address and protocol. You can use this feature to analyze network
traffic over specific protocols on your network.

For each flow, Network Packet Capture reports the number of packets and the
total number of bytes sent from the source to the destination. Each flow event
also contains information about the source and destination hosts, such
as their IP address. For bi-directional flows, Network Packet Capture reports
statistics for the reverse flow.

Network Packet Capture collects and reports statistics up to and including the
transport layer.

**Configuration options**

You can specify the following options for capturing flows.

#### `enabled`

Enables flows support if set to true. Set to false to disable network
flows support without having to delete or comment out the flows section.
The default value is true.

#### `timeout`

Timeout configures the lifetime of a flow. If no packets have been
received for a flow within the timeout time window, the flow is killed
and reported. The default value is 30s.

#### `period`

Configure the reporting interval. All flows are reported at the very
same point in time. Periodical reporting can be disabled by setting the
value to -1. If disabled, flows are still reported once being timed out.
The default value is 10s.

{{fields "flow"}}

## Protocols

### AMQP

**Configuration options**

Also see [Common protocol options](#common-protocol-options).

#### `max_body_length`

The maximum size in bytes of the message displayed in the request or
response fields. Messages that are bigger than the specified size are
truncated. Use this option to avoid publishing huge messages when
[`send_request`](#send-request-option) or
[`send_response`](#send-response-option) is enabled. The default is
1000 bytes.

#### `parse_headers`

If set to true, Network Packet Capture parses the additional arguments specified in
the headers field of a message. Those arguments are key-value pairs that
specify information such as the content type of the message or the
message priority. The default is true.

#### `parse_arguments`

If set to true, Network Packet Capture parses the additional arguments specified in
AMQP methods. Those arguments are key-value pairs specified by the user
and can be of any length. The default is true.

#### `hide_connection_information`

If set to false, the connection layer methods of the protocol are also
displayed, such as the opening and closing of connections and channels
by clients, or the quality of service negotiation. The default is true.

Fields published for AMQP packets.

{{fields "amqp"}}

{{event "amqp"}}

### Cassandra

**Configuration options**

Also see [Common protocol options](#common-protocol-options).

#### `send_request_header`

If this option is enabled, the raw message of the response
(`cassandra_request.request_headers` field) is sent to Elasticsearch.
The default is true. Enable `send_request` first before enabling this
option.

#### `send_response_header`

If this option is enabled, the raw message of the response
(`cassandra_response.response_headers` field) is included in published
events. The default is true. enable `send_response` first before enable
this option.

#### `ignored_ops`

This option indicates which Operator/Operators captured will be ignored.
currently support: `ERROR` ,`STARTUP` ,`READY` ,`AUTHENTICATE`
,`OPTIONS` ,`SUPPORTED` , `QUERY` ,`RESULT` ,`PREPARE` ,`EXECUTE`
,`REGISTER` ,`EVENT` , `BATCH` ,`AUTH_CHALLENGE`,`AUTH_RESPONSE`
,`AUTH_SUCCESS` .

#### `compressor`

Configures the default compression algorithm being used to uncompress
compressed frames by name. Currently only `snappy` is can be configured.
By default no compressor is configured.

Fields published for Apache Cassandra packets.

{{fields "cassandra"}}

{{event "cassandra"}}

### DHCP

**Configuration options**

See [Common protocol options](#common-protocol-options).

Fields published for DHCPv4 packets.

{{fields "dhcpv4"}}

{{event "dhcpv4"}}

### DNS

The DNS protocol supports processing DNS messages on TCP and UDP.

**Configuration options**

Also see [Common protocol options](#common-protocol-options).

#### `include_authorities`

If this option is enabled, dns.authority fields (authority resource
records) are added to DNS events. The default is false.

#### `include_additionals`

If this option is enabled, dns.additionals fields (additional resource
records) are added to DNS events. The default is false.

Fields published for DNS packets.

{{fields "dns"}}

{{event "dns"}}

### HTTP

**Configuration options**

Also see [Common protocol options](#common-protocol-options).

#### `hide_keywords`

A list of query parameters that Network Packet Capture will automatically censor in
the transactions that it saves. The values associated with these
parameters are replaced by `'xxxxx'`. By default, no changes are made to
the HTTP messages.

Network Packet Capture has this option because, unlike SQL traffic, which typically
only contains the hashes of the passwords, HTTP traffic may contain
sensitive data. To reduce security risks, you can configure this option
to avoid sending the contents of certain HTTP POST parameters.

This option replaces query parameters from GET requests and top-level
parameters from POST requests. If sensitive data is encoded inside a
parameter that you don’t specify here, Network Packet Capture cannot censor it.
Also, note that if you configure Network Packet Capture to save the raw request and
response fields (see the [`send_request`](#send-request-option) and
the [`send_response`](#send-response-option) options), sensitive data
may be present in those fields.

#### `redact_authorization`

When this option is enabled, Network Packet Capture obscures the value of
`Authorization` and `Proxy-Authorization` HTTP headers, and censors
those strings in the response.

You should set this option to true for transactions that use Basic
Authentication because they may contain the base64 unencrypted username
and password.

#### `send_headers`

A list of header names to capture and send to Elasticsearch. These
headers are placed under the `headers` dictionary in the resulting JSON.

#### `send_all_headers`

Instead of sending a white list of headers to Elasticsearch, you can
send all headers by setting this option to true. The default is false.

#### `redact_headers`

A list of headers to redact if present in the HTTP request. This will
keep the header field present, but will redact it’s value to show the
header’s presence.

#### `include_body_for`

The list of content types for which Network Packet Capture exports the full HTTP
payload. The HTTP body is available under `http.request.body.content`
and `http.response.body.content` for these Content-Types.

In addition, if [`send_response`](#send-response-option) option is
enabled, then the HTTP body is exported together with the HTTP headers
under `response` and if [`send_request`](#send-request-option)
enabled, then `request` contains the entire HTTP message including the
body.

In the following example, the HTML attachments of the HTTP responses are
exported under the `response` field and under
`http.request.body.content` or `http.response.body.content`:

```yaml
Network Packet Capture.protocols:
- type: http
  ports: [80, 8080]
  send_response: true
  include_body_for: ["text/html"]
```

#### `decode_body`

A boolean flag that controls decoding of HTTP payload. It interprets the
`Content-Encoding` and `Transfer-Encoding` headers and uncompresses the
entity body. Supported encodings are `gzip` and `deflate`. This option
is only applicable in the cases where the HTTP payload is exported, that
is, when one of the `include_*_body_for` options is specified or a POST
request contains url-encoded parameters.

#### `split_cookie`

If the `Cookie` or `Set-Cookie` headers are sent, this option controls
whether they are split into individual values. For example, with this
option set, an HTTP response might result in the following JSON:

```json
"response": {
  "code": 200,
  "headers": {
    "connection": "close",
    "content-language": "en",
    "content-type": "text/html; charset=utf-8",
    "date": "Fri, 21 Nov 2014 17:07:34 GMT",
    "server": "gunicorn/19.1.1",
    "set-cookie": {
      "csrftoken": "S9ZuJF8mvIMT5CL4T1Xqn32wkA6ZSeyf",
      "expires": "Fri, 20-Nov-2015 17:07:34 GMT",
      "max-age": "31449600",
      "path": "/"
    },
    "vary": "Cookie, Accept-Language"
  },
  "status_phrase": "OK"
}
```

-   Note that `set-cookie` is a map containing the cookie names as keys.

The default is false.

#### `real_ip_header`

The header field to extract the real IP from. This setting is useful
when you want to capture traffic behind a reverse proxy, but you want to
get the geo-location information. If this header is present and contains
a valid IP addresses, the information is used for the
`network.forwarded_ip` field.

#### `max_message_size`

If an individual HTTP message is larger than this setting (in bytes), it
will be trimmed to this size. Unless this value is very small
(less than 1.5K), Network Packet Capture is able to still correctly follow the transaction
and create an event for it. The default is 10485760 (10 MB).

Fields published for HTTP packets.

{{fields "http"}}

{{event "http"}}

### ICMP

**Configuration options**

Also see [Common protocol options](#common-protocol-options).

**`enabled`**

The ICMP protocol can be enabled/disabled via this option. The default
is true.

If enabled Network Packet Capture will generate the following BPF filter:
`"icmp or icmp6"`.
Fields published for ICMP packets.

{{fields "icmp"}}

{{event "icmp"}}

### Memcached

**Configuration options**

Also see [Common protocol options](#common-protocol-options).

#### `parseunknown`

When this option is enabled, it forces the memcache text protocol parser
to accept unknown commands.

The unknown commands MUST NOT contain a data part.

#### `maxvalues`

The maximum number of values to store in the message (multi-get). All
values will be base64 encoded.

The possible settings for this option are:

-   `maxvalue: -1`, which stores all values (text based protocol multi-get)
-   `maxvalue: 0`, which stores no values (default)
-   `maxvalue: N`, which stores up to N values

#### `maxbytespervalue`

The maximum number of bytes to be copied for each value element.

Values will be base64 encoded, so the actual size in the JSON document
will be 4 times the value that you specify for `maxbytespervalue`.

#### `udptransactiontimeout`

The transaction timeout in milliseconds. The defaults is 10000
milliseconds.

Quiet messages in UDP binary protocol get responses only if there is an
error. The memcache protocol analyzer will wait for the number of
milliseconds specified by `udptransactiontimeout` before publishing
quiet messages. Non-quiet messages or quiet requests with an error
response are published immediately.

Fields published for Memcached packets.

{{fields "memcached"}}

{{event "memcached"}}

### MongoDB

**Configuration options**

The `max_docs` and `max_doc_length` settings are useful for limiting the
amount of data Network Packet Capture indexes in the `response` fields.

Also see [Common protocol options](#common-protocol-options).

#### `max_docs`

The maximum number of documents from the response to index in the
`response` field. The default is 10. You can set this to 0 to index an
unlimited number of documents.

Network Packet Capture adds a `[...]` line at the end to signify that there were
additional documents that weren’t saved because of this setting.

#### `max_doc_length`

The maximum number of characters in a single document indexed in the
`response` field. The default is 5000. You can set this to 0 to index an
unlimited number of characters per document.

If the document is trimmed because of this setting, Network Packet Capture adds the
string `...` at the end of the document.

Note that limiting documents in this way means that they are no longer
correctly formatted JSON objects.

Fields published for MongoDB packets.

{{fields "mongodb"}}

{{event "mongodb"}}

### MySQL

**Configuration options**

Also see [Common protocol options](#common-protocol-options).

#### `max_rows`

The maximum number of rows from the SQL message to publish to
Elasticsearch. The default is 10 rows.

#### `max_row_length`

The maximum length in bytes of a row from the SQL message to publish to
Elasticsearch. The default is 1024 bytes.

### `statement_timeout`

The duration for which prepared statements are cached after their last
use. Valid time units are "ns", "us" (or "µs"), "ms", "s", "m", "h". The
default is `1h`.

Fields published for MySQL packets.

{{fields "mysql"}}

{{event "mysql"}}

### NFS

**Configuration options**

See [Common protocol options](#common-protocol-options).

Fields published for NFS packets.

{{fields "nfs"}}

{{event "nfs"}}

### PostgreSQL

**Configuration options**

Also see [Common protocol options](#common-protocol-options).

#### `max_rows`

The maximum number of rows from the SQL message to publish to
Elasticsearch. The default is 10 rows.

#### `max_row_length`

The maximum length in bytes of a row from the SQL message to publish to
Elasticsearch. The default is 1024 bytes.

Fields published for PostgreSQL packets.

{{fields "pgsql"}}

{{event "pgsql"}}

### Redis

**Configuration options**

Also see [Common protocol options](#common-protocol-options).

#### `queue_max_bytes` and `queue_max_messages`

store requests in memory until a response is received. These settings
impose a limit on the number of bytes (`queue_max_bytes`) and number of
requests (`queue_max_messages`) that can be stored. These limits are
per-connection. The default is to queue up to 1MB or 20.000 requests per
connection, which allows to use request pipelining while at the same
time limiting the amount of memory consumed by replication sessions.

Fields published for Redis packets.

{{fields "redis"}}

{{event "redis"}}

### SIP

**Configuration options**

Also see [Common protocol options](#common-protocol-options).

#### `parse_authorization`

If set to true Network Packet Capture will parse the authorization headers
and include them in events. The default is true.

#### `parse_body`

If set to true, Network Packet Capture parses the SIP body when the body
contains Session Description Protocol data. The default is true.

Fields published for SIP packets.

{{fields "sip"}}

{{event "sip"}}

### Thrift

[Apache Thrift](https://thrift.apache.org/) is a communication protocol
and RPC framework initially created at Facebook. It is sometimes used in
[microservices](http://martinfowler.com/articles/microservices.html)
architectures because it provides better performance when compared to
the more obvious HTTP/RESTful API choice, while still supporting a wide
range of programming languages and frameworks.

Network Packet Capture works based on a copy of the traffic, which means that you
get performance management features without having to modify your
services in any way and without any latency overhead. Network Packet Capture
captures the transactions from the network and indexes them in
Elasticsearch so that they can be analyzed and searched.

Network Packet Capture indexes the method, parameters, return value, and exceptions
of each Thrift-RPC call. You can search by and create statistics based
on any of these fields. Network Packet Capture automatically fills in the `status`
column with either `OK` or `Error`, so it’s easy to find the problematic
RPC calls. A transaction is put into the `Error` state if it returned an
exception.

Network Packet Capture also indexes the `event.duration` field so you can get
performance analytics and find the slow RPC calls.

Thrift supports multiple [transport and protocol
types](http://en.wikipedia.org/wiki/Apache_Thrift). Currently Network Packet Capture
supports the default `TSocket` transport as well as the `TFramed`
transport. From the protocol point of view, Network Packet Capture currently
supports only the default `TBinary` protocol.

Network Packet Capture also has several configuration options that allow you to get
the right balance between visibility, disk usage, and data protection.
You can, for example, choose to obfuscate all strings or to store the
requests but not the responses, while still capturing the response time
for each of the RPC calls. You can also choose to limit the size of
strings and lists to a given number of elements, so you can fine tune
how much data you want to have stored in Elasticsearch.

The Thrift protocol has several specific configuration options.

Providing the Thrift IDL files to Network Packet Capture is optional. The binary
Thrift messages include the called method name and enough structural
information to decode the messages without needing the IDL files.
However, if you provide the IDL files, Network Packet Capture can also resolve the
service name, arguments, and exception names.

**Configuration options**

Also see [Common protocol options](#common-protocol-options).

#### `transport_type`

The Thrift transport type. Currently this option accepts the values
`socket` for TSocket, which is the default Thrift transport, and
`framed` for the TFramed Thrift transport. The default is `socket`.

#### `protocol_type`

The Thrift protocol type. Currently the only accepted value is `binary`
for the TBinary protocol, which is the default Thrift protocol.

#### `idl_files`

The Thrift interface description language (IDL) files for the service
that Network Packet Capture is monitoring. Providing the IDL files is optional,
because the Thrift messages contain enough information to decode them
without having the IDL files. However, providing the IDL enables
Network Packet Capture to include parameter and exception names.

#### `string_max_size`

The maximum length for strings in parameters or return values. If a
string is longer than this value, the string is automatically truncated
to this length. Network Packet Capture adds dots at the end of the string to mark
that it was truncated. The default is 200.

#### `collection_max_size`

The maximum number of elements in a Thrift list, set, map, or structure.
If a collection has more elements than this value, Network Packet Capture captures
only the specified number of elements. Network Packet Capture adds a fictive last
element `...` to the end of the collection to mark that it was
truncated. The default is 15.

#### `capture_reply`

If this option is set to false, Network Packet Capture decodes the method name from
the reply and simply skips the rest of the response message. This
setting can be useful for performance, disk usage, or data retention
reasons. The default is true.

#### `obfuscate_strings`

If this option is set to true, Network Packet Capture replaces all strings found in
method parameters, return codes, or exception structures with the `"*"`
string.

#### `drop_after_n_struct_fields`

The maximum number of fields that a structure can have before Network Packet Capture
ignores the whole transaction. This is a memory protection mechanism (so
that Network Packet Capture’s memory doesn’t grow indefinitely), so you would
typically set this to a relatively high value. The default is 500.

Fields published for Thrift packets.

{{fields "thrift"}}

{{event "thrift"}}

### TLS

TLS is a cryptographic protocol that provides secure communications on
top of an existing application protocol, like HTTP or MySQL.

Network Packet Capture intercepts the initial handshake in a TLS connection and
extracts useful information that helps operators diagnose problems and
strengthen the security of their network and systems. It does not
decrypt any information from the encapsulated protocol, nor does it
reveal any sensitive information such as cryptographic keys. TLS
versions 1.0 to 1.3 are supported.

It works by intercepting the client and server "hello" messages, which
contain the negotiated parameters for the connection such as
cryptographic ciphers and protocol versions. It can also intercept TLS
alerts, which are sent by one of the parties to signal a problem with
the negotiation, such as an expired certificate or a cryptographic
error.

Detailed information that is not defined in ECS is added under the
`tls.detailed` key. The [`include_detailed_fields`](#include_detailed_fields) configuration flag
is used to control whether this information is exported.

The fields under `tls.detailed.client_hello` contain the algorithms and
extensions supported by the client, as well as the maximum TLS version
it supports.

Fields under `tls.detailed.server_hello` contain the final settings for
the TLS session: The selected cipher, compression method, TLS version to
use and other extensions such as application layer protocol negotiation
(ALPN).

**Configuration options**

The `send_certificates` and `include_detailed_fields` settings are
useful for limiting the amount of data Network Packet Capture indexes, as multiple
certificates are usually exchanged in a single transaction, and those
can take a considerable amount of storage.

Also see [Common protocol options](#common-protocol-options).

#### `send_certificates`

This setting causes information about the certificates presented by the
client and server to be included in the detailed fields. The server’s
certificate is indexed under `tls.detailed.server_certificate` and its
certification chain under `tls.detailed.server_certificate_chain`. For
the client, the `client_certificate` and `client_certificate_chain`
fields are used. The default is true.

#### `include_raw_certificates`

You can set `include_raw_certificates` to include the raw certificate
chains encoded in PEM format, under the `tls.server.certificate_chain`
and `tls.client.certificate_chain` fields. The default is false.

#### `include_detailed_fields`

Controls whether the [TLS fields](https://www.elastic.co/guide/en/beats/packetbeat/current/exported-fields-tls_detailed.html) are added to exported documents. When
set to false, only [ECS TLS](https://www.elastic.co/guide/en/ecs/8.2/ecs-tls.html) fields are included.
exported are included. The default is `true`.

#### `fingerprints`

Defines a list of hash algorithms to calculate the certificate’s
fingerprints. Valid values are `sha1`, `sha256` and `md5`.

The default is to output SHA-1 fingerprints.

Fields published for TLS packets.

{{fields "tls"}}

{{event "tls"}}

## Licensing for Windows Systems

The Network Packet Capture Integration incorporates a bundled Npcap installation on Windows hosts. The installation is provided under an [OEM license](https://npcap.com/oem/redist.html) from Insecure.Com LLC ("The Nmap Project").