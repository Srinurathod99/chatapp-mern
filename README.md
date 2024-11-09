workflow of the project:

User Authentication:
 Sign-Up: A new user registers, and the server hashes their password using bcrypt for secure storage.
 Login: When a user logs in, the server retrieves the hashed password from the MongoDB database and verifies it against the provided password. Successful verification allows the user to access the chat.


Connecting to the Server and Creating the Online Users Map:
Map Creation: When the server starts, a Map object called onlineUsers is created. This map will hold user IDs as keys and their socket IDs as values, tracking which users are online.
WebSocket Connection: Once authenticated, the client establishes a WebSocket connection with the server using Socket.io. When a user connects, the server adds their user ID and socket ID to the onlineUsers map, indicating that the user is online and available to receive messages.


Sending and Receiving Messages:
Frontend: Users type messages, which are managed by useState in React and then sent to the server via the Socket.io connection.
Backend: When a message is received, the server checks the onlineUsers map to see if the recipient is online. If they are, the server retrieves the recipient’s socket ID from the map and forwards the message to that socket, ensuring direct communication.

Storing Chat History:
All messages are saved in MongoDB using Mongoose to ensure that users can view past conversations. Mongoose schemas and models define the structure of these documents and handle the Create, Read, Update, and Delete (CRUD) operations on chat data.

Displaying Messages:
Frontend: React fetches message history from the server upon loading the chat component. Messages are displayed in real-time as they arrive, using useEffect to manage updates, while useRef allows automatic scrolling to the latest message.

Disconnecting and Managing Users:
When a user disconnects, the server removes their socket ID from the onlineUsers map, accurately updating the user's status.
