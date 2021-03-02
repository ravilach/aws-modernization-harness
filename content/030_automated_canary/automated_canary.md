# Automated Canary Analysis Configuration

With all of the CD Abstraction pieces out of the way, now it is time to define the Workflow which will power the canary analysis. 

Create a new Workflow for the Sample App. 

Setup -> AWS Canary Lab -> Workflows + Add Workflow

Name: Sample App Canary
Workflow Type: Canary Deployment
Environment: The EKS Cluster


Once you hit Submit, can add a canary phase under “Deployment Phases” with + Add Phase



In the Phase definition, select the Sample App Service and your Infrastructure Definition.


Once you hit Submit, your Canary Phase will be empty. This step will be filling out the Prometheus Details.



In the Verify section, click + Add Step. Search for “prom” as a function to add. 



Select Prometheus and then click next. This is where the Prometheus queries will be entered. 

Prometheus Server: EKS Prometheus
Metric Name: normal_call
Metric Type: Throughput
Group Name: custom
Query: io_harness_custom_metric_normal_call{kubernetes_pod_name="$hostName"}



Add another metric for Errors with + Add
Metric Name: error_call
Metric Type: Error
Group Name: custom
Query: io_harness_custom_metric_error_call{kubernetes_pod_name="$hostName"}



Then set the Analysis Time to 5 mins and Algorithm to Sensitive. 



When completed it should look like this. 



Click Submit and you now have Prometheus added to analyze the canary. 



As a good practice, can automate the removal of the resources if the canary should fail. On the right hand side under Rollback Steps, in the Deploy Phase, + Add Step.


Search for Del for the Delete Step.



Select Delete.
Resources: ${k8s.canaryWorkload}


Hit Submit and the final canary should look like this. 


Now you are ready to deploy your application.
