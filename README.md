Certainly! Below is a simple example of a Java project that includes the Maven configuration, Spotless plugin for code formatting, and GitHub Actions workflow for enforcing conventional commits.


1. Java Project Structure:
Create a simple Java class, for example, src/main/java/com/example/MyApp.java:

java
Copy code
package com.example;

public class MyApp {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
2. Maven Configuration (pom.xml):
Add the Spotless Maven plugin and other necessary configurations to your pom.xml:

xml
Copy code
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.example</groupId>
    <artifactId>java-conventional-commits</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <maven.compiler.source>11</maven.compiler.source>
        <maven.compiler.target>11</maven.compiler.target>
    </properties>

    <build>
        <plugins>
            <plugin>
                <groupId>com.diffplug.spotless</groupId>
                <artifactId>spotless-maven-plugin</artifactId>
                <version>5.15.0</version>
                <executions>
                    <execution>
                        <id>format</id>
                        <phase>pre-commit</phase>
                        <goals>
                            <goal>check</goal>
                        </goals>
                    </execution>
                </executions>
                <configuration>
                    <java>
                        <googleJavaFormat>
                            <version>1.11</version>
                        </googleJavaFormat>
                    </java>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>
3. GitHub Actions Workflow (.github/workflows/conventional-commits.yml):
Create a GitHub Actions workflow file:

yaml
Copy code
name: Conventional Commits

on:
  pull_request:
    types:
      - synchronize
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Set up JDK 11
      uses: actions/setup-java@v2
      with:
        java-version: 11

    - name: Build with Maven
      run: mvn clean install

    - name: Check Conventional Commits
      run: |
        sudo apt-get install -y nodejs npm
        npm install -g @commitlint/cli @commitlint/config-conventional
        git log --format=%B ${{ github.event.before }}..${{ github.sha }} | commitlint --from=${{ github.sha }}^ --to=${{ github.sha }}
4. Testing:
Make some sample commits with conventional commit messages and push them to the repository. Observe the GitHub Actions workflow for any errors or linting failures.

5. Enforce in Pull Requests:
Consider enabling branch protection rules on GitHub to enforce this workflow before allowing pull requests to be merged.

This is a minimal example, and you can expand upon it based on the specific needs of your Java project. Adjust dependencies, plugins, and configurations as necessary.
