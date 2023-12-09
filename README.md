# SonarQube

**Create a SonarQube Project**
To run the SonarQube scanner on your code, you will first need to create a project token. There are many ways to create a token.

Click on the **Manually** icon on the bottom left.

```
docker pull sonarsource/sonar-scanner-cli
```

On the next page, create a project by following these steps:

1. Set the project display name to `temp`
2. Set the project key to `temp` (this will happen by default).
3. Ensure the `main` branch is selected
4. Press the `Next` button to continue.

*Note: If you get an error message, try again.*

Please select `Use the global setting` and then click on `Create project`.

On the next page, where it asks how you want to analyze your repository, select the `Locally` option.

In the next step, you will generate a token for your project.

Before you can scan your code, you will need to generate a token. You can generate a token on the **Analyze your project** page at the **Provide a token** step.

Next, you will see the token that has been generated. You want to highlight the token text, copy it, and then paste it in a safe place. You will need it later to submit your scans.

Then click the **Continue** button.

This will take you to a page where you will need to answer some questions about your project’s configuration, such as the language and the operating system (OS) used.

**Important!**

It is very important to copy this command to a safe place! You will see the command that is generated so that you can run the scanner on your code. This command is unique to your project.

Make sure that you save that command somewhere since you will be needing it in a future step!

**Ready the SonarQube Scanner**

It is important to understand that the SonarQube server that stores the results of scans is separate and distinct from the SonarQube scanner which performs the actual scanning. Up until now, we’ve created a database for storing the analysis results and provisioned a SonarQube server for serving the UI.

To get the SonarQube scanner to work in the IDE, you can either install it locally or pull its docker image and run its docker container. In this lab, you will be pulling the docker image and running its docker container.

1. First, we will use the docker pull command to download the `sonarsource/sonar-scanner-cli` image from Docker hub so that it is available locally for use.

```
docker pull sonarsource/sonar-scanner-cli
```

2. Run the following bash `alias` command in the terminal, which creates an alias `sonar-scanner` for running the scanner later using the `scanner-cli` docker container:

```
alias sonar-scanner='docker run --rm -v "$(pwd):/usr/src" sonarsource/sonar-scanner-cli'
```

Any arguments that you pass into the `sonar-scanner` command will be passed into the container version as well. This is how you can easily run commands in Docker containers as if they were actually installed on your computer.

Now that we have the scanner ready let’s get ourselves a project to run an analysis on!

# Using Dynamic Analysis

If you ask a developer what their top three goals are, you are most likely going to hear the following:

* Write bug-free code
* Meet design specifications
* Prevent security issues

It is the testing and evaluation of an application during runtime. Also referred to as dynamic code scanning, dynamic analysis can identify security issues that are too complicated for static analysis alone to reveal.

Dynamic application security testing (DAST) looks at the application from the outside-in — by simulating attacks against a web application and analyzing the application's responses to discover security vulnerabilities in the application.

A real-world application that you will be testing is the OWASP Juice Shop app, which is an application developed for security training purposes. For a detailed introduction, full list of features, and architecture overview, please visit the official project page: https://owasp-juice.shop.

You will be guided through setting up OWASP ZAP and using it to run an analysis of the Juice Shop application in the Cloud IDE with Docker so that everything can be done in a terminal on the right panel. You should be able to replicate this lab easily in any environment with Docker installed, including your developer workstation.

To test the OWASP Juice Shop app, you must fetch and run the application. Open up a terminal by clicking `Terminal -> New Terminal` in the top menu bar. Copy and paste the following commands in the terminal window to fetch Juice Shop's docker image, and then run the application in the current Cloud IDE.

```
docker pull bkimminich/juice-shop
docker run --rm -p 3000:3000 bkimminich/juice-shop
```

After running the two commands, wait until you see the message “info: Server listening on port 3000” in the terminal before proceeding with the lab. If you do not see this message, try leaving this lab and then restarting it.

```
info: Server listening on port 3000
```

**Launch the Juice Shop UI**

<u>**Run OWASP ZAP**</u>

1. Open a new terminal window using `Terminal > New Terminal` to issue new docker commands.
2. In the terminal, execute the `docker pull` command to download/pull the docker image of OWASP ZAP. *(Note: It may take some time to download.)*
   
   ```
   docker pull owasp/zap2docker-stable
   ```

   Now that the tool is installed in the current Cloud IDE, you can start a vulnerability scan of the Juice Shop app.
   
3. Next, copy the URL of the app from the address bar of the running application in Cloud IDE to your clipboard. Then run the following command and replace the `{TARGET_URL}` with the URL you copied.

    ```
    docker run -t owasp/zap2docker-stable zap-baseline.py -t {TARGET_URL}
    ```

