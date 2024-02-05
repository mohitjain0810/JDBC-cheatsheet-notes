# `Pre Pared Statement Handling`
```
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

public class Main {

    private static final String URL = "jdbc:mysql://localhost:3306/mydatabase";
    private static final String USERNAME = "root";
    private static final String PASSWORD = "mohitjain08";

    private Connection connection;

    public Main() {
        try {
            Class.forName("com.mysql.cj.jdbc.Driver");
            connection = DriverManager.getConnection(URL, USERNAME, PASSWORD);
            System.out.println("Driver loaded and connection established successfully");
        } catch (ClassNotFoundException | SQLException e) {
            throw new RuntimeException(e);
        }
    }

    public ResultSet retrieveData(String name, String jobTitle) {
        try {
            String query = "SELECT * FROM employees WHERE name = ? AND job_title = ?";
            PreparedStatement preparedStatement = connection.prepareStatement(query);
            preparedStatement.setString(1, name);
            preparedStatement.setString(2, jobTitle);
            return preparedStatement.executeQuery();
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }

    public int insertData(int id, String name, String jobTitle, double salary) {
        try {
            String query = "INSERT INTO employees (id, name, job_title, salary) VALUES (?, ?, ?, ?)";
            PreparedStatement preparedStatement = connection.prepareStatement(query);
            preparedStatement.setInt(1, id);
            preparedStatement.setString(2, name);
            preparedStatement.setString(3, jobTitle);
            preparedStatement.setDouble(4, salary);
            return preparedStatement.executeUpdate();
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }

    // Add more methods for update and delete if needed

    public void closeConnection() {
        try {
            if (connection != null && !connection.isClosed()) {
                connection.close();
                System.out.println("Connection closed successfully");
            }
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }

    public static void main(String[] args) {
        Main main = new Main();

        // Example usage of retrieveData
        ResultSet resultSet = main.retrieveData("Hemant", "Devops Developer");
        // Process the result set as needed

        // Example usage of insertData
        int rowsAffected = main.insertData(5, "John", "Backend Developer", 50000.0);
        if (rowsAffected > 0) {
            System.out.println("Insertion successful. " + rowsAffected + " row(s) affected");
        } else {
            System.out.println("Insertion failed");
        }

        // Close connection when done
        main.closeConnection();
    }
}
```