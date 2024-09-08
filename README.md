Banking Management System
Overview
This project is a server-side implementation of a banking management system designed to handle operations such as user authentication, account management, and financial transactions. The system supports three types of users: normal users, joint users, and administrators. The server manages concurrent client connections and ensures data consistency through file-based storage and file locking mechanisms.


Features
User Authentication: Verify credentials for normal users, joint users, and admins.
Account Management: Create, modify, and delete user accounts.
Transaction Processing: Handle deposits, withdrawals, and balance inquiries.
Concurrency Control: Use file locks to manage simultaneous access to user data.
Multi-User Support: Separate handling of normal users, joint users, and admins.



File Structure
server.c: Contains the server logic, including handling client connections and invoking functions for various operations.
data.c: Implements the core functions for data retrieval, authentication, transaction processing, and account management.
normalUser.dat: Stores records for normal user accounts.
jointUser.dat: Stores records for joint user accounts.
admin.dat: Stores records for admin accounts.



Data Structures
Normal User
typedef struct {
    int userID;
    char name[30];
    char password[10];
    int account_no;
    float balance;
    int status; // ACTIVE or CLOSED
} normalUser;



Joint User
typedef struct {
    int userID;
    char name1[30];
    char name2[30];
    char password[10];
    int account_no;
    float balance;
    int status; // ACTIVE or CLOSED
} jointUser;



Admin
typedef struct {
    int userID;
    char username[30];
    char password[10];
} admin;



Functionality
Authentication
checkNormalUser(normalUser currUser): Verifies if the provided normalUser credentials match an active account.
checkJointUser(jointUser currUser): Verifies if the provided jointUser credentials match an active account.
checkAdmin(admin currUser): Verifies admin credentials.



Transactions
depositMoney(int accType, int ID, float amt): Deposits a specified amount into a normal or joint user account.
withdrawMoney(int accType, int ID, float amt): Withdraws a specified amount from a normal or joint user account, ensuring sufficient balance.
getBalance(int accType, int ID): Retrieves the current balance of a normal or joint user account.



Account Management
addNormalUser(normalUser record): Adds a new normal user account.
addJointUser(jointUser record): Adds a new joint user account.
deleteNormalUser(int ID): Marks a normal user account as CLOSED and resets the balance.
deleteJointUser(int ID): Marks a joint user account as CLOSED and resets the balance.
modifyNormalUser(normalUser modUser): Modifies an existing normal user account.
modifyJointUser(jointUser modUser): Modifies an existing joint user account.
alterPassword(int accType, int ID, char newPwd[10]): Updates the password for a normal or joint user account.



Concurrency Handling
File Locking:
The system uses file locks (flock) to prevent concurrent access issues.
Read Lock (F_RDLCK): Used during read operations to prevent data from being written simultaneously.
Write Lock (F_WRLCK): Used during write operations to prevent both reading and writing simultaneously.
Unlock (F_UNLCK): Releases the lock after the operation is complete.



Security Considerations
Password Storage: Passwords are stored as plain text, which is not secure for production use. Implementing password hashing (e.g., using bcrypt) is recommended.
File Security: Access to data files should be restricted using appropriate file permissions to prevent unauthorized access.



Error Handling
File Operations: The current implementation assumes success for file operations like open, read, and write. It’s important to add error handling for cases where files are missing, corrupted, or inaccessible.
Data Integrity: Additional checks should be implemented to ensure data consistency, especially during concurrent access.



Setup and Usage
Prerequisites
A Unix-like operating system (e.g., Linux)
GCC compiler
Compilation
bash
Copy code
gcc server.c data.c -o banking_system -lpthread
Running the Server
bash
Copy code
./banking_system



Client Interaction
Clients will interact with the server through a user interface (not provided in the code) that connects to the server via sockets. The server handles requests to authenticate users, perform transactions, and manage accounts.



Future Enhancements
Security Improvements: Implement password hashing and secure file access.
Database Integration: Replace flat files with a robust relational database system (e.g., MySQL, PostgreSQL) for better data management.
Enhanced Error Handling: Add comprehensive error handling and logging mechanisms.
User Interface: Develop a client-side application (e.g., web or desktop) for user interaction.
Multi-Threading Improvements: Optimize thread management to avoid potential deadlocks or resource contention issues.


Conclusion
The Banking Management System is a functional server-side application that provides essential banking operations. While the current implementation effectively handles user data and transactions, enhancements in security, error handling, and database integration are necessary for production-level deployment.
