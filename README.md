Simple Java-based Lambda Microservice
Build and Deploy
Prerequisites
AWS CLI
Configure AWS user credentials
Create S3 bucket
Docker
Deploy
The script deploy.sh can be used to deploy the application to any AWS account.

As a prerequisite it is necessary to install and configure the AWS CLI. Additionally, it needs a S3 bucket in the same AWS region in which the application will be deployed. The name of the S3 bucket needs to be configured in the Variable S3_BUCKET in this script.

The application can be packaged and deployed by running

bash deploy.sh
Setup a new project
The following steps assume that Maven is run in a Docker container. If Maven is already installed on the machine it can be used directly without using Docker.

Create Docker volume for Maven repository
docker volume create maven-repository
Run Maven in Container
docker run --rm -it -v $PWD:/usr/src/app -v maven-repository:/root/.m2/repository -w /usr/src/app maven:3.6-jdk-8 /bin/bash
Create project with Maven
docker run --rm -v $PWD:/usr/src/app -v maven-repository:/root/.m2/repository -w /usr/src/app maven:3.6-jdk-8 mvn archetype:generate -DgroupId=com.carpinuslabs.todo -DartifactId=todo-app -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeVersion=1.4 -DinteractiveMode=false
Add dependencies
<dependency>
  <groupId>com.amazonaws</groupId>
  <artifactId>aws-lambda-java-core</artifactId>
  <version>1.2.0</version>
</dependency>
<dependency>
  <groupId>com.amazonaws</groupId>
  <artifactId>aws-lambda-java-events</artifactId>
  <version>2.2.5</version>
</dependency>
Configure packaging
<packaging>jar</packaging>
Configure compiler settings
<maven.compiler.source>1.8</maven.compiler.source>
<maven.compiler.target>1.8</maven.compiler.target>
Add shade plugin
<build>
  <plugins>
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-shade-plugin</artifactId>
      <version>2.3</version>
      <configuration>
        <createDependencyReducedPom>false</createDependencyReducedPom>
      </configuration>
      <executions>
        <execution>
          <phase>package</phase>
          <goals>
            <goal>shade</goal>
          </goals>
        </execution>
      </executions>
    </plugin>
  </plugins>
</build>
Build project in Container
docker run --rm -v $PWD:/usr/src/app -v maven-repository:/root/.m2/repository -w /usr/src/app maven:3.6-jdk-8 mvn clean package -DskipTests
