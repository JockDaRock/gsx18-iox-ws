# Building and Deploying Edge Application w/ IOx

## Building our edge application

### First we need to download the main source code for our app with our terminal.
* Open the iTerm app
* Create a new directory and move in to that directory by typing in the following commands.
```bash
$ mkdir iox1

$ cd iox1
```

* We will then use the git client to download our source code.
```bash
$ git clone https://github.com/CiscoDevNet/iox-docker-python-web
```

* Then change directories to the folder we just downloaded.

```bash
$ cd iox-docker-python-web
```

### Let's Edit our application
* Now we will open our python app in our code editor.

```bash
$ code main.py
```

* We will now edit the application to make it a bit more personal ;).

Change...
```python
return {"system": 1, "datetime": current_time}
```

to

```python
return {"system": 1, "datetime": current_time, "name": "YOUR-NAME"}
```

* Save you application and return to the command line to open the Dockerfile we will use to build our containerized application with Docker.

```bash
$ code Dockerfile
```

* We are not going to edit this application but we will examine the parts of it.

* Next let's open the package.yaml file to examine that as well.

```bash
$ code package.yaml
```

### Now it is time to build the application

* From the command line we will use the docker cli program to build our application.

```bash
$ docker build -t ioxwebapp:latest .
```

* Now let's quickly test our container with the following command.

```bash
$ docker run -it -p 40000:8000 ioxwebapp:latest
```

* Once the app is running navigate to [http://0.0.0.0:40000/time](http://0.0.0.0:40000/time) in your browser.

* Now that we have tested the app to make sure it is working, let's prepare the app for IOx with the ioxclient on our command line.

* Let's build our ioxclient profile to connect to an instance of IOx.

```bash
$ ioxclient profiles create
```

* Now that we have created our IOx profile, let's use ioxclient to finish building our application.

```bash
$ ioxclient docker package ioxwebapp:latest .
```

### Deploying apps using ioxclient

* Once we have built our application we can use the ioxclient to deploy our application.

```bash
$ ioxclient app install ioxwebapp package.tar
```

* Once the application is deployed we need to activate our application.  While being deactivated, the application can still be updated or removed.  Activation helps us manage life cycles of the app.

```bash
$ ioxclient app activate --payload activation.json ioxwebapp
```

* Now that our application has been activated we can now start it...

```bash
$ ioxclient app start ioxwebapp
```

* Now lets see if our application is running on IOx... Navigate to [http://10.10.20.51:8000/time](http://10.10.20.51:8000/time).

**Congrats on building and deploying your first IoT Edge application with IOx!!!**

### Mass Deployment: Deploying apps using FogDirector

> In production Apps will be deployed using FogDirector.  FogDirector is an imporatnat part of the IOx ecosystem as it can manage up to 150,000 IOx instances.

* Access Fog Director and the IOx Local Manager at [https://10.10.20.50/](https://10.10.20.50/)

> Note: If you receive a security warning, it is because Fog Director is using a self-signed certificate and you can confirm the security exception to proceed.

* When prompted to access the Fog Director, use the following credentials username: **admin**, password: **admin_123**.  The information to access the Sandbox is also available in the left-hand Instructions panel.

* Once you are logged in, navigate to the devices page.

* Click the add button to add the virtual IOx devices to Fog Director


* Enter in the IP address, `10.10.20.51`, for one of the IOx devices from your Sandbox reservation along with the username (**cisco**) / password (**cisco**).

* Click Save & Add More button and repeat the process for the 2nd IOx device, `10.10.20.52`.

* Click Save & Close once you have completed adding the 2nd IOx device to Fog Director.

* Navigate back to the Apps tab on FogDirector.

* At the bottom of the page click the **Add New App**.

* When the popup window appears, Select Create from Docker image

* In the Image or ID section, type in `jockdarock/buttongetapi`

* In the Image tags ection, type in `latest`

>**Note:** By default Fog Director pulls from hub.docker.com so we will not need to fill out this information.  We will also not need to supply a registry name or password since this is a public docker image.

* Click the radio button labeled `Generate from Docker image`.

* Once you have completed those tasks, you can click the submit button and Fog Director will start to download the docker image and prepare the application for deployment.

* Once the process is finished, Fog Director will bring you to the web page that represents the IOx application. From there we will click the publish button.

* After we have published the app, click the app box to navigate to the app site.

* From there we will click install.

* Next we will add the devices we want to install the app to and then click next.

* Click the down arrow on **Configure Networking**

* On the networking box, click the down arrow and select **iox-nat0** and then click the **REASSIGN NETWORKS** button.

* Now click the **Done Let's Go** button at the bottom of the page.

* Once the app is successfully installed let's check to make sure it is working... [http://10.10.20.51:8282/api_call.html](http://10.10.20.51:8282/api_call.html) or [http://10.10.20.52:8282/api_call.html](http://10.10.20.52:8282/api_call.html)