ZAP will now start its crawling activity of the site and builds a sitemap, and the related output can be reviewed in the terminal. This will take a few minutes to execute.

**Interpret the scan results**

A number of items tested came back with `PASS:` so we will not look at those.

You can use the numbers next to the vulnerability names to read about the alert on the ZAP Proxy Web site. Using the following URL:

```
https://www.zaproxy.org/docs/alerts/{NUMBER}
```

# Software Composition Analysis (SCA)

Modern applications may be developed using 3rd-party and open-source components. Other people code these components. How do you know if the components are safe? How can you verify their security? You can do this by learning how to use software composition analysis tools to inspect the components used in your applications. You can ensure your code is safe from any known component vulnerabilities.

Software Composition Analysis (SCA) is the process of identifying areas of risk that result from the use of third-party and open-source components during application development.

SCA tools can identify several risk factors, including:

* Outdated components.
* Components with known vulnerabilities.
* Component quality.
  * From a security standpoint, a component might be considered lower quality if it is poorly maintained or has a very small community supporting it.
* Transitive dependencies.
  * SCA tools can track vulnerabilities from transitive dependencies. When a component relies upon another component, that dependency is referred to as transitive.
* External services
  * A component may interact with external services, such as Web APIs. SCA tools can identify when this interaction might be a vulnerability.

**Dependency-Check** is a Software Composition Analysis (SCA) tool that attempts to detect publicly disclosed vulnerabilities contained within a project's dependencies. Dependencies are the software components that your code relies on for additional functionality. The SCA tool will generate a report listing the dependency, any identified Common Platform Enumeration (CPE) identifiers, and the associated Common Vulnerability and Exposure (CVE) entries.

#### Installing the OWASP SCA Tool

1. Run the `wget` command to download and install the OWASP dependency-check script:

```
wget -O dependency-check.zip https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-CD0267EN-SkillsNetwork/labs/module2/data/dependency-check.zip && unzip dependency-check.zip && chmod +x dependency-check/bin/dependency-check.sh && sudo echo "alias dependency-check=$(pwd)/dependency-check/bin/dependency-check.sh" >> ~/.bashrc && source ~/.bashrc
```

Now you'll be able to run the OWASP SCA tool using dependency-check on the command line.

#### Download the Source Code

Next, we'll need some source code to scan. We'll use a popular example of a vulnerable application called **OWASP Juice Shop**, explicitly created for security training purposes.

1. Run the following `git clone` command to download the source code:

    ```
    git clone https://github.com/juice-shop/juice-shop.git
    ```

#### Run SCA on Juice Shop Components
The OWASP SCA tool can create a Hypertext Markup Language (HTML) report or JavaScript Object Notation (JSON) output. In this step, we'll run the tool and output the results to a JSON output.

1. Use the `dependency-check` command on the Juice Shop source code with the following options to produce a JSON output:

```
dependency-check -f JSON --prettyPrint --scan juice-shop
```
> **Note** 
This may take a while because the first time you run it, it must download all of the updates from the CVE database. Subsequent runs will complete more quickly.

The command will produce a file called `dependency-check-report.json` which may contain information about any vulnerable components found by the OWASP SCA Tool.

#### Analyzing JSON Results
When we ran the command, the report `dependency-check-report.json` was saved in the  folder. You can view the file by opening it manually or by pressing this button:

Since JSON can be challenging to read, you might want to modify the output to only display the name of the affected files.

To do this you can use the `jq` command and append the `'.dependencies[].filePath'` parameter to filter the JSON output to only display the path of the affected files.

1. Use the following `jq` command to list the software components that were found to be vulnerable by the OWASP tool:
   
   ```
   jq '.dependencies[].filePath' dependency-check-report.json
   ```

   > Note: This search will return the `filePath` attribute of all of the items in the `dependencies[]` array in the file `dependency-check-report.json`.

In a secure Software Development Life Cycle (SDLC), each component should be thoroughly checked and verified for security. Any dependencies that might be vulnerable should be upgraded or replaced.

#### Creating an HTML report
Reports outputted to JSON format can be difficult to read. It may be preferable to send your scan output to an HTML report instead. HTML reports are easier for us to read and interpret.

1. Use the `dependency-check` command to create an HTML report from a `--scan` of the `juice-shop` folder:
   
   ```
   dependency-check --scan juice-shop
   ```

Once the process has completed, a report called `dependency-check-report.html` will be located in your file explorer. Unfortunately, the Cloud IDE environment does not have a way to render this HTML file, so you must download the report and view it in your web browser.

### Conclusion
You have learned how Software Composition Analysis (SCA) can help you detect known vulnerabilities in any dependencies your code is dependent on. This is an integral part of secure software application development.