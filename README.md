### step 1 
write docker-compose.yml

```
services:
  pgvector:
    image: pgvector/pgvector:pg17
    container_name: zerocom-pgvector


    environment:
      POSTGRES_USER: ${POSTGRES_USER:-zerocom}
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD:-zerocompass}
      POSTGRES_DB: ${POSTGRES_DB:-zerocom}

    ports:
      - "127.0.0.1:5432:5432"

    volumes:
      - zerocom_pgvector_data:/var/lib/postgresql/data

    healthcheck:
      test:
        [
          "CMD-SHELL",
          "pg_isready -U $${POSTGRES_USER} -d $${POSTGRES_DB}"
        ]
      interval: 5s
      timeout: 5s
      retries: 5

    


volumes:
  zerocom_pgvector_data:

```

### step 2

create .env file with 
POSTGRES_USER=zerocom
POSTGRES_PASSWORD=replace_with_a_strong_local_password
POSTGRES_DB=zerocom

### step 3
start container
'docker compose up -d'

Container status check
'docker compose ps'

Logs check
'docker compose logs postgres'


### step 4
database connect
'docker compose exec pgvector psql -U zerocom -d zerocom'

on PostgreSQL prompt
'CREATE EXTENSION IF NOT EXISTS vector;'

verify
'SELECT extname
FROM pg_extension
WHERE extname = 'vector';'