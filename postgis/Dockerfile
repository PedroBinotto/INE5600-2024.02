# Base image for PostgreSQL and PostGIS
FROM postgis/postgis:latest

# Install required tools
RUN apt-get update && apt-get install -y \
    postgresql-client \
    postgis \
    gdal-bin \
    && rm -rf /var/lib/apt/lists/*

# Set environment variables for PostgreSQL
ENV POSTGRES_USER=usuario
ENV POSTGRES_PASSWORD=senha
ENV POSTGRES_DB=estados_db

# Copy the shapefile data to the container
COPY ./estados /data/estados

# Command to wait for PostgreSQL to be ready and then import shapefile
CMD ["sh", "-c", "until pg_isready -U $POSTGRES_USER -h db; do echo waiting for postgres; sleep 2; done; \
    shp2pgsql -I -s 4326 /data/estados/estados_2010.shp public.estados | psql -U $POSTGRES_USER -d $POSTGRES_DB -h db"]
