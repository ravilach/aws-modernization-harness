# Deploy Your Canary

It is now time to deploy your application and enjoy safety of an automated canary analysis. 

The sample application has a stable and unstable version to deploy for demonstration. To toggle between the two, will take advantage of Harness Workflow Variables.

Back in the Application, add two Workflow Variables. 

Setup -> AWS Canary Lab -> Workflows -> Sample App Canary

On the bottom right, click on the “Workflow Variables” pencil icon. 



Add a pair of variables. 
First: verify_canary / Text / yes
Second: metric_verification / Text / Prometheus



Click Save, and your Workflow should look like this.



The last step is to define what happens after a successful Canay, which would be to promote. Can click on the + Add Phase after the Canary in Deployment Phase. 

Service: Sample App
Infrastructure Definition: My EKS Cluster



Once you hit Submit, your Workflow should look like this and you are ready to deploy.



Now you are ready to deploy. Click on Deploy on the top right. 
Verify_canary: no
Artifact Build: Tag # stable 



Click Submit and stable/baseline is headed out the door. 



Now for the fun, you can deploy another version of the application. This can represent a change going into production. 

Continuous Deployment -> Deployments -> Start New Deployment 

Application: AWS Canary Lab
Workflow: Sample App Canary
Verify_canary: yes
Artifact Build: #unstable 


Hit Submit and this part will take a few minutes to analyze depending how quickly you redeployed. The learning interval is 5 minutes and the baseline needs to also learn.  

As expected, the canary analysis is expected to fail because of the application being unstable. Just like that, you now have an automated canary deployment. 



The rollback was completed without any admin or user intervention. 



Happy Deploying!

-The Harness Team
