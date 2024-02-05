# `Simple Transaction Processing `

```
import java.io.FileNotFoundException;
import java.io.IOException;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.SQLException;

public class transactionJDBC {
    public static void main(String[] args) {
        String url = "jdbc:mysql://localhost:3306/mydatabase";
        String username = "root";
        String password = "mohitjain08";
        String withdrawQuery = "UPDATE accounts SET balance = balance - ? WHERE account_number = ?";
        String depositQuery = "UPDATE accounts SET balance = balance + ? WHERE account_number = ?";
        try{
            Class.forName("com.mysql.cj.jdbc.Driver");
            System.out.println("Driver loaded successfully");
        }catch (ClassNotFoundException e){
            System.out.println(e.getMessage());
        }

        try {
            Connection con = DriverManager.getConnection(url, username, password);
            System.out.println("Connect Established Successfully");
            con.setAutoCommit(false);

            try {
                PreparedStatement withdrawStatement = con.prepareStatement(withdrawQuery);
                PreparedStatement depositStatement = con.prepareStatement(depositQuery);

                withdrawStatement.setDouble(1,500.00);
                withdrawStatement.setString(2,"account123");

                depositStatement.setDouble(1,500.00);
                depositStatement.setString(2,"account456");
                
                int rowsAffectedWithdraw = withdrawStatement.executeUpdate();
                int rowsAffectedDeposit = depositStatement.executeUpdate();

                //transaction handling
                if(rowsAffectedWithdraw>0 && rowsAffectedDeposit>0){
                    con.commit();
                    System.out.println("Transaction Successful");
                }else {
                    con.rollback();
                    System.out.println("Transaction failed!");
                }
            }catch (SQLException e){
                System.out.println(e.getMessage());
            }

        }catch (SQLException e){
            System.out.println(e.getMessage());
        }
    }
}
```