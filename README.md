# ec2ddns
Emulate a dynamic DNS system for internal EC2 traffic.

While AWS has some features in place to try to do this, they didn't seem to work as straightforward as I expected. This script can be added to any instances and will register itself with a Route 53 zone just like a DDNS client would.

## Requirements

* AWS CLI installed and usable

    * `pip install awscli`
    * `aws configure` needs to be ran to select default region (TODO: add in a sensible default)

* The AWS API needs to be accessible

    * your ~/.aws/credentials or ~/.aws/config needs to have valid keys (created by `aws configure`) and EC2/Route 53 privileges, OR
    * the instance in which this runs needs to be in an IAM role that has (at minimum)
        * `ec2:DescribeInstances` (still trying to determine how best to limit this)
        * `route53:ChangeResourceRecordSets` for the zone ID(s) you plan on using
        * `route53:ListHostedZonesByName` for "*"
        * (you can reference the route53-policy.json for the minimum required Route 53 statements)

## Assumptions

* You have a Route 53 zone you can use.

## Notes

* Right now this only deals with the (first) private IPv4 address on an instance. Public elastic IPs are easy enough to use, but internal solutions aren't as readily available.

## TODO

* Add in a sensible default region (check ~/.aws/config first, then AWS_ environment var, if neither exist, then us-east-1 it)
