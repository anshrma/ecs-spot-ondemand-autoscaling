
![https://aws.amazon.com/ec2/spot/](https://img.shields.io/badge/Amazon-EC2%20Spot%20Instances-orange.svg) ![https://aws.amazon.com/asl/](https://img.shields.io/badge/License-Amazon_Software_License-orange.svg)

# Powering your Amazon ECS Cluster with a mix of Amazon EC2 Spot Instances and Amazon EC2 OnDemand Instances with independent AutoScaling


## Getting Started

This CloudFormation template will deploy two ECS Clusters, one with OnDemand Instances and another with Spot Instances.
The respective ECS Services running tasks at the respective clusters registers the containers with the same target group, thereby routing traffic to on-demand and spot instances.
The respective ECS Service uses CPU Utilization and Memory Utilization metrics to increase/decrease the number of tasks.
Respective cluster's CPU Reservation and Memory Reservation Metrics are used to independently increase/decrease the number of instances (spot or on-demand).
UserData for spot instance LaunchConfiguration runs a script, which listens to EC2 Spot Instance termination notice to automatically set the ECS container instances in **DRAINING** state when a Spot Instance termination notice is detected. The handler script also publishes a message to an SNS topic created by the CloudFormation stack.

### Architecture

![Architecture](images/ecs-spot-ondemand-autoscaling.jpeg)

## Pre-Requisites
This example uses [AWS Command Line Interface](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html) to run Step-3 below.

Please follow [instructions](http://docs.aws.amazon.com/cli/latest/userguide/installing.html) if you haven't installed AWS CLI. Your CLI [configuration](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html) need PowerUserAccess and IAMFullAccess [IAM policies](http://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html) associated with your credentials

```console
aws --version
```

Output from above must yield **AWS CLI version >= 1.11.37**

## Quick setup just in two steps


#### 1. Clone the repository

```console
git clone ssh://git.amazon.com/pkg/ec2-spot-labs
```

#### 2. Run bin/deploy
```console
cd ec2-spot-labs/ecs-ec2-spot-autoscaling
bin/deploy
```

Here are the inputs required to launch CloudFormation templates:
  * **S3 Bucket**: Enter S3 Bucket for storing your CloudFormation templates and scripts. This bucket must be in the same region where you wish to launch all the AWS resources created by this example.
  * **CloudFormation Stack Name**: Enter CloudFormation Stack Name to create stacks

Sit back and relax until all the resources are created for you. After the templates are created, you can open ELB DNS URL to see the ECS Sample App

## Resources created in this exercise

Count | AWS resources
| --- | --- |
4   | [AWS CloudFormation templates](https://aws.amazon.com/cloudformation/)
1   | [Amazon VPC](https://aws.amazon.com/vpc/) (10.215.0.0/16)
2  | [Amazon ECS Cluster](https://aws.amazon.com/ecs/)
2  | [Amazon ECS Service](https://aws.amazon.com/ecs/)
1  | [t2.small EC2 on-demand instance](https://aws.amazon.com/ec2/pricing/on-demand/)
1  | [c3.large EC2 spot instance](https://aws.amazon.com/ec2/spot/pricing/)
1  | [Application Load Balancer](https://aws.amazon.com/elasticloadbalancing/applicationloadbalancer/)
1  | [Application Load Balancer Target Groups](https://aws.amazon.com/elasticloadbalancing/applicationloadbalancer/)


### Pricing

AWS CloudFormation is a free service; however, you are charged for the AWS resources you include in your stacks at the current rates for each. For more information about AWS pricing, go to the detail page for each product on [http://aws.amazon.com](http://aws.amazon.com).


## AWS services used

* [AWS CloudFormation](https://aws.amazon.com/cloudformation/)
* [Amazon EC2 Container Service (ECS)](https://aws.amazon.com/ecs/)
* [Amazon EC2 Spot Instances](https://aws.amazon.com/ec2/spot/)
* [Amazon EC2 OnDemand Instances](https://aws.amazon.com/ec2/pricing/on-demand/)
* [Amazon CloudWatch](https://aws.amazon.com/cloudwatch/)
* [Amazon Simple Notification Service (SNS)](https://aws.amazon.com/sns/)

## Contributing

Comments, feedback, and pull requests are always welcome.

## Authors

* [**Anuj Sharma**](https://github.com/anshrma)

## License

This project is licensed under the [Amazon Software License](https://aws.amazon.com/asl/) - see the [LICENSE.txt](LICENSE.txt) file for details.
