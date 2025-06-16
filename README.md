<div align="center">
  <img src="https://cdn-icons-png.flaticon.com/512/6295/6295417.png" alt="Shipment Tracker API" width="100" height="100">
  <h1>Shipment Tracker - Backend API</h1>
  <p>A robust Node.js API for tracking cargo shipments with MongoDB integration</p>
  
  <div>
    <img src="https://img.shields.io/badge/Node.js-18.x-339933?style=for-the-badge&logo=node.js" alt="Node.js">
    <img src="https://img.shields.io/badge/Express-4.18.2-000000?style=for-the-badge&logo=express" alt="Express">
    <img src="https://img.shields.io/badge/MongoDB-8.15.2-47A248?style=for-the-badge&logo=mongodb" alt="MongoDB">
  </div>
</div>

<hr>

## 📋 Table of Contents

- [Overview](#overview)
- [API Endpoints](#api-endpoints)
- [Data Models](#data-models)
- [Technologies](#technologies)
- [Installation](#installation)
- [Environment Variables](#environment-variables)
- [Available Scripts](#available-scripts)
- [Folder Structure](#folder-structure)
- [Assumptions](#assumptions)
- [Database Setup](#database-setup)
- [Docker Setup](#docker-setup)

<hr>

## 🔍 Overview

This is the backend API for the Shipment Tracker application, built using Node.js, Express, and MongoDB. It provides a comprehensive set of endpoints for managing and tracking shipments, including creating new shipments, updating their locations, and calculating estimated arrival times.

The API is designed to be robust, scalable, and easy to integrate with the frontend application or other services.

<hr>

## 🔌 API Endpoints

<div align="center">
  <table>
    <tr>`
      <th>Method</th>
      <th>Endpoint</th>
      <th>Description</th>
    </tr>
    <tr>
      <td><code>GET</code></td>
      <td><code>/api/shipments</code></td>
      <td>Retrieve all shipments with filtering and sorting options</td>
    </tr>
    <tr>
      <td><code>GET</code></td>
      <td><code>/api/shipments/:trackingNumber</code></td>
      <td>Retrieve details of a specific shipment</td>
    </tr>
    <tr>
      <td><code>POST</code></td>
      <td><code>/api/shipments</code></td>
      <td>Create a new shipment</td>
    </tr>
    <tr>
      <td><code>PATCH</code></td>
      <td><code>/api/shipments/:trackingNumber/location</code></td>
      <td>Update the current location of a shipment</td>
    </tr>
    <tr>
      <td><code>PATCH</code></td>
      <td><code>/api/shipments/:trackingNumber/status</code></td>
      <td>Update the status of a shipment</td>
    </tr>
    <tr>
      <td><code>GET</code></td>
      <td><code>/api/shipments/:trackingNumber/history</code></td>
      <td>Retrieve the location history of a shipment</td>
    </tr>
    <tr>
      <td><code>GET</code></td>
      <td><code>/api/shipments/:trackingNumber/eta</code></td>
      <td>Calculate and retrieve the ETA based on current location</td>
    </tr>
    <tr>
      <td><code>GET</code></td>
      <td><code>/api/shipments/nearby</code></td>
      <td>Find shipments near a specific location</td>
    </tr>
    <tr>
      <td><code>GET</code></td>
      <td><code>/health</code></td>
      <td>Health check endpoint</td>
    </tr>
  </table>
</div>

<hr>

## 📊 Data Models

### Shipment Model

```javascript
{
  trackingNumber: String,            // Unique identifier
  origin: {                          // Origin location
    type: 'Point',
    coordinates: [Number, Number],   // [longitude, latitude]
    address: String,
    timestamp: Date
  },
  destination: {                     // Destination location
    type: 'Point',
    coordinates: [Number, Number],
    address: String,
    timestamp: Date
  },
  currentLocation: {                 // Current location
    type: 'Point',
    coordinates: [Number, Number],
    address: String,
    timestamp: Date
  },
  status: String,                    // pending, in_transit, out_for_delivery, delivered, exception
  estimatedDelivery: Date,           // Estimated delivery date
  customer: {                        // Customer information
    name: String,
    email: String,
    phone: String
  },
  items: [{                          // Items in shipment
    description: String,
    quantity: Number,
    weight: Number,
    dimensions: {
      length: Number,
      width: Number,
      height: Number
    }
  }],
  history: [{                        // Location history
    location: {
      type: 'Point',
      coordinates: [Number, Number],
      address: String,
      timestamp: Date
    },
    status: String,
    description: String,
    timestamp: Date
  }]
}
```

<hr>

## 🛠️ Technologies

- **Node.js** - JavaScript runtime
- **Express** - Web framework
- **MongoDB** - NoSQL database
- **Mongoose** - MongoDB object modeling
- **Winston** - Logging
- **Express Validator** - Request validation
- **Helmet** - Security middleware
- **Cors** - Cross-origin resource sharing

<hr>

## 📥 Installation

Follow these steps to set up the backend API:

```bash
# Clone the repository
git clone https://github.com/yourusername/tracker-backend.git

# Navigate to the project directory
cd tracker-backend

# Install dependencies
npm install

# Set up environment variables (see Environment Variables section)
cp .env.example .env
# Edit .env with your configuration

# Start the development server
npm run dev
```

The API will be available at [http://localhost:5000](http://localhost:5000)

<hr>

## 🔐 Environment Variables

Create a `.env` file in the root directory with the following variables:

```
# Server configuration
PORT=5000
NODE_ENV=development

# MongoDB connection string
MONGO_URI=mongodb+srv://<username>:<password>@<cluster-address>/<database-name>

# Optional: JWT secret (if implementing authentication)
# JWT_SECRET=your_jwt_secret_here

# Optional: Logging level
# LOG_LEVEL=info
```

<hr>

## 📜 Available Scripts

In the project directory, you can run:

- `npm start` - Starts the server in production mode
- `npm run dev` - Starts the server with nodemon for development
- `npm test` - Runs the test suite
- `npm run lint` - Runs ESLint to check code quality

<hr>

## 📁 Folder Structure

```
tracker-backend/
├── src/
│   ├── config/                # Configuration files
│   │   ├── config.js          # Environment variables
│   │   └── database.js        # Database connection
│   ├── controllers/           # Request handlers
│   │   └── shipment.controller.js
│   ├── middleware/            # Custom middleware
│   │   ├── auth.middleware.js
│   │   └── error.middleware.js
│   ├── models/                # Database models
│   │   └── shipment.model.js
│   ├── routes/                # API routes
│   │   └── shipment.routes.js
│   ├── utils/                 # Utility functions
│   │   └── logger.js
│   └── server.js              # Entry point
├── .env                       # Environment variables
├── .gitignore                 # Git ignore file
├── package.json               # Dependencies and scripts
└── README.md                  # This file
```

<hr>

## 🤔 Assumptions

- MongoDB Atlas is used as the database service
- The API will be consumed primarily by the React frontend application
- Geospatial queries are supported by the MongoDB instance
- Authentication and authorization will be implemented in a future version
- The API will be deployed to a Node.js-compatible hosting environment
- Error logging is important for debugging and monitoring
- The application might need to scale horizontally in the future

<hr>

## 💾 Database Setup

This API uses MongoDB for data storage. You can use MongoDB Atlas (cloud) or a local MongoDB instance.

### MongoDB Atlas Setup

1. Create a free account at [MongoDB Atlas](https://www.mongodb.com/cloud/atlas)
2. Create a new cluster
3. Create a database user with read/write permissions
4. Get your connection string from the "Connect" button
5. Add your connection string to the `.env` file

### Local MongoDB Setup

1. Install MongoDB Community Edition
2. Start the MongoDB service
3. Use the connection string: `mongodb://localhost:27017/shipment-tracker`

<hr>

## 🐳 Docker Setup

You can also run the application using Docker:

```bash
# Build the Docker image
docker build -t shipment-tracker-api .

# Run the container
docker run -p 5000:5000 --env-file .env shipment-tracker-api
```

### Docker Compose

```yaml
version: '3'
services:
  api:
    build: .
    ports:
      - "5000:5000"
    env_file: .env
    depends_on:
      - mongo
  mongo:
    image: mongo
    ports:
      - "27017:27017"
    volumes:
      - mongo-data:/data/db

volumes:
  mongo-data:
```

<div align="center">
  <p>Built with ❤️ by Your Name</p>
</div> 