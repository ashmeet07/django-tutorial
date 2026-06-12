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
It act like a fallback type protocol when websockets along with  SSE are blocked by aggressive corporate firewalls. 
- The client sends HTTPs request to the server and hangs on onto and wait if the server has new data then sends and client process and again this process continous like websockets.
- Fallback system like socket.io for real-time-libary

It all works on Application Layer : HTTPs -> TCP, WebSockets ->TCP, WebTransport -> UDP(QUIC), WebRTC -> (UDP)


Conclusion: 
If you are building a standard web app, stick to HTTPS (REST/GraphQL) and WebSockets (or SSE).

If you are building for smart hardware or microservices, look into MQTT or gRPC.

If you are pushing the absolute limits of low-latency web performance (and don't mind sacrificing compatibility with older browsers), experiment with WebTransport.



CoAP (Constrained Application Protocol)
It basically work on UDP not TCP and mimics the connection of HTTP which basically works on TCP so using micro-controllers using UDP with satellite-linked with compressed header than HTTP.

P2P Overlaid Networks (BitTorrent/IPFS)
While WebRTC allows p-to-p connection between two browsers, decentralized file sharing networks use custome connections link thousands of nodes simultaneously 

AMQP (Advanced Message Queuing Protocol)
It basically works for RabbitMQ which use this protocol to establish Producer/Consumer relationship which basically produce and consume the message. 

eg. Banking System, Heavy backend pipelines.


In Websockets: 
Django uses asynchronous requests using channels (long-lived websockets connections).
- Instead of WSGI server like gunicorn, an ASGI application runs on asynchronous server like Daphne || Uvicorn 

# Views vs Consumers(The Code Shift)
In views you write Views to handle requests but in django websockets you write Consumer. 

- A view lives and dies in miliseconds.
- A Consumer is live until user close the website like a stateful object. 

# Channels Layers (The Communication Backbone)

Browser -> WebSocket -> ASGI Server (Daphne/Uvicorn) -> Routing -> Consumer -> Channel Layer -> Redis -> Channel Layer -> Consumer -> WebSockets -> Browser   

This is the full flow of the WebSockets in django.

## Browser Opens Connection  ( Step 1)

const socket = new WebSocket(
        url_app_route,
)

Request : HTTP Upgrade -> WebSocket Connection 

## ASGI Server (Step 2)

Unlike normal django Django -> WSGI 

Websockets required: Django Channels -> ASGI -> Daphne Uvicorn    //ASGI Receives connection 

## Routing ( Step 3 )

websockets_urlpattern = [
        Path("ws/chat", ChatConsumer.as_asgi())
]

Flow : ws/chat -> ChatConsumer

## Consumer (Step 4)

It is equivalent of a Django View.

HTTP : url -> view 

Websocket : url -> Consumer 

eg. class ChatConsumer(AsyncWebsocketConsumer)

Handels: connect(), receive(), disconnect()

## Connect ( Step 5)

User joins : 
        async def connect(self):
Usually :
        await self.channel_layer.group_add(...)
and:
        await self.accept()

## Channel Name Creation ( Step 6 )

Every connection gets channel_name

Unique address for websockets. 

## Group Creation (Step 7)

eg. 
        room_name="chat_room"
join
        await self.channel_layer.group_add(
                room_name, 
                self.channel_name
        )

chat_room:
        |
        |User A
        |User B
        |User C


## Channel Layer (Step 8)

It is channel layer where it is a "message_bus" it allows consumer A ----- consumer B (Communication)

## Redis (Backbone) (Step 9)

Usually : 
        CHANNEL_LAYERS= {
                "default" : {
                        "BACKEND":
                                "channels_redis.core.RedisChannelLayer"
                }
        }

Architecture:
        Consumer-> Channel Layer-> Redis -> Channel Layer-> Consumer

Redis Stores: Groups, Channels, Messages   (temporarily)


# What happens when user sends a message ?

Browser: 
        socket.send("Hello")

Consumer receive:
        async def receive(self, text_data)
Inside receive:
        await self.channel_layer.group_send(
                "chat_room", 
                {
                        "type":"chat_message",
                        "message": "Hello"
                }
        )

## Group Sends (Step 10 )

This doesn't directly send to browser.

Instead: group_send() -> Redis. //Redis stores events

## Redis  Fan-Out (Step 11)

It looks like chat_room (User A, User B, User C Redis forward network to all channels)

## Event Handler (Step 12)

Channels Finds: 
        "type": "chat_message"
and calls:
        async def chat_message(self, event) //automatically 

eg. 
        async def chat_message(self, event)

Recives:
        {
                "type":"chat_message", 
                "message":"Hello"
        }

## Send (Step 13)

now actually response happen.

await self.send(
        text_data=json.dump(...)
)

Flow : Consumer -> send() -> ASGI -> WebSockets -> Browser


# Full Backbone Flow

User A -> WebSocket -> Consumer -> group_send() -> Channel Layer -> Redis -> Channel Layer -> chat_message() -> Consumer -> send() -> WebSocket -> User B


### Why redis   is needed 

without redis  server 1 wont communicate server 2  in distributed  environment it becomes central message broker.

# Production Architecture

Browser -> Load Balancer -> {Daphne1, Daphne2, Daphne3,....} ->Redis   //all instance share message through redis.

# Scope 

It is equivalent django request object  : self.object 

It Contains: [User , Path, Headers, Query Params, Session, Client IP]

eg. 
        self.scope["user"] //very commonly used.

# Middleware 

HTTP has : Middleware

Channel has : AuthMIddleware, Custom JWT Middleware  eg. JwtAuthMiddleware

# Authentication 

ways to auth websockets. 
        Session Auth 
        JWT Auth 
        Token  Auth 
        Custom Auth  //Common interview topics 

Sync Vs Async Consumers 
        WebsocketConsumer vs AsyncWebsocketConsumer

        Need to know 
                sync_to_async
                database_sync_to_async

# Database Access

Wrong 
        await User.objects.all()
Correct 
        @database_sync_to_async


Channel Layer Methods

group_add()
group_send()
group_discard()
send()
receive()

# Direct Channel Messaging

Instead of group you can send directly to specific group.

await self.channel_layer.send(
        channel_name,
        event
)   # usefull for notification 

# Presence System

eg.
Online Users
Offline Users
Last Seen 
 
 Implementation concepts:  Redis, Groups, Disconnect Handling    //Asked in real-world interviews

# Scaling 

production architecture : Nginx -> Load Balancer -> Daphne Instances -> Redis       //Need to understand why redis is required when multiple  server exist

# WebSocket Lifecycle 

Handshake, Connect, Authentication, Join Group, Receive  Message, Send Message, Disconnect, Cleanup    //IMP Interview question 


# Security 

JWT Validation 
Origin Validation 
Rate Limmiting 
Authentication
Authorization 
Message Validation 


# Real Time Design Patterns 

Common system built using channels: Chat system etc.
