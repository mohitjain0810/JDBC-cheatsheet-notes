# `Image Handling `
``` 
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.OutputStream;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

public class ImageHandler {

    private static final String URL = "jdbc:mysql://localhost:3306/mydatabase";
    private static final String USERNAME = "root";
    private static final String PASSWORD = "mohitjain08";

    private Connection connection;

    public ImageHandler() {
        try {
            Class.forName("com.mysql.cj.jdbc.Driver");
            connection = DriverManager.getConnection(URL, USERNAME, PASSWORD);
            System.out.println("Driver loaded and connection established successfully");
        } catch (ClassNotFoundException | SQLException e) {
            throw new RuntimeException(e);
        }
    }

    public void retrieveImage(int imageId, String folderPath) {
        try {
            String query = "SELECT image_data FROM image_table WHERE image_id = ?";
            PreparedStatement preparedStatement = connection.prepareStatement(query);
            preparedStatement.setInt(1, imageId);
            ResultSet resultSet = preparedStatement.executeQuery();

            if (resultSet.next()) {
                byte[] imageData = resultSet.getBytes("image_data");
                String imagePath = folderPath + "extractedImage.jpg";
                OutputStream outputStream = new FileOutputStream(imagePath);
                outputStream.write(imageData);
                System.out.println("Image retrieved successfully");
            } else {
                System.out.println("Image not found");
            }
        } catch (SQLException | IOException e) {
            throw new RuntimeException(e);
        }
    }

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
}
```
- `Now, in your Main class, you can use these classes as follows:`

```
public class Main {

    public static void main(String[] args) {
        PreparedStatementsHandler preparedStatementHandler = new PreparedStatementsHandler();
        ImageHandler imageHandler = new ImageHandler();

        // Example usage of retrieveData
        ResultSet resultSet = preparedStatementHandler.retrieveData("Hemant", "Devops Developer");
        // Process the result set as needed

        // Example usage of insertData
        int rowsAffected = preparedStatementHandler.insertData(5, "John", "Backend Developer", 50000.0);
        if (rowsAffected > 0) {
            System.out.println("Insertion successful. " + rowsAffected + " row(s) affected");
        } else {
            System.out.println("Insertion failed");
        }

        // Example usage of retrieveImage
        imageHandler.retrieveImage(1, "D:\\image_folder_by_jdbc\\");

        // Close connections when done
        preparedStatementHandler.closeConnection();
        imageHandler.closeConnection();
    }
}
```