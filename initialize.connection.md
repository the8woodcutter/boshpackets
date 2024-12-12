### ------------------------------------------------------------------------------------------------------------------------
## Example 1.
* Requesting an HTTP session

```
POST /webclient HTTP/1.1
Host: httpcm.jabber.org
Accept-Encoding: gzip, deflate
Content-Type: text/xml; charset=utf-8
Content-Length: 104

<body content='text/xml; charset=utf-8'
      hold='1'
      rid='1573741820'
      to='jabber.org'
      route='xmpp:jabber.org:9999'
      secure='true'
      wait='60'
      xml:lang='en'
      xmlns='http://jabber.org/protocol/httpbind'/>
```

### ------------------------------------------------------------------------------------------------------------------------
## Example 2.
* Session creation response

```
HTTP/1.1 200 OK
Content-Type: text/xml; charset=utf-8
Content-Length: 128

<body authid='ServerStreamID'
      wait='60'
      inactivity='30'
      polling='5'
      requests='2'
      accept='deflate,gzip'
      sid='SomeSID'
      secure='true'
      charsets='ISO_8859-1 ISO-2022-JP'
      xmlns='http://jabber.org/protocol/httpbind'/>
```

### ------------------------------------------------------------------------------------------------------------------------
## Example 3.
* Session creation response with stream features

```
HTTP/1.1 200 OK
Content-Type: text/xml; charset=utf-8
Content-Length: 417

<body authid='ServerStreamID'
      wait='60'
      inactivity='30'
      polling='5'
      requests='2'
      accept='deflate,gzip'
      sid='SomeSID'
      charsets='ISO_8859-1 ISO-2022-JP'
      xmlns='http://jabber.org/protocol/httpbind'
      xmlns:stream='http://etherx.jabber.org/streams'>
  <stream:features>
    <mechanisms xmlns='urn:ietf:params:xml:ns:xmpp-sasl'>
      <mechanism>DIGEST-MD5</mechanism>
      <mechanism>PLAIN</mechanism>
    </mechanisms>
    <bind xmlns='urn:ietf:params:xml:ns:xmpp-bind'>
    <session xmlns='urn:ietf:params:xml:ns:xmpp-session'>
  </stream:features>
</body>
```

  ## ***

##9.
* Additional Preconditions

      Initializing an HTTP session is the first precondition to sending XML message, presence, and IQ stanzas. However, before processing XML stanzas from the client, the connection manager MUST require completion of additional preconditions using either of the following methods:

          * XMPP authentication, resource binding, and IM session creation, in the following order:
              * Optionally, TLS negotiation as defined in Section 5 of RFC 3920 (this is NOT RECOMMENDED since channel encryption and compression SHOULD be negotiated at the HTTP layer; see the Security Considerations section below)
              * SASL authentication as defined in Section 6 of RFC 3920
              * Resource Binding as defined in Section 7 of RFC 3920
              * IM Session Establishment as defined in Section 3 of RFC 3921, if appropriate

          * Simultaneous authentication and resource binding as defined in Non-SASL Authentication, upon which a Jabber server will also establish an IM session on behalf of the connected resource.

      It is RECOMMENDED to use the XMPP methods as defined in RFC 3920 and RFC 3921, rather than using older non-SASL authentication.

  ## ***


### ------------------------------------------------------------------------------------------------------------------------
## Example 4.
* SASL authentication step 1

```
POST /webclient HTTP/1.1
Host: httpcm.jabber.org
Accept-Encoding: gzip, deflate
Content-Type: text/xml; charset=utf-8
Content-Length: 172

<body rid='1573741821'
      sid='SomeSID'
      xmlns='http://jabber.org/protocol/httpbind'>
  <auth xmlns='urn:ietf:params:xml:ns:xmpp-sasl' mechanism='DIGEST-MD5'/>
</body>
```

### ------------------------------------------------------------------------------------------------------------------------
## Example 5.
* SASL authentication step 2

```
HTTP/1.1 200 OK
Content-Type: text/xml; charset=utf-8
Content-Length: 250

<body xmlns='http://jabber.org/protocol/httpbind'>
  <challenge xmlns='urn:ietf:params:xml:ns:xmpp-sasl'>
    cmVhbG09InNvbWVyZWFsbSIsbm9uY2U9Ik9BNk1HOXRFUUdtMmhoIixxb3A9
    ImF1dGgiLGNoYXJzZXQ9dXRmLTgsYWxnb3JpdGhtPW1kNS1zZXNzCg==
  </challenge>
</body>
```

### ------------------------------------------------------------------------------------------------------------------------
# Example 6.
* SASL authentication step 3

