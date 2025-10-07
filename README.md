# Air Quality Monitor

A full-stack web application for monitoring air quality data with real-time analytics, built with React, Node.js, Express, and MongoDB.

## 🌟 Features

- **Real-time Air Quality Monitoring**: Track AQI, PM2.5, PM10, O3, NO2, SO2, and CO levels
- **Interactive Dashboard**: Beautiful, responsive UI with live data visualization
- **Advanced Analytics**: Statistical analysis and trend visualization
- **Multi-location Support**: Monitor air quality across different cities and countries
- **Health Recommendations**: Personalized advice based on current air quality conditions
- **Weather Integration**: Temperature, humidity, pressure, and wind data
- **Export Functionality**: Download data in CSV format
- **Docker Support**: Easy deployment with containerization

## 🛠️ Tech Stack

### Frontend
- **React 18** - UI framework
- **TailwindCSS** - Styling
- **React Router** - Navigation
- **Recharts** - Data visualization
- **Axios** - HTTP client
- **React Hot Toast** - Notifications

### Backend
- **Node.js** - Runtime environment
- **Express** - Web framework
- **MongoDB** - Database
- **Mongoose** - ODM
- **CORS** - Cross-origin resource sharing
- **Helmet** - Security middleware
- **Rate Limiting** - API protection

### Deployment
- **Docker** - Containerization
- **Docker Compose** - Multi-container orchestration
- **Nginx** - Reverse proxy (optional)

## 🚀 Quick Start

### Prerequisites

- Node.js 18+ 
- MongoDB 5.0+
- Docker (optional)

### Installation

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd air-quality-monitor
   ```

2. **Install dependencies**
   ```bash
   npm run install-all
   ```

3. **Set up environment variables**
   ```bash
   # Backend
   cp backend/env.example backend/.env
   # Edit backend/.env with your configuration
   
   # Frontend
   cp frontend/.env.example frontend/.env
   # Edit frontend/.env with your configuration
   ```

4. **Start MongoDB**
   ```bash
   # Using Docker
   docker run -d -p 27017:27017 --name mongodb mongo:7.0
   
   # Or install MongoDB locally
   # Follow MongoDB installation guide for your OS
   ```

5. **Start the application**
   ```bash
   # Development mode (both frontend and backend)
   npm run dev
   
   # Or start individually
   npm run server  # Backend only
   npm run client  # Frontend only
   ```

6. **Access the application**
   - Frontend: http://localhost:3000
   - Backend API: http://localhost:5000
   - API Health Check: http://localhost:5000/api/health

## 🐳 Docker Deployment

### Using Docker Compose (Recommended)

1. **Clone and navigate to the project**
   ```bash
   git clone <repository-url>
   cd air-quality-monitor
   ```

2. **Start all services**
   ```bash
   docker-compose up -d
   ```

3. **Access the application**
   - Frontend: http://localhost:3000
   - Backend API: http://localhost:5000
   - MongoDB: localhost:27017

### Using Individual Dockerfiles

1. **Build and run MongoDB**
   ```bash
   docker run -d -p 27017:27017 --name mongodb mongo:7.0
   ```

2. **Build and run Backend**
   ```bash
   cd backend
   docker build -t air-quality-backend .
   docker run -d -p 5000:5000 --name backend air-quality-backend
   ```

3. **Build and run Frontend**
   ```bash
   cd frontend
   docker build -t air-quality-frontend .
   docker run -d -p 3000:3000 --name frontend air-quality-frontend
   ```

## 📊 API Endpoints

### Air Quality Data
- `GET /api/air-quality` - Get all air quality data (with pagination and filtering)
- `GET /api/air-quality/latest` - Get latest air quality data for a location
- `GET /api/air-quality/stats` - Get air quality statistics
- `GET /api/air-quality/:id` - Get air quality data by ID
- `POST /api/air-quality` - Create new air quality data
- `PUT /api/air-quality/:id` - Update air quality data
- `DELETE /api/air-quality/:id` - Delete air quality data

### Health Check
- `GET /api/health` - Basic health check
- `GET /api/health/detailed` - Detailed health information

### Query Parameters
- `page` - Page number for pagination
- `limit` - Number of items per page
- `city` - Filter by city name
- `country` - Filter by country
- `startDate` - Filter by start date (ISO string)
- `endDate` - Filter by end date (ISO string)
- `minAqi` - Minimum AQI value
- `maxAqi` - Maximum AQI value
- `sortBy` - Sort field (default: timestamp)
- `sortOrder` - Sort order (asc/desc, default: desc)

## 🔧 Configuration

### Backend Environment Variables

```env
NODE_ENV=development
PORT=5000
MONGODB_URI=mongodb://localhost:27017/air_quality_db
JWT_SECRET=your_jwt_secret_here
API_RATE_LIMIT=100
CORS_ORIGIN=http://localhost:3000
```

### Frontend Environment Variables

```env
REACT_APP_API_URL=http://localhost:5000/api
NODE_ENV=production
```

## 📁 Project Structure

```
air-quality-monitor/
├── backend/
│   ├── controllers/          # Route controllers
│   ├── models/              # Database models
│   ├── routes/              # API routes
│   ├── scripts/             # Database scripts
│   ├── Dockerfile           # Backend Docker config
│   ├── package.json         # Backend dependencies
│   ├── server.js            # Backend entry point
│   └── env.example          # Environment variables example
├── frontend/
│   ├── public/              # Static files
│   ├── src/
│   │   ├── components/      # React components
│   │   ├── pages/           # Page components
│   │   ├── services/        # API services
│   │   ├── utils/           # Utility functions
│   │   ├── App.js           # Main App component
│   │   └── index.js         # Frontend entry point
│   ├── Dockerfile           # Frontend Docker config
│   ├── nginx.conf           # Nginx configuration
│   └── package.json         # Frontend dependencies
├── docker-compose.yml       # Docker Compose configuration
├── Dockerfile              # Main Dockerfile
├── package.json            # Root package.json
└── README.md               # This file
```

## 🧪 Sample Data

The application includes sample air quality data for demonstration:
- New York, USA
- London, UK  
- Tokyo, Japan

Sample data is automatically loaded when using Docker Compose.

## 🔒 Security Features

- **Rate Limiting**: API request rate limiting
- **CORS Protection**: Configurable cross-origin resource sharing
- **Helmet**: Security headers
- **Input Validation**: Request data validation
- **Error Handling**: Comprehensive error handling

## 📈 Performance Features

- **Database Indexing**: Optimized database queries
- **Pagination**: Efficient data loading
- **Caching**: Static asset caching
- **Compression**: Gzip compression
- **Lazy Loading**: Component lazy loading

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🆘 Support

If you encounter any issues or have questions:

1. Check the [Issues](https://github.com/your-repo/issues) page
2. Create a new issue with detailed information
3. Contact the development team

## 🔮 Future Enhancements

- [ ] Real-time notifications
- [ ] Mobile app (React Native)
- [ ] Machine learning predictions
- [ ] Historical data analysis
- [ ] Multi-language support
- [ ] Advanced filtering options
- [ ] Data export in multiple formats
- [ ] User authentication and profiles
- [ ] Custom dashboard widgets
- [ ] API rate limiting per user

---

**Built with ❤️ for better air quality monitoring**
