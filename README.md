# Skeleton of project spring boot with spring data jpa and wildfly 
Skeleton of project spring boot javaee container with wildfly

## Requirements
For this implementation you need the following items with current versions or higher

[JDK 8][1], [Lombok][2], [Maven][3], [PostgreSQL 9.4][4], [Wildfly][5]

## Setting up wildfly

### PostgreSql Driver
In the "${jboss_home}/modules/system/layers/base", create the following folder structure: "org/postgresql/main". 
Download [postgresql-9.4.1211][6] and unzip download inside the created folder. Create a module.xml file with the following content :
```
<?xml version="1.0" encoding="UTF-8"?>
<module xmlns="urn:jboss:module:1.1" name="org.postgresql">
	<resources>
		<resource-root path="postgresql-9.4.1211.jar"/>
	</resources>
	<dependencies>
		<module name="javax.api"/>
		<module name="javax.transaction.api"/>
	</dependencies>
</module>
```

### Standalone.xml
In the "${jboss_home}/standalone/configuration", make the following changes to the standalone.xml file :
- Search the subsystem :```<subsystem xmlns="urn:jboss:domain:datasources:4.0">```
- Create the datasource:
```
<datasource jta="true" jndi-name="java:/PostgresDS" pool-name="PostgresDS" enabled="true" use-ccm="true">
	<connection-url>jdbc:postgresql://localhost:5432/db</connection-url>
    	<driver-class>org.postgresql.Driver</driver-class>
    	<driver>postgresql</driver>
	<security>
		<user-name>your username installation</user-name>
		<password>your password installation</password>
    	</security>
	<validation>
		<valid-connection-checker class-name="org.jboss.jca.adapters.jdbc.extensions.postgres.PostgreSQLValidConnectionChecker"/>
		<background-validation>true</background-validation>
		<exception-sorter class-name="org.jboss.jca.adapters.jdbc.extensions.postgres.PostgreSQLExceptionSorter"/>
    	</validation>
</datasource>
```
- Add driver:
```
<driver name="postgresql" module="org.postgresql">
	<driver-class>org.postgresql.Driver</driver-class>
</driver>
```
[1]: http://www.oracle.com/technetwork/java/javase/downloads/index.html
[2]: https://projectlombok.org/
[3]: https://maven.apache.org/
[4]: http://www.postgresql.org/
[5]: http://wildfly.org/downloads/
[6]: https://jdbc.postgresql.org/download.html
