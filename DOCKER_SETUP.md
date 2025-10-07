# Docker Setup Guide

This guide explains how to run the Air Quality Monitor application using Docker and Docker Compose.

## Prerequisites

- Docker (version 20.10 or higher)
- Docker Compose (version 2.0 or higher)

## Quick Start

### 1. Clone and Setup

```bash
git clone <repository-url>
cd air-quality-monitor
```

### 2. Environment Configuration

Copy the environment template and configure:

```bash
cp docker.env .env
```

Edit `.env` file with your configuration:

```env
# MongoDB Configuration
MONGO_ROOT_USERNAME=admin
MONGO_ROOT_PASSWORD=your_secure_password
MONGO_DATABASE=air_quality_db

# JWT Configuration
JWT_SECRET=your_super_secure_jwt_secret_key

# API Configuration
CORS_ORIGIN=http://localhost:3000
REACT_APP_API_URL=http://localhost:5000/api
```

### 3. Run the Application

#### Production Mode
```bash
docker-compose up -d
```

#### Development Mode
```bash
docker-compose -f docker-compose.dev.yml up -d
```

### 4. Access the Application

- **Frontend**: http://localhost:3000
- **Backend API**: http://localhost:5000
- **MongoDB**: localhost:27017

## Services

### MongoDB
- **Image**: mongo:7.0
- **Port**: 27017
- **Data**: Persisted in Docker volume
- **Health Check**: MongoDB ping command

### Backend API
- **Port**: 5000
- **Health Check**: Custom healthcheck endpoint
- **Dependencies**: MongoDB
- **Environment**: Production/Development

### Frontend
- **Port**: 3000
- **Server**: Nginx (production) / React dev server (development)
- **Dependencies**: Backend API
- **Health Check**: HTTP endpoint check

### Nginx (Optional)
- **Ports**: 80, 443
- **Purpose**: Reverse proxy and SSL termination
- **Dependencies**: Frontend and Backend

## Docker Commands

### Basic Commands

```bash
# Start all services
docker-compose up -d

# Start specific service
docker-compose up -d mongodb

# View logs
docker-compose logs -f

# View logs for specific service
docker-compose logs -f backend

# Stop all services
docker-compose down

# Stop and remove volumes
docker-compose down -v

# Rebuild and start
docker-compose up -d --build

# Scale services
docker-compose up -d --scale backend=2
```

### Development Commands

```bash
# Start development environment
docker-compose -f docker-compose.dev.yml up -d

# View development logs
docker-compose -f docker-compose.dev.yml logs -f

# Stop development environment
docker-compose -f docker-compose.dev.yml down
```

### Maintenance Commands

```bash
# View running containers
docker ps

# View all containers
docker ps -a

# View container logs
docker logs <container_name>

# Execute command in container
docker exec -it <container_name> /bin/sh

# View container resource usage
docker stats

# Clean up unused resources
docker system prune -a
```

## Environment Variables

### Backend Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `NODE_ENV` | production | Application environment |
| `PORT` | 5000 | Backend server port |
| `MONGODB_URI` | mongodb://admin:password123@mongodb:27017/air_quality_db?authSource=admin | MongoDB connection string |
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

## Health Checks

All services include health checks:

- **MongoDB**: `mongosh --eval "db.adminCommand('ping')"`
- **Backend**: `node healthcheck.js`
- **Frontend**: `wget --spider http://localhost:3000`
- **Nginx**: `wget --spider http://localhost`

## Troubleshooting

### Common Issues

1. **Port Already in Use**
   ```bash
   # Check what's using the port
   netstat -tulpn | grep :3000
   
   # Kill the process
   sudo kill -9 <PID>
   ```

2. **MongoDB Connection Issues**
   ```bash
   # Check MongoDB logs
   docker-compose logs mongodb
   
   # Test MongoDB connection
   docker exec -it air-quality-mongodb mongosh
   ```

3. **Backend API Issues**
   ```bash
   # Check backend logs
   docker-compose logs backend
   
   # Test API endpoint
   curl http://localhost:5000/api/health
   ```

4. **Frontend Build Issues**
   ```bash
   # Rebuild frontend
   docker-compose build frontend
   
   # Check frontend logs
   docker-compose logs frontend
   ```

### Debug Mode

Run containers in debug mode:

```bash
# Backend debug
docker-compose run --rm backend /bin/sh

# Frontend debug
docker-compose run --rm frontend /bin/sh

# MongoDB debug
docker-compose run --rm mongodb mongosh
```

## Production Deployment

### Security Considerations

1. **Change Default Passwords**
   - Update MongoDB root password
   - Use strong JWT secret
   - Configure proper CORS origins

2. **SSL/TLS Configuration**
   - Add SSL certificates to nginx/ssl/
   - Update nginx configuration for HTTPS
   - Use environment variables for SSL paths

3. **Resource Limits**
   ```yaml
   services:
     backend:
       deploy:
         resources:
           limits:
             memory: 512M
             cpus: '0.5'
   ```

### Monitoring

```bash
# View resource usage
docker stats

# View service health
docker-compose ps

# View logs with timestamps
docker-compose logs -f -t
```

## Backup and Restore

### MongoDB Backup

```bash
# Create backup
docker exec air-quality-mongodb mongodump --out /backup

# Copy backup from container
docker cp air-quality-mongodb:/backup ./mongodb-backup

# Restore backup
docker exec -i air-quality-mongodb mongorestore /backup
```

### Volume Backup

```bash
# Backup volumes
docker run --rm -v air-quality-monitor_mongodb_data:/data -v $(pwd):/backup alpine tar czf /backup/mongodb-backup.tar.gz -C /data .
```

## Cleanup

```bash
# Remove all containers and volumes
docker-compose down -v

# Remove all images
docker-compose down --rmi all

# Clean up everything
docker system prune -a --volumes
```
