# Google-Cloud-Skills-Boost

## Cloud Architecture: Design, Implement, and Manage

### 1 : Deploy a Compute Instance with a Remote Startup Script

#### Step 1 ####

  * Navigate to Cloud Storage
  * Navigate to Buckets, click Create.
    * Name the bucket: Your Project ID
    * Move to 'Choose How to Control Access To Objects'
      * **Deselect** 'Enforce Public Access...'
      * Access Control: Choose *Fine Grained*
  * Click Create

#### Step 2 ####

  * Navigate to the Instructions Page
  * Under Student Resources, a file is provided. Download it.
  * Navigate back to the bucket created, and click on it.
    * There is an *Upload Files* button. Click on it and upload the file downloaded in the previous step
    * In the bucket, at the file recently downloaded, there are three dots on the right end, click on it and rename the file *install-web-sh*
    * Click the same 3 dots, and click on *Edit Access*
      * Click on *Add Entry* Then choose **Public** from the dropdown
      * Now Save
      
#### Step 3 ####

 * Navigate to Compute Engine: VM INSTANCES
 * Click on *Create Instance*
 * Leaving most defaults, scroll down to Firewalls
  * Select **Allow HTTP Traffic**
 * Continue scrolling to Advanced Settings: Management
  * in Management, scroll to *Metadata*. Click *Add Item*
   * For Key, type **startup-script-url**
   * for value:
     *  Open Cloud Storage in a new tab
     *  Navigate to the bucket we created, then our uploaded file
     *  Click on the uploaded file, and copy its **Public URL**
     *  Navigate back to your VM Instances tab, and paste it in this section  
 * Click on Create
 
 #### Step 4 ####
 * Once the instance is created, click on the **External IP**
 * A new tab will open with the default Apache Server Web Page
 
 #### Success! 🎉 ####
 
  ### References ###
  * https://mayankchourasia2.medium.com/deploy-a-compute-instance-with-a-remote-startup-script-eb4d44145149
  * https://www.youtube.com/watch?v=3dEoBZ9t_-Q

----


### 2 : Configure Secure RDP using a Windows Bastion Host

#### Task 1: A new non-default VPC has been created
```
gcloud compute networks create securenetwork --subnet-mode custom
```
#### Task 2: The new VPC contains a new non-default subnet within it
```
gcloud compute networks subnets create securenetwork-subnet --network=securenetwork --region us-central1 --range=192.168.16.0/20
```

#### Task 3: A firewall rule exists that allows TCP port 3389 traffic ( for RDP )
```
gcloud compute firewall-rules create rdp-ingress-fw-rule --allow=tcp:3389 --source-ranges 0.0.0.0/0 --target-tags allow-rdp-traffic --network securenetwork

```
#### Task 4: A Windows compute instance called vm-bastionhost exists that has a public ip-address to which the TCP port 3389 firewall rule applies.
```
gcloud compute instances create vm-bastionhost --zone=us-central1-a --machine-type=e2-medium --network-interface=subnet=securenetwork-subnet --network-interface=subnet=default,no-address --tags=allow-rdp-traffic --image=projects/windows-cloud/global/images/windows-server-2016-dc-v20220513
```

#### Task 5: A Windows compute instance called vm-securehost exists that does not have a public ip-address
```
gcloud compute instances create vm-securehost --zone=us-central1-a --machine-type=e2-medium --network-interface=subnet=securenetwork-subnet,no-address --network-interface=subnet=default,no-address --tags=allow-rdp-traffic --image=projects/windows-cloud/global/images/windows-server-2016-dc-v20220513

```
#### Task 6: The vm-securehost is running Microsoft IIS web server software.
```
gcloud compute reset-windows-password vm-bastionhost --user app_admin --zone us-central1-a
```
#### Now, copy the password for app_admin user, we need this to login into bastion vm 
 ---- 
#### Manaul steps
- Click on Add roles and features 

