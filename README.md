# 🚀 ERP CRM Accounting & Invoicing Software

<div align="center">
  <h1>Complete Business Management Solution</h1>
  
  <p><strong>Open Source ERP/CRM System with Invoice, Payment, Quote & Customer Management</strong></p>
  
  <p>A comprehensive business management platform built with modern technologies to streamline operations, manage customer relationships, and handle financial processes efficiently.</p>
</div>
  
  [![License: AGPL-3.0](https://img.shields.io/badge/License-AGPL%203.0-green.svg)](https://opensource.org/licenses/AGPL-3.0)
  [![Node.js](https://img.shields.io/badge/Node.js-18.x-brightgreen.svg)](https://nodejs.org/)
  [![React](https://img.shields.io/badge/React-18.x-blue.svg)](https://reactjs.org/)
  [![MongoDB](https://img.shields.io/badge/MongoDB-6.x-green.svg)](https://www.mongodb.com/)
  [![Express.js](https://img.shields.io/badge/Express.js-4.x-black.svg)](https://expressjs.com/)
  [![Ant Design](https://img.shields.io/badge/Ant%20Design-5.x-blue.svg)](https://ant.design/)
  
  <p>Built with ❤️ using the MERN Stack (MongoDB, Express.js, React.js, Node.js)</p>
  
  [Live Demo](https://cloud.idurarapp.com) • [Documentation](https://github.com/idurar/idurar-erp-crm) • [Report Bug](https://github.com/PremPMehta/ERP-CRM-Accounting-Invoicing-Software/issues)
</div>

---

## 📋 Table of Contents

- [✨ Features](#-features)
- [🛠️ Tech Stack](#️-tech-stack)
- [🚀 Quick Start](#-quick-start)
- [📦 Installation](#-installation)
- [🔧 Configuration](#-configuration)
- [🏗️ Project Structure](#️-project-structure)
- [📱 Screenshots](#-screenshots)
- [🤝 Contributing](#-contributing)
- [📄 License](#-license)
- [🙏 Acknowledgments](#-acknowledgments)

---

## ✨ Features

### 🏢 **Core Business Management**
- **📊 Invoice Management** - Create, edit, and track professional invoices
- **💰 Payment Management** - Monitor payments, transactions, and financial reports
- **📋 Quote Management** - Generate and manage customer quotes
- **👥 Customer Management** - Complete CRM with customer profiles and history
- **📈 Analytics Dashboard** - Real-time business insights and reports

### 🎨 **Modern User Interface**
- **🎯 Ant Design Framework** - Professional and responsive UI components
- **📱 Mobile Responsive** - Works seamlessly on all devices
- **🌙 Dark/Light Theme** - Customizable appearance
- **⚡ Fast Performance** - Optimized for speed and efficiency

### 🔧 **Technical Excellence**
- **🔒 Secure Authentication** - JWT-based user authentication
- **📊 Real-time Updates** - Live data synchronization
- **🔄 RESTful API** - Clean and well-documented API endpoints
- **📦 Modular Architecture** - Scalable and maintainable codebase

---

## 🛠️ Tech Stack

### **Frontend**
- **React.js 18** - Modern UI library
- **Ant Design 5** - Professional UI components
- **Redux Toolkit** - State management
- **React Router** - Navigation
- **Axios** - HTTP client
- **Vite** - Build tool

### **Backend**
- **Node.js 18** - JavaScript runtime
- **Express.js 4** - Web framework
- **MongoDB 6** - NoSQL database
- **Mongoose** - ODM for MongoDB
- **JWT** - Authentication
- **Bcrypt** - Password hashing

### **Development Tools**
- **ESLint** - Code linting
- **Prettier** - Code formatting
- **Git** - Version control
- **Docker** - Containerization (optional)

---

## 🚀 Quick Start

### Prerequisites
- Node.js 18+ 
- MongoDB 6+
- Git

### 1. Clone the Repository
```bash
git clone https://github.com/PremPMehta/ERP-CRM-Accounting-Invoicing-Software.git
cd ERP-CRM-Accounting-Invoicing-Software
```

### 2. Install Dependencies
```bash
# Install backend dependencies
cd backend
npm install

# Install frontend dependencies
cd ../frontend
npm install
```

### 3. Configure Environment
```bash
# Copy environment files
cp backend/.env.example backend/.env
cp frontend/.env.example frontend/.env
```

### 4. Start the Application
```bash
# Start backend server (from backend directory)
npm run dev

# Start frontend server (from frontend directory)
npm run dev
```

Visit `http://localhost:3000` to access the application!

---

## 📦 Installation

For detailed installation instructions, see our [Installation Guide](INSTALLATION-INSTRUCTIONS.md).

### Step-by-Step Setup

1. **Database Setup**
   - Create MongoDB Atlas account
   - Set up database cluster
   - Get connection string

2. **Environment Configuration**
   - Configure database URI
   - Set JWT secret
   - Configure email settings

3. **Application Setup**
   - Run database migrations
   - Create admin user
   - Configure initial settings

---

## 🔧 Configuration

### Environment Variables

#### Backend (.env)
```env
PORT=5000
MONGODB_URI=mongodb://localhost:27017/erp-crm
JWT_SECRET=your-secret-key
NODE_ENV=development
```

#### Frontend (.env)
```env
VITE_API_URL=http://localhost:5000/api
VITE_APP_NAME=ERP CRM
```

### Database Configuration
- MongoDB connection string
- Database name and collections
- Index optimization

---

## 🏗️ Project Structure

```
ERP-CRM-Accounting-Invoicing-Software/
├── 📁 backend/                 # Node.js/Express.js backend
│   ├── 📁 src/
│   │   ├── 📁 controllers/     # Route controllers
│   │   ├── 📁 models/         # MongoDB models
│   │   ├── 📁 routes/         # API routes
│   │   ├── 📁 middleware/     # Custom middleware
│   │   └── 📁 utils/          # Utility functions
│   ├── package.json
│   └── .env
├── 📁 frontend/               # React.js frontend
│   ├── 📁 src/
│   │   ├── 📁 components/     # React components
│   │   ├── 📁 pages/         # Page components
│   │   ├── 📁 store/         # Redux store
│   │   ├── 📁 utils/         # Utility functions
│   │   └── 📁 assets/        # Static assets
│   ├── package.json
│   └── index.html
├── 📁 doc/                    # Documentation
├── 📁 features/               # Feature documentation
└── README.md
```

---

## 📱 Screenshots

<div align="center">
  <img src="image.png" alt="ERP CRM Dashboard" width="800"/>
  
  *Modern dashboard with comprehensive business overview*
</div>

### Key Features Showcase
- **📊 Dashboard** - Real-time business metrics
- **📄 Invoices** - Professional invoice creation
- **👥 Customers** - Complete customer management
- **💰 Payments** - Payment tracking and reports
- **📋 Quotes** - Quote generation and management

---

## 🤝 Contributing

We welcome contributions! Please see our [Contributing Guide](CONTRIBUTING.md) for details.

### How to Contribute

1. **Fork the repository**
2. **Create a feature branch** (`git checkout -b feature/AmazingFeature`)
3. **Commit your changes** (`git commit -m 'Add some AmazingFeature'`)
4. **Push to the branch** (`git push origin feature/AmazingFeature`)
5. **Open a Pull Request**

### Development Guidelines

- Follow the existing code style
- Add tests for new features
- Update documentation as needed
- Ensure all tests pass

---

## 📄 License

This project is licensed under the **GNU Affero General Public License v3.0** - see the [LICENSE](LICENSE) file for details.

### Commercial Use
✅ **Yes, you can use this software for commercial purposes** - it's completely free for both personal and commercial use.

---

---

<div align="center">
  <p><strong>Built upon and refined with ❤️ by Prem Mehta</strong></p>
  
  <small>Based on [IDURAR ERP CRM](https://github.com/idurar/idurar-erp-crm) • Powered by React.js, Express.js, MongoDB & Ant Design</small>
</div>

