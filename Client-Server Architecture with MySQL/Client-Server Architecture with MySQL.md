# Client-Server Architecture with MySQL
## Ping and Traceroute network diagnostic utilities
Ping and traceroute are two essential network diagnostic utilities used to test and diagnose network connectivity and performance issues. Here's an explanation of each tool, including how to interpret their results:

### Ping
**Purpose:** The `ping` command is used to test the reachability of a host (another computer or device) on an Internet Protocol (IP) network and to measure the round-trip time for messages sent from the originating host to a destination computer.

**How It Works:**
- `ping` sends ICMP (Internet Control Message Protocol) Echo Request packets to the target host.
- The target host responds with ICMP Echo Reply packets.
- `ping` calculates the time taken for the round-trip (from the source to the destination and back).

**Basic Command:**
```sh
ping <hostname_or_IP_address>
```

**Key Metrics:**
- **Packets Sent and Received:** Shows the number of packets sent and the number of replies received.
- **Packet Loss:** Indicates the percentage of packets that did not receive a reply, which can signal network issues.
- **Round-Trip Time (RTT):** Includes the minimum, maximum, and average time taken for a packet to travel to the destination and back.

**Example Result:**
```sh
PING google.com (172.217.12.206): 56 data bytes
64 bytes from 172.217.12.206: icmp_seq=0 ttl=53 time=18.7 ms
64 bytes from 172.217.12.206: icmp_seq=1 ttl=53 time=19.2 ms
64 bytes from 172.217.12.206: icmp_seq=2 ttl=53 time=18.5 ms

--- google.com ping statistics ---
3 packets transmitted, 3 packets received, 0.0% packet loss
round-trip min/avg/max/stddev = 18.5/18.8/19.2/0.3 ms
```
**Interpretation:**
- All packets were received (0.0% packet loss), indicating good connectivity.
- The round-trip times are low (18.5 to 19.2 ms), suggesting a fast and responsive connection.

### Traceroute
**Purpose:** The `traceroute` command is used to trace the path that a packet takes from the source computer to the destination host. It helps identify the route and measure transit delays of packets across a network.

**How It Works:**
- `traceroute` sends packets with increasing Time-To-Live (TTL) values.
- Each router along the path decrements the TTL value by one before forwarding the packet.
- When a router receives a packet with TTL=1, it discards the packet and sends an ICMP "Time Exceeded" message back to the source.
- By increasing the TTL value incrementally, `traceroute` identifies each hop along the route to the destination.

**Basic Command:**
```sh
traceroute <hostname_or_IP_address>
```

**Key Metrics:**
- **Hops:** Each line in the output represents a router (hop) that the packet passed through.
- **RTT for Each Hop:** Displays three RTT measurements to each hop, showing the time taken for packets to reach each hop and return.

**Example Result:**
```sh
traceroute to google.com (172.217.12.206), 64 hops max
 1  192.168.1.1 (192.168.1.1)  1.123 ms  1.134 ms  1.106 ms
 2  10.0.0.1 (10.0.0.1)  10.578 ms  10.602 ms  10.580 ms
 3  203.0.113.1 (203.0.113.1)  20.342 ms  20.360 ms  20.332 ms
 4  198.51.100.1 (198.51.100.1)  30.562 ms  30.579 ms  30.561 ms
 5  172.217.12.206 (google.com)  40.482 ms  40.495 ms  40.470 ms
```
**Interpretation:**
- **Hop 1:** 192.168.1.1 is typically the local router or gateway, with very low latency (~1 ms).
- **Hop 2 to Hop 4:** Intermediate routers in the path with increasing latencies, indicating the geographical and network distance.
- **Hop 5:** The final destination (google.com) with a round-trip time of about 40 ms, suggesting a moderately fast connection.

### Summary
- **Ping** is used to check if a specific host is reachable and to measure the latency of the connection.
- **Traceroute** is used to map the path packets take to reach the host, identifying each hop and measuring the time taken at each step.
- **Interpreting Results:** Low packet loss and low RTT in `ping` results indicate good connectivity. In `traceroute`, each hop's RTT helps diagnose where delays or issues might be occurring in the network path.

