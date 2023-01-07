## Installation

1.  Download and install the latest version of [Apache Derby](https://db.apache.org/). You can find the download link [here](https://db.apache.org/derby/derby_downloads.html).
2.  Extract the downloaded archive to a location on your computer, for example `C:\Derby`.
3.  Set the `DERBY_HOME` environment variable to the path of the Derby installation folder. For example, if you extracted the Derby files to `C:\Derby`, set `DERBY_HOME` to `C:\Derby`. To do this, you can open a new terminal window and type:
```bash 
set DERBY_HOME="your derby path" 
```

### Add a database (optional)

You can add a new database by opening a new terminal window and typing:
```bash
java -jar %DERBY_HOME%\lib\derbyrun.jar ij
```

If everything went smootly, you should see something like `ij>` in the console. Next, to create and connect to a new database, type:
```bash
ij> CONNECT 'jdbc:derby:mydb;create=true';
```
> **Note**: you can replace 'mydb' with the name of your choice. In addition, the `create=true` flag will create the table if it doesn't exist.

### Populate a database (optional)

While staying connected to the database (see above), type:
```sql
ij> INSERT INTO FIRSTTABLE VALUES (10,'TEN'),(20,'TWENTY'),(30,'THIRTY');
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

The server will run on port `1527` unless specified otherwise. At the time of writing this guide, port 1527 is the default. For more information, you can refer to the documentation [here](https://db.apache.org/derby/quick_start.html)

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
