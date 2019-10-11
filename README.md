# MariaDB JDBC test 

Sample maven application featuring MariaDB Connector/J JDBC driver.

The `pom.xml` file of this project is configured with `mariaDB` dependency to quickly test SQL instructions through the JDBC driver.     
The `slf4j` dependency was also added for logging proposes.

## Build and Run the application

Amend your application as you wish in order to perform the JDBC tests. 
An example of the configuration can be as follows:

The parameter `profileSql=true` can be useful for debugging query execution times :) 

```java
import java.sql.*;

public class App {
    
    public static void main( String[] args ) throws SQLException {
        Connection connection = null;
        ResultSet resultSet = null;

        try {
            connection = DriverManager.getConnection("jdbc:mariadb://localhost:3306/DB?user=root&password=myPassword&profileSql=true&log=true");
            final Statement statement = connection.createStatement(ResultSet.TYPE_FORWARD_ONLY, ResultSet.CONCUR_READ_ONLY);
            resultSet = statement.executeQuery(QUERY);
            
            if(resultSet.next()){
                System.out.println(resultSet.getString(1));
            }

        } finally {
            if(resultSet != null){
                resultSet.close();
            }

            if(connection != null){
                connection.close();
            }
        }
    }
}
```

Now we just need to compile and run the application with the following commands:

```
mvn compile
mvn exec:java -Dexec.mainClass="com.example.App" 
```

Alternatively since the `pom.xml` is configured with the `maven-shade-plugin` we can run the compiled `.jar` with: 

```
mvn package
cd target
java -jar mariadb-jdbc-test-1.0-SNAPSHOT.jar
```

## References

* https://github.com/MariaDB/mariadb-connector-j
* https://mariadb.com/kb/en/library/about-mariadb-connector-j/#using-drivermanager
* http://www.slf4j.org/manual.html
