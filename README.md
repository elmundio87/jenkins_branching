#Automated Jenkins Branching

The purpose of this project is to reduce the amount of time taken to copy a set of jobs and configure them 
to build and deploy a new code branch. 

This is achieved via the use of XML job templates and the Jenkins CLI java utility, driven by an Ant script.

##Dependencies

+ Ant 1.9.0+ installed on the PATH
+ Java 1.7+ installed on the PATH

##Ant targets

    create-project			Create a skeleton for a project in the "projects" folder. Properties: project
	create-branch			Create a project branch based on a set of job templates. Properties: project+branch+branch+[branch.short]
	create-pipeline-view  	Create a pipeline view. Properties: project+branch+[branch.short]
	delete-branch    		Delete a project branch from Jenkins. Properties: project+branch+[branch.short]
	delete-pipeline-view  	Delete a pipeline view. Properties: project+branch+[branch.short]
	disable-branch   		Disable all jobs in a project branch. Properties: project+branch+[branch.short]
	enable-branch    		Enable all jobs in a project branch. Properties: project+branch+[branch.short]
	get-job          		Download a job from jenkins as an XML file. Properties: job
	update-branch    		Update an existing branch based on a set of job templates. Properties: project+branch+[branch.short]

##How to set up a project template

1. Create a folder in the "projects" directory called "TestProject"
2. For each job currently in Jenkins you want to template, run the following command;

        ant get-job -Djob=<job name>
	
3. The job definitions will now be in the root of the Jenkins-Branching project folder as XML files
4. Rename the xml files, removing project and branch references

    eg. "TestProject 5.1 - Build Package.xml" is renamed to "Build Package.xml"

5. Edit each xml file to replace branch name references with @BRANCH@ tokens
6. Move the XML files into the projects/TestProject folder

##Using the templates to create a set of jobs to build a branch

	ant create-branch -Dproject=<project name> -Dbranch=<branch name>

	eg. ant create-branch -Dproject=TestProject -Dbranch=5.2

##Create a pipeline view for a branch

	ant create-pipeline-view -Dproject=<project name> -Dbranch=<branch name>

	eg. ant create-pipeline-view -Dproject=TestProject -Dbranch=5.2

	This will create a pipeline view called TestProject 5.2 Pipeline

##Setting the first job for the generated pipeline

    Edit the file "pipeline_view" in the project, replacing "@PROJECT@[@BRANCH.SHORT@]-Build" with the name of the first job.

##Temporarily disabling a branch in Jenkins

	ant disable-branch -Dproject=<project name> -Dbranch=<branch name>

	eg. ant disable-branch -Dproject=TestProject -Dbranch=5.2

	All jobs defined in the TestProject folder with branch 5.2 will be temporarily disabled