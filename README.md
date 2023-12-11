# Provision ELB Target Group

## Description

This GitHub Action provisions an Elastic Load Balancer (ELB) target group. If a target group with the specified name already exists, the action will use the existing target group. Otherwise, it creates a new target group with the specified name in the specified VPC. The target group listens on a specified port for HTTP traffic, uses IP as target type, and uses HTTP health checks on the "/health" path every 30 seconds, expecting HTTP status codes in the range 200-299.

## Inputs

| Name                  | Description                                                  | Required | Default                  |
| --------------------- | ------------------------------------------------------------ | -------- | ------------------------ |
| `elbName`             | The name of the Elastic Load Balancer (ELB).                 | ✔        | 'ecs-modular-monolith'   |
| `targetGroupName`     | The name for the new target group.                           | ✔        |                          |
| `vpcId`               | The ID of the VPC where the target group is to be created.   | ✔        |                          |
| `port`                | The port on which the target group will listen.              | ✔        | 3000                     |

## Outputs

| Name                  | Description                                                  |
| --------------------- | ------------------------------------------------------------ |
| `targetGroupArn`      | The ARN of the created or existing target group.             |

## Example Usage

```yaml
jobs:
    provision-elb-target-group:
        runs-on: ubuntu-latest

        steps:
            - name: Checkout repository
                uses: actions/checkout@v3

            - name: Provision ELB Target Group
                id: elb-target-group
                uses: caring/gh-provision-elb-target-group@v1.0.0
                with:
                   elbName: 'your-elb-name'
                   targetGroupName: 'your-target-group-name'
                   vpcId: 'your-vpc-id'
                   port: 3000

            - name: Print Target Group ARN
                run: |
                    echo "Target group ARN:"
                    echo "${{ steps.elb-target-group.outputs.targetGroupArn }}"
