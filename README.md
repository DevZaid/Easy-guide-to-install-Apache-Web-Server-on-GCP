# Easy-guide-to-install-Apache-Web-Server-on-GCP

Introduction

Together will go through an easy step-by-step guide on how to create an VM instance, Install an Apache Web server Install and configure the Ops Agent, generate traffic, and view metrics. We are also going to create an alerting policy and test the alerting policy.

Use Case:

As a cloud administrator, you are in charge of monitoring your infrastructure. As you start your operations in Google Cloud, you should be able to collect data from your resources — such as logs and metrics — establish alerts and visualize the data on a dashboard.

Comparison:

In Amazon Web Services (AWS), you install the Amazon CloudWatch Agent on the Amazon Elastic Compute Cloud (EC2) instances to collect metrics and logs from your applications and infrastructure. You configure logs and log groups to collect log data from your resources by using Amazon CloudWatch. You can even configure one or more custom metrics to monitor any specific aspects of your infrastructure.

Objectives:

In this lab, you learn how to perform the following tasks:

Create a Compute Engine VM instance.
Install an Apache Web Server.
Install and configure the Ops Agent for the Apache Web Server.
Generate traffic and view metrics on the predefined Apache dashboard.
Create an alerting policy.
Steps 1:
Create a Compute Engine VM Instance:
In Google Cloud Console, you will navigate to Compute and then select Compute Engine, then create instance.


Select VM instance

Click on the create instance button.
Once you open the Compute Engine page, Click on Create Instance, then fill in the field for your Instance as follows:

In the Name field, enter andyquickstart-vm.
In the Machine type field, select e2-small.
Ensure the Boot disk is configured for Debian GNU/Linux.
In the Firewall field, select both Allow HTTP traffic and Allow HTTPS traffic.

Select the name of the instance, the machine Serie and the machine type.

Select the allowed traffic in the Firewall section.
Leave the rest of the fields at their default values and then click create. Once you click the button, your VM Instance will appear in the list of Instance in the Instance Tab with a green check mark on status section.


Make sure you see the green check marque.
Step 2:
Install an Apache Web Server:
To deploy an Apache Web Server on your Compute Engine VM instance, do the following:

Open a terminal to your instance, in the Connect column, click SSH.


Click the SSH button on the connect column.
Navigate to the SSH button and click on it. It will open the SSH terminal and you will have to authorized the connection. Once the Terminal is connected, we going to start by updating the package list of our instance by running the following command: # sudo apt-get update


The output of the previous command should look like this:


Once all the package lists are updated, to install an Apache2 HTTP Server, we going to run the following command: # sudo apt-get install apache2 php7.0


The output of the previous command should look like this:


Note: If the previous command fails, then use # sudo apt-get install apache2 php. If asked to continue the installation enter Y.

Open your browser and connect to your Apache2 HTTP server by using the URL http://EXTERNAL_IP, where EXTERNAL_IP is the external IP address of your VM. You can find this address in the External IP column of your VM instance.


Copy the external IP address and paste it to a new browser tab
Congratulations, your Apache2 Web server is Live.


Congratulations
Step 3:
Install and Configure the Ops Agent:
To collect logs and metrics from your Apache Web Server, you will need to install an Ops Agent but using the terminal with the SSH connection.

To open a terminal to your VM instance, navigate to your VM instance and in the connect column click SSH.

Once the SSH terminal is open and running, you going to install the Ops Agent by running the following command:

curl -sSO https://dl.google.com/cloudagents/add-google-cloud-ops-agent-repo.sh
sudo bash add-google-cloud-ops-agent-repo.sh — also-install

It should take few minutes to connect but once it’s done, the output should look like this:


It should say installation succeeded.
Then you going to copy the next command and paste the all thing into the terminal:


It will take few minutes before the request is complete, but the final output should look like this:


Step 4
Generate Traffic and view metrics:
Monitoring dashboards lets you view and analyze metrics related to your services. In this training, you generate metrics on your Apache Web Server and view metric data on the automatically created Apache GCE Overview dashboard.

To generate metrics on your Apache Web Server, do the following:

1. In the Google Cloud console, go to Compute Engine.

2. In the Connect column, click SSH to open a terminal to your VM instance.

3. To generate traffic on your Apache Web Server, run the following command:

