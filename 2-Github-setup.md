-- Connecting my Device with github --

cd into the .ssh 
create a key by using ssh-keygen
add public key to the github account and save

-- forking Repository --
Now i am going to fork the instructor Repository into my account so i can test commit

Now will clone the forked repository into my laptop folder


After cloning the repo i will cd into the cloned repo using Git
and run 
# git config core.sshCommand "ssh -i ~/.ssh/Gitops-CI -F /dev/null" #
What  this command will do ?
This command is going to use the private key of "Gitop-CI" when everytime there is a developer commit

Now open vscode and open the cloned repo made some change in any of the file and run commit and push for test.

## Creating Workflow ##
Now we are going to create a workflow using github actions like we do in jenkins (creating pipeline) 
for sonar code analysis and run test.

Steps:
Go to your repository 
Go to actions tab
click on setup a a workflow yourself
It is going to create a file in yaml format where we will write our workflow
After creating it we can directly write it using Vscode


name: Hprofile Actions
on: workflow_dispatch
jobs:
  Testing: 
    runs-on: ubuntu-latest
    steps:
      - name: Testing workflow
        run: echo "Workflow works!"

This is the workflow we write Firstly we have gave the name 
to the workflow then we run job names as testing
on a server ubuntu-latest 


## Code testing job in workflow ##
We can find the the tests on the github market place command

steps:
      - name: code checkout
        uses: actions/checkout@v4

      - name: Maven test
        run: mvn test

      - name: Checkstyle
        run: mvn checkstyle:checkstyle

Code Checkout:

It downloads your repository’s code so the workflow can use it for the next steps.
Maven Test:

Runs all the unit tests in your project using Maven to check if your code works as expected.
Checkstyle:

Checks your code's formatting and style (like indentation, naming, etc.) to make sure it follows coding standards.