# Basic SQL Commands

### 1. SHOW
The `SHOW` command is used to display information about databases, tables, and other objects in the database system.

**Examples:**
- **Show databases:**
  ```sql
  SHOW DATABASES;
  ```
- **Show tables in the current database:**
  ```sql
  SHOW TABLES;
  ```
- **Show columns in a specific table:**
  ```sql
  SHOW COLUMNS FROM table_name;
  ```

### 2. CREATE
The `CREATE` command is used to create databases, tables, indexes, or other objects.

**Examples:**
- **Create a database:**
  ```sql
  CREATE DATABASE my_database;
  ```
- **Create a table:**
  ```sql
  CREATE TABLE employees (
      id INT AUTO_INCREMENT PRIMARY KEY,
      first_name VARCHAR(50),
      last_name VARCHAR(50),
      email VARCHAR(100),
      hire_date DATE
  );
  ```

### 3. DROP
The `DROP` command is used to delete databases, tables, indexes, or other objects.

**Examples:**
- **Drop a database:**
  ```sql
  DROP DATABASE my_database;
  ```
- **Drop a table:**
  ```sql
  DROP TABLE employees;
  ```

### 4. SELECT
The `SELECT` command is used to retrieve data from one or more tables.

**Examples:**
- **Select all columns from a table:**
  ```sql
  SELECT * FROM employees;
  ```
- **Select specific columns from a table:**
  ```sql
  SELECT first_name, last_name FROM employees;
  ```
- **Select with a condition (using WHERE clause):**
  ```sql
  SELECT * FROM employees WHERE hire_date > '2023-01-01';
  ```

### 5. INSERT
The `INSERT` command is used to add new rows to a table.

**Examples:**
- **Insert a single row into a table:**
  ```sql
  INSERT INTO employees (first_name, last_name, email, hire_date) 
  VALUES ('John', 'Doe', 'john.doe@example.com', '2023-05-01');
  ```
- **Insert multiple rows into a table:**
  ```sql
  INSERT INTO employees (first_name, last_name, email, hire_date) 
  VALUES 
  ('Jane', 'Smith', 'jane.smith@example.com', '2023-06-01'),
  ('Emily', 'Jones', 'emily.jones@example.com', '2023-07-01');
  ```

### Putting It All Together

Let's assume we are working with a database called `company_db`. Here is a series of SQL commands that demonstrate these basic operations:

```sql
-- Show all databases
SHOW DATABASES;

-- Create a new database
CREATE DATABASE company_db;

-- Use the new database
USE company_db;

-- Show all tables in the current database (should be empty initially)
SHOW TABLES;

-- Create a new table called employees
CREATE TABLE employees (
    id INT AUTO_INCREMENT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    email VARCHAR(100),
    hire_date DATE
);

-- Show the columns in the employees table
SHOW COLUMNS FROM employees;

-- Insert a new employee into the employees table
INSERT INTO employees (first_name, last_name, email, hire_date) 
VALUES ('John', 'Doe', 'john.doe@example.com', '2023-05-01');

-- Insert multiple employees into the employees table
INSERT INTO employees (first_name, last_name, email, hire_date) 
VALUES 
('Jane', 'Smith', 'jane.smith@example.com', '2023-06-01'),
('Emily', 'Jones', 'emily.jones@example.com', '2023-07-01');

-- Select all records from the employees table
SELECT * FROM employees;

-- Select specific columns from the employees table
SELECT first_name, last_name FROM employees;

-- Select records with a condition
SELECT * FROM employees WHERE hire_date > '2023-01-01';

-- Drop the employees table
DROP TABLE employees;

-- Drop the company_db database
DROP DATABASE company_db;
```

This sequence of commands covers basic operations such as creating and dropping databases and tables, inserting data, and retrieving data with various `SELECT` queries.
