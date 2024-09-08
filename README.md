Overview

The Banking Management System is a robust server-side application designed to manage user authentication, account management, and financial transactions. It supports three user types: normal users, joint users, and administrators. The system effectively handles concurrent client connections, ensuring data consistency through file-based storage and sophisticated file locking mechanisms.

Features


User Authentication: Validates credentials for normal users, joint users, and administrators.
Account Management: Enables creation, modification, and deletion of user accounts.
Transaction Processing: Manages deposits, withdrawals, and balance inquiries.
Concurrency Control: Utilizes file locks to manage simultaneous access to user data.
Multi-User Support: Distinct handling for normal users, joint users, and administrators.
File Structure
server.c: Contains the server logic for client connections and operation functions.
data.c: Implements core functions for data retrieval, authentication, transaction processing, and account management.
normalUser.dat: Stores records for normal user accounts.
jointUser.dat: Stores records for joint user accounts.
admin.dat: Stores records for administrator accounts.



Data Structures
Normal User:

```
typedef struct {
    int userID;
    char name[30];
    char password[10];
    int account_no;
    float balance;
    int status; // ACTIVE or CLOSED
} normalUser;
```

Joint User:
```
typedef struct {
    int userID;
    char name1[30];
    char name2[30];
    char password[10];
    int account_no;
    float balance;
    int status; // ACTIVE or CLOSED
} jointUser;
```

Admin:

```
typedef struct {
    int userID;
    char username[30];
    char password[10];
} admin;
```


Functionality

Authentication:
checkNormalUser(normalUser currUser): Verifies credentials for normal users.
checkJointUser(jointUser currUser): Verifies credentials for joint users.
checkAdmin(admin currUser): Verifies credentials for administrators.

Transactions:
depositMoney(int accType, int ID, float amt): Deposits a specified amount into a user account.
withdrawMoney(int accType, int ID, float amt): Withdraws a specified amount from a user account, ensuring sufficient balance.
getBalance(int accType, int ID): Retrieves the current balance of a user account.

Account Management:
addNormalUser(normalUser record): Adds a new normal user account.
addJointUser(jointUser record): Adds a new joint user account.
deleteNormalUser(int ID): Marks a normal user account as CLOSED and resets the balance.
deleteJointUser(int ID): Marks a joint user account as CLOSED and resets the balance.
modifyNormalUser(normalUser modUser): Modifies an existing normal user account.
modifyJointUser(jointUser modUser): Modifies an existing joint user account.
alterPassword(int accType, int ID, char newPwd[10]): Updates the password for a user account.

Concurrency Handling
File Locking:
Read Lock (F_RDLCK): Prevents data from being written during read operations.
Write Lock (F_WRLCK): Prevents both reading and writing during write operations.
Unlock (F_UNLCK): Releases the lock after completion.

Security Considerations


Password Storage: Currently stored as plain text. Implement password hashing (e.g., using bcrypt) for better security.
File Security: Ensure access to data files is restricted with appropriate permissions.
Error Handling
File Operations: Add error handling for file operations like opening, reading, and writing.
Data Integrity: Implement additional checks to ensure data consistency, especially during concurrent access.
Setup and Usage
Prerequisites: A Unix-like operating system (e.g., Linux) and GCC compiler.

Compilation:

```
gcc server.c data.c -o banking_system -lpthread
```

Running the Server:
```
./banking_system
```
Client Interaction: 

Clients interact with the server via a user interface (not included in this code) that connects through sockets. The server manages user authentication, transactions, and account management.

Future Enhancements:
Security Improvements: Implement password hashing and secure file access.
Database Integration: Transition from flat files to a relational database system (e.g., MySQL, PostgreSQL).
Enhanced Error Handling: Introduce comprehensive error handling and logging mechanisms.
User Interface: Develop a client-side application (e.g., web or desktop) for enhanced user interaction.
Multi-Threading Improvements: Optimize thread management to avoid deadlocks or resource contention.


Conclusion:
The Banking Management System is a functional server-side application that handles essential banking operations. While effective in its current form, future enhancements in security, error handling, and database integration are recommended for production-level deployment.
