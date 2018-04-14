
# Steps to Deploy on AWS

* Download and install "Kops"
* Install the "aws" command line tool suite
* Create an AWS s3 bucket as a "state store"
  * This will store the state of the machine
* Create the cluster using kops

### Linux

``` python
curl -LO https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d '"' -f 4)/kops-linux-amd64
chmod +x kops-linux-amd64
sudo mv kops-linux-amd64 /usr/local/bin/kops
```

Kops [Setup](https://github.com/kubernetes/kops) Reference


Install the AWS CLI
``` python
pip3 install awscli --upgrade --user
```

AWS cli [Setup](https://docs.aws.amazon.com/cli/latest/userguide/installing.html) Reference

Configure the IAM role from the instance where you are going to setup the KOPS.  
Region is very important to specify while setting up the configuration
```bash
aws configure
AWS Access Key ID [None]: 
AWS Secret Access Key [None]: 
Default region name [None]: us-east-1
Default output format [None]: 
```

Create an AWS S3 bucket as a "state store"  
```bash
aws s3api create-bucket --bucket docxtract-k8s-test --region us-east-1 --create-bucket-configuration LocationConstraint=us-east-1
```
Once successfully creation of the s3 bucket  

Let's Create the Cluster using *kops*
* First step is making sure the kops knows where the state store is (.i.e nothing but our s3bucket which we created)
* We will set an Environment varaible where our Kops tool will expect it to be there
    ```bash
    export KOPS_STATE_STORE=s3://docxtract-k8s-test
    ```
* Create a cluster using cops
    ```bash
    kops create cluster docxtract-k8s-demo.k8s.local --zones us-east-1a --yes
    ```
    
 