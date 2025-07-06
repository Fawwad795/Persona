# Persona - Hostel Management System

## Overview
Persona is a comprehensive university management system designed to streamline administrative processes for educational institutions. The system provides a centralized platform for managing students, hostels, security, and services while focusing mainly on the attendance system.

## Features

### Student Management
- Student registration and profile management
- Student search and filtering capabilities
- Attendance tracking

### Security Features
- Fingerprint-based authentication system
- Security alerts and monitoring
- Access control management

### Services
- Mess service management
- Complaint registration and resolution
- Query submission and response system

### Dashboard
- Real-time statistics and analytics
- Interactive charts and graphs
- Daily motivational quotes

## Screenshots

![Dashboard](Screenshots/WhatsApp%20Image%202025-07-06%20at%2016.25.50_b53e1701.jpg)
*Main dashboard with statistics and analytics*

![Fingerprint Scanner](Screenshots/Screenshot%202025-07-06%20161732.png)
*The harware device for fingerprint scanning*

## Technologies Used

### Frontend
- Svelte - Component-based JavaScript framework
- Chart.js - Interactive charts and graphs
- Flaticon UI Icons - Modern icon library
- CSS3 with custom variables for theming

### Backend
- Node.js - JavaScript runtime
- Express.js - Web application framework
- MongoDB - NoSQL database
- Mongoose - MongoDB object modeling

### Hardware Integration
- NodeMCU (ESP8266) - For fingerprint scanner integration
- Fingerprint sensor module

## Database Concepts Covered

1. **NoSQL Database Design**
   - Document-oriented data modeling
   - Schema design for MongoDB
   - Relationships between collections

2. **Data Normalization**
   - Appropriate level of normalization for NoSQL
   - Reference and embedded document patterns

3. **CRUD Operations**
   - Create, Read, Update, Delete operations
   - Query optimization

4. **Database Migration**
   - MySQL to MongoDB migration utilities
   - Data transformation and mapping

## Setup Instructions

### Prerequisites
- Node.js (v14 or higher)
- MongoDB (v4.4 or higher)
- npm (v6 or higher)

### Frontend Setup
1. Navigate to the frontend directory:
   ```
   cd Persona/frontend
   ```

2. Install dependencies:
   ```
   npm install
   ```

3. Start the development server:
   ```
   npm run dev
   ```

4. The application will be available at `http://localhost:5000`

### Backend Setup
1. Navigate to the server directory:
   ```
   cd Persona/server
   ```

2. Install dependencies:
   ```
   npm install
   ```

3. Create a `.env` file in the server directory with the following variables:
   ```
   MONGODB_URI=mongodb://localhost:27017/persona
   PORT=5000
   API_KEY=your_google_generative_ai_api_key
   JWT_SECRET=your_jwt_secret
   ```

4. Start the server:
   ```
   npm start
   ```

### Hardware Setup (Optional - for fingerprint functionality)
1. Install PlatformIO extension in VSCode
2. Open the NodeMCUScanner directory in VSCode
3. Configure the WiFi credentials in the code
4. Flash the code to your NodeMCU device
5. Connect the fingerprint sensor according to the pin configuration in the code
6. For setting up the NodeMCU fingerprint scanner, follow the video at https://www.youtube.com/watch?v=v-t8AFjW08M

## Accessing the System
- Default admin credentials:
  - Username: admin
  - Password: admin123

