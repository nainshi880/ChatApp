# Chat App

A real-time chat application built with React and Node.js, featuring user authentication, profile management, and instant messaging.

## Features

- 🔐 User Authentication (Sign up / Login)
- 👤 User Profile Management
- 💬 Real-time Messaging
- 🔍 User Search
- 📸 Profile Picture Upload (Cloudinary)
- 🌐 Online Users Status
- 🎨 Modern UI with Tailwind CSS

## Tech Stack

### Frontend
- React
- React Router
- Socket.io Client
- Axios
- React Hot Toast
- Tailwind CSS
- Vite

### Backend
- Node.js
- Express
- Socket.io
- MongoDB (Mongoose)
- JWT Authentication
- Cloudinary (Image Upload)
- Bcrypt (Password Hashing)

## Project Structure

```
chat-app/
├── client/          # React frontend
│   ├── src/
│   │   ├── components/    # React components
│   │   ├── pages/         # Page components
│   │   ├── context/       # Context providers
│   │   └── assets/        # Static assets
│   └── package.json
├── server/          # Node.js backend
│   ├── controllers/       # Route controllers
│   ├── models/            # MongoDB models
│   ├── routes/            # API routes
│   ├── middleware/        # Auth middleware
│   └── lib/               # Utilities
└── README.md
```

## Setup Instructions

### Prerequisites
- Node.js (v14 or higher)
- MongoDB database
- Cloudinary account (for image uploads)

### Installation

1. Clone the repository
```bash
git clone https://github.com/yourusername/chat-app.git
cd chat-app
```

2. Install server dependencies
```bash
cd server
npm install
```

3. Install client dependencies
```bash
cd ../client
npm install
```

### Environment Variables

Create a `.env` file in the `server` directory:

```env
PORT=5000
MONGODB_URI=your_mongodb_connection_string
JWT_SECRET=your_jwt_secret_key
CLOUDINARY_CLOUD_NAME=your_cloudinary_cloud_name
CLOUDINARY_API_KEY=your_cloudinary_api_key
CLOUDINARY_API_SECRET=your_cloudinary_api_secret
```

Create a `.env` file in the `client` directory:

```env
VITE_BACKEND_URL=http://localhost:5000
```

### Running the Application

1. Start the server
```bash
cd server
npm start
```

2. Start the client (in a new terminal)
```bash
cd client
npm run dev
```

The application will be available at:
- Frontend: http://localhost:5173 (or the port shown in terminal)
- Backend: http://localhost:5000

## API Endpoints

### Authentication
- `POST /api/auth/signup` - User registration
- `POST /api/auth/login` - User login
- `GET /api/auth/check` - Check authentication status
- `PUT /api/auth/update-profile` - Update user profile

### Messages
- `GET /api/messages/:userId` - Get messages with a user
- `POST /api/messages` - Send a message

## License

MIT

