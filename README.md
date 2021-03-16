# :sparkles:Deploying WordPress site with MySQL Database on Kubernetes cluster using Ansible:sparkles:
Creating our own multinode kubernetes cluster is very tedious task.Also, deploying websites, eg: WordPress which is highly integrated and dependent on MySQL database in a manual way is very slow and prone to errors.So, here is the solution for automating the creation of kubernetes multinode cluster and deploying wordpress site using Ansible on AWS. 

## :heavy_exclamation_mark: Steps to be followed to achieve this task :heavy_exclamation_mark: 

:one: Create a ansible role to *provision ec2 instance*. Here, provisionEC2 role. 

:two: Setup ansible dynamic inventory. 

:three: Create a ansible role to configure *kubernetes master* node. Here, k8s_master role. 

:four: Create a ansible role to configure *kubernetes slave* nodes. Here, k8s_slaves role. 

:five: Create a ansible role to create Wordpress and MySQL pods. Here, wpSQL role. 

:six: Create a playbook that will run provisionEC2 role. Here, setupec2.yml playbook. 

:seven: Create a playbook that will run k8s_master,k8s_slaves and wpSQL role. Here, setup_cluster.yml file. 

 <br>
To create a role, use command :leftwards_arrow_with_hook:

      ansible-galaxy init provisionEC2 
     
Similarly, you can create other roles. 

<br>

### :beginner: How to setup dynamic inventory? :beginner:
:pushpin: Copy the hosts directory in your workspace. 

:pushpin: In ansible configuration file, change the inventory=<path_of_host_directory>(above folder) 

:pushpin: In ec2.ini file, at the end provide your AWS_ACCESS_KEY and AWS_SECRET_KEY 

<br>

### :dart: Setting up main playbooks :dart:
:pushpin: Here, setupec2.yml and setup_cluster are the main playbook. 

:pushpin: setupec2.yml playbook will launch 3 instances for us. Here, I have made the VPC, Subnet, Image, instance type fixed. You can add as a variable and include the variables in /vars/main.yml file in the provisionEC2 role. Also, the *OS_Name* list consists of tags to be provided to the instances launched. 

![ec2launch](https://miro.medium.com/max/875/1*lvKRd7sTC1_dwi4unz102Q.png) 

:pushpin: After running setupec2.yml playbook, wait for ssh to come up. You can also verify launched instances on AWS Portal. 

![ec2portal](https://miro.medium.com/max/875/1*UhEG8iIJCM0k9qmhAEGLaA.png)

:pushpin: Now, run the setup_cluster.yml playbook to configure master and two slave nodes. 


:exclamation: Copy the master token and provide as input when slaves role run. :exclamation:

![asktoken](https://miro.medium.com/max/875/1*7lEocQjVgZavp0r2XxL9Kw.png) 

:pushpin: Finally, you can verify by the output that, we can run kubectl command. That means it is configured successdfully :ok_hand: 

:pushpin: Now, the wpSQL role will run on Master Node as Host. As while configuring, we have configured Master node as k8s cluster client, so we launch pods using Master. 
![finalop](https://miro.medium.com/max/875/1*-AcTVfnWF0BbypMIaNvlQw.png) 

:pushpin: Now, the pod is exposed on a random port through which clients can connect. You can connect to the wordpress site using port number. 
![wpstart](https://miro.medium.com/max/875/1*Y7UMeYsk2YLnTKbRq3kNLw.png) 

:pushpin: Put all the relevant details and then we will be able to suucessfully connect to the Wordpress site
![logindet](https://miro.medium.com/max/875/1*8VGnOz0-bmPovksatNnorg.png)

<br>
### :smile: Now, you can see your wordpress site hosted :smile:
![finalwp](https://miro.medium.com/max/875/1*NWkAo6oeLGutxs_Ozyr6rQ.png)

To better understand all the internal concepts and configuration, feel free to read my [article](https://tirth1272.medium.com/automate-kubernetes-clusterusing-ansible-18238dae6239). If you have any queries or suggestions, feel free to contact me on [Linkedin](https://www.linkedin.com/in/tirupatel/).
