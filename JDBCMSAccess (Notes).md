import java.sql.*;
import java.util.Scanner;

public class JDBCMSAccess {
    public static void main(String[] args) throws SQLException {// m + TAB
        //throws SQLException --> method may throw an exception related to SQL operations

        //Create a reference to the Connection object, which allows us to red/write to the DBMS
        Connection con = DriverManager.getConnection("jdbc:ucanaccess://C:/Users/mukuwa.baffoe/MB.accdb");
        //Connects to Access Database(MB) [@path]
        //Uses DriverManager to establish connection

        //MB is the name of my Access database and .accdb is the file extension
        //Make sure the file path define above is correct
        //To check go to External Libraries and enter the path that the database is under, use path above
        // In database tools, click Compact and Repair --> clears bugs and errors

        //Create a reference to the STATEMENT object, which allows us to EXECUTE a SQL statement/query
        Statement st = con.createStatement();
        //creates Statement object (name:st) from the Connection object (name:con)
        //Statement obj: executes SQL queries against the database

        //////Optional Code///////
        // Scanner stringScanner = new Scanner(System.in);
        // String useInput = stringScanner.next();
        //how you do a sequel injection
        ////////////////////////

        ResultSet rs = st.executeQuery("Select * FROM Student");
        //executes SQL query to select all columns(*) from the Student table in the database
        //ResultSet (object)--> stores the result of the query, represents rows and columns returned SQL Query

        //Select * From Student --> hard code
        //Make sure the table within Access is named Student
        //The program will pull information within this table
        //Can change between tables

        //Get header row information
        ResultSetMetaData rsmd = rs.getMetaData();
        //Retrieves metadata about columns in result set (name: rs)
        //ResultSetMetaData [obj](name:rsmd)--> contains information (number of columns, column names, column types)


        //The metadata of a table is information about the table itself. How many columns, etc.
        //rsmd- result set metadata
        int numberColumns = rsmd.getColumnCount();
        //gets the number of columns from Acccess database


        //Print the header row, per se.
        for(int count =1; count <= numberColumns; count++) {
            //initalizes loop, states by setting count to 1 (index of the first column)
            //loop condition--> 'count<= numberColumns' makes loop continue until it reaches last column
            //'numberColumns--> represents total number of columns in the result set
            System.out.printf("%s\t", rsmd.getColumnName(count));
            //souf--> formats and prints name of each column to console (output/command window)
            //%s--> indicates string value will be inserted
            //%t--> adds tab character
        }//End: for
        //Loop prints column names (header row) of the result set (rs)
        //Iterates through each column & uses souf to print column name followed by a tab(\t)
        //Header row--> first row of the output, displays column names of table rerieved from database

        System.out.println();
       //After printing column names, add '\n' (new line character) to move cursor to next line of console
        //ensures next output will start on new line, seperate from the column headers


        //Loop through each row and print the table
        while (rs.next()) {
            System.out.println(rs.getString(1)+"\t" +rs.getString(2) +"\t" + rs.getString(3) + "\n");
            //loop iterates through each row of result set (nsme: rs)
            //for each row gets data from 1st three columns using 'rs.getString()'
            //sout--> prints to console seperated by tabs '\t'
            //after print, '\n' moves to the next line

            //Prints the data entries within the database
        }//End: while

        //VITAL: Close the connection
        rs.close();
        st.close();
        con.close();
        //close ResultSet, Statement, and Connection
        //releases database resources and frees up memory
        //IMPORTANT to close resources to avoid memory leader + ensure proper cleanup

    }//End:main
}//End:class
