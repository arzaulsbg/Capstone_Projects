# 📘 Project Report — AzaanCircles (Social Media Web Application)

## 👨‍💻 Author

**Name:** Arzaul Haque  
**Course:** B.Tech in AI & Data Science, Chandigarh University  
**Project Title:** AzaanCircles — A MERN Stack Social Media Application  
**Deployment:** AzaanCircles Live Demo (Heroku)

---

## 🧩 1. Project Overview

**AzaanCircles** is a full-stack social media web application that enables users to share posts, like and comment on them, follow other users, and chat in real-time.  
It is built using the **MERN stack (MongoDB, Express.js, React.js, Node.js)** and leverages **Socket.IO** for instant messaging between users.

### 🔹 Objectives

- Create a scalable, interactive social platform.  
- Implement secure authentication and authorization using JWT.  
- Integrate real-time communication using WebSockets.  
- Provide a responsive and engaging user interface.

---

## 🏗️ 2. Tech Stack

### Backend

- **Runtime & Framework:** Node.js (Express)  
- **Database:** MongoDB (with Mongoose ODM)  
- **Authentication:** JWT (jsonwebtoken)  
- **Security:** bcrypt (password hashing)  
- **Real-time Communication:** Socket.IO  
- **Utilities & Libraries:** dotenv, cors, express-validator, validator, bad-words  

### Frontend

- **Framework:** React (Create React App)  
- **Routing:** React Router v6  
- **UI Library:** Material UI (MUI)  
- **Markdown Rendering:** react-markdown  
- **Networking:** fetch API, socket.io-client  

### Versions

| Component | Version |
|------------|----------|
| express | ^4.18.1 |
| mongoose | ^6.3.2 |
| socket.io | ^4.5.1 |
| bcrypt | ^5.0.1 |
| react | ^18.x |
| react-router-dom | ^6.3.0 |
| @mui/material | ^5.6.4 |

---

## 💡 3. Key Features

| Category | Features |
|-----------|-----------|
| **Posts** | Create, edit, delete posts with Markdown support |
| **Likes** | Like/unlike posts; view users who liked them |
| **Comments** | Nested comment threads (reply/update/delete) |
| **Authentication** | Secure sign-up and login via JWT |
| **Profiles** | View user profile, posts, followers, liked posts |
| **Real-time Chat** | Private 1-to-1 chat via Socket.IO |
| **Search & Explore** | Search posts and browse random users |
| **Moderation** | Profanity filtering & posting cooldowns |
| **Responsive UI** | Mobile-friendly interface |

---

## 🔄 4. System Architecture

### Overview

The architecture follows the client-server model with WebSocket integration:

```
[React Client] ⇆ [Express REST API] ⇆ [MongoDB]
         │
         └── Socket.IO ⇄ Real-Time Messaging Layer
```

### Backend Flow

- `server.js` initializes Express, MongoDB connection, and Socket.IO.  
- `socketServer.js` handles user authentication via JWT during socket handshake.  
- Routes registered: `/api/users`, `/api/posts`, `/api/comments`, `/api/messages`

### Frontend Flow

- React SPA uses protected routes for authentication.  
- Uses fetch API for CRUD operations.  
- Uses `socketHelper.js` for socket connection lifecycle management.

---

## ⚙️ 5. API Endpoints Summary

### Users
- `POST /api/users/register` — Register new user  
- `POST /api/users/login` — Login user  
- `GET /api/users/:username` — Get user profile  
- `POST /api/users/follow/:id` — Follow user  
- `DELETE /api/users/unfollow/:id` — Unfollow user  

### Posts
- `GET /api/posts` — Retrieve posts with pagination & sorting  
- `POST /api/posts` — Create post  
- `PATCH /api/posts/:id` — Update post  
- `DELETE /api/posts/:id` — Delete post  
- `POST /api/posts/like/:id` — Like post  
- `DELETE /api/posts/like/:id` — Unlike post  

### Comments
- `POST /api/comments/:id` — Add comment to post  
- `PATCH /api/comments/:id` — Edit comment  
- `DELETE /api/comments/:id` — Delete comment  

