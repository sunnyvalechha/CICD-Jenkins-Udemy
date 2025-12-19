Step 1 - We got a project requirement.
Step 2 - Developers & DevOps team gather to discuss about tools & tech used in the project what is the time to deploy the application on lower env.
Step 3 - Developers write the code in JAVA language and pushed into github repository
Step 4 - Check out the code.
Step 5 - 


* Amazon ec2 instance t2.medium.
* sudo yum install -y java-17-amazon-corretto -y
* sudo yum install git -y
* git clone https://github.com/Shikhar82/springboot-hello.git
* Download maven - https://downloads.apache.org/maven >>>>>> https://downloads.apache.org/maven/maven-3/3.9.12/binaries/
* cd /opt
* wget https://downloads.apache.org/maven/maven-3/3.9.12/binaries/apache-maven-3.9.12-bin.tar.gz
* tar xvf apache-maven-3.9.12-bin.tar.gz
* export PATH=$PATH:/opt/apache-maven-3.9.12/bin
* mvn --version

# Checkout project from Github to maven server.

* yum install git -y
* git clone https://github.com/Shikhar82/springboot-hello.git
* cd springboot-hello
* Validate the package - mvn validate
* Build the package - mvn package		# it will include validate, compile & test.

# validate package build @ /home/ec2-user/springboot-hello # ls -lrth target # check timestamp of 'gs-spring-boot-0.1.0.jar'

* cd /root/springboot-hello/target =>> artifact jar file (gs-spring-boot-0.1.0.jar)
* Clean the artifact - mvn clean
* Run the app => java -jar gs-spring-boot-0.1.0.jar
* Access ==>> http://52.66.197.33:8080
* vi /root/springboot-hello/src/main/java/hello/HelloController.java	# update some changes in file
* mvn clean | mvn package | java -jar gs-spring-boot-0.1.0.jar
* /root/.m2		# location of repository

Note: Above build is very old so I made 2 changes in pom.xml

<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>3.2.0</version>
</parent>

<java.version>17</java.version>

* cd /target 
* java -jar gs-spring-boot-0.1.0.jar


# Sonarqube

* Downloads: https://www.sonarsource.com/products/sonarqube/downloads/
* SonarQube is a automated code quality and security analysis. 
* It helps development teams continuously inspect, monitor, and improve their codebases by detecting issues such as bugs, vulnerabilities, and code smells early in the development process. 
* Java is required for sonarqube.

* Installation: 
cd /opt
wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-25.12.0.117093.zip
unzip sonarqube-25.12.0.117093.zip

* logout from root user, not recomend to user root because sonar use elastic search and elastic will not work as root.

* sudo useradd --system --no-create-home --shell /bin/bash sonar
* sudo chown -R sonar:sonar /opt/sonarqube-25.12.0.117093
* ls -ld /opt/sonarqube-25.12.0.117093
* cd sonarqube-25.12.0.117093/bin/linux-x86-64/
* ./sonar.sh start | ./sonar.sh status
* Access ==>> http://65.0.18.233:9000/
* Username/password ==>> admin - Jenkins@8890

# Integrate sonar & Maven

google: maven sonar integration
https://docs.sonarsource.com/sonarqube-server/10.8/analyzing-source-code/scanners/sonarscanner-for-maven

<plugin>
        <groupId>org.sonarsource.scanner.maven</groupId>
        <artifactId>sonar-maven-plugin</artifactId>
        <version>yourPluginVersion</version>
      </plugin>

goole: SonarScanner for Maven
https://docs.sonarsource.com/sonarqube-server/10.8/analyzing-source-code/scanners/sonarscanner-for-maven
* Replace Version in above code: 5.5.0.6356

<plugin>
        <groupId>org.sonarsource.scanner.maven</groupId>
        <artifactId>sonar-maven-plugin</artifactId>
        <version>5.5.0.6356</version>
</plugin>

vi /root/springboot-hello/pom.xml # paste the copied text between plugins

Sonar UI => Account => Security => Gen token => Copy token to notepad
token: sqa_a6bb6919c333d69d9c38755fb5a3eb3f56f99205

mvn sonar:sonar -Dsonar.host.url=http://43.205.114.26:9000 -Dsonar.login=squ_f823218bba5f5fbac403b06b46712694afd43e93

* check Projects in sonarqube dashboard.

# PostgreSQL setup:

* sudo dnf search postgresql15-server
* sudo dnf install postgresql15-server* -y
* sudo postgresql-setup --initdb	# initialize db

Note:
 * Initializing database in '/var/lib/pgsql/data'
 * Initialized, logs are in /var/lib/pgsql/initdb_postgresql.log

* sudo systemctl status postgresql | sudo systemctl start postgresql
* id postgres | sudo su | passwd postgres > 123 |su - postgres
* psql	# login
* create database sonarqubedatabase;
* create user sonaruser with encrypted password 'Yesterday@123';
* grant all privileges on database sonarqubedatabase to sonaruser;
* \q # exit from db
* exit # exit from user
* cd /opt/sonarqube-25.12.0.117093/conf
* vi sonar.properties
* #sonar.jdbc.username= | #sonar.jdbc.password=		# put username & password
* sonar.jdbc.url=jdbc:postgresql://localhost/<database-name>
* sudo su | cd /var/lib/pgsql/data
* vi pg_hba.conf
* shift+g >> 

# "local" is for Unix domain socket connections only
local   all             all                                     md5
# IPv4 local connections:
host    all             all             0.0.0.0/32            md5

* stop postgreql | stop sonar
* start postgreql | start sonar 	# not starting
* tail -f /opt/sonarqube-25.12.0.117093/logs/sonar.log
* tail -n 100 /opt/sonarqube-25.12.0.117093/logs/es.log		# main logs

Error: 
max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]

* sysctl vm.max_map_count	# show value
* sudo sysctl -w vm.max_map_count=262144
* cd /opt/sonarqube-25.12.0.117093/bin/linux-x86-64/ | ./sonar.sh start

# Maven

- Maven is a JAVA project management tool or build tool.
- It manages source code, test code, libraries, configuration, and dependencies of a project.
- It is based on a POM file (project object model). It is the main configuration file of Maven
- A build tool takes care of everything for building a project.
      * Generate source code.
      * Generate documentation.
      * Complies source code.
      * Install the package code central repo, local repo, or server repo.
  
- Maven can build any number of projects into the desired output, such as '.jar', '.war'
- Maven helps in getting the right .jar file for each project, as there may be different versions of different packages
- Dependencies can be downloaded from 'mvnrepository.com'
- Maven pulls source code from GitHub.

Requirement for build:

* Source code --> Present in workspace
* Compiler -- (Remote repo --> Local repo --> Workspace)
* Dependencies -- (Remote repo --> Local repo --> Workspace)

Maven build life-cycle:

* Generate dependencies
* Compile code
* Unit test
* Package build
* Install (in local repo & artifactory)
* Deploy to servers
* Clean


Some important phases in the default lifecycle include:

* validate: Ensures the project structure and necessary information are correct.
* compile: Compiles the project's source code, typically found in src/main/java.
* test: Executes unit tests.
* package: Bundles the compiled code into a distributable format like a JAR or WAR.
* verify: Runs checks on integration test results to meet quality standards.
* install: Places the project's package in the local Maven repository for use by other local projects.
* deploy: Copies the final package to a remote repository for wider sharing.

Repositories in Maven:

* Local - /home/USER/.m2/repository
* Remote/Private - Nexus/Jfrog
* Central/Public - Public from Maven website

-- Commands:
* mvn install <packagename>
* mvn clean <packagename>
