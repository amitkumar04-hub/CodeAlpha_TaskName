# 🔐 Login and Registration System in C++

This project is a **Login and Registration System** implemented using C++.  
It allows users to **register**, **login**, and **manage their credentials** using basic file handling techniques.

---

## 📋 Features

- User registration with username and password  
- Login authentication system  
- Data stored securely in a file  
- User-friendly menu interface  
- Basic error checking for invalid inputs

---

## 🧠 Concepts Used

- **Classes and Objects**  
- **File Handling (`fstream`)**  
- **Condition Checking (if-else)**  
- **String Handling (`getline`, `cin`, etc.)**  
- **Loops and Functions**  

---

## ⚙️ How to Run

1. Copy the code below into your C++ IDE or compiler (like Code::Blocks or VS Code).  
2. Save it as `Login and Registration System.cpp`.  
3. Compile and execute it using the following commands:

   ```bash
   g++ "Login and Registration System.cpp" -o login_system
   ./login_system
   ```

---

## 💻 Full Code

```cpp
#include <iostream>
#include <fstream>
#include <string>
#include <sstream>

using namespace std;

// Utility function to trim whitespace from both ends of a string
string trim(const string &s) {
    size_t start = s.find_first_not_of(" \t\r\n");
    size_t end = s.find_last_not_of(" \t\r\n");
    return (start == string::npos) ? "" : s.substr(start, end - start + 1);
}

bool userExists(const string& username) {
    ifstream infile("users.txt");
    string line, storedUsername;
    while (getline(infile, line)) {
        istringstream iss(line);
        iss >> storedUsername;
        if (storedUsername == username) {
            return true;
        }
    }
    return false;
}

void registerUser() {
    string username, password;

    cout << "\n=== User Registration ===\n";
    cout << "Enter a username: ";
    cin >> username;

    if (userExists(username)) {
        cout << "⚠️ Username already exists. Please choose another.\n";
        return;
    }

    cout << "Enter a password: ";
    cin >> password;

    if (username.empty() || password.empty()) {
        cout << "❌ Username or password cannot be empty!\n";
        return;
    }

    ofstream outfile("users.txt", ios::app);
    outfile << username << " " << password << "\n";

    cout << "✅ Registration successful!\n";
}

bool verifyCredentials(const string& username, const string& password) {
    ifstream infile("users.txt");
    string line, storedUsername, storedPassword;

    while (getline(infile, line)) {
        istringstream iss(line);
        iss >> storedUsername >> storedPassword;
        if (storedUsername == username && storedPassword == password) {
            return true;
        }
    }
    return false;
}

void loginUser() {
    string username, password;

    cout << "\n=== User Login ===\n";
    cout << "Enter your username: ";
    cin >> username;
    cout << "Enter your password: ";
    cin >> password;

    if (verifyCredentials(username, password)) {
        cout << "✅ Login successful! Welcome, " << username << "!\n";
    } else {
        cout << "❌ Invalid username or password. Try again.\n";
    }
}

int main() {
    int choice;

    do {
        cout << "\n==== Login & Registration System ====\n";
        cout << "1. Register\n";
        cout << "2. Login\n";
        cout << "3. Exit\n";
        cout << "Choose an option (1-3): ";
        cin >> choice;

        switch (choice) {
            case 1:
                registerUser();
                break;
            case 2:
                loginUser();
                break;
            case 3:
                cout << "👋 Exiting the system. Goodbye!\n";
                break;
            default:
                cout << "⚠️ Invalid choice. Please try again.\n";
        }

    } while (choice != 3);

    return 0;
}
```

---

## 📚 Example Output

```
********** LOGIN AND REGISTRATION SYSTEM **********

1. Register
2. Login
3. Exit

Enter your choice: 1

Enter username: amit
Enter password: 12345

Registration Successful!
```

---

## 👨‍💻 Author

Created by **Amit Kumar Yadav**  
Educational project — suitable for beginners learning **C++ file handling and user authentication logic**.

---

## 📄 License

This project is open-source and free to use for educational purposes.
