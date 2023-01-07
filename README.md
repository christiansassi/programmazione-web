## Installation

1.  Download and install the latest version of [Apache Derby](https://db.apache.org/). You can find the download link [here](https://db.apache.org/derby/derby_downloads.html).
2.  Extract the downloaded archive to a location on your computer, for example `C:\Derby`.
3.  Set the `DERBY_HOME` environment variable to the path of the Derby installation folder. For example, if you extracted the Derby files to `C:\Derby`, set `DERBY_HOME` to `C:\Derby`. To do this, you can open a new terminal window as an administrator and type:
```bash 
setx DERBY_HOME "your derby path" 
```

### Add a database (optional)

You can add a new database by opening a new terminal window and typing:
```bash
java -jar "%DERBY_HOME%\lib\derbyrun.jar" ij
```

If everything went smootly, you should see something like `ij>` in the console. Next, to create and connect to a new database, type:
```bash
ij> CONNECT 'jdbc:derby:myDB;create=true';
```
> **Note**: you can replace 'myDB' with the name of your choice. In addition, the `create=true` flag will create the table if it doesn't exist.

### Create a table (optional)

While staying connected to the database (see [above](#add-a-database-optional)), type:
```sql
ij> CREATE TABLE myTable (ID INT PRIMARY KEY, NAME VARCHAR(100));
```

### Populate a table (optional)

While staying connected to the database (see [above](#add-a-database-optional)), type:
```sql
ij> INSERT INTO myTable VALUES (10,'TEN'),(20,'TWENTY'),(30,'THIRTY');
```

## Working with the Apache Derby server

You can start Derby server by opening a new terminal window and typing:
```bash
java -jar %DERBY_HOME%\lib\derbyrun.jar server start
```
> **Note**: if you close this window, the server will stop running. 

And, of course, to stop the server, type:
```bash
java -jar %DERBY_HOME%\lib\derbyrun.jar server shutdown
```

The server will run on port `1527` unless specified otherwise. At the time of writing this guide, port 1527 is the default. For more information, you can refer to the documentation [here](https://db.apache.org/derby/quick_start.html).

## Set up a Derby Database in IntelliJ IDEA

1. In the main menu, go to **View** > **Tool Windows** > **Database**.
2. In the **Database** tool window, click the **+** icon and select **Data Source** > **Derby**.
3. In the **Driver files** field, browse to the `derby.jar` file in the `DERBY_HOME/lib` folder.
4. In the **Name** field, enter a name for the database, for example *mydb*.
5. In the **Host** field, enter *localhost*.
6. In the **Port** field, enter the port used by Apache Derby server. The default one is *1527*.
> **Note**: be sure that `Connection type` is set on `default` and `Driver` on `Apache Derby (Remote)`.

Finally, test the connection:
1.  Click **Test Connection** to ensure that the connection to the Derby database is successful.
2.  Click **OK** to finish setting up the Derby database.

## Add Derby JDBC Driver to Apache Tomcat

1. Download **Derby JDBC Driver** [here](https://dbschema.com/jdbc-driver/Derby.html).
2. Once the downloaded is completed, extract all the files inside the **lib** folder of Apache Tomcat.

## Quick start

Here are some examples on how to use Apache Derby in Java.
```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

public class DerbyConnect {
  public static void main(String[] args) throws SQLException {
    // specify the location of the database
    String dbURL = "jdbc:derby://localhost:1527/myDB";

    // specify the connection properties
    String username = "";  // default username is an empty string
    String password = "";  // default password is an empty string

    // connect to the database
    Connection conn = DriverManager.getConnection(dbURL, username, password);
    
    // create a statement
    Statement stmt = conn.createStatement();

    // execute the query
    ResultSet rs = stmt.executeQuery("SELECT * FROM myTable");

    // process the result set
    while (rs.next()) {
      int id = rs.getInt("ID");
      String name = rs.getString("NAME");
      System.out.println("ID: " + id + ", Name: " + name);
    }

    // close the result set, statement, and connection
    rs.close();
    stmt.close();
    conn.close();
  }
}
```
In this example, the **SELECT** query is used to retrieve all rows from the myTable table. The `ResultSet` object returned by the executeQuery method is used to process the result set.

To execute an SQL **INSERT**, **UPDATE**, or **DELETE** query, you can use the `Statement.executeUpdate` method, which returns the number of rows affected by the query.
```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

public class DerbyConnect {
  public static void main(String[] args) throws SQLException {
    // specify the location of the database
    String dbURL = "jdbc:derby://localhost:1527/myDB";

    // specify the connection properties
    String username = "";  // default username is an empty string
    String password = "";  // default password is an empty string

    // connect to the database
    Connection conn = DriverManager.getConnection(dbURL, username, password);
    
    // create a statement
    Statement stmt = conn.createStatement();
    
    // Execute an INSERT query
    int count = stmt.executeUpdate("INSERT INTO MYTABLE (ID, NAME) VALUES (40, 'FORTY')");

    // Print the number of rows affected
    System.out.println(count + " rows affected");
    
    stmt.close();
    conn.close();
  }
}
```
