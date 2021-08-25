# cs122b-spring21-team-123
cs122b-spring21-team-123 created by GitHub Classroom

Project 1 Demo: https://www.youtube.com/watch?v=vvDFi8NqOwo

Project 2 Demo: https://www.youtube.com/watch?v=b3ZibqCY7Kc

Project 3 Demo: https://www.youtube.com/watch?v=po1lpweHt9Q

Project 4 Demo: https://www.youtube.com/watch?v=7qeuAWTjwf8

Project 5 Demo: https://www.youtube.com/watch?v=JEkq8iVC1mw

# Deployment

To deploy the application, first use "mvn package" in the repo. Then copy the war file to the tomcat webapps folder using "cp ./target/\*.war /var/lib/tomcat9/webapps/". The war file should be able to be seen under the tomcat webapps folder using "ls -lah /var/lib/tomcat9/webapps/". Then navigate to the website.

# Substring Matching Design

In order to provide accurate yet fast search functionality, we utilized the LIKE operator in SQL to match wildcards in specific cases.

For the Title, we matched the parameter anywhere in the movie's details (e.g. "%title%") because users will not necessarily search titles exactly. This caused some performance overhead, but was deemed necessary.

On the other hand, for Director and Stars, we matched the parameter to be at the beginning of the data in order to use the indexing of the SQL database. This sped up search results dramatically.

The most important part of gaining performance was omitting SQL filters (e.g. WHERE) when no parameter was provided.

# Contributions:

Kyle:

Project1:

Setup the base files for the project with basic unfinished implementation. Created the create_table.sql file to create the moviedb database. Recorded and uploaded the demo video.

Project2:

Setup the newly needed base files for the project with basic unfinished implementations. Implemented login, cart and purchase features. Recorded and uploaded the demo video.

Project3:

Setup base files for the project with basic unfinished implementations. Implemented reCAPTCHA, Password Encryption, and XML Parsing. Recorded and uploaded the demo video.

Project4:

Implemented full-text search and autocomplete with caching.

Project5:

Added JDBC Connection Pooling, logging functionality, ran JMeter tests and created video.

Marcus:

Project1:

Implemented functionality of various pages through MySQL queries. Adjusted look of site with CSS. Debugged performance and backend issues.

Project2:

Implemented movie list filtering functionality including by title, year, director, star, genre, and first letter of title. Created search and browse pages. Optimized SQL queries and fixed backend issues.

Project3:

Changed all Statements to PreparedStatements, enabled HTTPS, created Employee login and Dashboard with Add Star, Add Movie, and Metadata functionalities.

Project4:

Implemented Android functionality and recorded the video.

Project5:

Set up Master/Slave instances, created Load Balancers

# Connection Pooling
  
  In our Fablix code, when a query needs to be made, our code pulls a connection from the JDBC connection pool. The connection is then used for whatever queries are needed and then closed when all queries have been retrieved. Connection pooling ensures that our code doesn't need to create a new connection whenever we need to access the MySql database. This ends up saving time because we don't need to spend extra resources creating a new JDBC connection and whatever data structures that are needed to do so.
  
  For two backend SQL, the connection pool contains connections to each of the two backend sql databases.


# Master/Slave
  In order to implement Master/Slave functionality, it was necessary to create a second Data Source to handle servlets that wrote to the database, such as PlaceOrder and DashboardAddStar. This Data Source is in context.xml and points to the Master MySQL instance. Therefore, whenever a servlet wrote to the database, it wrote to the Master instance which propagated to the Slave.
   

# JMeter TS/TJ Time Logs
  First at the end of the log_processing.java file, change the desired location for the outputted avg_log.txt processed file. Then run the file with command line arguments where each command line argument is the path to a log file that you wish to process (can handle multiple files). It will output a single avg_log.txt file that averages all of the log files from the command line arguments and converts from ns to ms.


# JMeter TS/TJ Time Measurement Report

| **Single-instance Version Test Plan**          | **Graph Results Screenshot** | **Average Query Time(ms)** | **Average Search Servlet Time(ms)** | **Average JDBC Time(ms)** | **Analysis** |
|------------------------------------------------|------------------------------|----------------------------|-------------------------------------|---------------------------|--------------|
| Case 1: HTTP/1 thread                          | ![](https://github.com/kjstrong/CS122B-Fablix/blob/main/img/case_1_scaled_screenshot.png)   | 54                         | 13                                  | 13                        | The single thread had a standard query time which was used to measure the baseline of the other tests.           |
| Case 2: HTTP/10 threads                        | ![](https://github.com/kjstrong/CS122B-Fabflix/main/img/case_2_single_screenshot.png)   | 93                         | 50                                  | 50                        | Due to the extra load of 10 threads, the query time was predictably higher than that of the single thread test.           |
| Case 3: HTTPS/10 threads                       | ![](https://github.com/kjstrong/CS122B-Fabflix/main/img/case_3_single_screenshot.png)   | 100                         | 54                                  | 54                        | Similar to Case 2, the extra load of 10 threads increased the query time in comparison to the single thread           |
| Case 4: HTTP/10 threads/No connection pooling  | ![](https://github.com/kjstrong/CS122B-Fabflix/main/img/case_4_single_screenshot.png)   | 163                         | 105                                 | 104                        | The lack of connection pooling vastly increased the query time in comparison to the other cases.          |

| **Scaled Version Test Plan**                   | **Graph Results Screenshot** | **Average Query Time(ms)** | **Average Search Servlet Time(ms)** | **Average JDBC Time(ms)** | **Analysis** |
|------------------------------------------------|------------------------------|----------------------------|-------------------------------------|---------------------------|--------------|
| Case 1: HTTP/1 thread                          | ![](https://github.com/kjstrong/CS122B-Fabflix/main/img/case_1_scaled_screenshot.png)   | 57                         | 15                                  | 15                        | In comparison to the single-instance version, the scaled version did not show much change with 1 thread due to the sticky session only using one of two sessions.          |
| Case 2: HTTP/10 threads                        | ![](https://github.com/kjstrong/CS122B-Fabflix/main/img/case_2_scaled_screenshot.png)   | 99                         | 57                                  | 57                        | The scaled version query time was similar to the single instance version in the case of 10 threads, possibly due to the sticky session.           |
| Case 3: HTTP/10 threads/No connection pooling  | ![](https://github.com/kjstrong/CS122B-Fabflix/main/img/case_3_scaled_screenshot.png)   | 166                         | 122                                 | 122                        | With no connection pooling, the servlet and JDBC times were slower in comparison to those of the single-instance version.           |

