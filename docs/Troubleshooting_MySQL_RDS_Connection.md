# Troubleshooting MySQL Database Connection in AWS

## 1. Objective
The objective of this troubleshooting process is to establish a successful connection between an Ubuntu EC2 instance and a MySQL database hosted on Amazon RDS. The process involves checking the MySQL client, verifying database credentials, configuring security groups, and testing network connectivity.

## 2. Problem Description
While attempting to connect to the MySQL database from the Ubuntu EC2 instance, the connection process stopped at the password prompt. The MySQL client was installed successfully, but a successful database connection had not yet been established.

## 3. Environment Details
- **Cloud Platform:** Amazon Web Services (AWS)
- **Compute Service:** Amazon EC2
- **Operating System:** Ubuntu Linux
- **Database Service:** Amazon RDS for MySQL
- **Database Port:** 3306
- **Database Username:** admin
- **Region:** Asia Pacific (Mumbai), ap-south-1

## 4. Troubleshooting Steps

### Step 1: Verify the MySQL Client Installation
The following command was executed to check whether the MySQL client was installed:

```bash
mysql --version
```

The command returned the installed MySQL client version, confirming that the client was available on the EC2 instance.

### Step 2: Connect to the RDS Database
The following command was used to initiate a connection:

```bash
mysql -h <RDS-ENDPOINT> -P 3306 -u admin -p
```

The `-h` option specifies the RDS endpoint, `-P` specifies the port, `-u` specifies the username, and `-p` prompts for the database password.

The database password must be entered when prompted. The password does not appear on the terminal while typing.

### Step 3: Verify the Database Credentials
If the connection fails with an `Access denied` error, verify the database username and password. If the password is forgotten, reset it through the Amazon RDS console.

### Step 4: Check the RDS Security Group
The security group associated with the RDS database must allow inbound MySQL traffic from the EC2 instance.

The inbound rule should contain the following settings:

- **Type:** MySQL/Aurora
- **Protocol:** TCP
- **Port:** 3306
- **Source:** Security group associated with the EC2 instance

Using the EC2 security group as the source is recommended instead of allowing access from all IP addresses.

### Step 5: Test Network Connectivity
The following command can be used to test whether the EC2 instance can reach the RDS database on port 3306:

```bash
nc -zv <RDS-ENDPOINT> 3306
```

If the connection succeeds, network connectivity to the database port is available. If it fails or times out, check the security groups, network access control lists, routing, VPC configuration, and DNS resolution.

If the `nc` command is unavailable, install it using:

```bash
sudo apt update
sudo apt install netcat-openbsd -y
```

### Step 6: Verify the RDS Database Status
Open the Amazon RDS console and select the database instance. Verify that:

- The database status is **Available**.
- The RDS endpoint is correct.
- The database port is 3306.
- The security group configuration permits connections from the EC2 instance.

### Step 7: Verify the Connection
After resolving any credential or network issues, execute the MySQL connection command again.

If the connection succeeds, the terminal displays the MySQL monitor prompt:

```text
mysql>
```

Run the following SQL command to verify database access:

```sql
SHOW DATABASES;
```

If the databases are listed, the connection is working and the user has permission to view those databases.

## 5. Expected Outcome
The expected outcome is a successful connection from the Ubuntu EC2 instance to the Amazon RDS MySQL database. The user should be able to authenticate with valid credentials and execute SQL commands according to the permissions granted to the database account.

## 6. Conclusion
The troubleshooting process helps identify common causes of MySQL connection failures, including incorrect credentials, security group restrictions, network connectivity problems, and incorrect RDS endpoints. By systematically checking the MySQL client, database status, credentials, and network configuration, the connection issue can be diagnosed and resolved.

**Note:** The screenshot shows that the MySQL client is installed and that a connection attempt was started. Since the attempt was cancelled at the password prompt, a successful connection has not yet been confirmed. The troubleshooting steps must be completed before documenting the issue as resolved.
