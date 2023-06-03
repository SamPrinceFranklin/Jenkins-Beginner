# Jenkins - Beginner

## Description

This guide provides an easy way to install and create a simple Jenkins project.

## Installation

### Build the Jenkins BlueOcean Docker Image

```shell
docker build -t myjenkins-blueocean:2.332.3-1 .
```

### Create the network 'jenkins'
```shell
docker network create jenkins
```
## Run the Container
MacOS / Linux

```shell
docker run --name jenkins-blueocean --restart=on-failure --detach \
  --network jenkins --env DOCKER_HOST=tcp://docker:2376 \
  --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 \
  --publish 8080:8080 --publish 50000:50000 \
  --volume jenkins-data:/var/jenkins_home \
  --volume jenkins-docker-certs:/certs/client:ro \
  myjenkins-blueocean:2.332.3-1
```

### Get the Password
To get the initial admin password for Jenkins, run the following command:
  
  ```shell  
  docker exec jenkins-blueocean cat /var/jenkins_home/secrets/initialAdminPassword
```

### Connect to Jenkins
Once Jenkins is up and running, you can access it through the following URL:

https://localhost:8080/


# Creating a Simple Freestyle Job

This guide provides step-by-step instructions on creating a simple freestyle job in Jenkins.

## Step 1: Access Jenkins Server

1. Open your web browser and go to `http://localhost:8080` to access the Jenkins server.
2. Login with your credentials.

## Step 2: Create a New Freestyle Job

1. On the left menu bar, click on **New Item**.
2. Enter a name for the job. Avoid using spaces in the name, as Jenkins will create files based on this name. Instead, use underscores (_).
   - Example: I will use **my_first_job** as the name.
3. Select **Freestyle Project** as the job type.
4. Click **OK** to proceed.

## Step 3: Configure the Build

1. On the configuration page of your job, you will find various sections including **Build**, **Build Triggers**, **Build Environment**, **Post-build Actions**, etc.
2. In the **Build** section, select **Execute Shell** as the build step.
3. In the command section, enter the following command to display a simple message:
   ```shell
   echo "Hello Jenkins" 
   ```
4. Click **Save** to save the job.



## Step 4: Build the Job
1. On the left panel of the job page, click on **Build Now**.
2. Jenkins will start executing the job based on the defined configuration.
3. You can monitor the progress of the build on the job page.
4. Once the build is complete, you will see a blue ball indicating that the build was successful.
5. Click on the build number to see the console output of the build.

```shell
Started by user Sam Prince Franklin K
Running as SYSTEM
Building in workspace /var/jenkins_home/workspace/my_first_job
[my_first_job] $ /bin/sh -xe /tmp/jenkins2052611486694322613.sh
+ echo Hello Jenkins
Hello Jenkins
Finished: SUCCESS
```

Congratulations! You have successfully executed your first Jenkins job with the specified command.

---

Now lets deep dive into the Jenkins Configuration and explore the various options available.

## Default Environment Variables in Jenkins :

