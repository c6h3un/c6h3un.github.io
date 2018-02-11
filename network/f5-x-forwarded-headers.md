# Insert X-Forwarded-* headers for incoming requests

F5 will do SNAT automap for server loadbalancing, which means it will change the source ip to it's self ip and then send to the load balanced server. Since we want to know the source client ip address for Statistical purposes, we add the X-Forwarded-For, X-Forwarded-Proto,X-Forwarded-Port headers to the packets.

- Create the following irule and append to the target virtual server.
```
when CLIENT_ACCEPTED {
    if { [PROFILE::exists clientssl] } then {
        set client_protocol "https"
    } else {
        set client_protocol "http"
    }
}
when HTTP_REQUEST {
    HTTP::header insert "X-Forwarded-For" [IP::client_addr]
    HTTP::header insert "X-Forwarded-Proto" $client_protocol
    HTTP::header insert "X-Forwarded-Port" [TCP::client_port]
}
```