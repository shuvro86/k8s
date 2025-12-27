
kOps (Kubernetes Operations) is an open-source command-line tool that automates the deployment, management, and upgrading of production-grade, highly available Kubernetes clusters.

Run a simple free tier ec2 named kops in AWS. 

Now do the following steps in that instance : -



#install cops in AWS 

```sudo snap install aws-cli --classic```



#aws cli configure
```
aws configure
Access key ID : xxxxxxxxxxxxxxxx
Secret access key: xxxxxxxxxxxxxxxxxxxxxxxxxx
AZ: ap-south-1
```

#Install kops 

```
curl -Lo kops https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d '"' -f 4)/kops-linux-amd64
chmod +x kops
sudo mv kops /usr/local/bin/kops
```



#Install Kubectl 
----------------
```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
kubectl version --client
```

#S3 

```create a bucke3 named  -> devops-kops-bucket-789```


#Route 53 

```
Make the following entry for a hosted zone :     
kubevro.headwaydevops.dpdns.org    

Note:    
I've already a free domain named -> headwaydevops.dpdns.org in cloudflare 

Add below NS record in cloudflare : 
ns-98.awsdns-12.com.
ns-1696.awsdns-20.co.uk.
ns-882.awsdns-46.net.
ns-1271.awsdns-30.org.
```

#ssh-keygen
```
In root user home directory, run the below command just hitting 2-3 enter :

#ssh-keygen 
```







#These 2 are the main command to run the cluster 
```
kops create cluster --name=kubevro.headwaydevops.dpdns.org --state=s3://devops-kops-bucket-789 --zones=ap-south-1a,ap-south-1b --node-count=2 --node-size=t3.small --control-plane-size=t3.medium --dns-zone=kubevro.headwaydevops.dpdns.org --node-volume-size=12 --control-plane-volume-size=12 --ssh-public-key ~/.ssh/id_ed25519.pub


kops update cluster --name=kubevro.headwaydevops.dpdns.org --state=s3://devops-kops-bucket-789 --yes --admin
```


To check  
```
kops validate cluster --name=kubevro.headwaydevops.dpdns.org --state=s3://devops-kops-bucket-789
kubectl get no
```
Delete 

```
kops delete cluster --name=kubevro.headwaydevops.dpdns.org --state=s3://devops-kops-bucket-789 --yes
```

###Note: If needed again, just run the main 2 command (i.e kops create and update) to provision necessary setup for kops cluster.   
