<h1>SOAR EDR Project | Part 2</h1>

<p align="center">
<img src="https://snipboard.io/PImt2S.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<h2>Description</h2>
<br />
<p align="center">
Welcome to part two of five for the series on the SOAR EDR project. If you haven't seen part one where we go over how to build out a Playbook workflow, for this particular project, I highly recommend you go and read that first, as we'll be referencing that workflow throughout the series. 
<br />
<br />
<img src="https://snipboard.io/zVTRO9.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  Today, our objective is to install and set up LimaCharlie and making sure that the endpoint is connected and is generating events. By the end of this part, you should have one Windows machine reporting back to LimaCharlie and then we can begin creating our first detection and response rule. I'll be using Vultur, as the cloud provider, and we'll be spinning up a Windows server machine and if you want to follow along, I'll leave a link down below for those who want free $100 credit.
<br />
<br />
<img src="https://snipboard.io/gD23pn.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  If you don't want to use the cloud, that is perfectly fine. You can use any Windows machine, as long as it has internet access. For those who don't know what LimaCharlie is, it's a SecOps Cloud platform that offers a unified platform where you can build customized solutions effortlessly with open APIs and centralized telemetry with automated detection and response --- LimaCharlie is pretty powerful. If you know how to use it, and lucky for you, this project allows you to gain some hands-on experience. Let's get started.
<br />
<br />
<img src="https://snipboard.io/oeNFfu.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/p5Ajrg.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  I'll be using Vultur to spin up my virtual machine. Again, you don't need to use Vultur, but if you want to, I have a link down below for $100 credit that you can use after signing up.
<br />
<br />
<img src="https://snipboard.io/k2Ymvo.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  All you really need is a computer with internet connectivity. This could be an old computer sitting in the corner or even your virtual machine, but if you're using Vultur like I am, all you need to do is click on "Deploy +". Then "Deploy New Server".
<br />
<br />
<img src="https://snipboard.io/7sNMqo.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  From here, I'll select "Cloud Compute - Shared CPU. For the location. I'll select "Los Angeles". Let's scroll down here for the operating system, I'm going to select "Windows standard", the latest version.
<br />
<br />
<img src="https://snipboard.io/xfs980.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/7hnZxT.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/45MQpv.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  I'll pick the $24 plan. I'll disable "Auto Backups". For firewall group, you might not need to have a firewall group if you're following along. So let me just leave this as is for now.
<br />
<br />
<img src="https://snipboard.io/YqunTz.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/a2Hio8.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
For the server hostname, let's just say "MyDFIR-SOAR". When finished, let's go ahead and deploy that. It will take about 15 minutes to deploy, so what that means is that I can go and grab a drink or something to eat and then I'll come back.
<br />
<br />
<img src="https://snipboard.io/7UShxM.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/McVWOK.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
All right, so after a couple of minutes, it's still installing. So what we can do is just start setting up LimaCharlie. To get started, we can head over to LimaCharlie.io and click on login. You can sign up with Google, email, GitHub, or Microsoft. I'll go ahead and select sign up with email.
<br />
<br />
<img src="https://snipboard.io/BKoTDz.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/FpNzCX.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
You'll see verify your email. So make sure that you use a valid email. Once you validated your email, just go ahead and refresh it. Before you start today, please take a moment to answer these questions so we may best cater to your experience. So I would say go ahead and fill that out.
<br />
<br />
<img src="https://snipboard.io/Jrthce.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/VrPika.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  Then you get presented with this screen an overview of LimaCharlie. So it has your sensors, organizations, outputs, and add-ons. It's a nice little welcome guide to help you become more familiar with LimaCharlie's terminology. So let's begin creating our organization by clicking on this big blue button here.
<br />
<br />
<img src="https://snipboard.io/I6LhT8.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  Enter an organization name. I'll say "mydfir-SOAREDR". For the data residency region, I'll select "San Francisco". Now we have a choice for templates.
<br />
<br />
<img src="https://snipboard.io/kVlI6o.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  Then select "Create Organization". So now this is being created and it says it'll take up to 30 seconds. Let's head back over to our Vultur... and it's running. Awesome.
<br />
<br />
<img src="https://snipboard.io/3jBr1n.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  So let's go in here and it says, "Please note your server may still be finishing installing and booting up during the first few minutes of activation." What I can do is head over to "View Console", and we can see where it's at.
<br />
<br />
<img src="https://snipboard.io/OYKm81.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  So we do see a cntrl+alt+delete. Now that and we have our password here so. Just for your information, if you want to copy and paste, all you need to do is click on this clipboard and then paste in whatever you want.
<br />
<br />
<img src="https://snipboard.io/KnhApj.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/LnKRA0.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
<img src="https://snipboard.io/KnhApj.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
  Now we get our server manager screen. Let me just RP in it, so before I do that, I'm going to set up my firewall. To set up your firewall, I'll select "Network", go into firewall.
