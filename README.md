# DataMig
import java.sql.*;

public class DataMigration {
    public static void main(String[] args) {
        String sourceURL = "jdbc:mysql://source_host/source_database";
        String sourceUsername = "source_username";
        String sourcePassword = "source_password";

        String targetURL = "jdbc:mysql://target_host/target_database";
        String targetUsername = "target_username";
        String targetPassword = "target_password";

        try {
            // Establish connections to source and target databases
            Connection sourceConnection = DriverManager.getConnection(sourceURL, sourceUsername, sourcePassword);
            Connection targetConnection = DriverManager.getConnection(targetURL, targetUsername, targetPassword);

            // Fetch data from source table
            String selectQuery = "SELECT * FROM source_table";
            Statement selectStatement = sourceConnection.createStatement();
            ResultSet resultSet = selectStatement.executeQuery(selectQuery);

            // Insert data into target table
            String insertQuery = "INSERT INTO target_table (column1, column2, column3) VALUES (?, ?, ?)";
            PreparedStatement insertStatement = targetConnection.prepareStatement(insertQuery);

            while (resultSet.next()) {
                // Replace columnX with actual column names from your tables
                insertStatement.setString(1, resultSet.getString("column1"));
                insertStatement.setInt(2, resultSet.getInt("column2"));
                insertStatement.setString(3, resultSet.getString("column3"));
                insertStatement.executeUpdate();
            }

            System.out.println("Data migration completed.");

            // Close connections
            sourceConnection.close();
            targetConnection.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}

