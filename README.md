# -OOPS-EXP.9
C++ program to show custom exception hierarchy .Program that throws and catches specific exceptions for a banking transaction .

#include <iostream>
#include <exception>
#include <string>

using namespace std;

class BankingException : public exception {
    string msg;
public:
    BankingException(string m) : msg(m) {}
    const char* what() const noexcept override { return msg.c_str(); }
};
struct InsufficientFundsException : public BankingException {
    InsufficientFundsException(double d) : BankingException("Money short by ₹" + to_string(d)) {}
};
struct InvalidAmountException : public BankingException {
    InvalidAmountException(double a) : BankingException("Invalid amount: ₹" + to_string(a)) {}
};

class BankAccount {
    double balance;
public:
    BankAccount(double b) : balance(b) {}
    void withdraw(double amt) {
        if (amt <= 0) throw InvalidAmountException(amt);
        if (amt > balance) throw InsufficientFundsException(amt - balance);
        balance -= amt;
        cout << "Withdrawn: ₹" << amt << " | Balance: ₹" << balance << "\n";
    }};

int main() {
    BankAccount acc(5000.0);
    try {
        acc.withdraw(8000.0); // Triggers InsufficientFundsException
    } catch (const BankingException& e) {
        cout << "[Caught Error] " << e.what() << "\n";
    }
    return 0;
}
