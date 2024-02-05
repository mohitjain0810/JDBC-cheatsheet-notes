# `Batch Processing`

```
import java.sql.*;
import java.util.Scanner;

public class batchProcessing {
    public static void main(String[] args) {
        String url = "jdbc:mysql://localhost:3306/mydatabase";
        String username = "root";
        String password = "mohitjain08";
        String query = "INSERT INTO employees(id, name, job_title, salary) values (?, ?, ?, ?)";

        try{
            Class.forName("com.mysql.cj.jdbc.Driver");
            System.out.println("Driver loaded successfully");
        }catch (ClassNotFoundException e){
            System.out.println(e.getMessage());
        }

        try {
            Connection connection = DriverManager.getConnection(url, username, password);
            System.out.println("Connect Established Successfully");
            connection.setAutoCommit(false);

//     Statement statement = connection.createStatement();
//     statement.addBatch("INSERT INTO employees(id, name, job_title, salary) VALUES (7,'Vashu', 'HR manager', '65000.00')");
//     statement.addBatch("INSERT INTO employees(id, name, job_title, salary) VALUES (8, 'Karan', 'UI/ux', '89000.00')");
//     statement.addBatch("INSERT INTO employees(id, name, job_title, salary) VALUES (9, 'Kanha', 'web designer', '90000.00')");

//     int[] batchresult = statement.executeBatch();

//     connection.commit();

//     System.out.println("Batch execute successfully!!");

            //by user
            PreparedStatement preparedStatement = connection.prepareStatement(query);

            Scanner scanner = new Scanner(System.in);

            while (true){
                System.out.print("Enter id: ");
                int id = scanner.nextInt();

                System.out.print("Enter name: ");
                String name = scanner.nextLine();
                scanner.nextLine();

                System.out.print("Enter job title: ");
                String jobTitle = scanner.nextLine();

                System.out.print("Enter salary: ");
                double salary = scanner.nextDouble();

                preparedStatement.setInt(1,id);
                preparedStatement.setString(2,name);
                preparedStatement.setString(3,jobTitle);
                preparedStatement.setDouble(4,salary);

                preparedStatement.addBatch();

                System.out.println("Add more values. Y/N ");
                String decision = scanner.nextLine();
                scanner.nextLine();

                if(decision.toUpperCase().equals("N")){
                    break;
                }
            }
            int[] batchResult = preparedStatement.executeBatch();

            connection.commit();

            System.out.println("Batch execute successful");

        }catch (SQLException e){
                System.out.println(e.getMessage());
        }
    }
}
```
