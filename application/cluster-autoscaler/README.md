# Cluster Autoscaler

## Overview and Architecture
Cluster autoscaler requires following k8s resourses to be created in kube-system namespace:
    - service account that will be used by autoscaler 
        - NOTE: must have annotatiions that bind SA with aws autoscaler role to auth with AWS and gain permissions to interact with nessesary AWS resources
    - now we need to gain permissions insede of k8s cluster therefor we create ClusterRole and RoleBinding 
        - Cluster role will define all nessesary prmissions that autoscaller needs
        - RoleBinding will bind ClusterRole with cluster autoscaler SA
    - finally we can create a deployment with 1 replica and k8s.gcr.io/autoscaling/cluster-autoscaler image
        - NOTE: image version must match cluster k8s version


## Issues
- cluster autoscaler version should be the same as eks cluster, othervise CA cant talk to Kubernetes APIs => autoscaller pod failing to start with CrashLoopBackOff status

## How to Run/Execute
- list of dynamic values that must be updated (verified) before applying autoscaler module:
    - iamRoleForEksAutoscaler: this var contains arn of AWS IAM role that will be maped to autoscaler SA
    - clusterAutoscalerImageVersion: version of cluster-autoscaler image (must match the vesion of eks k8s version)
    - autoScalingGroupName: autoscaling group name that is used by EKS

- module defined with helm so we will use helm install command to provision k8s objects defined in it
    - the comand is: helm install cluster-autoscaler /path/to/chart -f /path/to/values/dev-values.yaml --namespace kube-system

## Resources
CA config options:
https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md
Whole CA infra overview:
https://www.youtube.com/watch?v=MZyrxzb7yAU&t=624s

## Additional Information
[Any other relevant information]
