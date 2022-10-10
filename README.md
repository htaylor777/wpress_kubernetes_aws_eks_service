# EKS-Cluster (cloned)
Launching Wordpress with MYSQL on K8s using AWS EKS Service. Amazon Elastic Kubernetes Service (Amazon EKS) gives you the flexibility to start, run, and scale Kubernetes applications in the AWS Cloud or on-premises.There are a lot of benefits for using Amazon EKS like Serverless option, Security, Simplified management and much more. You can get more information on AWS EKS service from [here](https://aws.amazon.com/eks/).

## Usage and Full Guide:

Wanna know how i developed this project, then head over to my blog for full walkthrough from [here 👆](https://apeksh742.medium.com/aws-eks-service-for-deploying-wordpress-mysql-659841cce3b5)

|||||||||||||| Shortcut Steps: |||||||||||||||||||||||||||||||||||||

Get your Acess Key and Secret Access Key and configure your AWS cli using

  -> aws configure

2: Create a Cluster file using yml code. You can get some help from here or you can get my repo from earlier and make some changes.  After Cluster formation you can verify from AWS Console.

  -> eksctl create cluster -f cluster.yml

3. Run command to update or create kubectl configuration file which will allow us to contact to our cluster through kubectl command.

   -> aws eks update-kubeconfig  --name mycluster
 
4. Head back to your AWS console and create an EFS file system and attach VPC and Subnets created by our Cluster.
Note: While choosing subnets choose subnet which is used by your nodegroups.

5.  Create a namespace for isolated and more control over your work

   -> kubectl config set-context --current --namespace=wordpress-mysql

6.  run 3 files which will run and make your EFS provisioned and availaible for cluster and storage.yaml will make make pvc for permanent storage for both Wordpress and MYSQL.

    -> kubectl create -f efs-provisioner.yaml
    -> kubectl create -f rbac.yaml
    -> kubectl create -f storage.yaml 

7. Now create a secret which is more secure and enter a password which you would choose and later refer it to in deploy-wordpress.yaml & deploy-mysql.yaml

    -> kubectl create secret generic mysql-pass --from-literal=password=mydb

8. Now the final step is to deploy your MYSQL and Wordpress yaml files.

    -> kubectl create -f deploy-wordpress.yaml
    -> kubectl create -f deploy-mysql.yaml

9. You can get your Load Balancer IP from

    -> kubectl get all

Then enter your IP in your browser and boom!. There comes your Wordpress setup page, fill them and now you will be landed to your Dashboard.

Now the most beautiful part is if you delete this pod, then also a new pod will be created with same data and same public IP.

If you want to delete your cluster use below command

eksctl delete cluster -f cluster.yml
and also delete your EFS system through AWS console.

GitHub - Apeksh742/EKS-Cluster
Contribute to Apeksh742/EKS-Cluster development by creating an account on GitHub.
github.com