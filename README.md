# OpsPilot AI

An AI-powered business operations automation platform built with n8n.

## Phase 0: Infrastructure Foundation

This is the initial infrastructure setup for OpsPilot AI, providing the foundation for building automated business operations workflows.

## Project Structure

```
opspilot-ai/
├── workflows/          # n8n workflow definitions (to be added in later phases)
├── database/           # Database schemas and migrations (to be added in later phases)
├── test-data/          # Sample data for testing (to be added in later phases)
├── docs/               # Documentation (to be added in later phases)
├── screenshots/        # Project screenshots (to be added in later phases)
├── dashboard/          # Dashboard application (to be added in later phases)
├── docker-compose.yml   # Docker Compose configuration
├── .env.example         # Environment variables template
├── .gitignore          # Git ignore rules
└── README.md           # This file
```

## Prerequisites

- Docker Desktop installed and running
- Docker Compose installed (included with Docker Desktop)

## Quick Start

### 1. Configure Environment Variables

Copy the example environment file and update it with your credentials:

```bash
cp .env.example .env
```

Edit `.env` and replace the placeholder values with secure credentials:
- `POSTGRES_PASSWORD` - Set a strong password for PostgreSQL
- `N8N_ENCRYPTION_KEY` - Generate a random 32-character string for encryption
- `N8N_BASIC_AUTH_PASSWORD` - Set a strong password for n8n authentication

**Important:** Never commit `.env` to version control. It's already included in `.gitignore`.

### 2. Start the Services

Start all services in detached mode:

```bash
docker-compose up -d
```

This will:
- Pull the required Docker images (PostgreSQL 15 Alpine and n8n latest)
- Create and start the PostgreSQL container
- Wait for PostgreSQL to be healthy
- Create and start the n8n container with PostgreSQL as its database
- Persist data using Docker volumes

### 3. Access n8n

Once the services are running, access n8n at:
- **URL**: http://localhost:5678
- **Username**: admin (or the value set in `N8N_BASIC_AUTH_USER`)
- **Password**: The password you set in `N8N_BASIC_AUTH_PASSWORD`

## Stopping the Services

To stop all services:

```bash
docker-compose down
```

To stop and remove all data (volumes):

```bash
docker-compose down -v
```

**Warning:** The `-v` flag will delete all persisted data, including your n8n workflows and PostgreSQL data. Use with caution.

## Verifying the Setup

### Check n8n

1. Open your browser and navigate to http://localhost:5678
2. Log in with the credentials from your `.env` file
3. You should see the n8n interface with an empty workflow canvas

### Check PostgreSQL

You can verify PostgreSQL is running by checking the container status:

```bash
docker-compose ps
```

Both `opspilot-postgres` and `opspilot-n8n` should show as "Up" with healthy status.

To connect to PostgreSQL directly (optional):

```bash
docker-compose exec postgres psql -U opspilot_user -d opspilot_db
```

## Data Persistence

The setup uses Docker volumes to persist data:
- `postgres_data` - PostgreSQL database files
- `n8n_data` - n8n configuration, workflows, and credentials

Data persists across container restarts and recreations unless you explicitly remove volumes with `docker-compose down -v`.

## Troubleshooting

### Services won't start

Check container logs:
```bash
docker-compose logs
```

### n8n can't connect to PostgreSQL

Ensure PostgreSQL is healthy before n8n starts. The healthcheck in `docker-compose.yml` handles this automatically. If issues persist, check:
- Environment variables in `.env` match between services
- No port conflicts on your system

### Port already in use

If port 5678 or 5432 is already in use, modify the `N8N_PORT` in `.env` or the port mapping in `docker-compose.yml`.

## Next Steps

This is Phase 0 of the OpsPilot AI project. Future phases will include:
- n8n workflow automation
- AI agent integration
- Dashboard development
- Business operations templates

## Security Notes

- Always use strong, unique passwords in production
- Never commit `.env` to version control
- Consider using secrets management in production (e.g., Docker Secrets, AWS Secrets Manager)
- Enable HTTPS in production environments
- Regularly update Docker images for security patches

## License

[To be determined]
