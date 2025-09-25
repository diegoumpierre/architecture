# BFF (Backend for frontend)
- Specific for a client, eg. Mobile or Web 
- Logic oriented to the client
- Data aggregation (get all the response and just aggregated.  Have one field and split in two items - very danger to start put businnes logic - trigger)
- Flow Control
- Allways custom code
- Usually used from front-end languages, eg. Express server with Node
- rendering logic (emoji, colors, size)

# API Gateway
- Attend all the clients, eg. Mobile and web.
- Routing logic
- Common use to authentication and authorization
- Share the session
- Can be custom code or base solution
- Netflix Zuul, Spring Cloud Gateway, AWS Gateway