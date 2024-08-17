# Challenge18-NoSQL-MongoLink-API

## Description

The **NoSql MongoLink API** is a RESTful API for a social network web application. It allows users to share their thoughts, react to friends’ thoughts, and manage friend lists using a NoSQL database (MongoDB) and Mongoose ODM. This project includes functionalities such as creating, updating, and deleting users and thoughts, as well as managing reactions and friendships.

## User Story

```md
AS A social media startup
I WANT an API for my social network that uses a NoSQL database
SO THAT my website can handle large amounts of unstructured data
```

## Acceptance Criteria

```md
GIVEN a social network API
WHEN I enter the command to invoke the application
THEN my server is started and the Mongoose models are synced to the MongoDB database
WHEN I open API GET routes in Insomnia for users and thoughts
THEN the data for each of these routes is displayed in a formatted JSON
WHEN I test API POST, PUT, and DELETE routes in Insomnia
THEN I am able to successfully create, update, and delete users and thoughts in my database
WHEN I test API POST and DELETE routes in Insomnia
THEN I am able to successfully create and delete reactions to thoughts and add and remove friends to a user’s friend list
```

## Models

### User

- `username`
  - String
  - Unique
  - Required
  - Trimmed

- `email`
  - String
  - Required
  - Unique
  - Must match a valid email address

- `thoughts`
  - Array of `_id` values referencing the `Thought` model

- `friends`
  - Array of `_id` values referencing the `User` model (self-reference)

**Schema Settings:**
- Virtual `friendCount` to retrieve the length of the user's `friends` array field on query.

### Thought

- `thoughtText`
  - String
  - Required
  - 1 to 280 characters

- `createdAt`
  - Date
  - Default to the current timestamp
  - Use a getter method to format the timestamp

- `username`
  - String
  - Required

- `reactions`
  - Array of nested documents created with the `reactionSchema`

**Schema Settings:**
- Virtual `reactionCount` to retrieve the length of the thought's `reactions` array field on query.

### Reaction (Schema Only)

- `reactionId`
  - Mongoose ObjectId
  - Default value is a new ObjectId

- `reactionBody`
  - String
  - Required
  - 280 character maximum

- `username`
  - String
  - Required

- `createdAt`
  - Date
  - Default value to the current timestamp
  - Use a getter method to format the timestamp

## API Routes

### `/api/users`

- `GET` all users
- `GET` a single user by `_id` and populated thought and friend data
- `POST` a new user
- `PUT` to update a user by `_id`
- `DELETE` to remove a user by `_id`
- **BONUS**: Remove a user's associated thoughts when deleted.

### `/api/users/:userId/friends/:friendId`

- `POST` to add a new friend to a user's friend list
- `DELETE` to remove a friend from a user's friend list

### `/api/thoughts`

- `GET` all thoughts
- `GET` a single thought by `_id`
- `POST` to create a new thought (push the created thought's `_id` to the associated user's `thoughts` array field)
- `PUT` to update a thought by `_id`
- `DELETE` to remove a thought by `_id`

### `/api/thoughts/:thoughtId/reactions`

- `POST` to create a reaction stored in a single thought's `reactions` array field
- `DELETE` to pull and remove a reaction by the reaction's `reactionId` value

## Getting Started

1. **Install MongoDB**: Follow the [MongoDB installation guide](https://coding-boot-camp.github.io/full-stack/mongodb/how-to-install-mongodb) to install MongoDB locally.
   
2. **Clone the Repository**:
   ```bash
   git clone https://github.com/AJKaur02/nosql-mongolink-api.git
   ```

3. **Navigate to Project Directory**:
   ```bash
   cd nosql-mongolink-api
   ```

4. **Install Dependencies**:
   ```bash
   npm install
   ```

5. **Start MongoDB**:
   ```bash
   brew services start mongodb-community
   ```

6. **Start the Server**:
   ```bash
   npm start
   ```
   
## Technologies Used

- Node.js
- Express.js
- MongoDB
- Mongoose

## Contributing

Contributions are welcome! Please open an issue or submit a pull request for any changes or improvements.

## License

This project is licensed under the MIT License.

---