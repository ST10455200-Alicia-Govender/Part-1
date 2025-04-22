# Part-1
Registration and login feature
package registrationlogin;

import java.util.regex.Pattern;
import java.util.Scanner;

public class RegistrationLogin {

    static class Login {
        private String username;
        private String password;
        private String cellNumber;

        public Logic(String username, String password, String cellNumber) {
            this.username = username;
            this.password = password;
            this.cellNumber = cellNumber;
        }

        public boolean isUsernameValid() {
            return username.contains("_") && username.length() <= 5;
        }

        public boolean isPasswordValid() {
            return password.length() >= 8 &&
                   Pattern.compile("[A-Z]").matcher(password).find() &&
                   Pattern.compile("[0-9]").matcher(password).find() &&
                   Pattern.compile("[!@#$%^&*(),.?\":{}|<>"]").matcher(password).find();
        }

        public boolean isCellNumberValid() {
            return Pattern.matches("0\\d{9}", cellNumber);
        }

        public String getUsername() {
            return username;
        }

        public String getPassword() {
            return password;
        }
    }

    static class Login {
        private User registeredUser;

        public boolean registerUser(String username, String password, String cellNumber) {
            User user = new User(username, password, cellNumber);

            if (!user.isUsernameValid()) {
                System.out.println("Username is not correctly formatted, please ensure that your username contains an underscore and is no more than five characters in length.");
                return false;
            } else {
                System.out.println("Username successfully captured.");
            }

            if (!user.isPasswordValid()) {
                System.out.println("Password is not correctly formatted; please ensure that the password contains at least eight characters, a capital letter, a number, and a special character.");
                return false;
            } else {
                System.out.println("Password successfully captured.");
            }

            if (!user.isCellNumberValid()) {
                System.out.println("Cell phone number incorrectly formatted. Ensure it starts with 0 and is 10 digits long.");
                return false;
            } else {
                System.out.println("Cell phone number successfully added.");
            }

            registeredUser = user;
            return true;
        }

        public boolean loginUser(String username, String password) {
            if (registeredUser != null &&
                registeredUser.getUsername().equals(username) &&
                registeredUser.getPassword().equals(password)) {
                return true;
            }
            return false;
        }

        public String returnLoginStatus(boolean loginSuccess, String firstName, String lastName) {
            if (loginSuccess) {
                return "Welcome " + firstName + " " + lastName + ", it is great to see you again.";
            } else {
                return "Username or password incorrect, please try again.";
            }
        }
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Login login = new Login();

        System.out.println("--- User Registration ---");
        System.out.print("Enter username: ");
        String username = scanner.nextLine();

        System.out.print("Enter password: ");
        String password = scanner.nextLine();

        System.out.print("Enter SA Cell Number (e.g., 0821234567): ");
        String cellNumber = scanner.nextLine();

        boolean registered = login.registerUser(username, password, cellNumber);

        if (registered) {
            System.out.println("\n--- User Login ---");
            System.out.print("Enter username: ");
            String loginUsername = scanner.nextLine();

            System.out.print("Enter password: ");
            String loginPassword = scanner.nextLine();

            System.out.print("Enter your first name: ");
            String firstName = scanner.nextLine();

            System.out.print("Enter your last name: ");
            String lastName = scanner.nextLine();

            boolean isLoggedIn = login.loginUser(loginUsername, loginPassword);
            System.out.println(login.returnLoginStatus(isLoggedIn, firstName, lastName));
        }

        scanner.close();
    }
}
