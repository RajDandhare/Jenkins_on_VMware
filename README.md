# Jenkins_in_VMware

## DevOps CICD pipeline [Jenkins freestyle] 

_This project will show how to run Jenkins on VMware and run containers using Docker. [For beginner]_

> VM's Specification: <br /> Cores : 2 <br /> RAM : 4Gb <br /> OS : Rocky Linux 9

### **Step 1 :** Installing Jenkins, Docker, and Git on VMware

**Jenkins Installation :** 

    sudo mkdir /jenkins
    sudo wget [https://updates.jenkins.io/download/war/2.459/jenkins.war](https://updates.jenkins.io/latest/jenkins.war) -P /jenkins # [must be latest]

You need Java openjdk to run the jenkins file. [must be latest]

    sudo yum -y install java-21-openjdk-demo.x86_64

Now run the file using command 

    sudo java -jar /jenkins/jenkins.war

When the above command execution complites it will show you CODE for Jenkins copy it and save it in text file. 
<br /><br />![jenkins-code](/../main/Pics/jenkins-code.png) 

Now open the VM's Browser and search :-
> http://localhost:8080 <br /> #Else you can search it on your Device on which the VM is runing and search <br /> http://[VM's IP address]:8080

It will ask you for the CODE that we just copyed, paste it.
<br /><br />![jenkins-askscode](/../main/Pics/jenkins-askscode.png)<br /><br />

After that it will ask Plugins for Jenkins, Just select suggested plugins it will start the installation. Then you will be ask for signup on Jenkins.<br />
Next you will be ask for Login which you just signup for Jenkins. It will take you to Blank Dashboard.

<br /><br />![jenkins-dashboard](/../main/Pics/jenkins-dashboard.png)

Thats it for the Jenkins Installation!!!!

> [!Caution]
> Now, open new terminal and don't close or stop the terminal where the Jenkins.war file is running.

**Docker Installation :**

    sudo dnf check-update    #update the system it need 
    sudo dnf config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
    sudo dnf install docker-ce docker-ce-cli containerd.io

To start the docker service, 

    sudo systemctl start docker
    sudo systemctl status docker
    sudo systemctl enable docker    #make sure the service is 'active' after system is restarted/reboot 

You need to have Docker Account to Create your own docker images:-

> [!Tip]
> How to create Docker Account link : https://docs.docker.com/docker-id/

How to login in VM using terminal:

    sudo docker login --username "your_email_address"    #Enter the password of docker account

**Git Installation :**

    sudo yum install git 
    sudo git config --global user.email "email_address"
    sudo git config --global user.name "username"

### **Step 2 :** Create a repositorie in GitHub

> how to create new repository
> https://docs.github.com/en/repositories/creating-and-managing-repositories/quickstart-for-repositories

We have to Create two files in that Repo. name 'Dockerfile' and 'index.html', For example you can use files which is avilable in this Repo.<br />
'Dockerfile' is need for building the images and 'index.html' be our website file.

We need this Repo. in our VM, make directory in '/' name 'git_files' 

    sudo mkdir /git_files
    cd /git_files
    git clone 'URL of your git repo.'

### **Step 3 :**  Creating Freestyle Pipeline

Goto Jenkins DashBoard in Browser, Click on 'New item' to create First step of pipeline.
<br />Select Freestyle Project, Name the project 'git_pull'. <br />Then do the same as shown in below figures:-

<br /><br />![jenkins-git_pull-1](/../main/Pics/jenkins-git_pull-1.png)
<br /><br />![jenkins-git_pull-2](/../main/Pics/jenkins-git_pull-2.png)
<br /><br />![jenkins-git_pull-3](/../main/Pics/jenkins-git_pull-3.png)

Save it and Go back to DashBoard and click on New item for Second step of pipline.
<br />Select Freestyle Project, Name the project 'docker_build'. <br />Then do the same as shown in below figures:-

<br /><br />![jenkins-docker_build-1](/../main/Pics/jenkins-docker_build-1.png)
<br /><br />![jenkins-docker_build-2](/../main/Pics/jenkins-docker_build-2.png)

Save it and Go back to DashBoard and click on New item for Third and Final step of pipline.
<br />Select Freestyle Project, Name the project 'docker_deploy'. <br />Then do the same as shown in below figures:-

<br /><br />![jenkins-docker_deploy-1](/../main/Pics/jenkins-docker_deploy-1.png)
<br /><br />![jenkins-docker_deploy-2](/../main/Pics/jenkins-docker_deploy-2.png)

### **Step 4 :** Before runing the Pipeline of Jenkins....

Before we run our Jenkins Pipeline we have to run Docker container for not getting error in step3 'docker_deploy' pipeline 

Run this commands in new terminal without closing the privious treminal were the Jenkins.war file is runing.

    docekr run --name w1 http

### **Step 5 :** Run the Pipeline!!!

Final step... goto dashboard and click on play button of git_pull. 

<br /><br />![jenkins-dashboard](/../main/Pics/jenkins-dashboard-1.png)

And see the Build Executor Status if its complite or not.

> [!NOTE]
> you might need to refresh the page to check the every step status. [green or red]

After Build is complite goto VM's Browser and search

> http://localhost:5500

If you see your web page the pipeline was created Susseccfully!!!

it might look like this if you use the provided index.html
<br /><br />![jenkins-webpage](/../main/Pics/jenkins-webpage.png)

<br /><br /><br /><br /><br /><br /><br />_If there is error in pipeline you see the error by clicking on the name which the step is failed and cliking on number of build History -> see console output_