```
POST /webclient HTTP/1.1
Host: httpcm.jabber.org
Accept-Encoding: gzip, deflate
Content-Type: text/xml; charset=utf-8
Content-Length: 418

<body rid='1573741822'
      sid='SomeSID'
      xmlns='http://jabber.org/protocol/httpbind'>
  <response xmlns='urn:ietf:params:xml:ns:xmpp-sasl'>
    dXNlcm5hbWU9InNvbWVub2RlIixyZWFsbT0ic29tZXJlYWxtIixub25jZT0i
    T0E2TUc5dEVRR20yaGgiLGNub25jZT0iT0E2TUhYaDZWcVRyUmsiLG5jPTAw
    MDAwMDAxLHFvcD1hdXRoLGRpZ2VzdC11cmk9InhtcHAvZXhhbXBsZS5jb20i
    LHJlc3BvbnNlPWQzODhkYWQ5MGQ0YmJkNzYwYTE1MjMyMWYyMTQzYWY3LGNo
    YXJzZXQ9dXRmLTgK
  </response>
</body>
```

### ------------------------------------------------------------------------------------------------------------------------
## Example 7.
* SASL authentication step 4

```
HTTP/1.1 200 OK
Content-Type: text/xml; charset=utf-8
Content-Length: 190

<body xmlns='http://jabber.org/protocol/httpbind'>
  <challenge xmlns='urn:ietf:params:xml:ns:xmpp-sasl'>
    cnNwYXV0aD1lYTQwZjYwMzM1YzQyN2I1NTI3Yjg0ZGJhYmNkZmZmZAo=
  </challenge>
</body>
```

### ------------------------------------------------------------------------------------------------------------------------
## Example 8.
* SASL authentication step 5

```
POST /webclient HTTP/1.1
Host: httpcm.jabber.org
Accept-Encoding: gzip, deflate
Content-Type: text/xml; charset=utf-8
Content-Length: 152

<body rid='1573741823'
      sid='SomeSID'
      xmlns='http://jabber.org/protocol/httpbind'>
  <response xmlns='urn:ietf:params:xml:ns:xmpp-sasl'/>
</body>
```

### ------------------------------------------------------------------------------------------------------------------------
## Example 9.
* SASL authentication step 6

```
HTTP/1.1 200 OK
Content-Type: text/xml; charset=utf-8
Content-Length: 121

<body xmlns='http://jabber.org/protocol/httpbind'>
  <success xmlns='urn:ietf:params:xml:ns:xmpp-sasl'/>
</body>
```

      _**Note:** Because the context for a client's communication with a connection manager in the HTTP transport binding is HTTP rather than XML streams (as in the TCP binding), there is no need to re-start communications (e.g., by generating a new SID) at this point._

### ------------------------------------------------------------------------------------------------------------------------
## Example 10.
* Resource binding request

```
POST /webclient HTTP/1.1
Content-Type: text/xml; charset=utf-8
Content-Length: 240

<body rid='1573741824'
      sid='SomeSID'
      xmlns='http://jabber.org/protocol/httpbind'>
  <iq id='bind_1'
      type='set'
      xmlns='jabber:client'>
    <bind xmlns='urn:ietf:params:xml:ns:xmpp-bind'>
      <resource>httpclient</resource>
    </bind>
  </iq>
</body>
```

### ------------------------------------------------------------------------------------------------------------------------
## Example 11.
* Resource binding result

```
HTTP/1.1 200 OK
Content-Type: text/xml; charset=utf-8
Content-Length: 221

<body xmlns='http://jabber.org/protocol/httpbind'>
  <iq id='bind_1'
      type='result'
      xmlns='jabber:client'>
    <bind xmlns='urn:ietf:params:xml:ns:xmpp-bind'>
      <jid>stpeter@jabber.org/httpclient</jid>
    </bind>
  </iq>
</body>
```

### ------------------------------------------------------------------------------------------------------------------------
## Example 12.
* IM session request

```
POST /webclient HTTP/1.1
Content-Type: text/xml; charset=utf-8
Content-Length: 261

<body rid='1573741825'
      sid='SomeSID'
      xmlns='http://jabber.org/protocol/httpbind'>
  <iq from='stpeter@jabber.org/httpclient'
      id='sess_1'
      to='jabber.org'
      type='set'
      xmlns='jabber:client'>
    <session xmlns='urn:ietf:params:xml:ns:xmpp-session'/>
  </iq>
</body>
```

### ------------------------------------------------------------------------------------------------------------------------
## Example 13.
* IM session result

```
HTTP/1.1 200 OK
Content-Type: text/xml; charset=utf-8
Content-Length: 175

<body xmlns='http://jabber.org/protocol/httpbind'>
  <iq from='jabber.org'
      id='sess_1'
      to='stpeter@jabber.org/httpclient'
      type='result'
      xmlns='jabber:client'/>
</body>
```