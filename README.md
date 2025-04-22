# Part-1
Registration and login feature
package registrationlogin;

class User {
    private String username;
    private String password;
    private String cellNumber;

    public User(String username, String password, String cellNumber) {
        this.username = username;
        this.password = password;
        this.cellNumber = cellNumber;
    }

    public String getUsername() {
        return username;
    }

    public String getPassword() {
        return password;
    }
}
// This code was developed with guidance from the following tutorial:
// Parthipan N. (2017, August 14). How to do unit testing in NetBeans using JUNIT [Video]. YouTube. https://www.youtube.com/watch?v=De6wcVWoT1A

class Login {
    private java.util.Map<String, User> users = new java.util.HashMap<>();

    public boolean checkUserName(String username) {
        return username.contains("_") && username.length() <= 5;
    }

    public boolean checkPasswordComplexity(String password) {
        return password.length() >= 8 &&
               password.matches(".*[A-Z].*") &&
               password.matches(".*[0-9].*") &&
               password.matches(".*[!@#$%^&*(),.?\":{}|<>].*");
    }

    public boolean checkCellPhoneNumber(String cellNumber) {
        return cellNumber.matches("^\\+\\d{1,3}\\d{1,10}$");
    }

    public String registerUser(String username, String password, String cellNumber) {
        if (!checkUserName(username)) {
            return "Username is not correctly formatted, please ensure that your username contains an underscore and is no more than five characters in length.";
        }

        if (!checkPasswordComplexity(password)) {
            return "Password is not correctly formatted; please ensure that the password contains at least eight characters, a capital letter, a number, and a special character.";
        }

        if (!checkCellPhoneNumber(cellNumber)) {
            return "Cell phone number incorrectly formatted or does not contain international code.";
        }

        users.put(username, new User(username, password, cellNumber));
        return "User registered successfully.";
    }

    public boolean loginUser(String username, String password) {
        return users.containsKey(username) && users.get(username).getPassword().equals(password);
    }

    public String returnLoginStatus(boolean loginSuccess) {
        return loginSuccess ? "Welcome back!" : "Username or password incorrect, please try again.";
    }
}
// This code was developed with guidance from the following tutorial:
// Parthipan N. (2017, August 14). How to do unit testing in NetBeans using JUNIT [Video]. YouTube. https://www.youtube.com/watch?v=De6wcVWoT1A

public class RegistrationSystem {
    public static void main(String[] args) {
        java.util.Scanner scanner = new java.util.Scanner(System.in);
        Login loginSystem = new Login();

        System.out.println("Welcome! Please register an account.");

        System.out.print("Enter a username: ");
        String username = scanner.nextLine();

        System.out.print("Enter a password: ");
        String password = scanner.nextLine();

        System.out.print("Enter your cell phone number with international code (e.g., +27821234567): ");
        String cellNumber = scanner.nextLine();

        System.out.println(loginSystem.registerUser(username, password, cellNumber));

        System.out.println("\nLogin to your account");
        System.out.print("Username: ");
        String loginUsername = scanner.nextLine();

        System.out.print("Password: ");
        String loginPassword = scanner.nextLine();

        boolean loginSuccess = loginSystem.loginUser(loginUsername, loginPassword);
        System.out.println(loginSystem.returnLoginStatus(loginSuccess));

        scanner.close();
    }
}
// This code was developed with guidance from the following tutorial:
// Parthipan N. (2017, August 14). How to do unit testing in NetBeans using JUNIT [Video]. YouTube. https://www.youtube.com/watch?v=De6wcVWoT1A

public class LoginTest {

    Login loginSystem = new Login();

    // Test if the username is correctly formatted
    @org.junit.Test
    public void testCheckUserNameValid() {
        assert loginSystem.checkUserName("kyl_1");
    }

    // Test if the username is incorrectly formatted
    @org.junit.Test
    public void testCheckUserNameInvalid() {
        assert !loginSystem.checkUserName("kyle!!!!!!");
    }

    // Test if the password follows the rules
    @org.junit.Test
    public void testCheckPasswordComplexityValid() {
        assert loginSystem.checkPasswordComplexity("Ch&&sec@ke99!");
    }

    // Test if a weak password is caught
    @org.junit.Test
    public void testCheckPasswordComplexityInvalid() {
        assert !loginSystem.checkPasswordComplexity("password");
    }

    // Test if phone number is valid
    @org.junit.Test
    public void testCheckCellPhoneNumberValid() {
        assert loginSystem.checkCellPhoneNumber("+27338963976");
    }

    // Test if phone number is invalid
    @org.junit.Test
    public void testCheckCellPhoneNumberInvalid() {
        assert !loginSystem.checkCellPhoneNumber("08966553");
    }

    // Test full registration with good info
    @org.junit.Test
    public void testRegisterUserValid() {
        String result = loginSystem.registerUser("kyl_1", "Ch&&sec@ke99!", "+27338963976");
        assert result.equals("User registered successfully.");
    }

    // Test with bad username
    @org.junit.Test
    public void testRegisterUserInvalidUserName() {
        String result = loginSystem.registerUser("kyle!!!!!!", "Ch&&sec@ke99!", "+27338963976");
        assert result.equals("Username is not correctly formatted, please ensure that your username contains an underscore and is no more than five characters in length.");
    }

    // Test with bad password
    @org.junit.Test
    public void testRegisterUserInvalidPassword() {
        String result = loginSystem.registerUser("kyl_1", "password", "+27338963976");
        assert result.equals("Password is not correctly formatted; please ensure that the password contains at least eight characters, a capital letter, a number, and a special character.");
    }

    // Test with bad phone number
    @org.junit.Test
    public void testRegisterUserInvalidCellNumber() {
        String result = loginSystem.registerUser("kyl_1", "Ch&&sec@ke99!", "08966553");
        assert result.equals("Cell phone number incorrectly formatted or does not contain international code.");
    }

    // Test login with correct info
    @org.junit.Test
    public void testLoginUserValid() {
        loginSystem.registerUser("kyl_1", "Ch&&sec@ke99!", "+27338963976");
        assert loginSystem.loginUser("kyl_1", "Ch&&sec@ke99!");
    }

    // Test login with wrong password
    @org.junit.Test
    public void testLoginUserInvalid() {
        loginSystem.registerUser("kyl_1", "Ch&&sec@ke99!", "+27338963976");
        assert !loginSystem.loginUser("kyl_1", "wrongpassword");
    }
} 