![image](https://user-images.githubusercontent.com/104570014/168892931-33d20ae2-c0c5-424b-af2f-f73e5ae78368.png)
![image](https://user-images.githubusercontent.com/104570014/168893306-de8312ea-8d42-475c-b93b-fd06e32387c6.png)
![image](https://user-images.githubusercontent.com/104570014/168893438-8c8968d9-08b3-4f92-b13e-6ef8c7a7a6e6.png)
![image](https://user-images.githubusercontent.com/104570014/168893562-601a4d6a-710a-4124-91fd-5176cfad5bd1.png)
![image](https://user-images.githubusercontent.com/104570014/168893704-1500a6fa-0a29-4926-be2b-35078a8e0a12.png)

- Click next 
- Click on install 




#### Success! 🎉 ####

### References ###
* https://www.youtube.com/watch?v=ZeY_GOU55S8 
* https://www.youtube.com/watch?v=yRMWtokgboE
 
 ----
 
### 3 : Build and Deploy a Docker Image to a Kubernetes Cluster

#### Task 1: Built Docker image with tag V1
    gsutil cp gs://$DEVSHELL_PROJECT_ID/echo-web.tar.gz .
    
#### Task 2: Extract downloaded zip
    tar -xvf echo-web.tar.gz

    gcloud builds submit --tag gcr.io/$DEVSHELL_PROJECT_ID/echo-app:v1 .
    
#### Task 3: Create Kubernetes cluster
    gcloud container clusters create echo-cluster --num-nodes 2 --zone us-central1-a --machine-type n1-standard-2

#### Task 4: Deploy application to Kubernetes cluster
    kubectl create deployment echo-web --image=gcr.io/qwiklabs-resources/echo-app:v1

#### Task 5: Expose app to allow external access
    kubectl expose deployment echo-web --type=LoadBalancer --port=80 --target-port=8000
    
#### Task 6: Extract Kubernetes services
    kubectl get svc
      
    
#### Success! 🎉 ####

### References ###
* https://www.youtube.com/watch?v=XLTW8Yeq8xM
* https://www.youtube.com/watch?v=S06ew_wCbSs
 
----

### 4 : Scale Out and Update a Containerized Application on a Kubernetes Cluster

#### Step 1 ####

    gsutil cp gs://sureskills-ql/challenge-labs/ch05-k8s-scale-and-update/echo-web-v2.tar.gz .

    tar xvzf echo-web-v2.tar.gz

    gcloud builds submit --tag gcr.io/$DEVSHELL_PROJECT_ID/echo-app:v2 .

    gcloud container clusters get-credentials echo-cluster --zone us-central1-a

    kubectl create deployment echo-web --image=gcr.io/qwiklabs-resources/echo-app:v1

    kubectl expose deployment echo-web --type=LoadBalancer --port 80 --target-port 8000

    kubectl edit deploy echo-web

Copy and paste the above code block and execute it in the Cloud Shell 

#### Step 2 ####

* A File will open, and we need to make one change.
* Navigate down to *containers:* then to *image:*
* Press *i* to start inserting text
* Change the **v1** you see there to **v2**, to indicate the update of the image version
* Press Esc, then type **:wq** Now press enter.


#### Step 3 ####

    kubectl scale deploy echo-web --replicas=2

#### Success! 🎉 ####

### References ###
* https://www.youtube.com/watch?v=UGAL0jIxpuk
* https://www.youtube.com/watch?v=6LYGSUPVQbQ
 
----

### 5 : Migrate a MySQL Database to Google Cloud SQL

#### Step 1 ####

    export ZONE=us-central1-a

    gcloud sql instances create wordpress --tier=db-n1-standard-1 --activation-policy=ALWAYS --gce-zone $ZONE

    gcloud sql users set-password --host % root --instance wordpress --password Password1*
    
You can change the zone if the exercise requires you to choose a different one at this stage

#### Step 2 ####

    export ADDRESS=34.67.95.207/32

    gcloud sql instances patch wordpress --authorized-networks $ADDRESS --quiet
    
Navigate to your VM Instances in Compute Engine
  * Copy the External IP of the instance **blog**
  * Paste it in the *ADDRESS=* part of the above code
  
Execute the commands

#### Step 3 ####
Navigate to your VM Instances in Compute Engine
* On the SSH button of the instance **blog**, click the dropdown button
* Select *View Gcloud Command*
* Copy that command

Return to the Cloud Shell, and paste that command, and execute it

For the Paraphrase, press Enter twice

We are now in the blog VM instance

#### Step 4 ####
    MYSQLIP=$(gcloud sql instances describe wordpress --format="value(ipAddresses.ipAddress)")

    mysql --host=$MYSQLIP \
        --user=root --password
        
You will be prompted to provide a password. Copy and paste: **Password1***

Now press enter

Copy and paste the following

    CREATE DATABASE wordpress;
    CREATE USER 'blogadmin'@'%' IDENTIFIED BY 'Password1*';
    GRANT ALL PRIVILEGES ON wordpress.* TO 'blogadmin'@'%';
    FLUSH PRIVILEGES;

Once done, type *exit* and press enter

#### Step 5 ####

    sudo mysqldump -u root -pPassword1* wordpress > wordpress_backup.sql

    mysql --host=$MYSQLIP --user=root -pPassword1* --verbose wordpress < wordpress_backup.sql

    sudo service apache2 restart

    cd /var/www/html/wordpress

    sudo nano wp-config.php
    
Copy the above code and run it in the Cloud Shell

A file will be opened (the wp-config.php)

* Scroll down to **MYSQL HOSTNAME**

Open SQL on your Cloud Console in a new tab.
* On the database created, named *wordpress*, copy its **Public IP Address**

Return to the cloud shell at MYSQL HOSTNAME
* Replace *localhost* with the IP Address just copied

 *Make sure the IP Address is still within single quotaton marks in the file*
 
Now press CTRL+X, then SHIFT+Y then ENTER to save those changes


#### Success! 🎉 ####

### References ###
* https://youtu.be/sY3RHI6nvQ4
 
----
 
### 1 : Google Cloud Essential Skills: Challenge Lab

 #### Step 1 ####
 
 Navigate to Compute Engine: VM INSTANCES
 Click on *Create Instance*
 Change the name, and the zone to those specified in the instructions page
 Scroll down to Firewall
  * Select **Allow HTTP Traffic**
  * * Select **Allow HTTPS Traffic**
 Click on CREATE
 
 #### Step 2 ####
 
 Once the VM Instance is created, press the SSH button
 
 Copy and run the followinig commands in the Shell
 
  To update the VM and install the Apache2 Server
 
     sudo apt update && sudo apt -y install apache2

  To verify that Apache is running
 
     sudo systemctl status apache2
 
  To overwrite the Apache web server default web page

    echo '<!doctype html><html><body><h1>Hello World!</h1></body></html>' | sudo tee /var/www/html/index.html
    
    
#### Success! 🎉 ####

### References ###
* https://cloud.google.com/compute/docs/tutorials/basic-webserver-apache

----

----


## Congratulations 🏅 
## Leave a <img src="https://em-content.zobj.net/thumbs/120/samsung/349/star_2b50.png" width="50" height="50"> if this has been helpful to you

<img src="https://em-content.zobj.net/thumbs/120/samsung/349/waving-hand_1f44b.png"><img src="https://em-content.zobj.net/thumbs/120/samsung/349/waving-hand_1f44b.png"><img src="https://em-content.zobj.net/thumbs/120/samsung/349/waving-hand_1f44b.png">
