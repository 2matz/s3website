# S3 Website Template

CloudFormation template for simple S3 website.

## Installation
1. Install awscli tools.  
http://aws.amazon.com/cli/  
or  
```brew install awscli```
1. Clone template.
```
cd /path/to/project-dir
git clone git@github.com:2matz/s3website.git
```

## Required
- IAM privilege
 - S3
 - Route53

## Parameter usage
- ```FQDN``` - Website's FQDN. ex)www.example.com
- ```WebsiteHostedZone``` - Route53 HostedZone. ex)example.com
- ```LogBucket``` - S3 bucket for access log. ex)bucket-for-log

## How to use
First, Create Route53 HostedZone before create stack cause.  

** CloudFormation can't create Route53 HostedZone

```
aws route53 create-hosted-zone \
    --name example.com \
    --caller-reference FQDN-YYYY-MM-DD \
    --profile example
```
```
{
    "Location": "https://route53.amazonaws.com/2013-04-01//hostedzone/XXXXXXXXXXXXXX",
    "HostedZone": {
        "ResourceRecordSetCount": 2,
        "CallerReference": "FQDN-YYYY-MM-DD",
        "Config": {},
        "Id": "/hostedzone/XXXXXXXXXXXXXX",
        "Name": "example.com."
    },
    "ChangeInfo": {
        "Status": "PENDING",
        "SubmittedAt": "2014-05-29T01:49:37.721Z",
        "Id": "/change/XXXXXXXXXXXXXX
    },
    "DelegationSet": {
        "NameServers": [
            "ns-1330.awsdns-38.org",
            "ns-959.awsdns-55.net",
            "ns-30.awsdns-03.com",
            "ns-1756.awsdns-27.co.uk"
        ]
    }
}
```

Second, Create CloudFormation stack.

```
aws cloudformation create-stack \
    --stack-name example \
    --parameters \
        ParameterKey=FQDN,ParameterValue="www.example.com" \
        ParameterKey=WebsiteHostedZone,ParameterValue="example.com" \
        ParameterKey=LogBucket,ParameterValue="example-log" \
    --template-body file:////path//to//template.json \
    --profile example
```

Let's access to http://www.example.com

## Template validation
```
aws cloudformation validate-template \
    --template-body file:////path//to//template.json \
    --profile example
```