| Variable Name | Description |
| --- | --- |
| BUILD_ID | The current build ID, identical to BUILD_NUMBER for builds created in 1.597+ |
| BUILD_NUMBER | The current build number, such as "153" |
| BUILD_TAG | String of "jenkins-${JOB_NAME}-${BUILD_NUMBER}". All forward slashes ("/") in the JOB_NAME are replaced with dashes ("-"). Convenient to put into a resource file, a jar file, etc for easier identification. |
| BUILD_URL | Full URL of this build, like http://server:port/jenkins/job/foo/15/ |
| EXECUTOR_NUMBER | The unique number that identifies the current executor (among executors of the same machine) that’s carrying out this build. This is the number you see in the "build executor status", except that the number starts from 0, not 1. |
| JENKINS_HOME | The absolute path of the directory assigned on the master node for Jenkins to store data. |
| JENKINS_URL | Full URL of Jenkins, like http://server:port/jenkins/ (NOTE: only available if Jenkins URL set in system configuration) |
| JOB_NAME | Name of the project of this build, such as "foo" or "foo/bar". (To strip off folder paths from a Bourne shell script, try: ${JOB_NAME##*/}) |
| JOB_BASE_NAME | Short Name of the project of this build stripping off folder paths, such as "foo" for "bar/foo". (Since 1.609) |
| NODE_LABELS | Whitespace-separated list of labels that the node is assigned. |
| NODE_NAME | Name of the node the current build is running on. Set to "master" for master node. (To strip off folder paths from a Bourne shell script, try: ${NODE_NAME##*/}) |
| WORKSPACE | The absolute path of the directory assigned to the build as a workspace. |

For an example of how to use these variables in a shell script, we will echo the value of the BUILD_ID variable and the BUILD_URL variable by editing the BUILD COMMAND Section of the job.

```shell
echo "Hello Jenkins"
echo "The build ID of this job is ${BUILD_ID}"
echo "The build URL of this job is ${BUILD_URL}"
```

Output :

```shell
Started by user Sam Prince Franklin K
Running as SYSTEM
Building in workspace /var/jenkins_home/workspace/my_first_job
[my_first_job] $ /bin/sh -xe /tmp/jenkins17269523199175919977.sh
+ echo Hello Jenkins
Hello Jenkins
+ echo The build ID of this job is 2
The build ID of this job is 2
+ echo The build URL of this job is http://localhost:8080/job/my_first_job/2/
The build URL of this job is http://localhost:8080/job/my_first_job/2/
Finished: SUCCESS
```

---

## Exploring the Workspace

In this step, we will explore the workspace in Jenkins by executing some commands and examining the output. Follow the instructions below:

1. In the Jenkins job configuration, navigate to the build section.

2. Click on "Add build step" and select "Execute shell" from the dropdown menu.

3. In the command section, enter the following commands:

```shell
##Previous Commands
ls -ltr
echo "1234" > test.txt
ls -ltr
```

These commands will list the contents of the workspace directory, create a file named "test.txt" with the content "1234", and then list the contents again to see the changes.

4. Save the job configuration.


### Build the Job
1. Click on the "Save" button to save the job configuration.
2. On the Jenkins job page, click on "Build Now" in the left panel to trigger a build.

### Output 
After the build is executed, you will see the output of the commands in the Jenkins console. The output will look similar to the following:
```shell
Started by user Sam Prince Franklin K
Running as SYSTEM
Building in workspace /var/jenkins_home/workspace/my_first_job
[my_first_job] $ /bin/sh -xe /tmp/jenkins4473774077294296340.sh
+ echo Hello Jenkins
Hello Jenkins
+ echo The build ID of this job is 3
The build ID of this job is 3
+ echo The build URL of this job is http://localhost:8080/job/my_first_job/3/
The build URL of this job is http://localhost:8080/job/my_first_job/3/
+ ls -ltr
total 0
+ echo 1234
+ ls -ltr
total 4
-rw-r--r-- 1 jenkins jenkins 5 Jun  3 04:58 test.txt
Finished: SUCCESS
```

The output shows the execution of each command and the corresponding results. It provides information about the workspace directory, the execution of the "ls" command before and after creating the "test.txt" file, and additional echo statements.

Note: The actual output may vary based on your Jenkins configuration and workspace directory.

### Deleting the Workspace
In this step, we will delete the workspace directory and see how Jenkins handles the deletion. Follow the instructions below:

1. In the Jenkins job configuration, navigate to the build section.
2. Select the "Delete workspace before build starts" checkbox.
3. Save the job configuration.

---

### Cloning a Git Repository in Jenkins

In this step, we will configure a Jenkins job to clone a Git repository and examine the contents of the cloned repository. Follow the instructions below:

1. Create a new item in Jenkins by clicking on "New Item" in the Jenkins dashboard.
2. Enter a name for the job, such as "my_python_job".
3. Select "Freestyle project" and click "OK" to proceed.
4. In the job configuration page, scroll down to the "Source Code Management" section.
5. Select "Git" as the source control management option.
6. Paste the URL of the Git repository (https://github.com/SamPrinceFranklin/Jenkins-Beginner) in the "Repository URL" field.Optionally, configure the branch or commit you want to clone in the "Branches to build" field.
7. In the "Build" section, click on "Add build step" and select "Execute shell".
8. In the command section, enter the following command:
```shell
python3 helloworld.py
```
9. Save the job configuration.
10. Click on "Build Now" in the left panel to trigger a build.


---

## License

This project is licensed under the [MIT License](LICENSE).

## Contact

For any questions or support, please contact [samprince.franklink2020@vitstudent.ac.in](mailto:samprince.franklink2020@vitstudent.ac.in).

