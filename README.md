import java.sql.*;

public class registration{

    private Connection connection;

    public registration(String url, String user, String password) {
        try {
            connection = DriverManager.getConnection(url, user, password);
            System.out.println("database is connected");
        } catch (SQLException e) {
            System.err.println("Error connecting to the database: " + e.getMessage());
        }
    }

    // Create
    public void createRegistration(String name, String email, String dob, String phone) {
        String query = "INSERT INTO Registration (Name, Email, DateOfBirth, Phone) VALUES (?, ?, ?, ?)";
        try (PreparedStatement stmt = connection.prepareStatement(query)) {
            stmt.setString(1, name);
            stmt.setString(2, email);
            stmt.setDate(3, Date.valueOf(dob)); // Convert string to SQL Date
            stmt.setString(4, phone);
            stmt.executeUpdate();
            System.out.println("Registration created successfully");
        } catch (SQLException e) {
            System.err.println("Error creating registration: " + e.getMessage());
        }
    }

    // Read
    public void readRegistration(int id) {
        String query = "SELECT * FROM Registration WHERE ID = ?";
        try (PreparedStatement stmt = connection.prepareStatement(query)) {
            stmt.setInt(1, id);
            ResultSet rs = stmt.executeQuery();
            if (rs.next()) {
                System.out.println("ID: " + rs.getInt("ID"));
                System.out.println("Name: " + rs.getString("Name"));
                System.out.println("Email: " + rs.getString("Email"));
                System.out.println("Date of Birth: " + rs.getDate("DateOfBirth"));
                System.out.println("Phone: " + rs.getString("Phone"));
                System.out.println("Created At: " + rs.getTimestamp("CreatedAt"));
            } else {
                System.out.println("No record found with ID: " + id);
            }
        } catch (SQLException e) {
            System.err.println("Error reading registration: " + e.getMessage());
        }
    }

    // Update
    public void updateRegistration(int id, String name, String email, String dob, String phone) {
        String query = "UPDATE Registration SET Name = ?, Email = ?, DateOfBirth = ?, Phone = ? WHERE ID = ?";
        try (PreparedStatement stmt = connection.prepareStatement(query)) {
            stmt.setString(1, name);
            stmt.setString(2, email);
            stmt.setDate(3, Date.valueOf(dob));
            stmt.setString(4, phone);
            stmt.setInt(5, id);
            int rowsUpdated = stmt.executeUpdate();
            if (rowsUpdated > 0) {
                System.out.println("Registration updated successfully!");
            } else {
                System.out.println("No record found with ID: " + id);
            }
        } catch (SQLException e) {
            System.err.println("Error updating registration: " + e.getMessage());
        }
    }

    // Delete
    public void deleteRegistration(int id) {
        String query = "DELETE FROM Registration WHERE ID = ?";
        try (PreparedStatement stmt = connection.prepareStatement(query)) {
            stmt.setInt(1, id);
            int rowsDeleted = stmt.executeUpdate();
            if (rowsDeleted > 0) {
                System.out.println("Registration deleted successfully!");
            } else {
                System.out.println("No record found with ID: " + id);
            }
        } catch (SQLException e) {
            System.err.println("Error deleting registration: " + e.getMessage());
        }
    }

    public static void main(String[] args) {
        // Initialize the database connection
        String url = "jdbc:mysql://localhost:3306/myjlcdb";
        String user = "root";
        String password = "trisha";

        registration db = new registration(url, user, password);

   db.createRegistration("Trisha", "trisha.@gmail.com", "2004-07-26", "123456789");
   db.readRegistration(1);
   db.updateRegistration(1, "Prasad", "prasad.@gmail.com", "1995-07-20", "9862214321");
   db.deleteRegistration(1);
    }
}
