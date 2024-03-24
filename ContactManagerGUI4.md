import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.sql.*;

public class ContactManagerGUI4 extends JFrame implements ActionListener {
    private JButton searchButton, inputButton;
    private JPanel mainPanel;

    public ContactManagerGUI4() {
        setTitle("Contact Manager");
        setSize(400, 150);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

        // Create buttons
        searchButton = new JButton("Search for Contact");
        inputButton = new JButton("Input Contact Information");

        // Add action listeners
        searchButton.addActionListener(this);
        inputButton.addActionListener(this);

        // Create panel for buttons
        JPanel buttonPanel = new JPanel(new GridLayout(1, 2));
        buttonPanel.add(searchButton);
        buttonPanel.add(inputButton);

        // Create main panel
        mainPanel = new JPanel(new BorderLayout());
        mainPanel.add(new JLabel("Please select an action:"), BorderLayout.NORTH);
        mainPanel.add(buttonPanel, BorderLayout.CENTER);

        // Add main panel to the frame
        getContentPane().add(mainPanel);
    }

    @Override
    public void actionPerformed(ActionEvent e) {
        if (e.getSource() == searchButton) {
            openSearchGUI();
        } else if (e.getSource() == inputButton) {
            openInputGUI();
        }
    }

    // Method to open GUI for searching contact
    private void openSearchGUI() {
        dispose(); // Close the current window
        JFrame searchFrame = new JFrame("Search for Contact");
        searchFrame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

        // Create input fields
        JTextField firstNameField = new JTextField(20);
        JTextField lastNameField = new JTextField(20);

        // Create buttons
        JButton searchButton = new JButton("Search");
        JButton backButton = new JButton("Back");

        // Add action listeners
        searchButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                searchContact(firstNameField.getText(), lastNameField.getText());
            }
        });
        backButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                searchFrame.dispose(); // Close the search window
                openMainGUI(); // Re-open the main GUI
            }
        });

        // Create panel for input fields and buttons
        JPanel inputPanel = new JPanel(new GridLayout(3, 2));
        inputPanel.add(new JLabel("First Name:"));
        inputPanel.add(firstNameField);
        inputPanel.add(new JLabel("Last Name:"));
        inputPanel.add(lastNameField);
        inputPanel.add(searchButton);
        inputPanel.add(backButton);

        // Add input panel to the search frame
        searchFrame.getContentPane().add(inputPanel);
        searchFrame.pack();
        searchFrame.setLocationRelativeTo(null); // Center the search frame
        searchFrame.setVisible(true);
    }

    // Method to open GUI for inputting contact information
    private void openInputGUI() {
        dispose(); // Close the current window
        JFrame inputFrame = new JFrame("Input Contact Information");
        inputFrame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

        // Create input fields
        JTextField firstNameField = new JTextField(20);
        JTextField lastNameField = new JTextField(20);
        JTextField zipCodeField = new JTextField(20);
        JTextField phoneNumberField = new JTextField(20);
        JTextField emailField = new JTextField(20);

        // Create buttons
        JButton inputButton = new JButton("Input");
        JButton backButton = new JButton("Back");

        // Add action listeners
        inputButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                insertContact(firstNameField.getText(), lastNameField.getText(), zipCodeField.getText(), phoneNumberField.getText(), emailField.getText());
            }
        });
        backButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                inputFrame.dispose(); // Close the input window
                openMainGUI(); // Re-open the main GUI
            }
        });

        // Create panel for input fields and buttons
        JPanel inputPanel = new JPanel(new GridLayout(6, 2));
        inputPanel.add(new JLabel("First Name:"));
        inputPanel.add(firstNameField);
        inputPanel.add(new JLabel("Last Name:"));
        inputPanel.add(lastNameField);
        inputPanel.add(new JLabel("Zip Code:"));
        inputPanel.add(zipCodeField);
        inputPanel.add(new JLabel("Phone Number:"));
        inputPanel.add(phoneNumberField);
        inputPanel.add(new JLabel("Email:"));
        inputPanel.add(emailField);
        inputPanel.add(inputButton);
        inputPanel.add(backButton);

        // Add input panel to the input frame
        inputFrame.getContentPane().add(inputPanel);
        inputFrame.pack();
        inputFrame.setLocationRelativeTo(null); // Center the input frame
        inputFrame.setVisible(true);
    }

    // Method to open the main GUI
    private void openMainGUI() {
        JFrame mainFrame = new JFrame("Contact Manager");
        mainFrame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        mainFrame.getContentPane().add(mainPanel);
        mainFrame.pack();
        mainFrame.setLocationRelativeTo(null); // Center the main frame
        mainFrame.setVisible(true);
    }

    // Method to insert a new contact into the database
    private void insertContact(String firstName, String lastName, String zipCode, String phoneNumber, String email) {
        try {
            Connection con = DriverManager.getConnection("jdbc:ucanaccess://C:/Users/mukuw/MB.accdb");
            String query = "INSERT INTO Student (FirstName, LastName, ZipCode, PhoneNumber, Email) VALUES (?, ?, ?, ?, ?)";
            PreparedStatement ps = con.prepareStatement(query);
            ps.setString(1, firstName);
            ps.setString(2, lastName);
            ps.setString(3, zipCode);
            ps.setString(4, phoneNumber);
            ps.setString(5, email);
            ps.executeUpdate();
            JOptionPane.showMessageDialog(null, "Contact added successfully!");
            con.close();
        } catch (SQLException ex) {
            JOptionPane.showMessageDialog(null, "Error: " + ex.getMessage());
        }
    }

    // Method to search for a contact based on first name and last name
    private void searchContact(String firstName, String lastName) {
        try {
            Connection con = DriverManager.getConnection("jdbc:ucanaccess://C:/Users/mukuw/MB.accdb");
            String query = "SELECT * FROM Student WHERE FirstName = ? AND LastName = ?";
            PreparedStatement ps = con.prepareStatement(query);
            ps.setString(1, firstName);
            ps.setString(2, lastName);
            ResultSet rs = ps.executeQuery();

            if (rs.next()) {
                String foundFirstName = rs.getString("FirstName");
                String foundLastName = rs.getString("LastName");
                String zipCode = rs.getString("ZipCode");
                String phoneNumber = rs.getString("PhoneNumber");
                JOptionPane.showMessageDialog(null, "Contact found:\n" +
                        "First Name: " + foundFirstName + "\n" +
                        "Last Name: " + foundLastName + "\n" +
                        "Zip Code: " + zipCode + "\n" +
                        "Phone Number: " + phoneNumber);
            } else {
                JOptionPane.showMessageDialog(null, "Contact not found.");
            }
            con.close();
        } catch (SQLException ex) {
            JOptionPane.showMessageDialog(null, "Error: " + ex.getMessage());
        }
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> {
            ContactManagerGUI3 gui = new ContactManagerGUI3();
            gui.setVisible(true);
        });
    }
}
