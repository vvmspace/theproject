# Containers

We use Docker Compose for containerization.

## Docker Compose files

Docker Compose files are stored in `docker` directory in the root of the project.

## Volumes

All volumes are stored in physical `docker/volumes` directory in the container and mapped to the host system.

`docker/volumes` should contain `.gitkeep` file and be added to `.gitignore`

## Environment variables

Docker Compose files use environment variables from `.env` files in the root of the project.