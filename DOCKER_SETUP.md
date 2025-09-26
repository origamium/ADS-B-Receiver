# Docker Setup Guide for ADS-B Receiver

This guide explains the improved Docker Compose configuration for the ADS-B Receiver project.

## Key Improvements

### 1. Network Configuration
- **Custom Network**: All services now run on a dedicated `adsb-network` bridge network
- **Configurable Subnet**: Network subnet can be customized via `ADSB_NETWORK_SUBNET` environment variable
- **Service Isolation**: Better network isolation between ADS-B services and other containers on the host

### 2. Restart Policies
- **Consistent Policy**: All services now use the same configurable restart policy
- **Production Ready**: Default restart policy is `unless-stopped` which is more appropriate for production
- **Customizable**: Restart policy can be changed via `RESTART_POLICY` environment variable

### 3. Volume Management
- **Named Volumes**: Critical data volumes are now managed as Docker named volumes
- **Configurable Paths**: Volume mount paths can be customized via environment variables
- **Better Permissions**: Improved device mounting with proper read/write permissions

### 4. Health Checks
- **Service Health**: Health checks added for critical services (ultrafeeder, fr24, piaware)
- **Dependency Management**: Services wait for their dependencies to be healthy before starting
- **Better Monitoring**: Health status can be monitored via `docker compose ps`

### 5. Resource Management
- **Memory Limits**: Resource limits set for each service to prevent memory issues
- **Production Optimization**: Memory reservations ensure proper resource allocation

### 6. Port Configuration
- **Configurable Ports**: All exposed ports can be customized via environment variables
- **Conflict Resolution**: Easy to resolve port conflicts in complex environments

## Setup Instructions

### 1. Copy Environment File
```bash
cp .env.example .env
```

### 2. Edit Configuration
Edit the `.env` file with your specific values:
- Set your geographic location (FEEDER_LAT, FEEDER_LONG, FEEDER_ALT_M)
- Configure your SDR device serial number
- Add your feeder keys from various services
- Customize ports if needed to avoid conflicts

### 3. Start Services
```bash
docker compose up -d
```

### 4. Monitor Health
```bash
docker compose ps
docker compose logs -f ultrafeeder
```

## Environment Variables

### Required Variables
- `FEEDER_LAT`: Your latitude (e.g., 35.6762)
- `FEEDER_LONG`: Your longitude (e.g., 139.6503)
- `FEEDER_ALT_M`: Your altitude in meters (e.g., 10)
- `ADSB_SDR_SERIAL`: Your SDR device serial number

### Service Keys (obtain from respective services)
- `FR24_KEY`: FlightRadar24 sharing key
- `PIAWARE_KEY`: FlightAware feeder ID
- `AIRNAV_KEY`: AirNav RadarBox sharing key
- `PLANEFINDER_KEY`: PlaneFinder share code

### Optional Customization
- `RESTART_POLICY`: Container restart policy (default: unless-stopped)
- `ADSB_NETWORK_SUBNET`: Custom network subnet (default: 172.20.0.0/16)
- Port variables: Customize exposed ports if you have conflicts

## Advanced Configuration

### Custom Network Subnet
If the default network subnet conflicts with your existing networks:
```bash
ADSB_NETWORK_SUBNET=192.168.100.0/24
```

### Custom Volume Paths
To store data in specific locations:
```bash
ULTRAFEEDER_GLOBE_HISTORY_PATH=/custom/path/globe_history
ULTRAFEEDER_GRAPHS1090_PATH=/custom/path/graphs1090
```

### Resource Tuning
The current resource limits are conservative. For systems with limited memory, you can modify the Docker Compose file to reduce memory limits, or for high-performance systems, increase them.

## Troubleshooting

### Check Service Health
```bash
docker compose ps
```

### View Logs
```bash
docker compose logs ultrafeeder
docker compose logs fr24
```

### Restart Services
```bash
docker compose restart ultrafeeder
```

### Network Issues
If you experience network connectivity issues:
```bash
docker network ls
docker network inspect adsb-network
```

## Migration from Previous Version

If you're upgrading from the previous Docker Compose configuration:

1. **Backup Data**: Your data volumes will be preserved, but backup important configurations
2. **Update Environment**: Copy `.env.example` to `.env` and configure your settings
3. **Restart Stack**: `docker compose down && docker compose up -d`

The improved configuration maintains compatibility with existing data while providing better flexibility and reliability.