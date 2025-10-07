# 🐳 Docker Setup for Air Quality Monitor

This project includes comprehensive Docker configuration for running the full-stack Air Quality Monitor application.

## 🚀 Quick Start

### Prerequisites
- Docker Desktop (Windows/Mac) or Docker Engine (Linux)
- Docker Compose v2.0+

### 1. Environment Setup
```bash
# Copy environment template
cp docker.env .env

# Edit configuration (optional)
# The .env file contains all necessary defaults
```

### 2. Start the Application

#### Production Mode
```bash
# Using PowerShell (Windows)
.\scripts\docker-start.ps1

# Using Bash (Linux/Mac)
./scripts/docker-start.sh

# Or manually
docker-compose up -d
```

#### Development Mode
```bash
# Using PowerShell (Windows)
.\scripts\docker-dev.ps1

# Using Bash (Linux/Mac)
./scripts/docker-dev.sh

# Or manually
docker-compose -f docker-compose.dev.yml up -d
```

### 3. Access the Application
- **Frontend**: http://localhost:3000
- **Backend API**: http://localhost:5000
- **MongoDB**: localhost:27017

## 📁 Docker Files Structure

```
├── Dockerfile                    # Root multi-stage build
├── docker-compose.yml           # Production configuration
├── docker-compose.dev.yml       # Development configuration
├── docker.env                   # Environment template
├── backend/
│   └── Dockerfile               # Backend service
├── frontend/
│   ├── Dockerfile               # Frontend production build
│   ├── Dockerfile.dev           # Frontend development
│   └── nginx.conf               # Nginx configuration
└── scripts/
    ├── docker-start.ps1         # Windows start script
    ├── docker-dev.ps1           # Windows dev script
    ├── docker-stop.ps1          # Windows stop script
    ├── docker-start.sh          # Linux/Mac start script
    ├── docker-dev.sh            # Linux/Mac dev script
    └── docker-stop.sh           # Linux/Mac stop script
```

## 🔧 Services

### MongoDB
- **Image**: mongo:7.0
- **Port**: 27017
- **Data**: Persisted in Docker volume
- **Health Check**: MongoDB ping command

### Backend API
- **Port**: 5000
- **Health Check**: Custom healthcheck endpoint
- **Dependencies**: MongoDB
- **Features**: JWT authentication, rate limiting, CORS

### Frontend
- **Port**: 3000
- **Server**: Nginx (production) / React dev server (development)
- **Dependencies**: Backend API
- **Features**: Hot reload (dev), optimized build (prod)

### Nginx (Optional)
- **Ports**: 80, 443
- **Purpose**: Reverse proxy and SSL termination
- **Dependencies**: Frontend and Backend

## 🌍 Environment Variables

### Backend Variables
| Variable | Default | Description |
|----------|---------|-------------|
| `NODE_ENV` | production | Application environment |
| `PORT` | 5000 | Backend server port |
| `MONGODB_URI` | mongodb://admin:password123@mongodb:27017/air_quality_db?authSource=admin | MongoDB connection |
| `JWT_SECRET` | your_jwt_secret_here_change_in_production | JWT signing secret |
| `JWT_EXPIRES_IN` | 7d | JWT token expiration |
| `API_RATE_LIMIT` | 100 | API rate limit per window |
| `CORS_ORIGIN` | http://localhost:3000 | CORS allowed origin |

### Frontend Variables
| Variable | Default | Description |
|----------|---------|-------------|
| `REACT_APP_API_URL` | http://localhost:5000/api | Backend API URL |
| `NODE_ENV` | production | Application environment |

### MongoDB Variables
| Variable | Default | Description |
|----------|---------|-------------|
| `MONGO_ROOT_USERNAME` | admin | MongoDB root username |
| `MONGO_ROOT_PASSWORD` | password123 | MongoDB root password |
| `MONGO_DATABASE` | air_quality_db | Default database name |

## 🛠️ Development Features

### Hot Reload
- Frontend changes automatically reload
- Backend changes restart the server
- Volume mounting for live code updates

### Debugging
```bash
# Access container shell
docker-compose exec backend /bin/sh
docker-compose exec frontend /bin/sh
docker-compose exec mongodb mongosh

# View logs
docker-compose logs -f backend
docker-compose logs -f frontend
docker-compose logs -f mongodb
```

## 🔒 Security Features

### Production Security
- Non-root user execution
- Proper signal handling with dumb-init
- Health checks for all services
- Resource limits and restart policies
- SSL/TLS support via Nginx

### Authentication
- JWT-based authentication
- Password hashing with bcrypt
- Role-based access control
- Rate limiting and CORS protection

## 📊 Monitoring & Health Checks

All services include comprehensive health checks:

```bash
# Check service status
docker-compose ps

# View health check logs
docker-compose logs healthcheck

# Monitor resource usage
docker stats
```

## 🧹 Maintenance

### Backup
```bash
# MongoDB backup
docker exec air-quality-mongodb mongodump --out /backup
docker cp air-quality-mongodb:/backup ./mongodb-backup

# Volume backup
docker run --rm -v air-quality-monitor_mongodb_data:/data -v $(pwd):/backup alpine tar czf /backup/mongodb-backup.tar.gz -C /data .
```

### Cleanup
```bash
# Stop and remove containers
docker-compose down

# Remove volumes
docker-compose down -v

# Clean up everything
docker system prune -a --volumes
```

## 🚨 Troubleshooting

### Common Issues

1. **Port Already in Use**
   ```bash
   # Check port usage
   netstat -tulpn | grep :3000
   
   # Kill process
   sudo kill -9 <PID>
   ```

2. **MongoDB Connection Issues**
   ```bash
   # Check MongoDB logs
   docker-compose logs mongodb
   
   # Test connection
   docker exec -it air-quality-mongodb mongosh
   ```

3. **Build Issues**
   ```bash
   # Rebuild without cache
   docker-compose build --no-cache
   
   # Clean up and rebuild
   docker-compose down
   docker system prune -f
   docker-compose up -d --build
   ```

### Debug Mode
```bash
# Run with debug output
docker-compose up --build

# Check container logs
docker logs <container_name>

# Inspect container
docker inspect <container_name>
```

## 📚 Additional Resources

- [Docker Documentation](https://docs.docker.com/)
- [Docker Compose Documentation](https://docs.docker.com/compose/)
- [MongoDB Docker Image](https://hub.docker.com/_/mongo)
- [Node.js Docker Image](https://hub.docker.com/_/node)
- [Nginx Docker Image](https://hub.docker.com/_/nginx)

## 🤝 Contributing

When contributing to the Docker setup:

1. Test both production and development modes
2. Ensure all environment variables are documented
3. Update health checks if adding new services
4. Test on different operating systems
5. Update this README with any changes

## 📄 License

This Docker setup is part of the Air Quality Monitor project and follows the same license terms.