timeout 120 bash -c — ‘while true; do curl localhost; sleep $((RANDOM % 4)) ; done’

Note: The previous command generates traffic by making a request to the Apache Web Server every four seconds.

To view the Apache GCE Overview dashboard, do the following:

In the Google Cloud console, search for Monitoring in the top search bar and navigate to the Monitoring service.

Make sure you select: infrastructure and application quality check
2. In the navigation pane, select Dashboards.


3. In All Dashboards, select the Apache GCE Overview dashboard. The dashboard opens.


In the dashboard, there are several charts that contain information about your Apache and Compute Engine integration:


Step 5:
Create an alerting policy.
1. To set up an email notification channel, do the following:

In the Google Cloud console > Monitoring select Alerting and then click Edit notification channels.

In the Email section, click Add new and enter your desired Email Address.

Name the Email Channel: An email address you have access to

To create an alerting policy that monitors a metric and sends an email notification when the traffic rate on your Apache Web Server exceeds 4 KiB/s, do the following:

2. In the Google Cloud console > Monitoring select Alerting and then click Create policy.


3. Select the time series to be monitored:

Click Select a metric and enter VM instance into the filter bar.

In the Active metric categories list, select Apache.

In the Active metrics list, select workload/apache.traffic.


4. In the Transform data section, select the following values:

Rolling window: 1 min

Rolling window function: rate


5. In the Configure alert trigger section, select the following values and click Next:

Alert trigger: Any time series violations

Threshold position: Above threshold

Threshold value: 4000


6. In the Configure notifications and finalize alert section, select the following values:

Notification channels: An email address you have access to

Incident autoclose duration: 30 min

Name the alert policy: Apache traffic above threshold.

7. Click Create policy. Your alerting policy is now active.

Step 6:
Test the alerting policy:
To test the alerting policy you just created, do the following:

1. Navigate to Cloud Console > Compute Engine.

2. In the Connect column, click SSH to open a terminal to your VM instance.

3. In the terminal, enter the following command:

timeout 120 bash -c — ‘while true; do curl localhost; sleep $((RANDOM % 4)) ; done’

The previous command generates traffic in your Apache Web Server.

After the traffic rate threshold value of 4 KiB/s is exceeded in your Apache Web Server, an email notification is sent. It might take several minutes for this process to complete.

The email notification you receive looks similar to the following:


Congratulations:
In this tutorial, you learned how to install Ops Agent on a VM and use it to set an alerting policy to notify a recipient of potential issues with the instance.

Google Ops Agent and Amazon CloudWatch Agent are both monitoring agents that allow you to collect metrics and logs from your application and infrastructure in the cloud. This information, in turn, enables users to monitor the health and performance of applications and infrastructure in the cloud. Here are some similarities and differences between the two services:

Similarities:

Both Ops Agent and Cloudwatch Agent allow users to collect logs and metrics from a virtual machine instance.
Both OpsAgent and Cloudwatch Agent can be installed through a remote connection to the virtual machine (VM) or through the corresponding command-line interface (CLI).
After you have installed the agent on your VM, the host metrics, process metrics, and logs will automatically be routed to the monitoring service (Cloud Logging and Cloud Monitoring in Google Cloud and Amazon CloudWatch in AWS) without user intervention.
Once the data is collected in Cloud Logging and Cloud Monitoring or CloudWatch, you can visualize it in a centralized dashboard by using the corresponding console (Cloud Console for Google Cloud and Amazon Management Console for AWS).
Differences:

Just like in AWS, you also create a configuration file for Ops Agent to log metrics. In AWS, this configuration file is in JSON format, whereas in Google Cloud, a default YAML-based unified configuration is used.
In Google Cloud, you create an alerting policy to get notified in response to an event, while in AWS, you use alarms. In AWS, the alarms need the integration of a notification service — such as Simple Notification Service, Simple Queue Service, or Simple Email Service — into CloudWatch to get notified in response to an event. Whereas, in Google Cloud, the notification service is integrated within the alerting policy.
After the OpsAgent or CloudWatch Agent is installed, logs and metrics are automatically routed to the monitoring service. Google Cloud has two dedicated monitoring services: Cloud Logging for logs and Cloud Monitoring for metrics. Whereas in AWS, the functionality of both services is combined in Amazon CloudWatch.
If you made it this far, thank you for reading! I really hope it was worth your time and that you learned something useful.
