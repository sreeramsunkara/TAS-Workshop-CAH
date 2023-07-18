![](./images/vmware-logo.png)

# VMware Tanzu Application Service Workshop 
### (9:00AM to 12:00PM on 01-28-2021)

- If you haven't yet, please go to the [prerequisites](https://github.com/sreeramsunkara/TAS-Workshop-CAH-Prerequisites#vmware-tanzu-application-service-workshop) page and execute the required steps for today's workshop.

#
## Introduction to Tanzu Application Service (TAS)

- [Tanzu Application Service](https://tanzu.vmware.com/application-service) is a PaaS (Platform as a Service) based on [Open Source Cloud Foundry](https://docs.cloudfoundry.org/concepts/overview.html).

- At a glance:
   - [Developers love TAS](https://www.youtube.com/watch?v=xdw_9dADM-4) because it frees and empowers them to do what they love most: to code.
   - TAS is a Cloud Native Polyglot platform supporting Java, Spring, .NET, NodeJS, ...
   - TAS is an HA platform that runs on Any Cloud delivering the same developer experience on AWS, Azure, GCP and vSphere.
   - TAS helps companies deliver better software, faster and more frequently.
   - TAS Operators can patch and upgrade the platform without downtime.
   - TAS = Speed + Stability + Scalability + Security + Savings

-----------------------------------------------------
## Guidelines & Conventions for this Workshop 

- This self-paced workshop includes presentations, videos, demos and most of all, hands-on labs. 
- The labs are interdependent and must be executed in order.
- The lab environments will only be available during the ~5hrs (a limited window of time) dedicated for the joint start and self-paced completion of the workshop.
- Please use the [Workshop Google Sheet](https://docs.google.com/spreadsheets/d/1IRY5QVNfI5fkrqmpbwCagM5kffEysAnsXoYI68mdlKo/edit?usp=sharing) to claim a UserID for this workshop. For example, Sreeram Sunkara is user1.
- Update the same [Workshop Google Sheet](https://docs.google.com/spreadsheets/d/1IRY5QVNfI5fkrqmpbwCagM5kffEysAnsXoYI68mdlKo/edit?usp=sharing)  as you progress through the Labs, by placing an "X" in the appropriate column.
- Each workshop participant will be assigned a Ubuntu VM previously set up for the execution of hands-on Labs. Your Laptop or Desktop will only be used for two purposes: 
     - SSH'ing or PuTTY'ing into the Ubuntu VM 
     - Browsing web pages
- When carrying out hands-on labs, you will be asked to cut-&-paste the commands shown `in boxes like this one` from this GitHub page to your Ubuntu VM Terminal Window. However, when issuing commands, please make sure to alter the UserID to match the one you have claimed, e.g.:
  - `ssh -i cah.pem ubuntu@user3.tanzu4u.net` is for `user3` 
  - `ssh -i cah.pem ubuntu@user15.tanzu4u.net` is for `user15` 
- In order to simplify the cut-&-paste-&-replace steps described above, once you are operating on your Ubuntu VM Terminal, we will define environment variables that will hold your specific login name as claimed in the [Workshop Google Sheet](https://docs.google.com/spreadsheets/d/1IRY5QVNfI5fkrqmpbwCagM5kffEysAnsXoYI68mdlKo/edit?usp=sharing). In this way, the cut-&-paste steps will not require you to edit the command line before pressing `return`.
- As you work through the labs, please make every **effort to not just cut-&-paste-&-execute the labs without actually asking yourself a few questions**:
   - Why am I being asked to cut-&-paste-&-execute these commands?
   - What do I think these commands will do (before actually running them)?
   - What is the role of the person who will be executing these commands in the future?

- Throughout this document, when it's time for hands-on labs, you will see the following icon:
     
![](./images/lab.png)


-----------------------------------------------------
## TAS Workshop Architecture, Installation & Set-up 

- Architecture Diagram of Workshop Environment:

![](./images/TAS-Workshop-Env.png)


-----------------------------------------------------

## LAB-1: SSH into your Linux Workshop VM environment & Test the Command Line Interface tools

![](./images/lab.png)

- Let's start by logging into the Workshop environment from your machine (Mac, PC, Laptop, Desktop, Terminal, VDI). You will need to use the following private key: 
   - [cah.pem](https://raw.githubusercontent.com/sreeramsunkara/TAS-Workshop-CAH-Prerequisites/master/cah.pem) if using a Mac.
   - [cah.ppk](https://raw.githubusercontent.com/sreeramsunkara/TAS-Workshop-CAH-Prerequisites/master/cah.ppk) if using a Windows PC.

- In the pre-requisites section of this workshop, you were asked to use `ssh` or `PuTTY` to access the Ubuntu VM that has been assigned to your [UserID](https://docs.google.com/spreadsheets/d/1IRY5QVNfI5fkrqmpbwCagM5kffEysAnsXoYI68mdlKo/edit?usp=sharing). Please go ahead and create a Terminal Session into your VM. The example shown below applies to `user1` if he or she had downloaded the `cah.pem` key to a Mac. If you need, the `PuTTY` instructions for Windows PC users can be found [here](./PuTTY_and_SSH.md).

```
ssh -i ~/Downloads/cah.pem ubuntu@user1.tanzu4u.net 
```

- Once logged in, you can ignore any messages that ask you to perform a `do-release-upgrade`. 

- Please check whether the greeting information matches your UserID. For example, `user1` should see something like this:

```
my_number is 1
openjdk version "1.8.0_252"
OpenJDK Runtime Environment (build 1.8.0_252-8u252-b09-1~18.04-b09)
OpenJDK 64-Bit Server VM (build 25.252-b09, mixed mode)
Your UserID is user1
Your DevopsID is devops1
Your Namespace in the Shared-Cluster is namespace1
Your role in the Shared-Cluster is vmware-role1
```

- If you believe your greeting information to be wrong, please alert the workshop organizers. 

- If all is well, please proceed by executing the following commands. These commands will validate that your VM has all the necessary CLIs and frameworks for this workshop. Your VM may seem a bit slow in the beginning but it will speed up as you progress through the labs.

```
cf --version
git version
java -version
mvn -version
gfsh version
dotnet --version
```

- If any of the commands shown above did not work or produced and error, please alert the workshop organizers.

- Please proceed by executing the following commands. They are all you need to deploy your first App to the Cloud.

```
cd ~
git clone https://github.com/rm511130/chess
cd ~/chess; rm README.md; ls
cf login -a api.sys.salinas.cf-app.com --skip-ssl-validation -u $user -p password
cf push $user-chess
```

- Once the `cf push` operation ends, you should see a message similar to the one shown below:

```
name:              user1-chess
requested state:   started
routes:            user1-chess.apps.salinas.cf-app.com              <------ Use this URL to access your App
last uploaded:     Fri 17 Jul 15:58:21 EDT 2020
stack:             cflinuxfs3
buildpacks:        php_buildpack

type:            web
instances:       1/1
memory usage:    512M
start command:   $HOME/.bp/bin/start
     state     since                  cpu    memory          disk           details
#0   running   2020-07-17T19:58:39Z   0.7%   27.2M of 512M   246.4M of 1G
```

- Open a browser window to access your URL. In the example above, the URL is `https://user1-chess.apps.salinas.cf-app.com`. Your URL will begin with your UserID.

- Back on your Workshop VM, please execute a simple `ls` command to see the file that you `git cloned` to your Workshop VM. 

- _A little bit of trivia -_ you must take a look at the contents of `index.php` using the following command:

```
cat index.php
```

- `index.php` is written in JavaScript. It contain a very unique piece of code that makes no additional calls to any external libraries. It should not surprise you to know that Mr. Oscar Toledo is one of the world's top code obfuscators. 

**Let's recap:** 
- You ssh'ed into your Workshop VM and verified the versions of certain installed CLIs (Command Line Interface) such as the cf CLI.
- You used `cf login` to point to a TAS platform and to login. You then used `cf push` to  push your first App to TAS: a game of chess.
- Please note that:
  - Your Chess App has a FQDN (fully qualified domain name)
  - Your App is secured by a valid Certificate which enables HTTPS communication
  - Your App was containerized using curated packages and deployed on the cloud in an HA environment
  - As a developer, you did not have to worry about container filesystems and dependencies.
  - Your App code, `index.php`, had no dependencies linked to the PaaS or IaaS you are using. You are completely cloud agnostic.
  - Your App is running on a platform that is 100% up to date with the latest known [CVE](https://cve.mitre.org/) patches
  - You did not have to open any tickets with Infrastructure, Operations, Networking, Security, ... to deploy your App.
  - And yet, access to your Chess App is going through routers, load balancers, firewalls and benefiting from valid certificates. 
  - Your Chess App has also been wired-in for logging (with log consolidation) and APM (Application Performance Monitoring).
  - No wonder developers love TAS.

- Congratulations, you have completed LAB-1.

Please update the [Workshop Google Sheet](https://docs.google.com/spreadsheets/d/1IRY5QVNfI5fkrqmpbwCagM5kffEysAnsXoYI68mdlKo/edit?usp=sharing) with an "X" in the appropriate column.


-----------------------------------------------------

## LAB-2: Apps Manager & CF CLI

![](./images/lab.png)

- The web-based [Apps Manager](https://docs.pivotal.io/platform/application-service/2-10/console/) application helps you manage users, organizations, spaces, and applications.
- Let's log into Apps Manager and take a look around. Please open a browser at [https://apps.sys.salinas.cf-app.com](https://apps.sys.salinas.cf-app.com) and log-in using your UserID and `password`. Make sure to use your UserID, the one you claimed in the [Workshop Google Sheet](https://docs.google.com/spreadsheets/d/1IRY5QVNfI5fkrqmpbwCagM5kffEysAnsXoYI68mdlKo/edit?usp=sharing), and not `user1`.

![](./images/apps-manager.png)

- Once you are logged-in, you should see a `Home` page similar to the one shown below. Don't click on anything just yet.

- Please let the meeting organizers know that you are ready for the **Apps Manager Demo**

- Everyone will be asked to pause their activities for a short demo of Apps Manager.

#### Apps Manager Home-Page View

![](./images/apps-manager-1to8.png)

- Follow the numbers on the picture above. We will let you know when to click on them.

1. Your `org` is a logical segmentation of your PaaS environment that allows `Org Managers` to control settings and resources, such as `spaces`, `domains`, and `members` associated with the selected `space`, in Apps Manager. Corporations typically use `Org` structures to align/organize TAS around Business Units, Company Divisions, Products, or Program Areas.

2. `Orgs` can have one or many `Spaces`. `Spaces` are often named after Life-Cycle-Mgmt phases such as Prod, Dev, QA, Test, Sandbox. It's within `Spaces` that Apps are executed. `Memory` is a resource managed at the `Org` level. In the diagram shown above, next to the #2 yellow pointer, your `Org` is using a certain percentage of the 10GB allocated to it by the Platform Team of administrators.

3. Your `Org` has only one `Space` at the present time. When `user9` first logged into TAS using `cf login` on the terminal session, he/she landed at the `workshop` space within `org9`. That's where we will find the `chess` App.

4. Yellow pointer #4 shows who is logged-in the Apps Manager session.

5. `Tools` is an easy way to get the `cf` CLI to get started.

6. `Docs` take you to [https://docs.pivotal.io/platform/application-service/2-10/overview/intro.html](https://docs.pivotal.io/platform/application-service/2-10/overview/intro.html)

7. `Marketplace` is where you will find services such as  databases, key-value stores, message queuing solutions, and much more.

8. `Home` and the  `Apps Manager`  on the top left of your browser, take you back to the `home page`.

#### Apps Manager Org-Level View

![](./images/AppsMgr-Org.png)

#### Apps Manager Space-Level View

![](./images/AppsMgr-Space.png)

#### Apps Manager App-Level View

![](./images/AppsMgr-App.png)

![](./images/lab.png)

### CF CLI

- Let's get back to your `ssh` session on your Workshop Linux VM. Let's execute a few commands using the `cf` CLI. 

- When looking at Apps Manager, you probably noticed that your Chess App was allocated, by default, 1GB of RAM. You also saw that only one instance of Chess was up and running. Let's change that by executing the following command to decrease the memory down to 100MB and increase the number of App Instances to 3. When asked `Are you sure you want to scale?`, please answer `y`.

```
cf scale $user-chess -m 100M -i 3
```

- Note: the command above will require the restart of your `Chess` App because we are literally reconfiguring the containers that are running your App. Later on, during Lab-4, we will see a technique for making such a change without any Application downtime.

- You can view the changes implemented by the command you just executed in [Apps Manager](https://apps.sys.salinas.cf-app.com), or you can also use the `cf` CLI as follows:

```
cf events $user-chess
```

- Let's create another `Space` under your `Org` and allow one of your colleagues to access this new `Space`. Execute the following command to create a `dev` space under your Org:

```
cf create-space dev -o org$my_number
```

- With the next command, you will be giving every `user` in this workshop access to the `dev` space you just created. 

```
for i in {1..20}; do cf set-space-role user$i org$my_number dev SpaceDeveloper; done
```

- You can now ask any of your colleagues to access the `dev space` in your `org`. The same will apply to you, so use the following command to see what `orgs` are available for you to target, and then target a new `org` that is not yours to access its `dev space`. The `cf target` command below will try to target the `org` and `dev space` of your preceding colleague: e.g. if you are `user5`, he `cf target` command below will attempt to target `org4` and its `dev space`. The `cf target` command may fail if your colleague hasn't yet given you access to his/her `org` and `dev space`, so you can always come back later to see the next two commands working, or you can adjust the `-o orgID` to one that is available to you.

```
cf orgs
cf target -o org$((my_number-1)) -s dev           
```

- Let's execute a few more `cf` CLI commands. Looking at the next few commands, it should be easy to understand what they do. The point of this exercise is just to give you an idea of what you can do with the `cf` CLI. If you have any questions, please ask them to the meeting organizers.

```
cd ~/chess
cf target -o org$my_number -s workshop
cf spaces
cf org-users org$my_number
cf apps
cf app $user-chess
cf marketplace
cf create-app-manifest $user-chess
cat $user-chess_manifest.yml
cf logs $user-chess --recent
cf quotas
```

- Let's now log into one of the containers that is running your `chess` App using the following command:

```
cf ssh $user-chess
```

- You should see a Linux prompt that looks something like this: `vcap@fd9dcac5-3ca3-4c5c-50dc-0024:~$`. Please proceed by executing the following commands:

```
ps -ef
whoami
cat /etc/*release | head -n 4
df -h
```

- The next command you will execute is destructive in nature. We will be `killing` a critical process in the container you are logged in to see what `TAS` is going to do. Please execute the following command:

```
kill -9 $(ps -ef | grep diego-sshd | head -n 1 | awk '{ print $2 }')
```

- You should see an error message similar to the one shown below:

```
FAILED
Error: wait: remote command exited without exit status or exit signal
```

- Now take a look at how your `chess` App is doing using the following command on your Workshop VM:

```
cf events $user-chess
cf app $user-chess
```

- As you may have noticed, the container that you killed was immediately substituted by a new one. TAS is constantly looking at the health of your Apps, as well as the health of the overall environment, to effectively maintain your App Instances running in the most efficient and HA manner possible. This means that TAS is multi-AZ (availability zones) aware and able to distribute your App Instances intelligently across different clusters or data-centers, in order to guarantee maximum availability.

**Let's recap:** 
   - You now know how to access your Apps Manager GUI.
   - You should also be comfortable with the concept of Orgs and Spaces, and the use of the `cf` CLI.
   - You created a new `space` within your `org` and granted access so that your colleagues could use it.
   - You experimented with `cf logs` and `cf events`
   - You saw how `TAS` quickly recovered your App after it was forced to crash.

- Congratulations, you have completed LAB-2.

Please update the [Workshop Google Sheet](https://docs.google.com/spreadsheets/d/1IRY5QVNfI5fkrqmpbwCagM5kffEysAnsXoYI68mdlKo/edit?usp=sharing) with an "X" in the appropriate column.
 

-----------------------------------------------------

## LAB-3: Running a Spring-Boot App on TAS

![](./images/java-spring-tiny.png) ![](./images/lab.png)   

- Execute the following commands:

```
cd ~ 
git clone https://github.com/rm511130/spring-petclinic.git
cd spring-petclinic
```
```
./mvnw -Dmaven.test.skip=true package
```

- Now let's `cf push` your App to `TAS`. Please execute the following commands:

```
cat manifest.yml
cf push
```

- Once the `cf  push` has finished executing, you should be able to access your `Petclinic` App using the URL shown in the output logs. In the example shown below, the URL for the `Petclinic` App can be seen on the 3rd line, next to the word `routes:`. It's `https://spring-petclinic-happy-crane-ie.apps.salinas.cf-app.com`, a uniquely generated URL, because the `manifest.yml` file contained the following line: `random-route: true`. 

```
name:              spring-petclinic
requested state:   started
routes:            sspring-petclinic-happy-crane-ie.apps.salinas.cf-app.com
last uploaded:     Sun 19 Jul 23:04:53 UTC 2020
stack:             cflinuxfs3
buildpacks:        client-certificate-mapper=1.11.0_RELEASE container-security-provider=1.16.0_RELEASE
                   java-buildpack=v4.29.1-offline-https://github.com/cloudfoundry/java-buildpack.git#864477c java-main java-opts
                   java-security jvmkill-agent=1.16.0_RELEASE open-j...

type:            web
instances:       1/1
memory usage:    2048M
start command:   JAVA_OPTS="-agentpath:$PWD/.java-buildpack/open_jdk_jre/bin/jvmkill-1.16.0_RELEASE=printHeapHistogram=1
                 -Djava.io.tmpdir=$TMPDIR -XX:ActiveProcessorCount=$(nproc)
                 -Djava.ext.dirs=$PWD/.java-buildpack/container_security_provider:$PWD/.java-buildpack/open_jdk_jre/lib/ext
                 -Djava.security.properties=$PWD/.java-buildpack/java_security/java.security $JAVA_OPTS" &&
                 CALCULATED_MEMORY=$($PWD/.java-buildpack/open_jdk_jre/bin/java-buildpack-memory-calculator-3.13.0_RELEASE
                 -totMemory=$MEMORY_LIMIT -loadedClasses=18868 -poolType=metaspace -stackThreads=250 -vmOptions="$JAVA_OPTS") && echo JVM
                 Memory Configuration: $CALCULATED_MEMORY && JAVA_OPTS="$JAVA_OPTS $CALCULATED_MEMORY" && MALLOC_ARENA_MAX=2
                 SERVER_PORT=$PORT eval exec $PWD/.java-buildpack/open_jdk_jre/bin/java $JAVA_OPTS -cp $PWD/.
                 org.springframework.boot.loader.JarLauncher
     state     since                  cpu    memory         disk         details
#0   running   2020-07-19T23:05:15Z   0.0%   355.2M of 2G   156M of 1G
```

![](./images/petclinic.png)

- Now execute the following commands to run a `cf run-task`:

```
export my_route=$(cf app spring-petclinic | grep route | awk '{ print $2 }')
export my_task="cf run-task spring-petclinic \"while (true); do curl -k http://"$my_route"; done\" --name $user-task"
echo $my_task | sh
echo $my_task
```

- The `Linux Shell` commands shown above assemble a simple one-line command to execute a `cf run-task`. The `task` performs `curl` commands against the URL of your `Petclinic` App. The actual one-line command is contained in the `$my_task` variable which was `echo'ed` out for you to see.

-  Now let's take a look at [Apps Manager](https://apps.sys.salinas.cf-app.com/). Please click on the various links shown in the following image. To get to your `Petclinic` App, you will need to navigate [`Apps Manager`](https://apps.sys.salinas.cf-app.com/), going from your `Org`, to your `Workshop Space`, and to your `App`.

![](./images/SpringAppTAS.png)

**Let's recap:** 
   - You used Maven to create a `jar` file of your `Petclinic` App.
   - You then used `cf push` to push your `Petclinic` App to `TAS`, and you used a browser to validate that it was working as expected.
   - You now have two Apps running on TAS: `Chess` & `Petclinic`. 
   - Your `Petclinic` App is being warmed-up by a never-ending `while (true)` loop which was executed as a `cf run-task`. We will come back to this area in a subsequent lab, when we will address App auto-scaling.
   - You used Apps Manager to see how your `Petclinic` App is performing. App Manager showed you metrics, App tracing details, and health-check details.

- Congratulations, you have completed LAB-3.

Please update the [Workshop Google Sheet](https://docs.google.com/spreadsheets/d/1IRY5QVNfI5fkrqmpbwCagM5kffEysAnsXoYI68mdlKo/edit?usp=sharing) with an "X" in the appropriate column.


-----------------------------------------------------

## LAB-4: Blue-Green, Zero-Downtime-Deployment

 ![](./images/lab.png)   

- Your `Chess` App comes in different colors: blue & green.
 
- Please keep your browser open on your `Chess` App at `http://userID-chess.apps.salinas.cf-app.com`. You will be asked to hit the refresh button multiple times to see the seamless transition between Blue-Chess to Green-Chess.

- Let's get started by making some changes to your `Chess` App. Please execute the following commands to change your `Chess` from blue to green:

```
cd ~/chess
sed -i 's/c0c/1ffe/g' index.php
sed -i 's/blue/green/g' index.php
```

- As soon as you have executed the command shown below, start to hit the refresh button on the browser to see your `Chess` App change from blue to green. The `watch` command shown below will also show you what is happening to your App:

```
cf v3-zdt-push $user-chess -b php_buildpack
watch cf app $user-chess
```

- Once the change has happened, you can use `CTRL-C` to exit the `watch` loop.

- Let's take a look at `Revisions` in your Apps Manager. Please navigate to the `Revisions` tab of your `Chess` App in the Apps Manager GUI.

- You can also define environment variables on-the-fly without needing to restage or restart your App. Please execute the following commands:

```
cf env $user-chess
cf v3-set-env $user-chess color green
cf env $user-chess
```

- You can see, from how the commands above behave, that if your App looks at environment variables to determine its behavior, the `cf v3-set-env` command is indeed a very powerful tool. 

- During Lab-2 we re-sized the `Chess` containers down from 1GB to 100MB RAM. This operation led to App Downtime because the App was restaged. Back then, we mentioned that there was a way of circumventing the need for downtime. Please execute the following commands to see how to resize your containers without any downtime. As soon as you start to execute the commands shown below, you can start to hit the refresh button on the browser to validate that your `Chess` App will not go down during the vertical resizing of the containers.

```
cd ~/chess
cf push $user-chess-v2 -m 99M -i 3 -b php_buildpack
cf map-route $user-chess-v2 apps.salinas.cf-app.com --hostname $user-chess
cf stop $user-chess
```

- You still have access to your first version of the `Chess` App with its 100MB of RAM, but the App now serving the request on the same URL is the v2 which is now sized to consume 99MB of RAM.

```
cf apps
cf unmap-route $user-chess-v2 apps.salinas.cf-app.com --hostname $user-chess-v2
cf apps
```
- We can complete the switch by deleting the old version of the `Chess` App. Please execute the following commands:

```
cf delete -f $user-chess
cf apps
```

- The `cf map-route` command enables the dark launching of new features and their eventual introduction into the mainstream application. You can also have many routes mapped to the same App. For example, please execute the following commands:

```
cf map-route spring-petclinic apps.salinas.cf-app.com --hostname $user-pets
cf apps
```

- Your `Petclinic` App now has an easier URL to remember, and it did not lose the previous URL.

**Let's recap:** 
- You performed zero-downtime deployment of a green-Chess App without any downtime.
- You introduced an environment variable to your environment without any downtime.
- You used `cf map-route` to introduce a new version of your App without any downtime and to simplify your `Petclinic` App URL.

- Congratulations, you have completed LAB-4.

Please update the [Workshop Google Sheet](https://docs.google.com/spreadsheets/d/1IRY5QVNfI5fkrqmpbwCagM5kffEysAnsXoYI68mdlKo/edit?usp=sharing) with an "X" in the appropriate column.


-----------------------------------------------------
## LAB-5: Application Autoscaling

 ![](./images/lab.png)   

- Let use a tiny program written in "C" to create some additional load on your "Petclinic" App. Please execute the following commands:

```
cd ~
git clone https://github.com/rm511130/curly
cd curly
export replace_me="chess.cfapps.io"
export replace_by=$user-pets.apps.salinas.cf-app.com
sed -i "s|$replace_me|$replace_by|g" cf_push_example_binary_buildpack.sh 
chmod +x cf_push_example_binary_buildpack.sh 
./cf_push_example_binary_buildpack.sh 
```

- Once the commands above have completed, go to your Apps Manager to see how `Petclinic` is performing.

- For the purposes of this Lab, we wish to keep the average %CPU between 15% and 25%. 

1. Using your Apps Manager, navigate to your `Petclinic` overview page and please click on **Enable Autoscaling**

2. Click on **Manage Autoscaling**

3. An autoscaling tab will pop from the right side of your browser. Click on _Scaling Rules_ **Edit**  

4. Click on **Add Rule**

5. The Rule Type should be **CPU Utilization**

6. In the box for _scale down if less than_ type in **15** 

7. In the box for _scale up if more than_ type in **25** and click on **Save**

8. On the _INSTANCE LIMITS_ section, set _Minimum_ to **1** and set _Maximum_ to **10** and then hit apply changes.

9. You can now **close** the _Autoscaling Tab_ by clicking on the *x* at the top left-hand corner of the tab.

10. Now observe the system at work.

- Can this type of scaling be done via scripts and configuration files? Yes, absolutely. The first step is to extend the `cf` CLI features by using plug-ins. Let's add the _Cloud Foundry CLI Autoscaler Plug-in_ to your Workshop VM. Please execute the following commands:

```
cf install-plugin -r CF-Community app-autoscaler-plugin
```

- Now let's check what are the `cf` CLI plugins available to you by executing the following command:

```
cf plugins
```

- From here on, you can read about the `cf autoscaler` commands [here](https://github.com/cloudfoundry/app-autoscaler-cli-plugin). For the purposes of this Lab, we will stop here and perform some clean-up. Please execute the following command:

```
cf delete -f curly
cf tasks spring-petclinic
cf terminate-task spring-petclinic $(cf tasks spring-petclinic | head -n 5 | tail -n 1 | awk '{print $1}')
```

**Let's recap:** 
- Autoscaling seamlessly scales our App horizontally by increasing or decreasing the number of App Instances to meet your desired throughput, latency, %CPU, ... goals.

- Congratulations, you have completed LAB-5.

Please update the [Workshop Google Sheet](https://docs.google.com/spreadsheets/d/1IRY5QVNfI5fkrqmpbwCagM5kffEysAnsXoYI68mdlKo/edit?usp=sharing) with an "X" in the appropriate column.


-----------------------------------------------------
## LAB-6: Services and Service Instances

 ![](./images/lab.png)   

- TAS makes it really easy to create and use services such as databases, message queues, key-value stores, cache engines, ... 

- Let's create a small MySQL database just for you. Please execute the following command on your Workshop VM:

```
cf marketplace | grep -i mysql
cf create-service p.mysql db-small $user-mysql
cf service $user-mysql
```

- As you may have noticed, we picked a platform-managed MySQL implementation `p.mysql` in order to keep this Lab 100% compatible with any IaaS provider. Had we picked an AWS specific MySQL, we would be locking our implementation to AWS for that service. 

- Let's get a new application called `MovieFun` up and running. Please execute the following command on your Workshop VM to list the buildpacks installed on TAS and available for you to use: 

```
cf buildpacks
```

- Let's download the `MovieFun` App and create a WAR file using Maven. Please execute the following commands:

```
cd ~
git clone https://github.com/rm511130/moviefun
cd moviefun
mvn clean package -DskipTests -Dmaven.test.skip=true
```
- Now let's run our `MovieFun` App on TAS. Please execute the following command:

```
cf push $user-moviefun
```

- The `MovieFun` App requires the use of the [TomEE Buildpack](https://github.com/cloudfoundry-community/tomee-buildpack) because it is an older JVM based application. The TomEE Buildpack is designed to run many JVM-based applications (Grails, Groovy, Java Main, Play Framework, Spring Boot, and Java EE Web Profile) with no additional configuration. It supports configuration of the standard components, and extension to add custom components. This specific buildpack has been customized to use the Tomcat Web Server.  

- You may have noticed that the `cf buildpacks` command did not show the `TomEE Buildpack` as being installed in TAS, but we were able to use its URL in the `manifest.yml` file. This technique opens your world to many more framework and languages available at: https://github.com/cloudfoundry-community/cf-docs-contrib/wiki/Buildpacks

- Let's take the URL of the `MovieFun` App you just deployed and take a look at how the `MovieFun` App works. 

- Please access the `MovieFun` using a browser and then click on **set-up** and then on **Go to main app** per the example shown below:

![](./images/MovieFun.png)

- Feel free to add or delete records, then execute the following command on your Workshop VM:

```
cf restart $user-moviefun
```

- Once the `MovieFun` App is back up and running, use the same URL to access the App and then click on **index** (on the very first page) and you should see that all the records were lost due to the App restart. Hold that thought!

- Let's get back to the MySQL DB: the `cf create-service p.mysql` process takes 3 to 4 minutes. Back on your Workshop VM Terminal, let's check whether your MySQL DB instance is up and running, i.e. `status: create succeeded`. Please execute the following command: 

```
cf service $user-mysql
```

- If you see `status: create succeeded`, then please proceed, otherwise please wait a minute or so until you see `status: create succeeded` in the output of the `cf service $user-my-sql` command.

- Let's bind your MySQL instance to your `MovieFun` App. Please execute the following commands:

```
cf bind-service $user-moviefun $user-mysql
cf restage $user-moviefun
```

- The `cf restage` is necessary because TAS will need to make configuration adjustments so that your `MovieFun` App may be able to use the MySQL DB instance that you created.

- Once the `cf restage` has finished, let's repeat the same process again:

1. Access your `MovieFun` app using a browser.
2. Click on **set-up** and then on **Go to main app** 
3. Execute the following command:

```
cf restart $user-moviefun
```

4. Once the `MovieFun` is up and running, open a browser to access the App and click on **index** to go straight to the `MovieFun` App screen and check whether your data survived the restart thanks to the MySQL DB.

- Please use Apps Manager to navigate to the home page of your `MovieFun` App. Then look for and click-on `Service`. You'll find `Service (1)` under `Overview` in the menu to the left of the Apps Manager GUI.

![](./images/mysql-creds.png)

- As shown in the picture above, please click on the 3 vertical dots and then on the **View Credentials** link to view the environment variables that were bound to your App. These credentials are unique and when the App Deployment happens in a production environment, we typically place them in CredHub to further protect them from prying eyes. Please execute the following command to see your App's environment variables:

```
cf env $user-moviefun
```

- You can also scale your `MovieFun` App horizontally and TAS will make sure that every instance of your App is bound correctly to the  MySQL DB. Please execute the following commands:

```
cf scale $user-moviefun -i 3
```

- Go back to Apps Manager and access the home page of your `MovieFun` App. Now follow the example shown below to **terminate instance**. Go ahead and click on **terminate** for all three App Instances:

![](./images/terminate-instance.png)

- As you have click on **terminate** for all three instances, you should see messages at the top of the screen stating that: `Success: Instance will be terminated and recreated.`

- Now try to access your `MovieFun` App, and verify that your data has not been lost because it has been persisted in the MySQL DB.

- Let's clean-up. Please execute the following commands:

```
cf unbind-service $user-moviefun $user-mysql
cf restage $user-moviefun
```

- Note we're not deleting the MySQL DB instance because we'll need it in the next Lab.

**Let's recap:** 
- Creating a Service requires one simple command: `cf create-service` which can be simplified to `cf cs`
- Binding a Service Instance to an App requires one simple command: `cf bind-service` which can be simplified to `cf bs`
- To make sure your App is aware of a newly bound Service Instance, you should also execute: `cf restage AppName`
- We used an older Java WAR file application called MovieFun to demonstrate how TAS can run all kinds of frameworks and languages with the help of buildpacks.

- Congratulations, you have completed LAB-6.

Please update the [Workshop Google Sheet](https://docs.google.com/spreadsheets/d/1IRY5QVNfI5fkrqmpbwCagM5kffEysAnsXoYI68mdlKo/edit?usp=sharing) with an "X" in the appropriate column.


-----------------------------------------------------
## LAB-7: Microservices-Based Application

 ![](./images/lab.png)   

- Let's deploy an App where a service calls another service that calls a DB. 

![](./images/cities-pic.png)


- Please execute the following commands on your Workshop VM:

```
cd ~
git clone https://github.com/rm511130/cities
cd cities
./gradlew clean build
```

- Once the build process has completed, please proceed with the following commands:

```
cd ~/cities/cities-hello
envsubst < manifest.yml_proto > manifest.yml
cat manifest.yml
cf push
```

- Once the `cf push` has completed, we can test our code by opening a browser at `http://userID-cities-hello.apps.salinas.cf-app.com` where `userID` maps to the UserID you have been using throughout this workshop. 

![](./images/Welcome-Lab7.png)

- If all is well and the screen-shot shown above is similar to what you saw when accessing the `cities-hello` URL, then please proceed by executing the following commands:

```
cd ~/cities/cities-service
envsubst < manifest.yml_proto > manifest.yml
sed -i 's/ourpcf.com/salinas.cf-app.com/g' manifest.yml
cat manifest.yml
cf push 
```

- Once the `cf push` has completed, we can test our `cities-service`. Please execute the following commands:

```
export CitiesServiceUrl=$(cf a | grep cities-service | awk '{ print $6 }')
echo $CitiesServiceUrl
curl $CitiesServiceUrl/cities/62
```

- You should see the following results indicating that your `cities-service` is bound to your MySQL DB and that it is responding to API calls.

```
{
  "name" : "PLANO",
  "county" : "COLLIN",
  "stateCode" : "TX",
  "postalCode" : "75074",
  "latitude" : "+33.109044",
  "longitude" : "-096.578819",
  "_links" : {
    "self" : {
      "href" : "http://cities-service.apps.salinas.cf-app.com/cities/62"
    }
  }
}
```

- If your results match the ones shown above, please proceed with the following commands:

```
cf create-user-provided-service cities-ws -p "{ \"citiesuri\":\"http://$CitiesServiceUrl\" }"
cf services
```
- We're using a `cf cups` _(cups = create-user-provided-service)_ command to pass the URI of your `cities-service` as an environment variable to the `cities-ui` service. The `manifest.yml` we will be using next, includes a line that binds the `cities-ws` _(ws = web service)_ to the  `cities-ui` service.

```
cd ~/cities/cities-ui
envsubst < manifest.yml_proto > manifest.yml
sed -i 's/ourpcf.com/salinas.cf-app.com/g' manifest.yml
cat manifest.yml
cf push
```

- Once the `cf push` has completed, we test the complete application by using a browser opened at: `http://userID-cities-ui.apps.salinas.cf-app.com where `userID` is the UserID you have been using throughout this workshop.

![](./images/cities-app.png)

- Once you are satisfied with the results, please execute the following commands to clean-up:

```
cf delete -f cities-hello
cf us cities-service $user-mysql
cf delete -f cities-service
cf us cities-ui cities-ws
cf ds -f cities-ws
cf delete -f cities-ui
cf ds -f $user-mysql
```

**Let's recap:** 
- You deployed several services to TAS in order to deploy a Postal Code search App:
   - A cities-hello service just to make sure the `gradle` command worked as expected
   - A cities-service that works with a MySQL DB to which it was bound
   - A cities-ws user-provided-service that wrapped the cities-service
   - A cities-ui service bound to the cities-ws user-provided-service
- After testing your `Cities` App, you proceeded with the deletion of the `Cities` App:
   - All the containers involved in running your App were unwired automatically from log collection and application performance monitoring.
   - All the URLs, Firewall entries, and routes were also cleaned-up by TAS automatically for you.
   - All containers and service definitions were deleted, and their resources returned to the pool of resources available to TAS.
   - All DB instances and their credentials were cleaned-up and their resources returned to the pool of resources available to TAS.
- While Lab-7 was being executed:
   - Other TAS users were isolated from your work (and vice-versa) given that their work was isolated within their Org.
   - You could have given access to your Org and Space to any one of your colleagues, to increase the size of the team working on the project.
   - No support tickets were opened at any point in time. 

- Congratulations, you have completed LAB-7.

Please update the [Workshop Google Sheet](https://docs.google.com/spreadsheets/d/1IRY5QVNfI5fkrqmpbwCagM5kffEysAnsXoYI68mdlKo/edit?usp=sharing) with an "X" in the appropriate column.


-----------------------------------------------------
## LAB-8: Docker Images

 ![](./images/lab.png)   

- Every time you `cf push` an App, TAS does a lot of work on your behalf. Some of this work is in the InfoSec space: i.e. security. VMware is constantly scanning and monitoring countless combinations of frameworks and libraries to make sure they are protected against known CVEs (Common Vulnerabilities and Exposures). Our SLA is to deliver to you patches that can be seamlessly implemented by Operations at the Operating System and Dependencies levels as TAS builds, deploys and runs your App in safe containers:

![](./images/cnb.png)

- The danger with Docker Images, when they are the object supplied to TAS, is that no one knows what's inside the "black-box". Pro-active scanning can detect issues with Docker Images, but it's up to the original developers to check whether the OS and middleware layers are up to date, functioning with each other, and not exposed to known CVEs. This is toil that TAS eliminates if you simply provide it your `code` instead of a pre-packaged `image`. Let TAS build, deploy, run and monitor your code as a platform-built image, and everyone can sleep better at night.

![](./images/docker-problem.png)

- Having explained the concept of platform-built images and the value it represents, given that TAS is essentially building OSS CNCF (Open Source Cloud Native Computing Foundation) compliant "Docker Images" for you, it should come as no surprise that TAS understands and can run Docker Images as containers. You are exposing yourself and your company to potential risks, but it can be done.

- Let's see how to run Docker Images in TAS. Please execute the following commands:

```
cd ~
mkdir -p fact
cd ~/fact
cf push $user-fact --docker-image rmeira/fact -m 100M
curl $user-fact.apps.salinas.cf-app.com/1000; echo
```

**Let's recap:** 
- TAS builds and runs container images using curated components at the OS and middleware/dependencies layers. All TAS needs is your `code`.
- TAS is able to run container images built using Docker on your PC or Mac. These images are executed in restricted containers that are not allowed to assume `root` level access, but what is placed in-side the Docker Image is the responsibility of the developer in charge. 
- By the way, the `fact` Docker Image used in this lab, was scanned on 7/21/2020 and presented no exposures to known CVEs.

- Congratulations, you have completed LAB-8.

Please update the [Workshop Google Sheet](https://docs.google.com/spreadsheets/d/1IRY5QVNfI5fkrqmpbwCagM5kffEysAnsXoYI68mdlKo/edit?usp=sharing) with an "X" in the appropriate column.

-----------------------------------------------------
## LAB-9: cf local

 ![](./images/lab.png)   

- If you find yourself stuck without access to a TAS environment, but needing to test a web-service locally, we have a `cf local` tool for you.

- Please execute the following commands to experiment with `cf local`:

```
cd ~
git clone https://github.com/rm511130/fact
cd ~/fact
cf local stage fact
cf local run fact
```

- Once the commands above have completed, you should see an output similar to the one shown below:

```
Running fact on port 44601...                                                  
[fact] 2020-07-21T23:17:11.924665826Z 2020/07/21 23:17:11 Starting Factorial Application...
[fact] 2020-07-21T23:17:11.924711377Z 2020/07/21 23:17:11 Using port8080
```

- In the example above, the `cf local run fact` command chose to use port `44601`, so that's the port we will use to locally test the `fact` program. 

- Please take note of the `port` number that your `cf local run fact` command chose to use.

- Your Workshop VM Terminal should be busy running the `cf local run fact` command. You can use a separate terminal window to test your program, but we suggest you press:

```
CTRL-Z
```

- A command prompt will be returned to you as a result of pressing `CRTL-Z`. Please execute the following command:

```
bg
``` 

- The `bg` command runs the previous process in the background. We can now test your `fact` program by using the following command. Please make sure to use your `port` number and **not** the `44601` shown below when you execute the next command:

```
curl 127.0.0.1:44601/100; echo
```

- You should see the results of `100!` on your screen.

- Time to execute the following command to bring that background process back to the foreground.

```
fg
```

- Now press `CTRL-C` to cancel out of the `cf local run fact`

**Let's recap:** 
- `cf local` allows you to test smaller chunks of code locally as-if you had TAS installed locally on your machine.

- Congratulations, you have completed LAB-9.

Please update the [Workshop Google Sheet](https://docs.google.com/spreadsheets/d/1IRY5QVNfI5fkrqmpbwCagM5kffEysAnsXoYI68mdlKo/edit?usp=sharing) with an "X" in the appropriate column.

-----------------------------------------------------
## LAB-10: .Net Core

![](./images/dotnet.png) ![](./images/lab.png) 

- This is a quick Lab to show you TAS support for .NET Core. Let's try it. Please execute the following commands:

```
cd ~; rm -rf dotnet
git clone https://github.com/rm511130/dotnet.git 
cd ~/dotnet
dotnet new global
dotnet new mvc
```
- Let's make a couple of edits to personalize the Welcome Message and to make sure our code is listening on port `8080` by running the following commands:

```
sed -i "s/5001/8080/g" tweaking_Index.cshtml_and_Program.cs.sh
source ./tweaking_Index.cshtml_and_Program.cs.sh
```

- Now let's `cf push` using the following command:

```
cf push $user-dotnet -b dotnet_core_2_3_11 -m 100M
```

- Once the `cf push` has completed, please open a browser to test your .NET Core Welcome App. The URL will be `http://userID-dotnet.apps.salinas.cf-app.com` where `UserID` is your UserID.

**Let's recap:** 
- TAS supports multiple languages:
  - In Lab-9 your code was written in Go Lang. 
  - The `Chess` App was an example of JavaScript. 
  - `MovieFun` and `Petclinic` exemplified Java and Spring.
  - `Curly` was written in `C`.
  - Lab-10 demonstrated TAS support for .NET Core.

- Congratulations, you have completed LAB-10 and finished the Workshop.

Please update the [Workshop Google Sheet](https://docs.google.com/spreadsheets/d/1IRY5QVNfI5fkrqmpbwCagM5kffEysAnsXoYI68mdlKo/edit?usp=sharing) with an "X" in the appropriate column.


-----------------------------------------------------
## CONGRATULATIONS

You have completed all the Labs in this Workshop.

During the course of this Workshop you deployed multiple applications and services. You saw how simple it is to scale and auto-scale application instances. You also experienced application health-management, security features and automated logging and reporting of critical application data. You played games (e.g. Chess), deployed an older Java Enterprise App called MovieFun, and also played with .NET Core and Go Lang.

You also didn't open any support tickets to request access to services such as MySQL or to bind services to applications.

Congratulations !! If you have a few extra minutes, we would like to hear from you. How was the workshop experience? 
Please send your comments and questions to sunkarasr@VMware.com.
Thanks to Ralph Meira(rmeira@vmware.com) for creating the content and labs. 

Thank you!!

