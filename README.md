# $${\color{red} \textbf{Project}: \textbf{Student}  \ \textbf{App}}$$


### $\color{{green} \ \textbf{Prerequisite:}}$    
- Ec2 instance 
- Java-1.8 
- Tomcat 
- Git 
- RDS 

### ${\color{blue} \textbf{Launch} \textbf{EC2}  \ \textbf{Instance}}$
Allow Ports security group: 
22 = SSH 
8080 = Tomcat 
3306 = Mysql / Mariadb

![instance](https://github.com/abhipraydhoble/Project-Student-App/assets/122669982/d7851745-1bfe-4f92-b7bb-18555f2dfd45)

$\color{red}{Connect \ to \ instance:}$

![connect](https://github.com/abhipraydhoble/Project-Student-App/assets/122669982/727778ca-e9ee-43c9-ab85-ff055f94d4a2)

![cli](https://github.com/abhipraydhoble/Project-Student-App/assets/122669982/0e6244e1-489c-42c1-ae89-27c8b7c37792)

- $\color{red}{install \ java }$
````
yum install java-1.8* -y 
````
- $\color{red}{Install \ Tomcat }$
Search tomcat 8 download  on browser

![tomcat](https://github.com/abhipraydhoble/Project-Student-App/assets/122669982/8e622609-b7df-4f26-b8e3-e787e5e16c95)

 ````
wget  https://dlcdn.apache.org/tomcat/tomcat-8/v8.5.99/bin/apache-tomcat-8.5.99.zip

unzip apache-tomcat-8.5.99.zip 
cd  apache-tomcat-8.5.99.zip 
cd bin 
[catalina.sh  -->this file is neccessary to start tomcat] 
chmod +x catalina.sh     [ give execute permission to file] 
````
### ${\color{blue} \textbf{Start} \textbf{Stop}  \ \textbf{Tomcat}}$
````
sh catalina.sh start   [ tomcat started ]
sh catalina.sh stop 
````
go to browser and public ip:8080

### ${\color{blue} \textbf{SetUp} \textbf{Student}  \ \textbf{Applicationt}}$
````
yum install git -y 
git clone https://github.com/abhipraydhoble/Student-App-Project.git 
cd Student-App-Project 
````
$\color{red}{Copy \ file \ from \ git \ directory \ to \ Tomcat}$

````
cp Student-App-Project/student.war apache-tomcat-8.5.93/webapps/ 
cp Student-App-Project/mysql-connector.jar apache-tomcat-8.5.93/lib/ 
````
### ${\color{blue} \textbf{Create} \textbf{Database}  \ \textbf{in} \ \textbf{RDS}}$
Go to RDS
download mariadb-server using  below command

````
yum install mariadb105-server -y
systemctl start mariadb    
systemctl enable mariadb  
systemctl status mariadb
````

### ${\color{blue} \textbf{Log} \textbf{Into}  \ \textbf{Database}}$

````
mysql -h "database-1.cxqukacgq5pj.us-east-1.rds.amazonaws.com"   -u admin -pPasswd123$
````
Note: replace rds-endpoint with actual endpoint value

```sql
show databases;
create database  studentapp;
use studentapp;
```
 
#### ${\color{blue} \textbf{Create} \textbf{Table}  \ \textbf{in} \ \textbf{DB}}$

```sql
 CREATE TABLE if not exists students(student_id INT NOT NULL AUTO_INCREMENT,  
	student_name VARCHAR(100) NOT NULL,  
	student_addr VARCHAR(100) NOT NULL,   
	student_age VARCHAR(3) NOT NULL,      
	student_qual VARCHAR(20) NOT NULL,     
	student_percent VARCHAR(10) NOT NULL,   
	student_year_passed VARCHAR(10) NOT NULL,  
	PRIMARY KEY (student_id)  
);
```
Logout from database:
```sql
 exit
```

## Modify apache-tomcat/conf/context.xml file

```
cd apache-tomcat-8.5.93/conf
vim context.xml
````
add below line [connection string] at line 21
````
 <Resource name="jdbc/TestDB" auth="Container" type="javax.sql.DataSource"
               maxTotal="100" maxIdle="30" maxWaitMillis="10000"
               username="USERNAME" password="PASSWORD" driverClassName="com.mysql.jdbc.Driver"
               url="jdbc:mysql://DB-ENDPOINT:3306/DATABASE"/>

````
* Change  
1.Username  
2.Password   
3.DB-ENDPOINT  
4.DATABASE Name 

$\color{blue}{Start \ tomcat}$
````
cd apache-tomcat-8.5.93/bin
./catalina.sh start or  sh catalina.sh start
````

- $\color{red}{google hit}$
IP:8080/student
