#include <iostream>
#include <vector>
using namespace std;

class Transaction {
public:
    virtual void execute() = 0;
};

class Account {
private:
    double balance;
public:
    Account(double initialBalance) : balance(initialBalance) {}

    void deposit(double amount) {
        balance += amount;
    }

    void withdraw(double amount) {
        balance -= amount;
    }

    double getBalance() {
        return balance;
    }
};

class ATM {
private:
    Account* account;
public:
    ATM(Account* acc) : account(acc) {}

    void cashWithdrawal(double amount) {
        if (amount <= account->getBalance()) {
            account->withdraw(amount);
            cout << "Cash withdrawn: PKR" << amount <<endl;
        } else {
            cout << "Insufficient funds!" <<endl;
        }
    }

    void deposit(double amount) {
        account->deposit(amount);
        cout << "Deposited: PKR" << amount <<endl;
    }

    void fundTransfer(Account* receiver, double amount) {
        if (amount <= account->getBalance()) {
            account->withdraw(amount);
            receiver->deposit(amount);
            cout << "Transferred: PKR" << amount << " to another account." <<endl;
        } else {
            cout << "Insufficient funds for transfer!" <<endl;
        }
    }

    void billPayment(double amount) {
        if (amount <= account->getBalance()) {
            account->withdraw(amount);
            cout << "Bill paid: PKR" << amount <<endl;
        } else {
            cout << "Insufficient funds for bill payment!" <<endl;
        }
    }

    void displayAccountInfo() {
        cout << "Current balance: PKR" << account->getBalance() << endl;
    }
};

int main() {
    Account userAccount(1000); // Initial balance $1000
    ATM atm(&userAccount);

    atm.cashWithdrawal(200);
    atm.deposit(500);
    
    Account receiverAccount(0);
    atm.fundTransfer(&receiverAccount, 300);

    atm.billPayment(100);

    atm.displayAccountInfo();

    return 0;
}
