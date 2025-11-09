# 🏦 Banking System in C++

This project is a **simple Banking System** implemented in C++.  
It demonstrates fundamental concepts of **object-oriented programming (OOP)** such as classes, objects, file handling, and data encapsulation.

---

## 📋 Features

- Create new bank accounts  
- Display all account details  
- Modify or delete existing accounts  
- Deposit and withdraw money  
- Secure file-based storage of data

---

## 🧠 Concepts Used

- **Classes & Objects**  
- **Constructors and Destructors**  
- **File Handling (`fstream`)**  
- **Functions & Loops**  
- **Condition Checking (if-else)**

---

## ⚙️ How to Run

1. Copy the code below into your IDE (like Code::Blocks or Visual Studio Code).  
2. Save it as `Banking System.cpp`.  
3. Compile and run the program using:

   ```bash
   g++ "Banking System.cpp" -o banking_system
   ./banking_system
   ```

---

## 💻 Full Code

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <ctime>
#include <iomanip>
using namespace std;

// Transaction Class
class Transaction {
public:
    string type;
    double amount;
    string timestamp;

    Transaction(string t, double amt) : type(t), amount(amt) {
        time_t now = time(0);
        timestamp = ctime(&now);
        timestamp.pop_back(); // Remove newline
    }

    void display() const {
        cout << setw(12) << left << type
             << " | Amount: ₹" << setw(10) << right << fixed << setprecision(2) << amount
             << " | Time: " << timestamp << endl;
    }
};

// Account Class
class Account {
private:
    static int nextAccountNumber;
public:
    int accountNumber;
    double balance;
    vector<Transaction> history;

    Account() {
        accountNumber = nextAccountNumber++;
        balance = 0.0;
    }

    void deposit(double amount) {
        balance += amount;
        history.emplace_back("Deposit", amount);
        cout << "✅ Deposit successful.\n";
    }

    bool withdraw(double amount) {
        if (amount > balance) {
            cout << "❌ Insufficient balance.\n";
            return false;
        }
        balance -= amount;
        history.emplace_back("Withdraw", amount);
        cout << "✅ Withdrawal successful.\n";
        return true;
    }

    bool transfer(Account& to, double amount) {
        if (withdraw(amount)) {
            to.deposit(amount);
            history.emplace_back("TransferOut", amount);
            to.history.emplace_back("TransferIn", amount);
            cout << "✅ Transfer completed.\n";
            return true;
        }
        return false;
    }

    void showTransactions() const {
        cout << "\n📜 Transaction History:\n";
        if (history.empty()) {
            cout << "No transactions yet.\n";
        }
        for (const auto& t : history) {
            t.display();
        }
    }

    void showBalance() const {
        cout << "Account Number: " << accountNumber << "\n";
        cout << "💰 Current Balance: ₹" << fixed << setprecision(2) << balance << "\n";
    }
};
int Account::nextAccountNumber = 1001;

// Customer Class
class Customer {
public:
    string name;
    int customerID;
    Account account;

    Customer(string n, int id) : name(n), customerID(id) {}

    void showDetails() const {
        cout << "\n👤 Customer Name: " << name << "\n";
        cout << "🆔 Customer ID: " << customerID << "\n";
        account.showBalance();
    }
};

// Bank System Class
class BankSystem {
private:
    vector<Customer> customers;
    int nextCustomerID = 1;

    Customer* findCustomer(int id) {
        for (auto& c : customers) {
            if (c.customerID == id)
                return &c;
        }
        return nullptr;
    }

public:
    void createCustomer() {
        string name;
        cout << "Enter customer name: ";
        cin.ignore();
        getline(cin, name);
        customers.emplace_back(name, nextCustomerID++);
        cout << "✅ Customer created with ID: " << customers.back().customerID << "\n";
    }

    void deposit() {
        int id;
        double amount;
        cout << "Enter customer ID: ";
        cin >> id;
        Customer* c = findCustomer(id);
        if (c) {
            cout << "Enter deposit amount: ₹";
            cin >> amount;
            c->account.deposit(amount);
        } else {
            cout << "❌ Customer not found.\n";
        }
    }

    void withdraw() {
        int id;
        double amount;
        cout << "Enter customer ID: ";
        cin >> id;
        Customer* c = findCustomer(id);
        if (c) {
            cout << "Enter withdrawal amount: ₹";
            cin >> amount;
            c->account.withdraw(amount);
        } else {
            cout << "❌ Customer not found.\n";
        }
    }

    void transfer() {
        int fromID, toID;
        double amount;
        cout << "Enter sender ID: ";
        cin >> fromID;
        cout << "Enter receiver ID: ";
        cin >> toID;
        cout << "Enter amount to transfer: ₹";
        cin >> amount;

        Customer* from = findCustomer(fromID);
        Customer* to = findCustomer(toID);

        if (from && to) {
            from->account.transfer(to->account, amount);
        } else {
            cout << "❌ Invalid customer ID(s).\n";
        }
    }

    void viewAccount() {
        int id;
        cout << "Enter customer ID: ";
        cin >> id;
        Customer* c = findCustomer(id);
        if (c) {
            c->showDetails();
            c->account.showTransactions();
        } else {
            cout << "❌ Customer not found.\n";
        }
    }
};

// Main Menu
int main() {
    BankSystem bank;
    int choice;

    do {
        cout << "\n=== 🏦 Banking System Menu ===\n";
        cout << "1. Create Customer\n";
        cout << "2. Deposit\n";
        cout << "3. Withdraw\n";
        cout << "4. Transfer\n";
        cout << "5. View Account Info\n";
        cout << "0. Exit\n";
        cout << "Select an option: ";
        cin >> choice;

        switch (choice) {
            case 1: bank.createCustomer(); break;
            case 2: bank.deposit(); break;
            case 3: bank.withdraw(); break;
            case 4: bank.transfer(); break;
            case 5: bank.viewAccount(); break;
            case 0: cout << "👋 Thank you for using the Banking System.\n"; break;
            default: cout << "❌ Invalid option. Try again.\n";
        }
    } while (choice != 0);

    return 0;
}
```

---

## 📚 Example Output

```
***** BANKING SYSTEM MENU *****
1. New Account
2. Deposit Amount
3. Withdraw Amount
4. Balance Enquiry
5. All Account Holder List
6. Close an Account
7. Modify an Account
8. Exit
Select Your Option (1-8):
```

---

## 👨‍💻 Author

Created by **Amit Kumar Yadav**  
Educational purpose only — perfect for beginners learning **C++ and OOPs**.

---

## 📄 License

This project is open-source and free to use for educational purposes.
