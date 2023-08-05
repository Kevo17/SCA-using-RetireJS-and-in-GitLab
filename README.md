<h1>SCA using RetireJS and in GitLab</h1>


<h2>Description</h2>
This hands-on lab focuses on utilizing RetireJS, a popular Software Component Analysis (SCA) tool, within the GitLab platform. RetireJS specifically targets JavaScript libraries and helps identify known vulnerabilities in web applications' front-end dependencies.

During this lab, participants will gain practical experience in integrating RetireJS into the GitLab pipeline to enhance the security of their JavaScript-based projects.
<br />


<h2>Languages and Utilities Used</h2>

- <b>RetireJS</b>
- <b>JSON</b>
- <b>GitLab</b>

<h2>Environments Used </h2>

- <b>Windows 11</b>
- <b>Linux</b> 

<h2>Program walk-through:</h2>

First, We need to download the source code of the project from our git repository: <br/>
```
git clone https://gitlab.practical-devsecops.training/pdso/django.nv webapp
``` 
<p align="center">
<img src="https://i.imgur.com/M5s7Gll.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

Lets cd into the application code so we can scan the app: <br/>
```
cd webapp
``` 
<p align="center">
<img src="https://i.imgur.com/oNW8Eta.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

First, we need to install Node JS and NPM: <br/>
```
curl -sL https://deb.nodesource.com/setup_16.x | bash -
```
```
apt install nodejs -y
```
 
<p align="center">
<img src="https://i.imgur.com/VDZKjNi.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
<img src="https://i.imgur.com/vsyZl7k.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

Then we can install the RetireJS tool on the system to find the insecure Javascript libraries: <br/>
```
npm install -g retire@3.2.3
```
 
<p align="center">
<img src="https://i.imgur.com/gKsCZJ1.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

Let’s explore what options retirejs gives us: <br/>
```
retire --help
``` 
<p align="center">
<img src="https://i.imgur.com/ywTqgxA.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

Front end dependencies managed via npm are stored in the package.json file. Let’s explore the contents of the package.json file: <br/>
```
cat package.json
``` 
<p align="center">
<img src="https://i.imgur.com/nnWcyWE.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

You will notice we need to install the npm packages available in the package.json file using the npm install command before running the scan: <br/>
```
npm install
```
<p align="center">
<img src="https://i.imgur.com/4pcKaNa.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

Let’s find if we are using any vulnerable libraries: <br/>
```
retire --outputformat json --outputpath retire_output.json
``` 
<p align="center">
<img src="https://i.imgur.com/iT6LNpJ.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

Once done, you can use the cat command to see the RetireJS output with the help of jq command: <br/>
```
- cat retire_output.json | jq
``` 
<p align="center">
<img src="https://i.imgur.com/zosDHei.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

Configure retirejs such that it only throws non zero exit code when high severity issues are present in the results: <br/>
```
retire --severity high --outputformat json --outputpath retire_output.json
``` 
<p align="center">
<img src="https://i.imgur.com/TmXfFou.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

Mark all high severity issues as False Positive and save the output in JSON format at location /webapp/retire_output.json: <br/>
```
cat >/webapp/.retireignore.json<<EOF
[
    {
        "component": "jquery",
        "justification" : "False Positive"
    },    
    {
        "component": "qs",
        "version": "0.6.6",
        "justification" : "False Positive"
    },
    {
        "component": "tough-cookie",
        "version": "2.2.2",
        "justification" : "False Positive"
    },
    {
        "component": "handlebars",
        "version": "4.0.5",
        "justification" : "False Positive"
    },
    {
        "component": "growl",
        "version": "1.9.2",
        "justification" : "False Positive"
    },
    {
        "component": "adm-zip",
        "version": "0.4.4",
        "justification" : "False Positive"
    },
    {
        "component":"dojo",
        "version": "1.4.2",
        "justification" : "False Positive"
    },
    {
        "component":"underscore.js",
        "version": "1.8.3",
        "justification" : "False Positive"
    }
]
EOF
```
<br/>
 


<br />
<br />

Run retirejs a second time: <br/>
```
retire --severity high --outputformat json --outputpath retire_output.json
``` 
<p align="center">
<img src="https://i.imgur.com/3PmgSwm.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
