# Configuration of OWASP ZAP
<img src="https://github.com/Rishabh-Tamrakar/DevSecOps/blob/master/images/zap-1.png" align="right" height="200" width="200">
The OWASP Zed Attack Proxy (ZAP) is one of the world’s most popular free security tool It can help you automatically find security vulnerabilities in your web applications while you are developing and testing your applications.
<img src="https://github.com/Rishabh-Tamrakar/DevSecOps/blob/master/OWASP%20Zed%20Attack%20Proxy%20(ZAP)/Images/1.jpg" align="middle" height="200" width="600">

•	Developer makes a new commit to the code repository.\
•	Assuming that the Github repository is already integrated with Jenkins CI service using a Jenkins Webhook, this will trigger the Github Checkout Build process.\
•	Once the Github Checkout Build is completed, it will initiate the ZAP build process to automate the DAST scan against deployed Environment.\
•	ZAP scan results will then be archived in Jenkins and pushed to Orchestron (in this case) for result correlation.\
•	Application Security Engineer & Developer can automatically raise ticket in Jira using results from Orchestron.\


**Features that make ZAP a great tool for Application Security Testing –**

•	AJAX spidering – AJAX spidering is performed during a penetration test to discover requests on an AJAX-rich web application, which cannot be discovered with the regular Spidering tool.

•	Fuzzing – A fuzzer is a security tool used to inject a variety of payloads to force the application to go to an undesired state, possibly exposing a potential vulnerability. In ZAP, a tester can choose from a wide range of open source fuzzing payload lists through  sources such as  Dirbuster, FuzzDBand JbroFuzz that  are inbuilt within ZAP making it easier to unearth vulnerabilities than ever before.

•	ZAP Jenkins Plugin – As more and more companies move towards DevSecOps or pursue Agile security testing methods, integrating DAST tools into their CI/CD pipeline manager such as Jenkins is becoming a norm. Having a plugin is essential for such integrations. ZAP’s Jenkins Plugin enables integration of security testing within the CI/CD pipeline.

•	Highly scriptable – A lot of security testing can be automated with scripts, which reduces time spent on manual testing, while giving the tester more time to focus on other important tasks. ZAP is one of the most extensible/scriptable tools that support any scripting language that allows JSR 223 scripting. ZAP has the feature to run scripts that can be embedded within it to access internal data structures & functionalities through the ZAP Script Add-on. It supports multiple scripting languages which include JavaScript, Zest, Python, Groovy and Ruby.



ZAP creates a proxy server and makes your website traffic pass through that server.\
It comprises of auto scanners that help you intercept the vulnerabilities in your website.
<img src="https://github.com/Rishabh-Tamrakar/DevSecOps/blob/master/OWASP%20Zed%20Attack%20Proxy%20(ZAP)/Images/Picture1.jpg" align="middle" height="250" width="500">

**Configuration of ZAP in Jenkins through Docker**

We can integrate ZAP to run full-fledged scanning on our web application without even installing the heavy-weight tool. This can be done  by using the Docker image with Owasp Zed Attack Proxy(OWASP-ZAP) preinstalled.

•	Under  Build Section, we will add Execute Shell build step and then follow the steps described below –

	
At first we will pull the stable release of OWASP ZAP by using the command-
docker pull owasp/zap2docker-stable

NOTE: - Apart from stable release we also have 3 other releases available for OWASP ZAP for web application scanning –
-	Latest weekly release.
-	Live release. (built whenever the zaproxy project is changed)
-	Bare release. (a very small Docker image, contains only the necessary required dependencies to run ZAP, ideal for CI environments) which we can use as per our requirement.


•	Now since we need to generate scanning report for the web-application scanning. So we will create the reports folder and give Jenkins user permission to write to the folder.
 
•	Now it’s time to run the scanning on our web-application using the docker image we have pulled in previous step.
The ZAP Docker images include packaged scans which make it easy to run ZAP for specific purposes. Thus we can run ZAP container in three different modes for application scanning-

	Baseline Scan - a time limited spider which reports issues found passively.\
	API Scan - A full scan of an API defined using OpenAPI / Swagger or SOAP.\
	Full Scan - A full spider, optional ajax spider and active scan which reports issues found actively and passively.

We can use either of these as per requirement. \
Here in our case we are using ZAP-Baseline Scan and ZAP-Full Scan for the demonstration.\
Command used  for baseline scan - 
```
docker run -v $(pwd)/Report:/zap/wrk/:rw -t owasp/zap2docker-stable  zap-baseline.py  -t TARGET_URL  -g Baseline.conf  -r BaseLine-Scan-Report.html 

Command used  for full scan - 

docker run -v $(pwd)/Report:/zap/wrk/:rw -t owasp/zap2docker-stable zap-full-scan.py -t TARGET_URL  -g full.conf -r Full-Scan-Report.html 
```

Note:-\
•	Since we are generating reports and config files so we need to mount the directory where those files will be generated in and give the permissions.\
•	By default it reports all alerts as Warnings but we can specify a config file which can change any rules to FAIL or IGNORE

<img src="https://github.com/Rishabh-Tamrakar/DevSecOps/blob/master/OWASP%20Zed%20Attack%20Proxy%20(ZAP)/Images/2.jpg" align="middle" height="400" width="700">

•	After scanning is done and reports are generated, we need to publish the generated HTML reports.

So under Post-Build Actions we will choose publish HTML reports option  and\
Provide the HTML directory to be archived where the reports are stored and the\
Index pages of both the reports and report title.


<img src="https://github.com/Rishabh-Tamrakar/DevSecOps/blob/master/OWASP%20Zed%20Attack%20Proxy%20(ZAP)/Images/3.jpg" align="middle" height="300" width="470">

•	Click on Apply and Save and Click Build Now. Once build is completed, we can view the HTML Report on Job Dashboard and other archived files from workspace.


<img src="https://github.com/Rishabh-Tamrakar/DevSecOps/blob/master/OWASP%20Zed%20Attack%20Proxy%20(ZAP)/Images/4.jpg" align="middle" height="300" width="700">



<img src="https://github.com/Rishabh-Tamrakar/DevSecOps/blob/master/OWASP%20Zed%20Attack%20Proxy%20(ZAP)/Images/5.jpg" align="middle" height="300" width="700">
