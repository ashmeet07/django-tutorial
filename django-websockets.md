# Websockets & configuration with django+react  (Bidirectional (Both ways))
*It is use to make two way communication bi-directional used for two way communication*

- Instead of request -> ,response <- model we use client ↔ server model (bi-directional) approach.
- Used for streaming live becaues ends are open from both the side.
- Transfer happen at anytime.

Normal HTTP request works like:


`Browser ----request----> Server`
`Browser <---response---- Server`

Websockets connection flow:
    - HTTP Handshake
        - The browser first sends  a normal HTTP request
        
        ```http
                GET /chat HTTP/1.1
                Host: example.com
                Upgrade: websocket
                Connection: Upgrade
```
        
        The server replies:
        
```http
        HTTP/1.1 101 Switching Protocols
        Upgrade: websocket
        Connection: Upgrade
```

        Now HTTP becomes a WebSocket connection.
        
# Required package in python 

        - channels
        //for production
        -  channels-redis
        //same as app we have to register channel in INSTALLED_APPS

        We need to specify ASGI_APP="project.asgi.application"

## asgi.py 

This is the first file which websockets first touch. [browser -> asgi.py -> consumer]

This tells Django HTTP? || Websocket? 

so from this we have learn't there there is another connections also exist like: 

HTTP 1.1 -> 3  Client -> Server

HTTP 1.1  It uses TCP always for each time when opening the browser 

HTTP 2 || 3  Uses concept of multiplexing for better efficiency in one connection stream download multiple files. While HTTP 3 Swap out the TCP to QUIC (which often runs on UDP) - Quick UDP Internet Connection this makes connection much resillient and durable to drop on. 

eg. Loading pages, Submitting forms, fetching api data.

SSE( Server-Sent-Events) TCP  Server -> Client
Think of SSE as a middle ground between standard HTTPS and Websockets.

- In this it uses (Server -> Client) approach means server continuosly pushing data to the client. 
- Majorly uses on real time dashboards without sending data back.



WebRTC(Peer-to-Peer) - Real Time Communication (UDP)   Peer -> Peer
While websocket connected to a client to server, WebRTC connects a client to client [P-to-P]
- This is best for (Zoom/Google meet in a browser) means a server sits in between for signaling both clients continous connection 

gRCP - Google Remote Procedure Call (HTTP/2)    Flexible/Bidirectional  
It is basically famous for connecting (microservices) & apps to the server.
- Usage of HTTP/2 but it sends highly compressed binary format called protocol buffers instead of bulky JSON. 
- Support standard requests, server streaming, client streaming & full bidirectional streaming.

WebTransport( The Next-Gen WebSocket)
This is the newest major addition to the web ecosystem.
- It is actively designed to solve the biggest flaws of the websockets.
- So instead of running TCP like websockets, webTransport runs on top of HTTP/3 & QUIC(UDP)
It basically do use UDP because if one packet is lost then it waits for retransmission (Head-of-Line-Blocking).
WebTransport allows you to open multiple independent streams over a single connection, and it support unreliable datagrams( where it doen't matter if one get lost)

eg. Next gen cloud gaming, video editing, high frequency stock trading.


MQTT ( Message Queuing Telemetry Transport)
If you are connecting strings that arn't web browsers, MQTT is the dominant standard.
- It basically works on publish/subscriber model in which basically the publish one publish to the broker and the subscriber one subscribe which basically subscribe and receive the message.
- It features a highly compressed binary header make it faster and less bandwidth utilised.

eg. embedded sensors, IoT devices edge devices as well etc.


HTTP Long Pooling the legacy bridge