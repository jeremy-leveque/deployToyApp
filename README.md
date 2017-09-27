## Deploy toy Application on AWS
Nice tutorial : http://www.ybrikman.com/writing/2015/11/11/running-docker-aws-ground-up/#launching-an-ec2-instance

### Create IAM user

### Install AWS CLI (optional, not used hereafter)
http://docs.aws.amazon.com/cli/latest/userguide/installing.html

  pip install awscli --upgrade --user

run `aws configure` to configure the CLI, you need an IAM user with its key ID and secret.

### Start EC2 instance

Setup security group, add `Personal TCP` on port `8000` for the DjangoReact application.
Get instance dns name from aws console.
Ssh to the instance using the user pem, the user name "ec2-user" and the dns name.

### Install docker
Enter :

```
[ec2-user]$ sudo yum update -y
[ec2-user]$ sudo yum install -y docker
[ec2-user]$ sudo service docker start
```

Add `ec2-user` to `docker` group

  sudo usermod -a -G docker ec2-user

Exit and come back.

### Install docker-compose
Instructions : https://docs.docker.com/compose/install/#install-compose

  sudo curl -L https://github.com/docker/compose/releases/download/1.16.1/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
  sudo chmod +x /usr/local/bin/docker-compose

### Install git

  sudo yum install git

### Clone the repo

  git clone https://github.com/AdrienAgnel/testDjangoApp.git

### Add the instance IP to the list of allowed hosts

  cd testDjangoAppcd/src/djangoreactredux/settings/
  vim base.py

Add instance IP to the list `ALLOWED_HOST`, the public IP is available in aws console.

### Start the application

  cd ~/testDjangoApp
  docker-compose up

### Test
Go to your browser : `<IP>:8000`

## Next steps

### Add domain name
L'ajout d'un nom de domaine se fait via un DNS (domain name system).

#### Modification du fichier de zone OVH : http://docs.aws.amazon.com/fr\_fr/Route53/latest/DeveloperGuide/MigratingDNS.html
 - création d'un fichier de zones côté AWS
 - récupération du fichier de zones OVH puis importation dans la console Route 53
 - problème de TTL : il faut un seul TTL par type d'entrée, pour que l'import fonctionne. Fichier OVH : deux TTL pour les types TXT -> il faut en enlever une
 - récupérer les NS AWS et les mettre côté OVH 
 
 #### Autre solution : créer un nom de domaine chez Amazon
https://aws.amazon.com/fr/getting-started/tutorials/get-a-domain/

### AWS ECS

### AWS ECR
tuto: https://eu-west-1.console.aws.amazon.com/ecs/home?region=eu-west-1#/firstRun
Registry: 442252299691.dkr.ecr.eu-west-1.amazonaws.com/test-cobble ou cobble2
