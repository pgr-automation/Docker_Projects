


```yaml
version: '3.8' # Version of Docker Compose syntax

services:
  app:
    image: myapp:latest             # Image to use for this container
    container_name: my_app_container # Assigns a name to the container
    build:                           # Building from Dockerfile
      context: ./app                 # Path to Dockerfile directory
      dockerfile: Dockerfile         # Dockerfile name (optional)
      args:                          # Build arguments (optional)
        - APP_ENV=production
    ports:
      - "8080:80"                    # Map host port 8080 to container port 80
    environment:                     # Set environment variables
      - APP_ENV=production
      - API_KEY=${API_KEY}           # Use an environment variable from host
    volumes:
      - ./app/data:/data             # Mount a host path to a container path
      - myapp_cache:/cache           # Named volume
    networks:
      - frontend                     # Network configuration (defined below)
      - backend
    depends_on:                      # Set container dependencies
      - db
    restart: always                  # Restart policy (e.g., "always", "on-failure")
    logging:                         # Logging options
      driver: "json-file"
      options:
        max-size: "10m"
        max-file: "3"
    healthcheck:                     # Healthcheck options
      test: ["CMD-SHELL", "curl -f http://localhost || exit 1"]
      interval: 1m30s
      timeout: 10s
      retries: 3
      start_period: 40s

  db:
    image: postgres:13               # Use the official PostgreSQL image
    container_name: my_db_container
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
      POSTGRES_DB: mydatabase
    volumes:
      - db_data:/var/lib/postgresql/data # Persistent storage
    networks:
      - backend
    restart: on-failure
    ports:
      - "5432:5432"
    depends_on:
      - redis
    healthcheck:
      test: ["CMD", "pg_isready", "-U", "user"]
      interval: 30s
      timeout: 10s
      retries: 5

  redis:
    image: redis:alpine
    container_name: my_redis_container
    ports:
      - "6379:6379"
    networks:
      - backend
    restart: unless-stopped

networks:
  frontend:
    driver: bridge                   # Bridge network for frontend
    ipam:                            # IP Address Management (optional)
      driver: default
      config:
        - subnet: "192.168.1.0/24"
  backend:
    driver: bridge                   # Bridge network for backend
    ipam:
      driver: default
      config:
        - subnet: "192.168.2.0/24"

volumes:
  db_data:                           # Named volume for PostgreSQL data
  myapp_cache:                       # Named volume for app cache
``` 

### Explanation of Key Options

    version: Specifies the version of Docker Compose file syntax to use.
    services: Defines each service/container.
        image: The image to use for the container.
        container_name: Specifies the container name.
        build: Allows building from a Dockerfile, with optional context (location), dockerfile name, and args (build arguments).
        ports: Maps host ports to container ports.
        environment: Sets environment variables in the container.
        volumes: Mounts volumes or directories, either as host paths or named volumes.
        networks: Configures networks for the container.
        depends_on: Defines service dependencies, controlling the startup order of services.
        restart: Configures the restart policy for containers (e.g., always, on-failure, unless-stopped).
        logging: Configures logging options, such as max-size and max-file to limit log size.
        healthcheck: Defines health check commands, intervals, timeouts, and retries.
    networks: Defines networks for inter-service communication.
        driver: Specifies the network driver, such as bridge.
        ipam: Manages network address spaces with subnets.
    volumes: Defines named volumes for persistent storage.

This configuration file provides a thorough look at the main options available in Docker Compose for defining services, networks, volumes, and container configurations. Customize it as needed for your application.