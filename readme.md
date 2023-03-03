## Deployment File
I ensure that my deployment file container port is the same as the target port on my service.yml file
Specified the strategy for the rolling update which will allow application update and roll back to be possible if there is any need for it
I set up my resources field in the container where i specified how much cpu need to be utilized before autoscaling occurs. The austoscaling follows the instruction specified in the (horizontal pod autoscaling)hpa.yml to adjust the amount of replicas to be created when there is high traffic.
Due to the usage of an external database, the details of the database is specified under the container env. This details corresponds to the content of the configmap.yml file

## Service File
I ensured that the selector app is the same as the selector matchlabel on the deployment.yml file
specified my service to be loadbalancer as this will ensure that the traffic is adequately distributed between the replicas
targetport number is the same as container port number

## Horizontal Pod autoscaling file
This is the file responsible for autoscaling when the traffic goes above the specified cpu utilization of 70%. The range of the amount of replicas that can be generated u=is stated in the file unde the minreplica and maxreplica. It is connected to its correspponding deployment by specifing the scaleTargerRef kind and name.

## Configmap File
The file contains the details of the external resource that will be conneced to our deployment and in this case our database. it is specified under the env in the deployment.yml file

## Your development team should not be able to run certain commands on your k8s cluster, but you want them to be able to deploy and roll back. What types of IAM controls do you put in place?
In order to ensure that the development team has a limited as accessibility, a role based access control is set-up which is assigned to the development team. The role based access control will be assigned a policy that ensures that the team can only run command that can deploy and roll back.

## How would you apply the configs to multiple environments (staging vs production)?
In order to apply configs to multiple environment, namespace will be set-up for each environment in kubernetes and the config is deployed in each namespace

## How would you auto-scale the deployment based on network latency instead of CPU?
Autoscaling based on network latency can be achieved by using a custom metric. It will involve installing a metric server that will retrieve netrics like the network latency. Under the metric in the hpa.yml, the resource name will be network latency while targetAverageUtilization will be set in milliseconds such that when the latency goes above the set target, new replica will be created.