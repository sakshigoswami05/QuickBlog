# ✍️ QuickBlog

> A full-stack AI-powered blogging platform built with the MERN stack.

**[🌐 Live Demo](https://quickblog-gs.vercel.app/)** · **[💻 Source Code](https://github.com/sakshigoswami05/QuickBlog)**

---

## 📖 Overview

**QuickBlog** is a full-stack blogging platform that combines a modern React frontend with a Node.js and Express backend to provide a complete blog management workflow.

The platform allows an administrator to create, manage, publish, and delete blog posts, generate blog content using **Google Gemini AI**, upload and optimize blog images using **ImageKit**, and manage user comments through a dedicated admin dashboard.

The application follows a client-server architecture with **MongoDB** as the database and exposes RESTful APIs for communication between the frontend and backend.

---

## ✨ Key Features

### 📝 Blog Management

* Create and publish blog posts
* Add blog title, subtitle, description and category
* Upload blog cover images
* Edit and manage published content
* Delete existing blogs
* Toggle blog publication status
* View individual blog posts
* Display only published blogs to visitors

### 🤖 AI-Powered Content Generation

QuickBlog integrates **Google Gemini** to assist with blog creation.

The administrator can provide a topic or prompt, and the backend sends it to the Gemini API to generate blog content.

```text
Topic / Prompt
      ↓
React Frontend
      ↓
REST API
      ↓
Express Backend
      ↓
Google Gemini API
      ↓
Generated Blog Content
      ↓
Editor
```

This reduces the effort required to create initial blog drafts while keeping the administrator in control of the final content.

### 🖼️ Image Upload & Optimization

Blog images are uploaded through the backend and stored using **ImageKit**.

The application also applies image transformations before serving the image:

* Automatic quality optimization
* WebP conversion
* Width optimization to 1280px

This helps reduce unnecessary image size while maintaining visual quality.

### 🔐 Admin Authentication

The application provides protected admin functionality using **JWT authentication**.

The authentication flow is:

```text
Admin Login
     ↓
Credentials Validation
     ↓
JWT Token Generated
     ↓
Token Stored by Client
     ↓
Authorization Header
     ↓
Protected API Routes
```

Protected operations include:

* Adding blogs
* Deleting blogs
* Publishing/unpublishing blogs
* Viewing all blogs in the dashboard
* Managing comments
* Generating AI content
* Accessing dashboard statistics

### 💬 Comment Management

Visitors can submit comments on blog posts.

Comments are stored in MongoDB and require administrator approval before appearing publicly.

```text
Visitor submits comment
          ↓
Comment stored
          ↓
Admin reviews comment
          ↓
    ┌─────┴─────┐
    ↓           ↓
 Approve      Delete
    ↓
Visible publicly
```

### 📊 Admin Dashboard

The admin dashboard provides an overview of the platform, including:

* Total number of blogs
* Total comments
* Number of drafts
* Recently created blogs
* Blog management
* Comment management

---

# 🏗️ System Architecture

QuickBlog follows a **MERN-based client-server architecture**.

```text
                    ┌──────────────────────┐
                    │      React Client    │
                    │                      │
                    │  Pages & Components  │
                    │  Context Management  │
                    │  Axios API Calls     │
                    └──────────┬───────────┘
                               │
                               │ HTTP / REST API
                               ▼
                    ┌──────────────────────┐
                    │   Node.js + Express  │
                    │                      │
                    │ Routes & Controllers │
                    │ Authentication       │
                    │ Business Logic       │
                    └───────┬──────┬───────┘
                            │      │
              ┌─────────────┘      └──────────────┐
              ▼                                   ▼
     ┌──────────────────┐                ┌─────────────────┐
     │     MongoDB      │                │ External APIs   │
     │                  │                │                 │
     │ Blogs            │                │ Gemini AI       │
     │ Comments         │                │ ImageKit        │
     └──────────────────┘                └─────────────────┘
```

---

# 🧩 Application Architecture

The project is divided into two major applications:

### Frontend

The frontend is responsible for:

* User interface
* Blog browsing
* Blog details
* Admin pages
* Form handling
* API communication
* Authentication state
* Client-side routing

### Backend

The backend handles:

* REST API endpoints
* Authentication
* Blog operations
* Comment operations
* Database communication
* Image uploads
* AI content generation
* Admin dashboard data

---

# 🔄 Core Workflows

## 1. Creating a Blog

```text
Admin
  ↓
Add Blog
  ↓
Enter Blog Details
  ↓
Upload Cover Image
  ↓
Image sent to Express API
  ↓
ImageKit Upload
  ↓
Optimized Image URL
  ↓
Blog stored in MongoDB
  ↓
Published / Saved as Draft
```

---

## 2. AI Blog Generation

QuickBlog integrates Google's Gemini model to generate blog content.

The backend receives a prompt and sends it to the Gemini API.

```javascript
const response = await ai.models.generateContent({
    model: "gemini-2.0-flash",
    contents: prompt,
});
```

The generated content is then returned to the frontend and can be used while creating a blog.

---

## 3. Authentication

Protected operations use JWT-based authentication.

When an administrator successfully logs in:

```text
Email + Password
       ↓
Credential Validation
       ↓
JWT Token
       ↓
Client Storage
       ↓
Authorization Header
       ↓
Protected Backend Route
```

The backend middleware validates the token before allowing access to protected resources.

---

## 4. Comment Approval

Comments are not immediately displayed publicly.

Instead:

```text
Comment Submitted
       ↓
Stored in MongoDB
       ↓
Admin Review
       ↓
Approved?
   ↙         ↘
 Yes         No
  ↓           ↓
Published    Remains Hidden
```

This provides moderation control over user-generated content.

---

# 🗄️ Data Models

The application uses MongoDB through **Mongoose**.

### Blog

A blog stores information such as:

* Title
* Subtitle
* Description
* Category
* Image
* Publication status
* Creation date

### Comment

A comment contains information such as:

* Associated blog
* Commenter's name
* Comment content
* Approval status
* Creation date

The comment-blog relationship allows comments to be retrieved for individual blog posts and managed from the admin dashboard.

---

# 🔌 REST API

The backend exposes RESTful endpoints for blog and admin operations.

### Blog APIs

| Method | Endpoint                   | Purpose                  |
| :----: | -------------------------- | ------------------------ |
| `POST` | `/api/blog/add`            | Create a blog            |
|  `GET` | `/api/blog/all`            | Fetch published blogs    |
|  `GET` | `/api/blog/:blogId`        | Fetch a specific blog    |
| `POST` | `/api/blog/delete`         | Delete a blog            |
| `POST` | `/api/blog/toggle-publish` | Publish/unpublish a blog |
| `POST` | `/api/blog/add-comment`    | Add a comment            |
| `POST` | `/api/blog/comments`       | Fetch approved comments  |
| `POST` | `/api/blog/generate`       | Generate AI blog content |

### Admin APIs

| Method | Endpoint                     | Purpose                    |
| :----: | ---------------------------- | -------------------------- |
| `POST` | `/api/admin/login`           | Admin authentication       |
|  `GET` | `/api/admin/blogs`           | Fetch all blogs            |
|  `GET` | `/api/admin/comments`        | Fetch all comments         |
|  `GET` | `/api/admin/dashboard`       | Fetch dashboard statistics |
| `POST` | `/api/admin/approve-comment` | Approve a comment          |
| `POST` | `/api/admin/delete-comment`  | Delete a comment           |

Protected endpoints require valid authentication.

---

# 🛠️ Technology Stack

### Frontend

* **React 19**
* **React Router**
* **Tailwind CSS**
* **Axios**
* **Quill**
* **Marked**
* **React Hot Toast**
* **Motion**

### Backend

* **Node.js**
* **Express.js**
* **Mongoose**
* **JWT**
* **Multer**

### Database & Services

* **MongoDB**
* **Google Gemini API**
* **ImageKit**

### Deployment

* **Vercel**

---

# 📁 Project Structure

```text
QuickBlog/
│
├── client/
│   │
│   ├── public/
│   │
│   └── src/
│       ├── assets/
│       ├── components/
│       │   ├── admin/
│       │   ├── BlogCard.jsx
│       │   ├── BlogList.jsx
│       │   ├── Footer.jsx
│       │   ├── Header.jsx
│       │   ├── Loader.jsx
│       │   ├── Navbar.jsx
│       │   └── Newsletter.jsx
│       │
│       ├── context/
│       │   └── AppContext.jsx
│       │
│       ├── pages/
│       │   ├── Blog.jsx
│       │   ├── Home.jsx
│       │   └── admin/
│       │       ├── AddBlog.jsx
│       │       ├── Comments.jsx
│       │       ├── Dashboard.jsx
│       │       ├── Layout.jsx
│       │       └── ListBlog.jsx
│       │
│       ├── App.jsx
│       ├── index.css
│       └── main.jsx
│
├── server/
│   ├── configs/
│   │   ├── db.js
│   │   ├── gemini.js
│   │   └── imageKit.js
│   │
│   ├── controllers/
│   │   ├── adminController.js
│   │   └── blogController.js
│   │
│   ├── middleware/
│   │   ├── auth.js
│   │   └── multer.js
│   │
│   ├── models/
│   │   ├── Blog.js
│   │   └── Comment.js
│   │
│   ├── routes/
│   │   ├── adminRoutes.js
│   │   └── blogRoutes.js
│   │
│   └── server.js
│
└── README.md
```

---

# ⚙️ Getting Started

## Prerequisites

Make sure the following are installed:

* Node.js
* npm
* MongoDB
* Git

You will also need API credentials for:

* Google Gemini
* ImageKit

---

## Installation

### Clone the repository

```bash
git clone https://github.com/sakshigoswami05/QuickBlog.git
cd QuickBlog
```

### Install frontend dependencies

```bash
cd client
npm install
```

### Install backend dependencies

```bash
cd ../server
npm install
```

---

## 🔐 Environment Variables

Create a `.env` file inside the `server` directory with the required configuration for:

```text
MONGODB_URI
JWT_SECRET
ADMIN_EMAIL
ADMIN_PASSWORD
GEMINI_API_KEY
IMAGEKIT_PUBLIC_KEY
IMAGEKIT_PRIVATE_KEY
IMAGEKIT_URL_ENDPOINT
```

Create the frontend environment configuration with the backend API base URL:

```text
VITE_BASE_URL
```

> Keep API keys, database credentials and authentication secrets private. Never commit `.env` files to GitHub.

---

# ▶️ Running the Project

### Start the backend

```bash
cd server
npm start
```

### Start the frontend

Open another terminal:

```bash
cd client
npm run dev
```

The Vite development server will provide the frontend locally.

---

# 🌐 Live Application

🚀 [**Open QuickBlog**](https://quickblog-gs.vercel.app/)

💻 [**View the Source Code**](https://github.com/sakshigoswami05/QuickBlog)

---

# 💡 Key Technical Highlights

### 1. Full-Stack Architecture

Designed and implemented a complete client-server application using the MERN stack, connecting a React frontend with RESTful Express APIs and MongoDB.

### 2. JWT-Based Authorization

Implemented protected backend routes using JSON Web Tokens to restrict administrative operations.

### 3. AI Integration

Integrated Google's Gemini API into the backend to generate blog content from administrator-provided prompts.

### 4. Cloud Image Management

Integrated ImageKit for image uploads, optimization and delivery with automatic WebP conversion and resizing.

### 5. Content Moderation

Implemented an approval workflow that allows administrators to review comments before they become publicly visible.

### 6. Admin Dashboard

Built a dedicated administrative interface for managing blogs, comments, drafts and platform statistics.

### 7. Centralized Client State

Used React Context to maintain shared application state such as authentication tokens, blogs and API configuration across components.

---

# 🔮 Future Enhancements

Potential improvements include:

* 🤖 AI-powered SEO suggestions
* 📝 AI-assisted blog summarization
* 🔍 Advanced blog search
* 🏷️ Category and tag filtering
* ❤️ Like and bookmark functionality
* 👤 User accounts and author profiles
* 📊 Detailed analytics dashboard
* 🌙 Dark mode
* 📱 Progressive Web App support
* 🔔 Notifications
* 💬 Real-time comment updates

---

# 👩‍💻 Author

### Sakshi Goswami

**B.Tech — Information Technology**
**National Institute of Technology, Raipur**

[GitHub Profile](https://github.com/sakshigoswami05)

---

<p align="center">

⭐ **If you found QuickBlog interesting, consider giving the repository a star!**

</p>
