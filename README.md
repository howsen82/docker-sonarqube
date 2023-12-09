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