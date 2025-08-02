# Phalcon + FrankenPHP + Docker Compose Setup

This repository contains a Docker Compose setup for running a Phalcon application with FrankenPHP, MySQL, Redis, and MongoDB.
It includes configurations for Caddy as a reverse proxy and load balancer.

<div style="background: url('https://frankenphp.dev/img/logo_darkbg.svg'); background-size: 100%; background-repeat: no-repeat;width: 100%; height: 111px; display: flex; align-items: center; justify-content: center; color: white; font-size: 24px; font-weight: bold; background-color: rgb(35 1 67 / 1); text-align: center; padding: 20px; border-radius: 10px; padding: 10px">
</div>

# Services

- **franken**: The main service running FrankenPHP.
- **mysql**: MySQL database service.
- **mongo**: MongoDB database service.
- **caddy**: Caddy web server for serving the application and handling HTTPS.
- Structured to support:

```text
.
├── docker-compose.yml
├── caddy/                  # Caddy configuration
│   ├── sites/
│   │   ├── caddysite.vn/   # Config domain primary (index.php)
│   │   └── localhost/      # Config localhost (index.php)
│   └── Caddyfile
├── mongo_data/             # Volume for MongoDB
├── mysql_data/             # Volume for MySQL

```

# Usage

1. Clone the repository:
   ```bash
   git clone https://github.com/tuannguyen29/frankenphp-and-phalconphp.git
   cd frankenphp
   ```
2. Build and start the services:
   ```bash
   docker-compose up -d --build
   ```
3. Access the application at `http://localhost:81` or at `http://caddysite.vn:81`
4. For HTTPS, ensure you have valid SSL certificates or use Caddy's automatic HTTPS feature.
5. To stop the services, run:
   ```bash
   docker-compose down
   ```
6. Composer install: (optional, if you have a `composer.json` file in your project)
    ```bash
    docker exec -it -w /data/caddysite.vn/ franken composer install
    ```

# Configuration

- **Caddy**: The Caddyfile is located in the `caddy` directory. You can customize it to suit your needs.
- **MySQL**: The MySQL root password and user credentials can be configured in the `docker-compose.yml` file under the `mysql` service.
- **MongoDB**: The MongoDB root username and password can be set in the `docker-compose.yml` file under the `mongo` service.
- **FrankenPHP**: The configuration files for FrankenPHP are located in the `franken/config` directory. You can modify them as needed.
- **Volumes**: Data for MySQL and MongoDB is persisted in the `mysql_data` and `mongo_data` directories, respectively. You can change these paths in the `docker-compose.yml` file.
- **Networks**: All services are connected to a default network created by Docker Compose. You can customize the network settings if needed.
- **Ports**: The services expose the following ports:
  - FrankenPHP: 81 (HTTP), 443 (HTTPS)
  - Redis: 6380
  - MySQL: 3307
  - MongoDB: 27018
  - Caddy: 80, 443
  - Caddy (HTTP/3): 443/udp
  - Caddy (HTTP/2): 443
  - Caddy (HTTP/1.1): 443
  - Caddy (HTTP/0.9): 443
  - Caddy (HTTP/0.8): 443
- **User Permissions**: The Caddy service runs as user `1000:1000` to avoid permission issues with mounted volumes. You can change this in the `docker-compose.yml` file if necessary.
- **Data Directories**: The `/data` directory is mounted to persist data across container restarts. You can change this path in the `docker-compose.yml` file.
- **Build Context**: The `franken` service is built from the Dockerfile located in the `franken` directory. You can modify the Dockerfile to customize the FrankenPHP setup.
- **Container Names**: Each service has a specific container name for easier management. You can change these names in the `docker-compose.yml` file if needed.
- **Environment Variables**: Environment variables for each service can be set in the `docker-compose.yml` file. Make sure to update them according to your requirements.

# Notes

- Ensure Docker and Docker Compose are installed on your machine.
- This setup is intended for development and testing purposes. For production use, consider additional security measures and optimizations.
- If you encounter any issues, check the logs of each service using `docker-compose logs <service_name>`.
- For more information on each service, refer to their official documentation:
  - [FrankenPHP](https://frankenphp.org/)
  - [MySQL](https://www.mysql.com/)
  - [MongoDB](https://www.mongodb.com/)
  - [Caddy](https://caddyserver.com/)
  - [Phalcon](https://phalcon.io/)
  - [Docker](https://www.docker.com/)
  - [Docker Compose](https://docs.docker.com/compose/)
  - [Docker Networking](https://docs.docker.com/network/)
  - [Docker Volumes](https://docs.docker.com/storage/volumes/)
- For any contributions or issues, feel free to open a pull request or issue on the repository.
- This setup is designed to be flexible and can be adapted to various use cases. Feel free to modify the configurations as needed.
- If you want to use a different version of MySQL or MongoDB, you can change the image tags in the `docker-compose.yml` file.
- The `docker-compose.yml` file is structured to allow easy addition of more services or modifications to existing ones.
- The `franken` service is set to run as a non-root user for security purposes. You can adjust the user settings in the Dockerfile if necessary.
- The Caddy service is configured to handle both HTTP and HTTPS traffic, with automatic HTTPS enabled by default. You can customize the Caddyfile to change the routing and SSL settings.
- The MySQL and MongoDB services are configured to restart automatically in case of failures, ensuring high availability during development.
- The volumes for MySQL and MongoDB are mapped to local directories to persist data across container restarts. You can change these paths in the `docker-compose.yml` file if you prefer a different location.
- The setup is designed to be modular, allowing you to easily add or remove services as needed. You can also extend the functionality by integrating additional services like Redis, Elasticsearch, or others as required by your application.

# Image
![FrankenPHP and PhalconPHP Docker Compose Setup](https://raw.githubusercontent.com/tuannguyen29/frankenphp-and-phalconphp/master/images/frankenphp-phalconphp-docker-compose.png)
![FrankenPHP and PhalconPHP Docker Compose Setup](https://raw.githubusercontent.com/tuannguyen29/frankenphp-and-phalconphp/master/images/frankenphp-phalconphp-docker-compose-2.png)
![FrankenPHP and PhalconPHP Docker Compose Setup](https://raw.githubusercontent.com/tuannguyen29/frankenphp-and-phalconphp/master/images/frankenphp-phalconphp-docker-compose-3.png)
