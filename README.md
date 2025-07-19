# Automation Platform Setup Guide

This guide will help you set up and run the complete Automation Platform stack, including backend services, database, frontend, and supporting services.

## Prerequisites

Before starting, ensure you have the following installed:
- Node.js (v16 or higher)
- npm or yarn
- Docker
- Git

## Architecture Overview

This project consists of several services:
- **Primary Backend** - Main application server and API
- **Frontend** - User interface
- **Database** - PostgreSQL database
- **Hooks** - Webhook processing service
- **Processor** - Data processing with Kafka
- **Worker** - Background job processing

## Quick Start

Follow these steps in order to get the entire system running:

### 1. Database Setup

First, start the PostgreSQL database using Docker:

```bash
docker run -p 5432:5432 -e POSTGRES_PASSWORD=mysecretpassword postgres
```

### 2. Primary Backend Service

Navigate to the primary backend directory and set it up:

```bash
cd primary-backend
npm install
```

Run database migrations:
```bash
npx prisma migrate dev
```

Generate Prisma client:
```bash
npx prisma generate
```

Seed the database with initial data:
```bash
npx prisma db seed
```

Start the primary backend server:
```bash
npm run dev
```

The primary backend will typically run on `http://localhost:3000` or similar.

### 3. Frontend Service

In a new terminal, navigate to the frontend directory:

```bash
cd frontend
npm install
npm run dev
```

The frontend will typically run on `http://localhost:3001` or similar.

### 4. Hooks Service

In a new terminal, navigate to the hooks service directory:

```bash
cd hooks
npm install
```

Generate Prisma client:
```bash
npx prisma generate
```

Start the hooks service:
```bash
npm run dev
```

### 5. Processor Service with Kafka

First, start Kafka using Docker:

```bash
docker run -p 9092:9092 -d apache/kafka:4.0.0
```

In a new terminal, navigate to the processor service directory:

```bash
cd processor
npm install
```

Generate Prisma client:
```bash
npx prisma generate
```

Start the processor service:
```bash
npm run dev
```

### 6. Worker Service

In a new terminal, navigate to the worker service directory:

```bash
cd worker
npm install
```

Generate Prisma client:
```bash
npx prisma generate
```

Start the worker service:
```bash
npm run dev
```



## Development Workflow

1. **Start Infrastructure**: Always start Docker services (PostgreSQL, Kafka) first
2. **Primary Backend First**: Start the primary backend service after running migrations
3. **Other Services**: Start remaining services in any order
4. **Frontend Last**: Start the frontend service last to ensure API connectivity

## Troubleshooting

### Common Issues

**Database Connection Issues**
- Ensure PostgreSQL container is running: `docker ps`
- Check if port 5432 is available
- Verify DATABASE_URL in .env files

**Prisma Issues**
- Run `npx prisma generate` after any schema changes
- Run `npx prisma migrate dev` for new migrations
- Reset database if needed: `npx prisma migrate reset`

**Kafka Connection Issues**
- Ensure Kafka container is running: `docker ps`
- Check if port 9092 is available
- Wait a few seconds for Kafka to fully start before starting processor service

**Port Conflicts**
- Check which ports are in use: `netstat -an | grep LISTEN`
- Modify port configurations in respective service configs

## Stopping Services

To stop all services:

1. Stop all npm processes (Ctrl+C in each terminal)
2. Stop Docker containers:
```bash
docker stop $(docker ps -q)
```

## Additional Commands

### Database Management
```bash
# View database in Prisma Studio
npx prisma studio

# Reset database
npx prisma migrate reset

# Deploy migrations to production
npx prisma migrate deploy
```

### Docker Management
```bash
# View running containers
docker ps

# Stop specific container
docker stop <container_id>

# Remove containers
docker rm $(docker ps -aq)
```

## Project Structure

```
Automation-Platform/
├── primary-backend/   # Main API server
├── frontend/          # User interface
├── hooks/             # Webhook processing
├── processor/         # Kafka data processing
├── worker/            # Background jobs
└── README.md         # This file
```

## Support

If you encounter issues during setup:
1. Check that all prerequisites are installed
2. Verify Docker is running
3. Ensure all ports are available
4. Check environment variables are properly configured
5. Review service logs for specific error messages
