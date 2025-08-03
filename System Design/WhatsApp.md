# Main Topics

- Websockets, Pub/Sub, GSI

- Websocket connection
- Pub/Sub -> Users subscribe to topic -> when on chat server the chat server connects to the given topic of pub/sub - when message triggered all userId topics get messages pushed to them.
- Inbox table for undelivered message per client device basis

# FR

- Users can create group chats <= 100
- Users can send/receive messages to 1 or more users
- Users should be able to send/receive media
- Offline message receiving

# Services

- Chat Server
- DB - Chat server information, its participants AND Messages Inbox of undelivered messages and Message table for all messages

![alt text](./Images/WhatsApp.png)

# Deep Dives

- General Establish a Websocket connection -> Chat server -> Dynamo DB (High R/W)

  - Since Websocket connection not all users will be connected will store undelivered messages in inbox
  - Upon receiving the messages the client will send an ack message - then delete message from inbox
  - Manage Media/Attachment using Blob Storage

- Billions of simultaneous users

  - Managing that many websockets on a single host system is not possible
  - If we add more chat servers then each chat participant needs to be on that server and if that participant is on another server that convolutes the scenario
  - We can do some form of consistent hashing where userId has chat server listed and then look up the chat registry and send the message to it -> This also leads to that each chat server would need to talk to each other
  - Best:
    - We are sending messages between servers -> Use Pub/Sub model
    - Create a subscription for userId
    - When user connects to chat server that server will connect to the topic the userId subscribes to -> Any messages on the topic are pushed to the user
    - Sending messages - we publish the messages to the relevant users of the given chatId

- Multiple devices for a single user
  - Clients table for the user devices
  - look up participants for chat also lookup clients
  - inbox will also store data per client basis - so when user logs in that chat is still saved
  - Pub/Sub unaffected
