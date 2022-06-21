# Prerequisite for ScratchOrg Pooling

Deploy the following additions to standard object "ScratchOrgInfo" such as a custom fields, validation rule, and workflow to a DevHub as prerequisites to enable the associated scratch org pool commands to work.

There are multiple options to install the supporting package to your environment

-  Installing the Unlocked package to your DevHub from URL

       1. Login to your DevHub
       2. Navigate to https://login.salesforce.com/packaging/installPackage.apexp?p0=04t1P000000gOqzQAE to install the **sfpower-scratchorg-pool** unlocked package into your DevHub.
       3. Select **Install for Admin Only**


-  Installing the Unlocked package to your DevHub using CLI


        sfdx force:package:install -p 04t1P000000gOqzQAE -u Devhub -r -a package -s AdminsOnly -w 30


### Issues with installing third party packages?


If you need to satisfy any compliance issues, such as installing third party packages are not permitted in production, You can utilize the below mechanisms. Please note that when you do this, any further updates have
to be manually synced up


-  Build an Org Depedendent Unlocked Package in your DevHub and deploy it


        1.  git clone https://github.com/dxatscale/sfpowerscripts
        2.  cd sfpowerscripts/prerequisites/scratchorgpool
        3.  sfdx force:package:create -n sfpower-scratchorg-pool -t Unlocked -r force-app -v <devhub_alias> --orgdependent
        4.  sfdx force:package:version:create -p sfpower-scratchorg-pool -x -v <devhub_alias> -w 30
        5.  Observe the package id created in sfdx-project.json
        6.  sfdx force:package:install -p <packageVersionId> -u <devhub_alias> -w 30

## Upgrading from 1.1.0-1 to 2.0.0-1

In order to upgrade from 1.1.0-1 to 2.0.0-1 the picklist value 'return' needs to be added to the 'Allocation_Status__c' 

### For users who are using the sfpower-scratchorg-pool unlocked package:

<b>First install the new version of the package in your DevHub: </b>

sfdx force:package:install --package 04t1P000000katQQAQ -u <devhub_alias>

<b>Secondly add the new picklist value using either the CLI or the Setup menu</b>

Update through the CLI: 
1. git clone https://github.com/dxatscale/sfpower-scratchorg-pool.git
2. sfdx force:source:deploy -p sfpower-scratchorg-pool/force-app/main/default/objects/ScratchOrgInfo/fields/Allocation_status__c.field-meta.xml -u <devhub_alias> -l NoTestRun

Update through Setup: 
1. Go to Setup -> Object Manager
2. Select 'ScratchOrgInfo' 
3. Go to 'Fields and Relationships' 
4. Select 'Allocation_Status__c' 
5. Add a new value 'Return' 
6. Your picklist values should reflect the below screenshot 
![image](https://user-images.githubusercontent.com/63215232/174715103-316cabb2-d053-4812-9306-25521542be56.png)

#### For users who are deploying the source code: 

1. git clone https://github.com/dxatscale/sfpower-scratchorg-pool.git
2. sfdx force:source:deploy -p sfpower-scratchorg-pool/force-app/main/default/objects/ScratchOrgInfo/fields/Allocation_status__c.field-meta.xml -u <devhub_alias> -l NoTestRun --predestructivechanges sfpower-scratchorg-pool/destructiveChanges/pre-deploy-destructive-changes.xml

#### For users who have built their own unlocked package: 

Update your existing source code within your package and create a new package version using "sfdx force:package:version:create -p sfpower-scratchorg-pool -x -v <devhub_alias> -w 30"




