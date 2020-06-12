# IDE-on-docker
eclipse IDE running on docker

These are the steps I used to create my development environment using docker images. 

Step 1.
  Create a docker volume for holding maven dependencies. This reduces the downloads from central maven repository.
  
-----------------------------------------------------------------------------------------------------------------------------

 creating and using local docker volume as the maven repository

 1. create docker volume
      docker volume create --name maven-repo

 2. load volume with maven artifacts
      docker run -it -v maven-repo:/root/.m2 maven mvn archetype:generate # will download artifacts

 3. use for subsequent maven builds
      docker run -it -v maven-repo:/root/.m2 maven mvn archetype:generate # will reuse downloaded artifacts

-----------------------------------------------------------------------------------------------------------------------------

Step 2.
  Create a maven project by running archetype generate and then edit pom.xml to fix dependencies. Note that the project files are created in the subdirectory temp3 of current working directory.
  
  docker run -it --rm -v maven-repo:/root/.m2 -v "$(pwd)"/temp3:/tmp -w /tmp maven mvn -DgroupId=com.drmsd -DartifactId=greeting -Dversion=1.0 --update-snapshots archetype:generate

  modify pom.xml and add compiler dependency.
  
  			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-compiler-plugin</artifactId>
				<configuration>
					<source>9</source>
					<target>9</target>
				</configuration>
			</plugin>

 if JAXB is required, add the following to pom.xml
 
 			<dependency>
				<groupId>javax.xml.bind</groupId>
				<artifactId>jaxb-api</artifactId>
				<version>2.3.0</version>
			</dependency>

invoke maven target for compile, package or install as needed. This will compile the skeleton project created in temp3 subdirectory. This project can be modified with eclipse IDE which will be installed in a separate docker image.

docker run -it --rm --name my-maven-project -v maven-repo:/root/.m2 -v "$(pwd)"/temp3:/tmp -w /tmp maven mvn clean compile