### Messages
- `GET /api/messages` — Fetch all conversations  
- `POST /api/messages/:id` — Send message  
- `GET /api/messages/:id` — Get messages with specific user  

---

## 🧱 6. Data Models (Mongoose)

| Model | Fields |
|--------|--------|
| User | username, email, password, biography, isAdmin |
| Post | poster, title, content, likeCount, commentCount, edited |
| Comment | commenter, post, content, parent, children, edited |
| Message | conversation, sender, content |
| Conversation | recipients, lastMessageAt |
| Follow | userId, followingId |
| PostLike | postId, userId |

---

## 💻 7. Client Structure

```
client/
 ├── src/
 │   ├── api/ (posts.js, users.js, messages.js)
 │   ├── components/
 │   ├── views/ (ExploreView, PostView, ProfileView, MessengerView, etc.)
 │   ├── helpers/ (authHelper.js, socketHelper.js)
 │   ├── App.js, index.js, config.js, theme.js
 └── public/
```

- Uses React Router v6 for route navigation.  
- Implements `theme.js` for consistent UI styling.  
- `authHelper.js` manages localStorage for JWT tokens.

---

## 🧰 8. Installation & Setup

### Environment Variables
```
MONGO_URI = <your_mongo_uri>
TOKEN_KEY = <random_secret>
PORT = 4000
```

### Commands
```bash
# Install dependencies
npm install && cd client && npm install

# Run backend and frontend concurrently
npm run dev

# Build frontend for production
cd client && npm run build
```

---

## 🧪 9. Testing & CI

Currently, the project has **no automated tests**.

### Recommended Additions:
- Unit tests using Jest + Supertest for API endpoints.  
- Socket event tests for message reliability.  
- GitHub Actions CI pipeline for testing and linting on each PR.

---

## 🔐 10. Security & Validation

- Passwords hashed using bcrypt.  
- JWT authentication for all protected routes.  
- Input validation via Mongoose validators and express-validator (partially implemented).  
- Profanity filtering via bad-words library.

### Recommendation:
- Store JWT in HTTP-only cookies for XSS protection.  
- Add refresh tokens for secure session renewal.

---

## 🚀 11. Performance & Scalability

### Current Issues:
- Pagination done in-memory (inefficient for large data sets).  
- Lack of database-level query optimization.

### Recommendations:
- Use `.limit()` and `.skip()` or cursor-based pagination.  
- Add indexes for common queries (e.g., createdAt, poster).  
- Implement rate-limiting and Redis caching for scalability.

---

## 🧩 12. Identified Bugs & Fix Suggestions

| Bug | Location | Fix |
|------|-----------|------|
| Incorrect status code 400 on success | postControllers.getUserLikes | Change to res.status(200) |
| Follow.find() returns array instead of document | userControllers.follow | Use findOne() or check length |
| Inconsistent user auth handling | Multiple controllers | Standardize req.user use |
| express-validator imported but unused | All routes | Add route-level validations |

---

## 🌱 13. Future Enhancements

- Add email verification and password reset.  
- Implement Docker and docker-compose for easy setup.  
- Add real-time notifications for likes/comments.  
- Introduce admin dashboard for content moderation.  
- Integrate Redis for socket presence and caching.

---

## 🗂️ 14. Repository Structure

```
social-media-app/
 ├── server.js
 ├── socketServer.js
 ├── routes/
 ├── controllers/
 ├── models/
 ├── utils/
 └── client/
```

---

## 🖼️ 15. Screenshots & Visuals

**Screenshots (from README):**  
- Explore Feed  
- Post View (with nested comments)  
- User Profile Page  
- Real-Time Chat (Messenger)  
- Search Page  

---

## 🧭 16. Conclusion

**AzaanCircles** successfully demonstrates the integration of a modern MERN stack with real-time communication, secure authentication, and a responsive front-end.  
It serves as a strong foundation for a scalable social platform and can be extended with microservices, testing, and Docker-based deployments in the future.
