![](./images/vmware-logo.png)

# VMware Tanzu Application Service Workshop 
### (8:30AM to 12:30PM on 07-22-2020)

- If you haven't yet, please go to the [prerequisites](https://github.com/rm511130/TAS-Workshop-Prerequisites#vmware-tanzu-application-service-workshop) page and execute the required steps for today's workshop.

#
## Introduction to Tanzu Application Service (TAS)

- [Tanzu Application Service](https://tanzu.vmware.com/application-service) is a PaaS (Platform as a Service) based on [Open Source Cloud Foundry](https://docs.cloudfoundry.org/concepts/overview.html).

- At a glance:
   - [Developers love TAS](https://www.youtube.com/watch?v=xdw_9dADM-4) because it frees and empowers them to do what they love most: to code.
   - TAS is a Cloud Native Polyglot platform supporting Java, Spring, .NET, NodeJs, ...
   - TAS is an HA platform that runs on Any Cloud delivering the same developer experience on AWS, Azure, GCP and vSphere.
   - TAS helps companies deliver better software, faster and more frequently.
   - TAS Operators can patch and upgrade the platform without downtime.
   - TAS = Speed + Stability + Scalability + Security + Savings

- Action item: reserve time on your calendar for the Sept 2 & 3, SpringOne 2020 event. It will be free and 100% on-line. [Read more](https://github.com/rm511130/SpringOne-2020/blob/master/README.md)

-----------------------------------------------------
## Guidelines & Conventions for this Workshop 

- This self-paced workshop includes presentations, videos, demos and most of all, hands-on labs. 
- The labs are interdependent and must be executed in order.
- The lab environments will only be available during the ~5hrs (a limited window of time) dedicated for the joint start and self-paced completion of the workshop.
- Please use the [Workshop Google Sheet](https://docs.google.com/spreadsheets/d/1pV7kOcfzq_bHbXP0pa79NtPMpY3zVHSAZ8HpHaHyrKI/edit?usp=sharing) to claim a userID for this workshop. For example, Ralph Meira is user1.
- Update the same [Workshop Google Sheet](https://docs.google.com/spreadsheets/d/1pV7kOcfzq_bHbXP0pa79NtPMpY3zVHSAZ8HpHaHyrKI/edit?usp=sharing)  as you progress through the Labs, by placing an "X" in the appropriate column.
- Each workshop participant will be assigned a Ubuntu VM previously set up for the execution of hands-on Labs. Your Laptop or Desktop will only be used for two purposes: 
     - SSH'ing or PuTTY'ing into the Ubuntu VM 
     - Browsing web pages
- When carrying out hands-on labs, you will be asked to cut-&-paste the commands shown `in boxes like this one` from this github page to your Ubuntu VM Terminal Window. However, when issuing commands, please make sure to alter the userID to match the one you have claimed, e.g.:
  - `ssh -i fuse.pem ubuntu@user3.pks4u.com` is for `user3` 
  - `ssh -i fuse.pem ubuntu@user15.pks4u.com` is for `user15` 
- In order to simplify the cut-&-paste-&-replace steps described above, once you are operating on your Ubuntu VM Terminal, we will define environment variables that will hold your specific login name as claimed in the [Workshop Google Sheet](https://docs.google.com/spreadsheets/d/1pV7kOcfzq_bHbXP0pa79NtPMpY3zVHSAZ8HpHaHyrKI/edit?usp=sharing). In this way, the cut-&-paste steps will not require you to edit the command line before pressing `return`.
- As you work through the labs, please make every effort to not just cut-&-paste-&-execute the labs without actually asking yourself a few questions:
   - Why am I being asked to cut-&-paste-&-execute these commands?
   - What do I think these commands will do (before actually running them)?
   - What is the role of the person who will be executing these commands in the future?

- Throughout this document, when it's time for hands-on labs, you will see the following icon:
     
![](./images/lab.png)


-----------------------------------------------------
## TAS Workshop Architecture, Installation & Set-up 

- Architecture Diagram of Workshop Environment:

![](.images/TAS-Workshop-Env.png)


-----------------------------------------------------

## LAB-1: SSH into your Linux Workshop VM environment & Test the Command Line Interface tools

- Let's start by logging into the Workshop environment from your machine (Mac, PC, Laptop, Desktop, Terminal, VDI). You will need to use the following private key: 
   - [fuse.pem](./fuse.pem) if using a Mac.
   - [fuse.ppk](./fuse.ppk) if using a Windows PC.

- Note that the examples shown below apply to `user1`. If, for example, you are `user11`, your Ubuntu VM will be at `user11.pks4u.com`.

![](./images/lab.png)

- In the pre-requisites section of this workshop, you were asked to use `ssh` or `PuTTY` to access the Ubuntu VM that has been assigned to your [UserID](https://docs.google.com/spreadsheets/d/1pV7kOcfzq_bHbXP0pa79NtPMpY3zVHSAZ8HpHaHyrKI/edit?usp=sharing). Please go ahead and create a Terminal Session into your VM. The example shown below applies to `user1` if he or she had downloaded the `fuse.pem` key to a Mac. If you need, the `PuTTY` instructions for Windows PC users can be found [here](./PuTTY_and_SSH.md).

```
ssh -i ~/Downloads/fuse.pem ubuntu@user1.pks4u.com 
```

- Once logged in, you can ignore any messages that ask you to perform a `do-release-upgrade`. 

- Please check whether the greeting information matches your UserID. For example, `user22` should see something like this:

```
my_number is 22
openjdk version "11.0.7" 2020-04-14
OpenJDK Runtime Environment (build 11.0.7+10-post-Ubuntu-2ubuntu218.04)
OpenJDK 64-Bit Server VM (build 11.0.7+10-post-Ubuntu-2ubuntu218.04, mixed mode, sharing)
Your UserID is user22
Your DevopsID is devops22
Your Namespace in the Shared-Cluster is namespace22
Your role in the Shared-Cluster is vmware-role22
```

- If you believe your greeting information to be wrong, please alert the workshop organizers. 

- If all is well, please proceed by executing the following commands. These commands will validate that your VM has all the necessary CLIs and frameworks for this workshop.

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
cf login -a api.sys.ourpcf.com -u $user -p password
cf push $user-chess
```

- Once the `cf push` operation ends, you should see the a message similar to the one shown below:

```
name:              user1-chess
requested state:   started
routes:            user1-chess.apps.ourpcf.com              <------ Use this URL to access your App
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

- Open a browser window to access your URL. In the example above, the URL is `https://user1-chess.apps.ourpcf.com`. Your URL will begin with your UserID.

- Execute a simple `ls` command to see the file that was `git cloned` to your Workshop VM.

**Let's recap:** 
- You ssh'ed into your Workshop VM and verified the versions of certain installed CLIs (Command Line Interface) such as the cf CLI.
- You used `cf login` to point to a TAS plaform and login. You then used `cf push` to  push your first App to TAS: a game of chess.
- Please note that:
  - Your Chess App has a FQDN (fully qualified domain name)
  - Your App is secured by a valid Certificate which enables HTTPS communication
  - Your App was containerized using curated packages and deployed on the cloud in an HA environment
  - As a developer, you did not need to worry about container filesystems and dependencies.
  - Your App code, `index.php`, had no dependencies linked to the PaaS or IaaS you are using. You are completely cloud agnostic.
  - Your App is running on a platform that is 100% up to date with the latest known [CVE](https://cve.mitre.org/) patches
  - You did not have to open any tickets with Infrastructure, Operations, Networking, Security, ... to deploy your App.
  - And yet, access to your Chess App is going through routers, load balancers, firewalls and benefiting from valid certificates. 
  - Your Chess App has also been wired-in for logging (with log consolidation) and APM (Application Performance Monitoring).
  - No wonder developers love TAS.

- Congratulations, you have completed LAB-1.

Please update the [Workshop Google Sheet](https://docs.google.com/spreadsheets/d/1pV7kOcfzq_bHbXP0pa79NtPMpY3zVHSAZ8HpHaHyrKI/edit?usp=sharing) with an "X" in the appropriate column.


-----------------------------------------------------

## LAB-2: Apps Manager & CF CLI

![](./images/lab.png)

- The web-based [Apps Manager](https://docs.pivotal.io/platform/application-service/2-9/console/) application helps you manage users, organizations, spaces, and applications.
- Let's log into Apps Manager and take a look around. Please open a browser at [https://apps.sys.ourpcf.com](https://apps.sys.ourpcf.com) and log-in using your UserID and `password`. Make sure to use your UserID, the one you claimed in the [Workshop Google Sheet](https://docs.google.com/spreadsheets/d/1pV7kOcfzq_bHbXP0pa79NtPMpY3zVHSAZ8HpHaHyrKI/edit?usp=sharing), and not `user1`.

![](./images/apps-manager.png)

- Once you are logged-in, you should see a `Home` page similar to the one shown below. Don't click on anything just yet.

- Please let the meeting organizers know that you are ready for the **Apps Manager Demo**

- Everyone will be asked to pause their activities for a short demo of Apps Manager.

#### Apps Manager Home-Page View

![](./images/apps-manager-1to8.png)

- Follow the numbers on the picture above. We will let you know when to click on them.

1. Your `org` is a logical segmentation of your PaaS environment that allows `Org Managers` to control settings and resources, such as `spaces`, `domains`, and `members` associated with the selected `space`, in Apps Manager. Corporations tipically use `Org` structures to align/organize TAS around Business Units, Company Divisions, Products, or Program Areas.

2. `Orgs` can have one or many `Spaces`. `Spaces` are often named after Life-Cycle-Mgmt phases such as Prod, Dev, QA, Test, SandBox. It's within `Spaces` that Apps are executed. `Memory` is a resource managed at the `Org` level. In the diagram shown above, next to the #2 yellow pointer, your `Org` is using a certain percentage of the 10GB allocated to it by the Platform Team of administrators.

3. Your `Org` has only one `Space` at the present time. When `user9` first logged into TAS using `cf login` on the terminal session, he/she landed at the `workshop` space witin `org9`. That's were we will find the `chess` App.

4. Yellow pointer #4 shows who is logged-in the Apps Manager session.

5. `Tools` is an easy way to get the `cf` CLI to get started.

6. `Docs` take you to [https://docs.pivotal.io/platform/application-service/2-9/overview/intro.html](https://docs.pivotal.io/platform/application-service/2-9/overview/intro.html)

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

- Let's get back to your ssh session on your Workshop Linux VM. Let's execute a few commands using the `cf` CLI. 

- When looking at Apps Manager, you probably noticed that your Chess App was allocated, by default, 1GB of RAM. You also saw that only one instance of Chess was up and running.

- Let's change that by execting the following command to decrease the demands on reserved memory to 100MB and increase the App Instances to 3:

```
cf scale $user-chess -m 100M -i 3
```

- You can view the changes implemented by the command you just executed in [Apps Manager](https://apps.sys.ourpcf.com), or you can also use the `cf` CLI as follows:

```
cf events $user-chess
```

- Let's create another Space under your Org, and allow one of your colleagues to access this new Space. Execute the following commands to create a `dev` space under your Org:

```
cf create-space dev -o org$my_number
```

- Now select a colleague from the [Workshop Google Sheet](https://docs.google.com/spreadsheets/d/1pV7kOcfzq_bHbXP0pa79NtPMpY3zVHSAZ8HpHaHyrKI/edit?usp=sharing) and give him/her access to the `dev` space you just created. In the example below, `user2` is given `SpaceDeveloper` access to the `dev` space in `org1`:

```
cf set-space-role user2 org1 dev SpaceDeveloper
```

- Ask your colleague to 
















