import java.util.Base64;
import java.util.HashMap;
import java.util.Scanner;

public class Main {
    private HashMap<String, String> passwordStore;

    public Main() {
        passwordStore = new HashMap<>();
    }

    private String encrypt(String password) {
        return Base64.getEncoder().encodeToString(password.getBytes());
    }

    private String decrypt(String encryptedPassword) {
        return new String(Base64.getDecoder().decode(encryptedPassword));
    }

    public void addPassword(String account, String password) {
        passwordStore.put(account, encrypt(password));
        System.out.println("Password added for account: " + account);
    }

    public String getPassword(String account) {
        String encryptedPassword = passwordStore.get(account);
        if (encryptedPassword != null) {
            return decrypt(encryptedPassword);
        } else {
            return null;
        }
    }

    public void deletePassword(String account) {
        if (passwordStore.remove(account) != null) {
            System.out.println("Password removed for account: " + account);
        } else {
            System.out.println("No account found with name: " + account);
        }
    }

    public static void main(String[] args) {
        Main pm = new Main();
        Scanner scanner = new Scanner(System.in);
        String command, account, password;

        while (true) {
            System.out.println("\nCommands: add, get, delete, exit");
            System.out.print("Enter command: ");
            command = scanner.nextLine();

            switch (command) {
                case "add":
                    System.out.print("Enter account name: ");
                    account = scanner.nextLine();
                    System.out.print("Enter password: ");
                    password = scanner.nextLine();
                    pm.addPassword(account, password);
                    break;
                case "get":
                    System.out.print("Enter account name: ");
                    account = scanner.nextLine();
                    String retrievedPassword = pm.getPassword(account);
                    if (retrievedPassword != null) {
                        System.out.println("Password for " + account + ": " + retrievedPassword);
                    } else {
                        System.out.println("No account found with name: " + account);
                    }
                    break;
                case "delete":
                    System.out.print("Enter account name: ");
                    account = scanner.nextLine();
                    pm.deletePassword(account);
                    break;
                case "exit":
                    scanner.close();
                    System.out.println("Exiting...");
                    return;
                default:
                    System.out.println("Invalid command!");
            }
        }
    }
}
