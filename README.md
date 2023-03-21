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
 
  #### References ####
  * https://mayankchourasia2.medium.com/deploy-a-compute-instance-with-a-remote-startup-script-eb4d44145149
  * https://www.youtube.com/watch?v=3dEoBZ9t_-Q
 
 
