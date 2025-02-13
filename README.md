import java.util.HashMap;
import java.util.InputMismatchException;
import java.util.Map;
import java.util.Scanner;

public class ATMmachine {
    private static final Scanner scanner = new Scanner(System.in);
    private static final Map<String, Account> accounts = new HashMap<>();
    
   public static void main(String[] args) {
        initializeAccounts();
                System.out.println("Welcome to the ATM");
        if (authenticateUser()) {
            showMainMenu();
        } else {
            System.out.println("Authentication failed.");
        }
    }
    
   private static void initializeAccounts() {
        // Example accounts (account number, PIN, balance)
        accounts.put("123456", new Account("123456", "1234", 1000.0));
        accounts.put("654321", new Account("654321", "4321", 2000.0));
    }

   private static boolean authenticateUser() {
        System.out.print("Enter account number: ");
        String accountNumber = scanner.nextLine();
            System.out.print("Enter PIN: ");
        String pin = scanner.nextLine();
             Account account = accounts.get(accountNumber);
        if (account != null && account.getPin().equals(pin)) {
            return true;
        } else {
            return false;
        }
    }

  private static void showMainMenu() {
        boolean running = true;
        while (running) {
            System.out.println("\nMain Menu:");
            System.out.println("1. Withdraw");
            System.out.println("2. Deposit");
            System.out.println("3. Balance Inquiry");
            System.out.println("4. Exit");
            System.out.print("Choose an option: ");
            
  int choice;
            try {
                choice = scanner.nextInt();
                scanner.nextLine(); // Consume newline
            } catch (InputMismatchException e) {
                scanner.nextLine(); // Clear the invalid input
                System.out.println("Invalid input. Please enter a number.");
                continue;
            }
            
  switch (choice) {
                case 1:
                    performWithdrawal();
                    break;
                case 2:
                    performDeposit();
                    break;
                case 3:
                    performBalanceInquiry();
                    break;
                case 4:
                    System.out.println("Thank you for using the ATM. Goodbye!");
                    running = false;
                    break;
                default:
                    System.out.println("Invalid choice. Please select a valid option.");
            }
        }
    }

  private static void performWithdrawal() {
        System.out.print("Enter account number: ");
        String accountNumber = scanner.nextLine();
        
  System.out.print("Enter amount to withdraw: ");
        double amount;
        try {
            amount = scanner.nextDouble();
            scanner.nextLine(); // Consume newline
        } catch (InputMismatchException e) {
            scanner.nextLine(); // Clear the invalid input
            System.out.println("Invalid amount. Please enter a valid number.");
            return;
        }
        
  Account account = accounts.get(accountNumber);
        if (account != null) {
            if (amount > 0 && amount <= account.getBalance()) {
                account.setBalance(account.getBalance() - amount);
                System.out.println("Withdrawal successful. New balance: " + account.getBalance());
            } else if (amount > account.getBalance()) {
                System.out.println("Insufficient funds.");
            } else {
                System.out.println("Invalid amount.");
            }
        } else {
            System.out.println("Account not found.");
        }
    }

  private static void performDeposit() {
        System.out.print("Enter account number: ");
        String accountNumber = scanner.nextLine();
        
   System.out.print("Enter amount to deposit: ");
        double amount;
        try {
            amount = scanner.nextDouble();
            scanner.nextLine(); // Consume newline
        } catch (InputMismatchException e) {
            scanner.nextLine(); // Clear the invalid input
            System.out.println("Invalid amount. Please enter a valid number.");
            return;
        }
        
   Account account = accounts.get(accountNumber);
        if (account != null) {
            if (amount > 0) {
                account.setBalance(account.getBalance() + amount);
                System.out.println("Deposit successful. New balance: " + account.getBalance());
            } else {
                System.out.println("Invalid amount.");
            }
        } else {
            System.out.println("Account not found.");
        }
    }

  private static void performBalanceInquiry() {
        System.out.print("Enter account number: ");
        String accountNumber = scanner.nextLine();
            Account account = accounts.get(accountNumber);
        if (account != null) {
            System.out.println("Your current balance is: " + account.getBalance());
        } else {
            System.out.println("Account not found.");
        }
    }
}

class Account {
    private String accountNumber;
    private String pin;
    private double balance;
    
  public Account(String accountNumber, String pin, double balance) {
        this.accountNumber = accountNumber;
        this.pin = pin;
        this.balance = balance;
    }

  public String getAccountNumber() {
        return accountNumber;
    }

   public String getPin() {
        return pin;
    }

   public double getBalance() {
        return balance;
    }

   public void setBalance(double balance) {
        this.balance = balance;
    }
}