<br />
<br />
<img src="https://snipboard.io/CE7X5w.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/e1YVdI.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  Now I already have a firewall, but let's just say I didn't have one. All I need to do is click on "Add Firewall Group" and I'll just say "SOAR-EDR Firewall". Right now it's accepting SSH from anywhere, and that's we do not want. So instead we will select MSRP, which is "Microsoft Remote Desktop protocol" on Port '3389'.
<br />
<br />
<img src="https://snipboard.io/0OPtHl.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/4PCsV5.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/AUObW4.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  Where do you want it to come from? Currently, it's set to anywhere. I'll select "My IP" and then make sure you select the plus button to add the firewall rule.
<br />
<br />
<img src="https://snipboard.io/mJTFKS.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/nomKGs.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  Once that's done, head back over to your virtual machine and then we can add in our firewall. To do that, we can select "Settings" and on the left, you want to select "Firewall".
<br />
<br />
<img src="https://snipboard.io/x9jMpr.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/n8X5lw.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  Right now, we see no firewall. So select the drop-down and select "SOAR-EDR Firewall". Click on "Update Firewall Group", and we're good to go. It says it may take up to 120 seconds for these changes to apply, but you know what, let's just try anyway. Awesome, we're in. 
<br />
<br />
<img src="https://snipboard.io/Afivp8.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/nq45vo.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/z08LMd.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  What I can do now is just start logging into LimaCharlie from the server itself, but now that I think about it, this might not be a good idea because of the specs that I have on this server. It's currently running 1 CPU and 2 gigs of RAM so that's probably going to take a long time to load up some stuff. So instead, I'll just use my host for the time being to navigate around LimaCharlie, but then when we need to install the agent on our server that is when we go back over to the server.
<br />
<br />
<img src="https://snipboard.io/rEvyNO.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  So over to LimaCharlie, we see "Sensors, we have "Atomic red team" and "reliable tasking" automatically created for us. What we can do is head over to installation keys, and from here, we'll select "Create Installation Key". So the description here is going to say, "MyDFIR-SOAR-EDR-Project".
<br />
<br />
<img src="https://snipboard.io/S6hWyj.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/sVBb1w.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/VL54hy.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  If you want, you can start inputting some tags, but I'll just leave it as blank. Click on "Create" and now we have an installation key here. So what I'm actually going to do is remove the other installation keys.
<br />
<br />
<img src="https://snipboard.io/xTPGZi.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/QvNhuz.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  So I'll remove the "Atomic red team" one. I'll also remove the "Yara", "the demo sensor", and the "reliable tasking". That way it's just a lot cleaner. Over to my sensors list, now before I begin and install LimaCharlie on our Windows machine, let's quickly go over the web interface for Lima Charlie.
<br />
<br />
<img src="https://snipboard.io/iQyXjO.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/QWsSqj.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  On the left, we have our "Sensors", and under sensors there's a bunch of options that you can select. "Sensors list", "Event Collection", "Payloads", "Sensor Call", "Deployed Versions", "Installation Keys", and "Artifact collection".
<br />
<br />
<img src="https://snipboard.io/QWsSqj.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  The two main important options, in my opinion, is going to be "Installation Keys" and "Sensors List". "Installation Keys" is what is going to be used to enroll your machine over to LimaCharlie. "Sensors list" will then contain your machine, once it has been successfully enrolled and we'll take a look at this shortly. 
<br />
<br />
<img src="https://snipboard.io/QWsSqj.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  Then on the left, we have "Query Console", "Artifacts", "Dashboard", "Detections", "Automation", "Extensions", "Outputs", "Organization Settings", "Access Management", "Billing", and "Platform Logs". 
<br />
<br />
<img src="https://snipboard.io/mPO4VZ.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  There are a lot of different options here, but the main thing that we're going to be playing around with in this project is going to be the "Detections" and the "Automations". So the DNR rules, are known as, "Detection and Response Rules" and we'll be creating our own custom rule... and that is in part three.
<br />
<br />
<img src="https://snipboard.io/zf8e9S.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  Now outside of this project, there are also some extensions that you can use with LimaCharlie. So you can have "Atomic red team", "Sigma", and other rules that you can use to help you detect and find evil. We will also be using "Outputs" for this project.
<br />
<br />
<img src="https://snipboard.io/c3ZBUP.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  "Outputs", essentially, allows you to integrate data from LimaCharlie into other Cloud tools. So for example, in our case we're going to be using Tines. So this is where we can start to configure LimaCharlie to push out detections over to Tines, but again, we'll see this in part three.
<br />
<br />
<img src="https://snipboard.io/RGuy0L.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  For now, let's head back over to "Sensors" and head over to "Installation Keys". From here, what we need to do is scroll down, and under "Center Downloads", we are currently interested in the EDR section, and the server is running Windows 64 bit. 
<br />
<br />
<img src="https://snipboard.io/dSW8Bn.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  So what I'll do is copy the link address over to our server. I'll go ahead and paste that in. While in downloads, I'll also scroll up in LimaCharlie and copy the "Sensor Key". The reason why is that if we take a look at our installation here, we do see "Windows.exe" and we do see "lc_ sensor.exe -i YOUR_INSTALLATION_KEY". So that is why we need to copy the sensor key, as that is going to be our installation key.
<br />
<br />
<img src="https://snipboard.io/FknhTu.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/Ynchek.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/2HulIT.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  Back to our server, LimaCharlie is completed. So what I'll do is open up Powershell with administrator privileges, and we want to navigate over to our "Downloads" directory. So I'll type in "cd Downloads". Then type in "dir".
<br />
<br />
<img src="https://snipboard.io/AMpU4t.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/MaCULi.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  We do see our LimaCharlie executable, so what I'll do is type in ".\hcp_win_x64_release_4.29.0.exe -i", with the installation key. Then hit "Enter".
<br />
<br />
<img src="https://snipboard.io/qIKfY2.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  Look at that, within a couple of seconds our agent installed it successfully. Now you might see an error that says "service installed", don't worry about it. What we can do is head over to "Services" and we can just hit "L" for Lima and we do see LimaCharlie.
<br />
<br />
<img src="https://snipboard.io/m7aTAW.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/ESnhPj.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  I'll go ahead and minimize my remote desktop, and over to LimaCharlie, let's head over to our sensors list. Now look at that, we have our computer name of "mydfir-soar-edr". Clicking on the sensor, we do see information about our server. 
<br />
<br />
<img src="https://snipboard.io/xpil5M.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/0CrNjT.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  So we have our "Hostname", "Network Access", and currently it is set to "Allowed". We also have the "Platform", the "Enrollment Date", "Internal IP", "Address", the "External IP", and we also see the "Sensor ID" as well. So that's good to know.
<br />
<br />
<img src="https://snipboard.io/0CrNjT.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  On the left-hand side, we have "Analytics", "Artifacts", and "Autoruns" as well. So this is a list of all the autoruns on the operating system. If you're familiar with "Persistence", this is essentially where you'll look to identify persistence. 
<br />
<br />
<img src="https://snipboard.io/b2CSV5.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/FvEQoy.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  Going over to the console. This is where you can run remote commands from LimaCharlie. For example, let's see if there's anything for "network". "netstat" pops up, perfect. So let's run "netstat".
<br />
<br />
<img src="https://snipboard.io/rej2s6.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  There you go we have our netstat output. It also includes the "process ID" as well. So if you identified a suspicious process, you can take a look at the netstat output to see if it's establishing any outbound connections towards a suspicious external IP address.
<br />
<br />
<img src="https://snipboard.io/N5fK6W.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  Clicking on "Detections", there are no detections found. Clicking on "Drivers". Any suspicious drivers that are being used can be found here as well. Looking at "Event Collection", these are all of the events being collected from the server.
<br />
<br />
<img src="https://snipboard.io/7QHPgN.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/IGBSil.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/qYG4FS.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Finally, we have "File System". A "File System" is an awesome option to have because let's just say we know that Malware exists under "ProgramData" directory. So we'll click on that and let's just say it exists under "Desktop". So we'll click on "Desktop".
<br />
<br />
<img src="https://snipboard.io/siWg2N.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/4uYAjk.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/L5Huh2.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
I know we do see "desktop.ini", but let's just pretend that this is a malicious executable. If we scroll to the right we have the "Created", "Modified", and "Accessed" timestamps.
<br />
<br />
<img src="https://snipboard.io/lXEaFg.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/wHFfhn.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
What we can do is "Inspect the File Hash". We can even search for it on VirusTotal if we want to. What we can do is also download the file and then perform basic static and dynamic malware analysis, or if you have the skills you can do advanced malware analysis as well. 
<br />
<br />
<img src="https://snipboard.io/5G6Bki.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/GZN1v6.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
We can copy the file path, move it, or even delete the file. Having access to the file system remotely is just an amazing capability to have as a defender. 
<br />
<br />
<img src="https://snipboard.io/1KiOwX.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Then we have file integrity monitoring, AKA FIM. This will show you if there are any changes to a particular file. We have a live feed, so all of the events that are being generated are live.
<br />
<br />
<img src="https://snipboard.io/8B7ZC3.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/RHmzAt.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Going over to "Network". This is going to take a while, but essentially, it's going to show you your "netstat" info, which we already saw through the commands.
<br />
<br />
<img src="https://snipboard.io/7e4BZJ.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Going over to "Packages", this is going to contain information related to your operating system packages. Over to "Processes", we get a listing of all of the active processes that are currently on our server.
<br />
<br />
<img src="https://snipboard.io/xc0Nq4.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/TMot9O.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
What we can do is click on it and we can kill the process, view the modules, suspend it, resume, download the memory strings, and view the memory map.
<br />
<br />
<img src="https://snipboard.io/vJ7pKs.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
What I like about this is that you can also see the command line. Imagine this, someone calls you up and says, "Hey man, my computer is running pretty slow and my mouse is moving by itself now." You might be like, "Okay, we might have something interesting here". So you load up LimaCharlie.
<br />
<br />
<img src="https://snipboard.io/TfBu4h.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Head over to "Processes", and what do you see? You see Excel spawning a Powershell script, running with encoded commands. So that is likely something strange. Then you can pivot over to "Network", to see if there's any network connections that were initiated by that Powershell encoded process.
<br />
<br />
<img src="https://snipboard.io/TfBu4h.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/eKucXL.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
What you can also do is, pivot over to "Autoruns". "Was there any persistence created by that Powershell? Yes or no", and then you can also check your "Services" as well. Was there any service that was recently installed? Is there any executable that is running in the "temp directory", "public users directory", or even "recycle bin". There's just a lot of information that you can use to pivot within LimaCharlie.
<br />
<br />
<img src="https://snipboard.io/OopXK6.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/LVC5Tg.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
There's also a timeline, which is incredibly important in analysis. So let's just say the user calls you up at '8:40:03', and says, "Hey something strange is on my computer." Well, take a look at the timeline and you can see surrounding events very quickly.
<br />
<br />
<img src="https://snipboard.io/uDtn02.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
You don't even need to run any queries. You can just go over to the timeline and start looking within the timeframe of when the situation occurred. Hopefully, you can find something interesting. If you do find something interesting, just click on it and you get all of your details on the right.
<br />
<br />
<img src="https://snipboard.io/HZzgTl.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Lastly, we have our "Users". So we see the users that exist on our server. Now going back over to that Powershell example, maybe Powershell created a new user afterward, who knows? But you can see the user here, and you can also see when they logged on as well.
<br />
<br />
<img src="https://snipboard.io/A5WNM2.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/Ql5gSG.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
We now have a machine with LimaCharlie installed and confirmed it is generating events. In part three, we will start to generate events for the password recovery tool, and begin creating our detection and response rule.
<br />
<br />
That is it for this part and I hope that this has been helpful for you so far. If you have stumbled across any errors along the way do plan on researching it. Remember to stay curious and do things differently.
