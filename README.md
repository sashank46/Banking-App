# Banking App

A simple banking backend built with **Java** and **Spring Boot**. It lets you create bank accounts, check balances, deposit money, withdraw money, and delete accounts — all through a REST API. Data is saved in a **MySQL** database.

---

## What This App Does

Think of this as the "engine" of a banking system. It does not have a website or app screen (no user interface). Instead, it gives you a set of web addresses (API endpoints) that other programs — like Postman, a mobile app, or a website — can use to talk to it.

With this app you can:
- Create a new bank account
- Look up a bank account by its ID
- See a list of all bank accounts
- Deposit money into an account
- Withdraw money from an account
- Delete an account

---

## What You Need Before You Start

Install these on your computer first:

1. **Java 17 or newer** — [Download here](https://adoptium.net/)
2. **Maven** — Not required, this project comes with its own copy (called `mvnw`), so you can skip installing this separately
3. **MySQL** — [Download here](https://dev.mysql.com/downloads/installer/)
4. **Postman** (optional, but helpful for testing) — [Download here](https://www.postman.com/downloads/)
5. A code editor like **IntelliJ IDEA** or **VS Code** (optional, only if you want to open and edit the code)

---

## Step 1: Set Up the Database

This app needs a MySQL database to store account data.

1. Open MySQL (using MySQL Workbench, terminal, or any tool you use).
2. Create a new database with this exact command:
   ```sql
   CREATE DATABASE banking_app;
   ```
3. That's it — you don't need to create tables yourself. The app will create them automatically the first time it runs.

---

## Step 2: Set Your Database Password

Open the file:
```
src/main/resources/application.properties
```

You will see something like this:
```properties
spring.application.name=banking-app
spring.datasource.url=jdbc:mysql://localhost:3306/banking_app
spring.datasource.username=root
spring.datasource.password=YOUR_MYSQL_PASSWORD
spring.jpa.hibernate.ddl-auto=update
```

Change `spring.datasource.username` and `spring.datasource.password` to match your own MySQL login details.

> ⚠️ **Important security note:** Right now, this file has a real password typed directly into it. This is not safe, especially if you plan to upload this project to GitHub — anyone would be able to see your password. A better approach is to use an environment variable instead. See the **"Keeping Your Password Safe"** section below for how to do this.

---

## Step 3: Run the App

You do **not** need to install Maven separately. This project includes wrapper scripts (`mvnw` and `mvnw.cmd`) that download everything needed automatically.

Open a terminal (Command Prompt, PowerShell, or Terminal app) inside the project folder, then run:

**On Windows:**
```bash
mvnw.cmd spring-boot:run
```

**On Mac/Linux:**
```bash
./mvnw spring-boot:run
```

Wait for the terminal to show a message like `Started BankingAppApplication`. This means the app is running successfully.

By default, the app runs at:
```
http://localhost:8080
```

---

## Step 4: Test the App

You can test the app using **Postman**, or any tool that can send web requests (even a browser for simple checks). Below are all the available features.

### 1. Create a New Account
- **Method:** POST
- **URL:** `http://localhost:8080/api/accounts`
- **Body (JSON):**
  ```json
  {
    "accountHolderName": "John Doe",
    "balance": 1000
  }
  ```

### 2. Get One Account (by ID)
- **Method:** GET
- **URL:** `http://localhost:8080/api/accounts/1`
  *(replace `1` with the actual account ID)*

### 3. Get All Accounts
- **Method:** GET
- **URL:** `http://localhost:8080/api/accounts`

### 4. Deposit Money
- **Method:** PUT
- **URL:** `http://localhost:8080/api/accounts/1/deposit`
- **Body (JSON):**
  ```json
  {
    "amount": 500
  }
  ```

### 5. Withdraw Money
- **Method:** PUT
- **URL:** `http://localhost:8080/api/accounts/1/withdraw`
- **Body (JSON):**
  ```json
  {
    "amount": 200
  }
  ```

### 6. Delete an Account
- **Method:** DELETE
- **URL:** `http://localhost:8080/api/accounts/1`

---

## Project Structure (What Each Folder Does)

```
src/main/java/net/javaguides/banking/
├── controller/       → Handles incoming web requests (the API endpoints above)
├── dto/               → Simple objects used to send/receive data (AccountDto)
├── entity/            → Defines what an "Account" looks like in the database
├── mapper/            → Converts between database objects and DTOs
├── respository/       → Talks directly to the MySQL database
└── service/           → Contains the actual business logic (deposit, withdraw, etc.)
```

> Note: the `respository` folder name has a small spelling mistake in the original code (should be "repository"). It still works fine, just something to be aware of if you're navigating the code.

---

## Keeping Your Password Safe (Recommended)

Instead of writing your real password inside `application.properties`, you can tell Spring Boot to read it from an environment variable. Here's how:

1. In `application.properties`, change the password line to:
   ```properties
   spring.datasource.password=${DB_PASSWORD}
   ```
2. Before running the app, set the environment variable:

   **On Windows (Command Prompt):**
   ```bash
   set DB_PASSWORD=your_real_password
   ```

   **On Mac/Linux:**
   ```bash
   export DB_PASSWORD=your_real_password
   ```
3. Then run the app as usual (Step 3 above).

This way, your real password never gets saved inside the code or accidentally uploaded to GitHub.

---

## Common Problems and Fixes

| Problem | Likely Cause | Fix |
|---|---|---|
| App won't start, database connection error | MySQL isn't running, or wrong username/password | Make sure MySQL is running and check `application.properties` |
| "Unknown database 'banking_app'" | You haven't created the database yet | Run `CREATE DATABASE banking_app;` in MySQL |
| Port 8080 already in use | Another app is using that port | Add `server.port=8081` (or any free port) to `application.properties` |
| 404 error when calling an API | Wrong URL or app not running | Double check the URL and make sure the app started successfully |

---

## Tech Stack

- **Java 17**
- **Spring Boot 4.1.0** (Web, Data JPA)
- **MySQL** (database)
- **Lombok** (reduces boilerplate code)
- **Maven** (build tool, comes bundled via wrapper)
