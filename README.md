# cassandra-getting-started

prerequisites: java and python

brew install cassandra

## Production recommendations

vi /opt/homebrew/etc/cassandra

cassandra.yaml:
https://github.com/jolynch/python_performance_toolkit/raw/master/notebooks/cassandra_availability/whitepaper/cassandra-availability-virtual.pdf
num_tokens: 16
allocate_tokens_for_local_replication_factor: 3
materialized_views_enabled: true

jvm.options

## get cluster name and ip address

SELECT cluster_name, listen_address FROM system.local;

DESCRIBE KEYSPACES;
USE test;
DESCRIBE TABLES;

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

-- Create a keyspace
CREATE KEYSPACE IF NOT EXISTS test WITH REPLICATION = {
'class': 'SimpleStrategy', 'replication_factor': '1'
};

-- Create a table
CREATE TABLE IF NOT EXISTS test.employee (
id uuid PRIMARY KEY,
first_name text,
last_name text,
created_at timestamp,
updated_at timestamp
);

-- Create indexes
CREATE INDEX ON employee (first_name);

CREATE INDEX ON employee (last_name);

CREATE INDEX ON employee (created_at);

CREATE INDEX ON employee (first_name, last_name);

CREATE MATERIALIZED VIEW employee_by_name AS
SELECT id, first_name, last_name, created_at, updated_at
FROM employee
WHERE first_name IS NOT NULL AND last_name IS NOT NULL
PRIMARY KEY ((first_name, last_name), id);

SELECT \* FROM system_schema.indexes WHERE keyspace_name = 'test' ;

-- Insert some data
INSERT INTO test.employee (id, first_name, last_name, created_at, updated_at) VALUES (uuid(), 'Chris', 'Paul', toTimestamp(now()), toTimestamp(now()));

INSERT INTO test.employee (id, first_name, last_name, created_at, updated_at) VALUES (uuid(), 'Chris2', 'Paul', toTimestamp(now()), toTimestamp(now()));

INSERT INTO test.employee (id, first_name, last_name, created_at, updated_at) VALUES (uuid(), 'Nathan', 'Drake', toTimestamp(now()), toTimestamp(now()));

INSERT INTO test.employee (id, first_name, last_name, created_at, updated_at) VALUES (uuid(), 'Sam', 'Sung', toTimestamp(now()), toTimestamp(now()));

INSERT INTO test.employee (id, first_name, last_name, created_at, updated_at) VALUES (uuid(), 'Sully', 'Van', toTimestamp(now()), toTimestamp(now()));
