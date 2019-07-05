Steps : To install SonarQube

Step1: Install java 8 jdk.
	Install Java SE: In order to run the web server, SonarQube requires Java SE.
	Download JavaSE from the oracle website http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html.
	Note: Select 'windows x64  187.41MB/MB   jdk_8u91-windows-x64.exe' and download.
	I am using version Java SE 8, for 64 bit OS download x64. Run the installation wizard and set up using the defaults.
	
	After instalation: Check java version and 32/64 bit ?.
	.Open cmd prompt and enter 'java -version'
	ex: java version "1.8.0_191"
		Java(TM) SE Runtime Environment (build 1.8.0_191-b12)
		Java HotSpot(TM) 64-Bit Server VM (build 25.191-b12, mixed mode)		
	
Step2:  SonarQube installation
	URL : https://docs.sonarqube.org/latest/setup/get-started-2-minutes/
	Download and unzip the folder. 
	
	ex: C:\path sonarqube\sonarqube-7.3\sonarqube-7.3\conf\sonar.properties  
	uncomment the username and password
		# User credentials.
		# Permissions to create tables, indices and triggers must be granted to JDBC user.
		# The schema must be created first.
		sonar.jdbc.username=SonarUser
		sonar.jdbc.password=SonarUser	

Step3: Analyzing with SonarQube Scanner
	Url : https://docs.sonarqube.org/display/SCAN/Analyzing+with+SonarQube+Scanner
	Download and Unzip the folder.

Step4: Create a config file 
	.Save as filename 'sonar-project.properties'
	.Add the config file under the project
	.Updated below code in 'sonar-project.properties' file.
		#must be unique in a given SonarQube instance
		sonar.projectKey=my:project                   //Your project key shoulb be unique
		# this is the name and version displayed in the SonarQube UI.
		sonar.projectName=My project
		sonar.projectVersion=1.0
		# Path is relative to the sonar-project.properties file. Replace "\" by "/" on Windows.
		# This property is optional if sonar.modules is set.
		sonar.sources=.
		# Encoding of the source code. Default is default system encoding
		sonar.sourceEncoding=UTF-8

Step5: Set path's in environment vaariable
	1.Open the environment vaariable
	2.Select path c:\ProgramData\Oracle\java\javapath; c:\ .................  under the system varialble
	3.Add below paths.
	ex:
		C:\sonarQube\sonarqube-7.3\sonarqube-7.3\bin\windows-x86-64
		C:\sonarQube\sonarqube-7.3\sonarqube-7.3\bin\windows-x86-64\StartSonar.bat
		C:\sonarQube\sonar-scanner-cli-3.2.0.1227-windows\sonar-scanner-3.2.0.1227-windows\bin	
	
Step6: Open command prompt and run StartSonar like C:\Users>StartSonar

Step7: Run project  
	path like E:\CommonCodeBase\Project>sonar-scanner -Dsonar.projectKey=myproject -Dsonar.sources=src
	. project root  E:\CommonCodeBase\Project
	. project root path + sonar-scanner -Dsonar.projectKey=myproject -Dsonar.sources=src  //'myproject/unique name' is your projectkey is added config file from Step4.
	
Note:
	1.Getting error for SCM
		To disabling the SCM sensor configuration we can avoid this error
		Please refer below url
		Url : https://stackoverflow.com/questions/36161448/tmatesoft-svn-core-svnauthenticationexception-svn-e170001
		
	2. StartSonar is getting error in command prompt, Please check the (java.exe) is running more than once in backgound? uisng 'Tasklist' cmd.
		> Tasklist 
		> taskkill /PID id /F           ex: id = 12345   (java.exe of PID)
		> StartSonar		
		
	3. Check the port is running backgound code analyzed
		cmd : netstat -ano
		check tcp: 0.0.0.0:90000
		taskkill /PID id /F       ex:id=12345
	
	4.Ignore the some libraries issues in sonarqube:
		adminstration-> General setting -> analysis scope -> Source File Exclusions (under Files)
		ex:	**/*jquery*
			**/*bootstrap*
			**/node_modules/**
			src/public/Externaljs/**/*   
			src/public/ExternalCSS/**/*
	
	5. Export to json/csv from sonarqube issues 
		url: https://stackoverflow.com/questions/42033059/exporting-sonarqube-reports-into-excel-based-on-major-minor-and-critical-cate
		
		add the projectKey
			ex: url: http://localhost:9000/api/issues/search?componentKeys=MagnifactMagniVizion
			OR
			ex: url: http://localhost:9000/api/issues/search?MagnifactMagniVizion=test_xxx_xx&statuses=OPEN,REOPENED&pageSize=500&pageIndex=1
			
		save the json report and convert json to csv form online