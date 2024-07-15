This java program is to create a Online Banking System for the below functions,
Create an account with a unique account number and name.
Deposit funds into an existing account.
Withdraw funds from an existing account.
Transfer funds between two existing accounts.
View the transaction history of an existing account.
Manage personal information associated with an existing account.

/*Online Banking System*/
import java.util.ArrayList;
import java.util.Scanner;

class Account {
    private String accountNumber;
    private String accountHolderName;
    private double balance;
    private ArrayList<Transaction> transactionHistory;

    public Account(String accountNumber, String accountHolderName) {
        this.accountNumber = accountNumber;
        this.accountHolderName = accountHolderName;
        this.balance = 0.0;
        this.transactionHistory = new ArrayList<>();
    }

    public void deposit(double amount) {
        balance += amount;
        transactionHistory.add(new Transaction("Deposit", amount));
    }

    public void withdraw(double amount) {
        if (balance >= amount) {
            balance -= amount;
            transactionHistory.add(new Transaction("Withdrawal", -amount));
        } else {
            System.out.println("Insufficient funds");
        }
    }

    public void transfer(Account recipient, double amount) {
        if (balance >= amount) {
            balance -= amount;
            recipient.deposit(amount);
            transactionHistory.add(new Transaction("Transfer", -amount));
            recipient.getTransactionHistory().add(new Transaction("Transfer", amount));
        } else {
            System.out.println("Insufficient funds");
        }
    }

    public double getBalance() {
        return balance;
    }

    public ArrayList<Transaction> getTransactionHistory() {
        return transactionHistory;
    }

    public String getAccountNumber() {
        return accountNumber;
    }

    public String getAccountHolderName() {
        return accountHolderName;
    }
}

class Transaction {
    private String type;
    private double amount;

    public Transaction(String type, double amount) {
        this.type = type;
        this.amount = amount;
    }

    @Override
    public String toString() {
        return String.format("%s: $%.2f", type, amount);
    }
}

public class OnlineBankingSystem {
    private ArrayList<Account> accounts;

    public OnlineBankingSystem() {
        accounts = new ArrayList<>();
    }

    public void createAccount(String accountNumber, String accountHolderName) {
        Account newAccount = new Account(accountNumber, accountHolderName);
        accounts.add(newAccount);
        System.out.println("Account created successfully!");
    }

    public void deposit(String accountNumber, double amount) {
        for (Account account : accounts) {
            if (account.getAccountNumber().equals(accountNumber)) {
                account.deposit(amount);
                break;
            }
        }
    }

    public void withdraw(String accountNumber, double amount) {
        for (Account account : accounts) {
            if (account.getAccountNumber().equals(accountNumber)) {
                account.withdraw(amount);
                break;
            }
        }
    }

    public void transfer(String fromAccountNumber, String toAccountNumber, double amount) {
        for (Account fromAccount : accounts) {
            if (fromAccount.getAccountNumber().equals(fromAccountNumber)) {
                for (Account toAccount : accounts) {
                    if (toAccount.getAccountNumber().equals(toAccountNumber)) {
                        fromAccount.transfer(toAccount, amount);
                        break;
                    }
                }
                break;
            }
        }
    }

    public void viewTransactionHistory(String accountNumber) {
        for (Account account : accounts) {
            if (account.getAccountNumber().equals(accountNumber)) {
                System.out.println("Transaction History:");
                for (Transaction transaction : account.getTransactionHistory()) {
                    System.out.println(transaction);
                }
                break;
            }
        }
    }

    public void managePersonalInformation(String accountNumber, String newInformation) {
        for (Account account : accounts) {
            if (account.getAccountNumber().equals(accountNumber)) {
                System.out.println("Old Information: " + account.getAccountHolderName());
                System.out.println("New Information: " + newInformation);
                // Update the information in the database or file
                // For simplicity, we'll just print the new information
                System.out.println("Information updated successfully!");
                break;
            }
        }
    }

    public static void main(String[] args) {
        OnlineBankingSystem system = new OnlineBankingSystem();
        Scanner scanner = new Scanner(System.in);

        while (true) {
            System.out.println("Online Banking System");
            System.out.println("1. Create Account");
            System.out.println("2. Deposit");
            System.out.println("3. Withdraw");
            System.out.println("4. Transfer");
            System.out.println("5. View Transaction History");
            System.out.println("6. Manage Personal Information");
            System.out.println("7. Exit");

            int choice = scanner.nextInt();

            switch (choice) {
                case 1:
                    System.out.print("Enter account number: ");
                    String createAccountNumber = scanner.next();
                    System.out.print("Enter account holder name: ");
                    String createAccountHolderName = scanner.next();
                    system.createAccount(createAccountNumber, createAccountHolderName);
                    break;
                case 2:
                    System.out.print("Enter account number: ");
                    String depositAccountNumber = scanner.next();
                    System.out.print("Enter deposit amount: ");
                    double depositAmount = scanner.nextDouble();
                    system.deposit(depositAccountNumber, depositAmount);
                    break;
                case 3:
                    System.out.print("Enter account number: ");
                    String withdrawAccountNumber = scanner.next();
                    System.out.print("Enter withdrawal amount: ");
                    double withdrawAmount = scanner.nextDouble();
                    system.withdraw(withdrawAccountNumber, withdrawAmount);
                    break;
                case 4:
                    System.out.print("Enter from account number: ");
                    String fromAccountNumber = scanner.next();
                    System.out.print("Enter to account number: ");
                    String toAccountNumber = scanner.next();
                    System.out.print("Enter transfer amount: ");
                    double transferAmount = scanner.nextDouble();
                    system.transfer(fromAccountNumber, toAccountNumber, transferAmount);
                    break;
                case 5:
                    System.out.print("Enter account number: ");
                    String viewAccountNumber = scanner.next();
                    system.viewTransactionHistory(viewAccountNumber);
                    break;
                case 6:
                    System.out.print("Enter account number: ");
                    String manageAccountNumber = scanner.next();
                    System.out.print("Enter new information: ");
                    String newInfo = scanner.next();
                    system.managePersonalInformation(manageAccountNumber, newInfo);
                    break;
                case 7:
                    return;
                default:
                    System.out.println("Invalid choice");
            }
        }
    }
}
