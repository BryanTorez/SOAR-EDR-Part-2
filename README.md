<h1>SOAR EDR Project | Part 2</h1>

<p align="center">
<img src="https://snipboard.io/PImt2S.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<h2>Description</h2>
<br />
<p align="center">
Welcome to part two of five for the series on the SOAR EDR project. If you haven't seen part one where we go over how to build out a Playbook workflow, for this particular project, I highly recommend you go and read that first, as we'll be referencing that workflow throughout the series. 
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  Today, our objective is to install and set up LimaCharlie and making sure that the endpoint is connected and is generating events. By the end of this part, you should have one Windows machine reporting back to LimaCharlie and then we can begin creating our first detection and response rule. I'll be using Vultur, as the cloud provider, and we'll be spinning up a Windows server machine and if you want to follow along, I'll leave a link down below for those who want free $100 credit.
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  If you don't want to use the cloud, that is perfectly fine. You can use any Windows machine, as long as it has internet access. For those who don't know what LimaCharlie is, its a SecOps Cloud platform that offers a unified platform where you can build customized solutions effortlessly with open APIs centralized telemetry with automated detection and response --- LimaCharlie is pretty powerful. If you know how to use it, and lucky for you, this project allows you to gain some hands-on experience. Let's get started.
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  I'll be using Vultur to spin up my virtual machine. Again, you don't need to use Vultur, but if you want to, I have a link down below for $100 credit that you can use after signing up.
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  All you really need is a computer with internet connectivity. This could be an old computer sitting in the corner or even your virtual machine, but if you're using Vultur like I am, all you need to do is click on "Deploy +". Then "Deploy New Server"
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  From here, I'll select "Cloud Compute - Shared CPU. For the location I'll select "San Francisco". Let's scroll down here for the operating system, I'm going to select "Windows standard", the latest version.
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  I'll pick the $24 plan. I'll disable "Auto Backups". For firewall group, you might not need to have a firewall group if you're following along. So let me just leave this as is for now
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
For the server hostname, let's just say "MyDFIR-SOAR". When finished, let's go ahead and deploy that. It will take about 15 minutes to deploy, so what that means is that I can go and grab a drink or something to eat and then I'll come back.
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
All right, so after a couple minutes it's still installing. So what we can do is just start setting up LimaCharlie. To get started, we can head over to LimaCharlie.io and click on login. You can sign up with Google, email, GitHub, or Microsoft. I'll go ahead and select sign up with email.
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
You'll see verify your email. So do make sure that you use a valid email. Once you validated your email, just go ahead and refresh it. Before you start today, please take a moment to answer these questions so we may best cater to your experience. So I would say go ahead and fill that out.
  
  Then you get presented with this screen an overview of LimaCharlie. So it has your sensors, organizations, outputs, and add-ons. It's a nice little welcome guide to help you become more familiar with LimaCharlie's terminology. So let's begin creating our organization by clicking on this big blue button here.
  
  Enter in an organization name. I'll say "mydfir-SOAREDR". For the data residency region, I'll select "San Francisco". Now we have a choice for templates. Let me put a dash here, just to make it look nicer.
  
  Then select "Create Organization". So now this is being created and it says it'll take up to 30 seconds. Let's head back over to our Vultur... and it's running. Awesome.
  
  So let's go in here and it says, "Please note your server may still be finishing installing and booting up during the first few minutes of activation." What I can do is head over to "View Console", and we can see where it's at.
  
  So we do see a cntrl+alt+delete. Now that and we have our password here so. Just for your information, if you want to copy and paste, all you need to do is click on this clipboard and then paste in whatever you want.
  
  Now we get our server manager screen. Let me just RP in it, so before I do that, I'm going to set up my firewall. To set up your firewall, I'll select "Network", go into firewall.
  
  Now I already have a firewall, but let's just say I didn't have one. All I need to do is click on "Add Firewall Group" and I'll just say "SOAR-EDR Firewall". Right now it's accepting SSH from anywhere, and that's we do not want. So instead we will select MSRP, which is "Microsoft Remote Desktop protocol" on Port '3389'.
  
  
  Where do you want it to come from? Currently, it's set to anywhere. I'll select "My IP" and then make sure you select the plus button to add the firewall rule.
  
  Once that's done, head back over to your virtual machine and then we can add in our firewall. To do that, we can select "Settings" and on the left, you want to select "Firewall".
  
  Right now, we see no firewall. So select the drop-down and select "SOAR-EDR Firewall". Click on "Update Firewall Group", and we're good to go. It says it may take up to 120 seconds for these changes to apply, but you know what, let's just try anyway. Awesome, we're in. 
  
  What I can do now is actually just start logging into LimaCharlie from the server itself, but now that I think about it, this might not be a good idea because of the specs that I have on this server. It's currently running 1 CPU and 2 gigs of RAM so that's probably going to take a long time to load up some stuff. So instead, I'll just use my host for the time being to navigate around LimaCharlie, but then when we need to install the agent on our server that is when we go back over to the server.
  
  
  So over to LimaCharlie, we do see sensors, and we do have "Atomic red team", and "reliable tasking" automatically created for us. What we can do is head over to installation keys, and from here, we'll select "Create Installation Key". So the description here is going to say, "MyDFIR-SOAR-EDR-Project".
  
  If you want, you can start inputting some tags, but I'll just leave it as blank. Click on "Create" and now we have an installation key here. So what I'm actually going to do is remove the other installation keys.
  
  So I'll remove the "Atomic red team" one. I'll also remove the "Yara", "the demo sensor", and the "reliable tasking". That way it's just a lot cleaner. Over to my sensors list, now before I begin and install LimaCharlie on our Windows machine, let's quickly go over the web interface for Lima Charlie.
  
  On the left, we have our "Sensors", and under sensors there's a bunch of options that you can select. "Sensors list", "Event Collection", "Payloads", "Sensor Call", "Deployed Versions", "Installation Keys", and "Artifact collection".
  
  The two main important options, in my opinion, is going to be "Installation Keys" and "Sensors List". "Installation Keys" is what is going to be used to enroll your machine over to LimaCharlie. "Sensors list" will then contain your machine, once it has been successfully enrolled and we'll take a look at this shortly. 
  
  Then on the left, we have "Query Console", "Artifacts", "Dashboard", "Detections", "Automation", "Extensions", "Outputs", "Organization Settings", "Access Management", "Billing", and "Platform Logs". 
  
  There are a lot of different options here, but the main thing that we're going to be playing around with in this project is going to be the "Detections" and the "Automations". So the DNR rules, are known as, "Detection and Response Rules" and we'll be creating our own custom rule... and that is in part three.
  
  Now outside of this project, there are also some extensions that you can use with LimaCharlie. So you can have "Atomic red team", "Sigma", and other rules that you can use to help you detect and find evil. We will also be using "Outputs" for this project.
  
  "Outputs", essentially, allows you to integrate data from LimaCharlie into other Cloud tools. So for example, in our case we're going to be using Tines. So this is where we can start to configure LimaCharlie to push out detections over to Tines, but again, we'll see this in part three.
  
  For now, let's head back over to "Sensors" and head over to "Installation Keys". From here, what we need to do is scroll down, and under "Center Downloads", we are currently interested in the EDR section, and the server is running Windows 64 bit. 
  
  So what I'll do is copy the link address over to our server. I'll go ahead and paste that in. While that downloads, I'll also scroll up in LimaCharlie and copy the "Sensor Key". The reason why is because if we take a look at our installation here, we do see "Windows.exe" and we do see lcore sensor dasi your installation key. So that is why we need to copy the sensor key as that is going to be our installation key on our server our lima charlie is completed so what I'll do is open up Powershell and it should already run as administrator but I'll just right click it and click on run as administrator anyways and we want to navigate over to our downloads directory so I'll type in CD downloads type in dir and we do see our lima charlie executable so what I'll do is type in hcp and hit tab because I'm lazy to write out the entire thing so once that's good type in dash i and then we'll paste in the installation key hit enter and look at that within a couple seconds success agent installed successfully now you might see an error that says service installed don't worry about it what we can do is head over to services and we can just hit L for Lima and we do see lima charlie I'll go ahead and minimize my remote desktop and over to lima charlie let's head over to our sensors list and look at that we have our computer name of my d-or dedr clicking on the sensor we do see information about our server so we have our host name network access currently it is set to allow we also have the platform the enrollment date internal IP address and the external IP and we also see the sensor ID as well so that's good to know on the left hand side we have analytics artifacts there's Auto runs as well so this is a list of all the auto runs on the operating system if you're familiar with persistence this is essentially where you'll look to identify persistence going over to console this is where you can run remote commands from Lima Charli for example let's see if there's anything for Network oh net stat perfect so let's run net stat and there you go we have our net stat output it also includes the process ID as well so if you identified a suspicious process you can take a look at the net stat output to see if it's establishing any outbound connections towards a suspicious external IP address clicking on detections so there's no detections found there's drivers any suspicious drivers that are being used can be found here as well looking at event collection so these are all of the events being collected from the server and then we have file system file system is an awesome option to have because let's just say we know that Mau exists under the program data directory so we'll click on that and let's just say it exists under desktop so we'll click on desktop and I know we do see desktop.ini let's just pretend that this is a malicious executable and if we scroll to the right we have the created modified and access timestamp and what we can do is we can just inspect the file hash very quickly we get our hash we can even search it on virus total if we wanted to what we can do is also download the file and then perform basic static and dynamic malware analysis or if you have the skills you can do Advanced malware analysis as well we can copy the file path move it or even delete the file having access to the file system remotely is just an amazing capability to have as a Defender and then we have integrity monitoring AKA fim which is file Integrity monitoring this will show you if there's any changes to a particular file we have a live feed so all of the events that are being generated live going over to network this is going to take a while but essentially it's going to show you your net stat stuff which we already saw through the commands going over to packages this is going to contain information related to your operating system packages over to processes we get a listing of all of the active processes that are currently on our server and what we can do as well is click on it and we can kill the process view the modules suspend it resume download the memory strings which is amazing and view Memory map what I like about this is that you can also see the command line imagine this someone calls you up and says hey my computer is running pretty slow and my mouse is moving by itself now you might be like okay we might have something interesting here so you load up lima charlie head over to processes and what do you see you see Excel spawning a Powershell script running with encoded commands so that is likely something very strange then you can pivot over to network to see if there's any network connections that were initiated by that Powershell encoded process what you can also do is Pivot over to Auto runs was there any persistence created by that Powershell yes or no and then you can also check your services as well was there any service that was recently installed is there any executable that is running in the temp directory public users directory or even recycle bin there's just a lot of information that you can use to Pivot within lima charlie and there's even a timeline so timeline analysis is incredibly important so let's just say the user calls you up at 8:40 03 and says hey something strange is on my computer well take a look at the timeline and you can surrounding events very quickly you don't even need to run any queries you can just go over to the timeline and just start looking from plus - 5 minutes and hopefully you can find something interesting if you do find something interesting just click on it and you get all of your details on the right and lastly we have our users so we see the users that exist on our server now going back over to that Powershell example maybe that Powershell created a new user afterwards who knows but you can see that user here and you can also see when they logged on as well we now have a machine with lima charlie installed and confirmed it is generating events in part three we will start to generate events for the password recovery tool and begin creating our detection and response rule that is it for the video and I hope that this has been helpful for you so far if you have stumbled across any errors along the way do plan on researching it and if you can find the answer leave it in the comment section down below remember to stay curious and do things differently
