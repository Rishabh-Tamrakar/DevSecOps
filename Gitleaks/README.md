# Configuration of Gitleaks
<img src="https://github.com/Rishabh-Tamrakar/DevSecOps/blob/master/images/gitleaks5.png" align="right" height="100" width="120">
The developer Zachary Rice published on GitHub utility called Gitleaks, which audits repos and finds confidential data, put there by mistake.


Gitleaks offers faster audits, the ability to audit organizations and users, and the ability to whitelist branches, commits, files, and regexes.\
Gitleaks clones a single repo or a set of repos then runs a regex check for keys or whatever you specify against all commits in default HEAD and/or all references.


The program checks git-repositories for the presence of RSA- and SSH-keys, as well as unique signs of access to Facebook or Amazon’s web services.

**Functions –**
* Scanning of all subdirectories, in addition to origin / HEAD.
* Concurrency control.
* Structured testing.
* Formation of white lists.
* Use of regular expressions.

|  `repo scan` |
|---|
| <p align="left"><img src="https://github.com/Rishabh-Tamrakar/DevSecOps/blob/master/Gitleaks/Images/repo-scan.gif"></p>  |

| `pre commit scan` |
|---|
|  <p align="left"><img src="https://github.com/Rishabh-Tamrakar/DevSecOps/blob/master/Gitleaks/Images/pre-commit-1.gif"></p> |


**Configuration and working Of  Gitleaks –**
Gitleaks can be integrated through Jenkins in two ways First by manually installing gitleaks on the instance on which Jenkins is running Second using pre-configured Docker image of gitleaks.\
Here we are manual installing for scanning and report generation.



• Run these commands to install gitleakes on machine with sudo user

```
wget https://github.com/zricethezav/gitleaks/releases/download/v3.2.2/gitleaks-linux-amd64
mv gitleaks-linux-amd64 gitleaks
chmod +x gitleaks
mv gitleaks /usr/local/bin/
gitleaks --version
```
•	Under  Build Section, we will add Execute Shell build step and then we will run the gitleaks command to scan our git repository and generate the report 
```
gitleaks -v --repo=GITHUB_REPOSITORY  --report=gitleaks_results.csv
```
Note :-  Reports can be generated either in CSV or in Json format.

<img src="https://github.com/Rishabh-Tamrakar/DevSecOps/blob/master/Gitleaks/Images/1.jpg" align="middle" height="250" width="800">

•	After the scanning of our git repository we need to buid our project if the scanning is passed and suucessful. So we will trigger other project for build and deployment.\
In Post-Build Actions, We will choose Build other projects and provide the project name to be triggered if git scanning is stable.

<img src="https://github.com/Rishabh-Tamrakar/DevSecOps/blob/master/Gitleaks/Images/2.jpg" align="middle" height="200" width="600">


•	Click on Apply and Save and Click Build Now. Once build is completed, we can view Gitleaks Report in our workspace .

<img src="https://github.com/Rishabh-Tamrakar/DevSecOps/blob/master/Gitleaks/Images/3.jpg" align="middle" height="200" width="700">
