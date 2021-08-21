# StreamSets Day Home Lab Environment
This document describes how to create a home lab environment that replicates what you used for StreamSets Days labs.
   
The environment includes:
* SQLPad to execute queries against SQL Server: [http://localhost:3000](http://localhost:3000)
* Microsoft SQL Server running as Developer

Dependencies and Requirements:
* Docker
* 8GB free memory
* 6GB Disk space

## Before You Begin

You will need to ensure Docker is installed.  Instructions for installing Docker:
* Docker on Windows	[https://docs.docker.com/docker-for-windows/install/](https://docs.docker.com/docker-for-windows/install/)
* Docker on Mac	[https://docs.docker.com/docker-for-mac/install/](https://docs.docker.com/docker-for-mac/install/)
* Docker on Linux	[https://docs.docker.com/engine/install/ubuntu/](https://docs.docker.com/engine/install/ubuntu/)

1. **Verify Docker works**

    Verify Docker works by executing this command line:

        docker run hello-world


    You should get output that starts with the text:

        Hello from Docker!
        This message shows that your installation appears to be working correctly.

    **TIP:** Depending on your Mac or Linux configuration, you may need to use “sudo” to get escalated privileges to execute commands like docker and docker-compose.  This document will not use sudo in its examples.  If you find you do not have rights then prefix your commands with sudo.  

    Example:
    
        sudo docker run hello-world

2. **Verify network ports are available**

    A common issue with running containers is network ports in use.  Let’s verify that ports 18630, 19630, and 3000 are available.  Run the following Docker command which runs a web server listening on ports 18630, 19630 and 3000:

        docker run --rm -p 3000:80 -p 18630:80 -p 19630:80 nginx

    After running, you should see output similar to this.  Ignore messages about “Pulling from library/nginx.   

    <p align="center"><img src="images/image1.png" /></p>

    If you don’t see "Configuration complete; ready for start up" then google for any error messages you see.  An error message like the following "Bind for 0.0.0.0:18630 failed"  indicates a port is in use.  You should look for other services holding the ports open.

    <p align="center"><img src="images/image2.png" /></p>

     You must resolve errors before continuing.

3. **Verify your web browser can access the docker ports**
    
    The previous step started a web server listening on ports.  Let’s verify your web browser can access the webserver.

    Open your browser and visit each of these pages:
    * StreamSets Transformer: [http://localhost:19630](http://localhost:19630)
    * StreamSets Data Collector:	[http://localhost:18630](http://localhost:18630)
    * SQLPad to execute queries against SQL Server: [http://localhost:3000](http://localhost:3000)

    You should see a page like this:

    <p align="center"><img src="images/image3.png" /></p>

    If you were able to access the 3 web pages then congratulations!  Your machine meets the requirements to create the environment.  If you are unable to access these addresses then check your corporate network policies or browser configuration.

    **_Press Ctrl-C at the command line to stop the test nginx webserver container._**


## Step 1 - Download the configuration file

Download and extract the the zip file: [https://github.com/streamsets/streamsetsday/archive/main.zip](https://github.com/streamsets/streamsetsday/archive/main.zip)  

This will create a folder “streamsets-days” containing these files and folders:

<p align="center"><img src="images/image4.png" /></p>

## Step 2 - Create the Docker network

Open a command prompt and change directory into the streamsets-days folder.  Then execute the following command:

    docker network create --attachable ssdays

This will create a docker network.  Containers attached to this network will be accessible to each other by service name.  (i.e. "ping sqlserver" will work inside a container)

## Step 3 - Starting the lab services

Open a command prompt and change directory into the streamsets-days folder.  Then execute the following command:

    docker-compose up

This will start all the applications and you will see each of them reporting their startup progress.  It may take 1-2 minutes to start.  The applications are ready once they stop printing their startup progress.  _You can stop the applications by pressing ctrl-c._

**TIP:** If you want the applications to run in the background, you can use the following command.

    docker-compose up -d

After the first time starting the applications you will see error messages "**Cannot insert duplicate key in object**"  You can ignore these errors.

You can now open the SQLPad web pages.  The username is "admin" and the password is "admin".  Note that the URL is different than what you used in the labs.
* SQLPad to execute queries against SQL Server: [http://localhost:3000](http://localhost:3000)

TIP: If you see the nginx Hello World page then you should click the refresh button to force a reload of the page.

## Step 4 - Deploy Data Collector and Transformer engines

Follow the instructions in Lab 1 to deploy the Data Collector and Transformer engines.  You can also refer to documentation: 
[https://docs.streamsets.com/portal/#platform-controlhub/controlhub/UserGuide/Deployments/Self.html#task_plh_xjt_jpb]

## Step 5 - Lab Pipelines

Solutions to the lab pipelines are provided as Sample Pipelines in each engine.  You can view the available Sample Pipelines by clicking on the "Sample Pipelines Library" button as shown below.

<p align="center"><img src="images/image9.jpg" /></p>

From the list of Sample Pipelines, select one of the Lab Solutions.

<p align="center"><img src="images/image10.jpg" /></p>

Click the Duplicate button to create an editable instance of the solution pipeline.

<p align="center"><img src="images/image11.jpg" /></p>

**NOTE:** The solution pipelines do not contain credentials.  You will need to re-enter the credentials as per the lab instructions.

## Stopping the applications

If you started the applications using the command “docker-compose up -d” then you can use this command line in the streamsets-days folder to stop the applications.

    docker-compose down

If you started the applications using the command “docker-compose up” then you can stop the applications by pressing ctrl-c in the terminal window you started the applications in.

Engines (Data Collector and Transformer) can be stopped in the Control Hub UI.
