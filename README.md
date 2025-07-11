# Connecting PostgreSQL and pgAdmin Using Docker

Students often face challenges installing PostgreSQL and pgAdmin due to differences in operating systems. This blog explores an **easy and convenient** way to set up both tools using **Docker** — a platform-independent solution that works seamlessly across **Windows, macOS, and Linux**.

## Data Architecture
<img width="479" height="482" alt="image" src="https://github.com/user-attachments/assets/56748b81-afa9-4ff2-af47-64b9ed9664bd" />

## Step 1: Pull PostgreSQL Image

Open your terminal and pull the official PostgreSQL Docker image:

```bash
docker pull postgres:15
```
## Step 2: Create a PostgreSQL Container

Run the following command to create and start a PostgreSQL container:

```bash
docker run --name postgres-demo \
  -e POSTGRES_DB=tutorial_db \
  -e POSTGRES_USER=tutorial_user \
  -e POSTGRES_PASSWORD=secure_password \
  -p 5433:5432 \
  -d postgres:15
```


Breakdown of the command:
--name postgres-demo: Names the container

-e POSTGRES_DB=tutorial_db: Creates a database named tutorial_db

-e POSTGRES_USER=tutorial_user: Creates a user named tutorial_user

-e POSTGRES_PASSWORD=secure_password: Sets the password

-p 5432:5432: Maps port 5432 inside the container to 5432 on your host

-d: Runs the container in the background

To confirm it's running:
```bash
docker ps
```

## Step 3: Set Up Directory Structure

Create folders to manage your SQL scripts and persistent data:


```bash
mkdir -p ~/postgres-tutorial/data
mkdir -p ~/postgres-tutorial/sql
```

## Step 4: Copy Data into Container

To copy files (like .csv or .sql) into the container:

```bash
docker cp /your/file/path postgres-demo:/tmp/
```

Verify the files:

```bash
docker exec -it postgres-demo ls -la /tmp/
```

## Step 5: Connect to PostgreSQL

Connect to the PostgreSQL container:

```bash
docker exec -it postgres-demo psql -U tutorial_user -d tutorial_db
```

## Step 6: Create a Table

Inside the psql prompt, run:

```sql
CREATE TABLE amazon_sales (
  order_id VARCHAR(50) PRIMARY KEY,
  order_date DATE,
  product TEXT,
  category VARCHAR(100),
  price INTEGER,
  quantity INTEGER,
  total_sales INTEGER,
  customer_name VARCHAR(100),
  customer_location VARCHAR(100),
  payment_method VARCHAR(50),
  status VARCHAR(50),
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

Verify table creation:

```sql
\d amazon_sales
```

## Step 7: Run Sample Queries

Check number of records:
```sql 
SELECT COUNT(*) AS total_records FROM amazon_sales;
```
postgres connection
<img width="1361" height="375" alt="image" src="https://github.com/user-attachments/assets/52ef7490-1849-40da-b728-857327c54129" />

Exit psql:
press control + d button keyboard



## Step 8: Set Up pgAdmin with Docker

Create a network so that pgAdmin can communicate with PostgreSQL:

```bash
docker network create postgres-network
```

Run the pgAdmin container:

```bash
docker run --name pgadmin-demo \
  --network postgres-network \
  -p 8080:80 \
  -e PGADMIN_DEFAULT_EMAIL=admin@example.com \
  -e PGADMIN_DEFAULT_PASSWORD=admin_password \
  -d dpage/pgadmin4
```

## Step 9: Access pgAdmin in Browser

Visit: http://localhost:8080/

Login credentials: Email: admin@example.com, Password: admin_password

Add a New Server in pgAdmin: 
Name: PostgreSQL
Host: postgres-demo
Port: 5432
Username: tutorial_user
password: secure_password

padmin connection:
<img width="1434" height="706" alt="image" src="https://github.com/user-attachments/assets/79aa218c-7937-495e-a6c7-b149646fe362" />
















