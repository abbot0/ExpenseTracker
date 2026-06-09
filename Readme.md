# ExpenseTracker - Setup Guide

A Java web application for tracking personal expenses, budgets, and reports.
Built with Java Servlets, JSP, Bootstrap 5, Chart.js, and MySQL/MariaDB.

---

## Prerequisites

Install these before starting:

| Software | Version | Download |
|---|---|---|
| Java JDK | 11 or higher | https://adoptium.net |
| Apache Maven | 3.9+ | https://maven.apache.org/download.cgi |
| Apache Tomcat | 10.1+ | https://tomcat.apache.org/download-10.cgi |
| MySQL or MariaDB | 8.0+ / 10.6+ | https://mariadb.org/download |

---

## Setup Steps

### 1. Set up the Database

Open a terminal and run:

```
mysql -u root -p
```

Then in the MySQL prompt:

```sql
CREATE DATABASE expense_tracker;
USE expense_tracker;
EXIT;
```

Then import the database:

```
mysql -u root -p expense_tracker < database/full_export.sql
```

If you MySQL root user has no password, remove the `-p` flag.

If your MySQL user is not root, edit `src/util/DBConnection.java` and update the USER and PASSWORD fields.

---

### 2. Check Database Credentials

Open `src/util/DBConnection.java` and verify:

```java
private static final String URL = "jdbc:mysql://localhost:3306/expense_tracker?useSSL=false&serverTimezone=UTC&allowPublicKeyRetrieval=true";
private static final String USER = "root";       // change if needed
private static final String PASSWORD = "";        // change if needed
```

---

### 3. Build the Project

Make sure Maven and Java are on your PATH, then run:

```
mvn clean package
```

This creates: `target/ExpenseTracker-1.0-SNAPSHOT.war`

---

### 4. Deploy to Tomcat

Copy the WAR file into Tomcats webapps folder:

Windows:
```
copy target\ExpenseTracker-1.0-SNAPSHOT.war C:\path\to\tomcat\webapps\ExpenseTracker.war
```

Mac/Linux:
```
cp target/ExpenseTracker-1.0-SNAPSHOT.war /path/to/tomcat/webapps/ExpenseTracker.war
```

---

### 5. Start Tomcat

Windows:
```
C:\path\to\tomcat\bin\startup.bat
```

Mac/Linux:
```
/path/to/tomcat/bin/startup.sh
```

---

### 6. Open the App

Go to: http://localhost:8080/ExpenseTracker/

---

## Login Credentials

| Username | Password |
|---|---|
| johndoe | password123 |
| janedoe | password123 |

---

## Project Structure

```
ExpenseTracker/
├── src/                        Java source code
│   ├── controller/             Servlets (Login, Expense, Budget, Reports, Dashboard)
│   ├── dao/                    Database access objects
│   ├── model/                  Java model classes
│   ├── util/                   DB connection, session utilities
│   └── filter/                 Authentication filter
├── WEB-INF/
│   ├── views/                  JSP pages (protected)
│   └── web.xml                 App configuration
├── WebContent/                 Public pages (login, register)
├── assets/                     CSS, JavaScript, vendor libraries
├── database/
│   ├── schema.sql              Database structure only
│   └── full_export.sql         Full database with sample data
└── pom.xml                     Maven build file
```

---

## Troubleshooting

Cannot connect to database:
- Make sure MySQL/MariaDB is running
- Check credentials in src/util/DBConnection.java
- Make sure the expense_tracker database exists

404 Not Found on all pages:
- Make sure the WAR deployed correctly (check Tomcat logs in tomcat/logs/catalina.out)
- Try restarting Tomcat after copying the WAR

ClassNotFoundException: com.mysql.cj.jdbc.Driver:
- Run mvn clean package again - Maven will download the MySQL connector automatically

Pages not loading after login:
- Clear browser cache and cookies
- Try an incognito/private window
