# PMD Custom Configuration

[![Maven Central](https://img.shields.io/maven-central/v/com.github.nhojpatrick.qa/nhojpatrick-qa-ruleset-pmd?style=plastic)](https://search.maven.org/artifact/com.github.nhojpatrick.qa/nhojpatrick-qa-ruleset-pmd)
[![CircleCI](https://img.shields.io/circleci/build/github/nhojpatrick/nhojpatrick-qa-ruleset-pmd/develop?label=circleci&style=plastic)](https://circleci.com/gh/nhojpatrick/nhojpatrick-qa-ruleset-pmd/tree/develop)
[![coveralls](https://img.shields.io/coveralls/github/nhojpatrick/reop/branch?style=plastic)](https://coveralls.io/github/nhojpatrick/nhojpatrick-qa-ruleset-pmd?branch=develop)
[![Known Vulnerabilities](https://snyk.io/test/github/nhojpatrick/nhojpatrick-qa-ruleset-pmd/develop/badge.svg?style=plastic)](https://snyk.io/test/github/nhojpatrick/nhojpatrick-qa-ruleset-pmd/develop)
[![GitHub](https://img.shields.io/github/license/nhojpatrick/nhojpatrick-qa-ruleset-pmd?style=plastic)](https://github.com/nhojpatrick/nhojpatrick-qa-ruleset-pmd/blob/develop/LICENSE)

PMD is a static code analysis tool to see if code adheres to 'your'
coding standing. This project contains custom pmd rules.

The initial version is basic pmd plus imports increased to 999.

To use pmd the following best practice is suggested, with these notes;
* Put into your parent pom so to simplify your projects.
* Configure the plugin via pluginManagement, as tools like versions-maven-plugin can detect available updates.
* Configure version range of this i.e. `<version>[1.0.0,1.0.999)</version>`, so latest `1.0.x` will automatically be used.
* Execution is done via a profile.
* Execution is as simple as;
  * fail at 1st violation, use `./mvnw -Pqa_pmd`.
  * find all violations (i.e. jenkins report), use `./mvnw -Pqa_pmd -Dpmd.maxAllowedViolations=999999`.

```
<project>
	...
	<build>
		...
		<pluginManagement>
			<plugins>
				...
				<plugin>
					<groupId>org.apache.maven.plugins</groupId>
					<artifactId>maven-pmd-plugin</artifactId>
					<version>${plugins.maven.maven-pmd-plugin}</version>
					<configuration>
						<rulesets>
							<ruleset>com/github/nhojpatrick/qa/ruleset/pmd/rules.xml</ruleset>
						</rulesets>
					</configuration>
					<dependencies>
						<dependency>
							<groupId>com.github.nhojpatrick.qa</groupId>
							<artifactId>nhojpatrick-qa-ruleset-pmd</artifactId>
							<version>[1.0.0,1.0.999)</version>
						</dependency>
					</dependencies>
				</plugin>
				...
			<plugins>
		<pluginManagement>
		...
	<build>
	...
	<profiles>
		...
			<id>qa_pmd</id>
			<build>
				<defaultGoal>verify</defaultGoal>
				<plugins>
					<plugin>
						<groupId>org.apache.maven.plugins</groupId>
						<artifactId>maven-pmd-plugin</artifactId>
						<executions>
							<execution>
								<id>pmd-check</id>
								<phase>validate</phase>
								<goals>
									<goal>check</goal>
									<goal>cpd-check</goal>
								</goals>
							</execution>
						</executions>
					</plugin>
				</plugins>
			</build>
			<properties>
				<pmd.verbose>true</pmd.verbose>
				<skipTests>true</skipTests>
				<sort.skip>true</sort.skip>
			</properties>
		</profile>
		...
	</profiles>
	...
	<properties>
		<plugins.maven.maven-pmd-plugin>3.13.0</plugins.maven.maven-pmd-plugin>
		<pmd.maxAllowedViolations>0</pmd.maxAllowedViolations>
	</properties>
</project>
```
