# OCP-Backups
Various ways of doing backups (clusters, apps, etcd, etc...) on OCP

## Applications Backup

This relies on OADP Operator - OpenShift Advanced Data Protection which relies on the Velero open source project.

Follow https://access.redhat.com/documentation/en-us/openshift_container_platform/4.10/html-single/backup_and_restore/index#oadp-setting-resource-limits-and-requests_installing-oadp-aws

### step 1
Deploy the OADP operator

### step 2
Create a cloud-credentials
oc create secret generic cloud-credentials --namespace oadp-operator --from-file cloud=cloud-credentials.txt

### step 3 - create an S3 bucket to hold the data for the backups
you'll need the name in the creation of the Data Protection Application (step 4)

### step 4 Deploy the Data Protection Application

Don’t forget to create a velero service account
oc create sa velero
Don’t forget to give it “root” access
oc adm policy add-scc-to-user anyuid -z velero

oc apply -f velero.yaml

### step 5 Deploy a sample application
Then deploy the application (under OCP-Backup/sample-app/)
Oc new-project wordpress
Oc adm policy add-scc-to-user anyuid -z default
Oc apply -k ./ (you must be in the OCP-Backup/sample-app/ folder)

### step 6 Take a backup  
oc apply -f backup.yaml

### step 7 Delete the sample application
Delete the app (oc delete project wordpress)

### step 8 
Restore using
Oc apply -f restore.yaml


A quick demo of this activity is available here - https://youtu.be/czONI4EaowA
