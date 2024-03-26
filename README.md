# Project : Student App

### GITHUB REPOSITORY LINK:
https://github.com/abhipraydhoble/Student-App-Project.git

### Prerequisite:
Ec2 instance </br>
Java-1.8 </br>
Tomcat </br>
Git </br>
RDS </br>

## LAUNCH EC2 INSTANCE
Allow Ports security group: </br>
22 = SSH </br>
8080 = Tomcat </br>
3306 = Mysql / Mariadb </br>

Connect to instance:
java install-1.8  </br>
# yum install java-1.8* -y </br>

Install Tomcat 
Search tomcat 8 download  on browser </br>
$ wget  https://dlcdn.apache.org/tomcat/tomcat-8/v8.5.99/bin/apache-tomcat-8.5.99.zip

$ unzip apache-tomcat-8.5.99.zip </br>
$ cd  apache-tomcat-8.5.99.zip </br>
$ cd bin </br>
[catalina.sh  -->this file is neccessary to start tomcat] </br>
$ chmod +x catalina.sh     [ give execute permission to file] </br>

### Start and Stop Tomcat using this command: </br>
$ sh catalina.sh start   [ tomcat started ] </br>
$ sh catalina.sh stop </br>

go to browser and public ip:8080

## SETUP STUDENT APPLICATION </br>

$ yum install git -y </br>
$ git clone https://github.com/abhipraydhoble/Student-App-Project.git </br>
$ cd Student-App-Project </br>

 *** Copy file from git directory to Tomcat ***</br>

# cp Student-App-Project/student.war apache-tomcat-8.5.93/webapps/ </br>
# cp Student-App-Project/mysql-connector.jar apache-tomcat-8.5.93/lib/ </br>

## SETUP DATABASE IN RDS:
Go to RDS
download mariadb-server using  below command

$ dnf install mariadb105-server </br>
$ systemctl start mariadb    </br>
$ systemctl enable mariadb  </br>
$ systemctl status mariadb  </br>

### Log in into database

<Mariadb> Create database with name studentapp  </br>
<Mariadb> Create database studentapp;    </br>
<Mariadb> Use studentapp;   --> Switch to newly created database   </br>

### Run this query to create  table:

 CREATE TABLE if not exists students(student_id INT NOT NULL AUTO_INCREMENT,
	student_name VARCHAR(100) NOT NULL,
	student_addr VARCHAR(100) NOT NULL,
	student_age VARCHAR(3) NOT NULL,
	student_qual VARCHAR(20) NOT NULL,
	student_percent VARCHAR(10) NOT NULL,
	student_year_passed VARCHAR(10) NOT NULL,
	PRIMARY KEY (student_id)
);

Logout from database:
<Mariadb> exit

 ## MODIFY context.xml:

$ cd apache-tomcat-8.5.93/conf
$ vim context.xml
add below line [connection string] at line 21

<Resource name="jdbc/TestDB" auth="Container" type="javax.sql.DataSource"
               maxTotal="100" maxIdle="30" maxWaitMillis="10000"
               username="USERNAME" password="PASSWORD" driverClassName="com.mysql.jdbc.Driver"
               url="jdbc:mysql://DB-ENDPOINT:3306/DATABASE"/>


* Change 
1.Username
2.Password
3.DB-ENDPOINT 
4.DATABASE Name

Start tomcat
$ cd apache-tomcat-8.5.93/bin
$ ./catalina.sh start or  sh catalina.sh start

google hit
IP:8080/student
