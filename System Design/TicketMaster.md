# Design Ticketmaster:

### Redis - Distributed Lock

- When Booking a ticket the eventId is stored in Redis DB
- This way whenever someone else tries to fetch tickets for the same event they're prevented by the event service to display the tickets (Can do constant polling)
- TTL on the lock - so that if user drops is available
- Mark the ticket as in progress so when another user tries to book it it will be unavailable temporarily
- Redlock Algorithm used for multiple instances of redis

### Popular Event:

- UI changed to create virtual line
- Queue added between booking service and client
- Websocket connection created for the queue to the client

### Searching Popular Events:

- CDC on Elastic Search (Has Inverted Indexing - text, GeoHash)
- Redish Cache on the Event Service
- CDN for popular search

### Payment:

- Stripe
- Provided Callback URL to Stripe Webhook with the bookingId
- Once Stripe processes payment the webhook responds to our backend booking service

### Throughput/Scalability:

- Load Balancer
- Horizontal Scaling

![TicketMaster](./Images/TicketMaster.png)
