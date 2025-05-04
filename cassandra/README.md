# Experimentation with Cassandra

## Multi-Node Setup

## QuickStart Cassandra

Step 1: Fetch the docker-image
```bash
docker pull cassandra:latest ``` Step 2: Start Cassandra
```bash
docker network create cassandra
docker run --rm -d --name cassandra --hostname cassandra --network cassandra cassandra
```
Step 3: Create files using Cassandra Query Language
```sql
-- Create a keyspace
CREATE KEYSPACE IF NOT EXISTS store WITH REPLICATION = { 'class' : 'SimpleStrategy', 'replication_factor' : '1' };

-- Create a table
CREATE TABLE IF NOT EXISTS store.shopping_cart (
userid text PRIMARY KEY,
item_count int,
last_update_timestamp timestamp
);

-- Insert some data
INSERT INTO store.shopping_cart
(userid, item_count, last_update_timestamp)
VALUES ('9876', 2, toTimeStamp(now()));
INSERT INTO store.shopping_cart
(userid, item_count, last_update_timestamp)
VALUES ('1234', 5, toTimeStamp(now()));
```
Step 4: Load data with CQLSH
```bash
docker run --rm --network cassandra -v "$(pwd)/data.cql:/scripts/data.cql" -e CQLSH_HOST=cassandra -e CQLSH_PORT=9042 -e CQLVERSION=3.4.6 nuvo/docker-cqlsh
```
Step 5: Interactive CQLSH
```bash
docker run --rm -it --network cassandra nuvo/docker-cqlsh cqlsh cassandra 9042 --cql version='3.4.5'
```
Step 6: Read some data
```sql
SELECT * FROM store.shopping_cart;
```
Step 7: Write some more data
```sql
INSERT INTO store.shopping_cart
(userid, item_count) VALUES ('4567', 20);
```
Step 8: Clean up
```bash
docker kill cassandra
docker network rm cassandra
```
