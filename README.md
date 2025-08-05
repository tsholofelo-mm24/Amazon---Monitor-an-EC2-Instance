# Amazon (Monitor An EC2 Instance)

**STEP 1: CONFIGURE AMAZON SNS**

* 1.1 In the AWS Management Console, enter **SNS** in the search bar, and then choose **Simple Notification Service**.
* 1.2	On the left, choose the button, choose **Topics**, and then choose **Create topic**.
* 1.3	On the Create topic page in the **Details** section, configure the following options:
    *	**Type**: Choose **Standard.**
    *	**Name**: Enter MyCwAlarm 


     <img width="624" height="162" alt="image" src="https://github.com/user-attachments/assets/70139381-49a2-4827-8203-9079d2154d77" />





* 1.4	Choose **Create topic**.




     <img width="624" height="201" alt="image" src="https://github.com/user-attachments/assets/cf9a06f9-6ae4-4bc8-9534-a9f0fb4c42a5" />




* 1.5	On the **Create subscription** page in the **Details** section, configure the following options:
    o	**Topic ARN**: Leave the default option selected.
    o	**Protocol**: From the dropdown list, choose **Email**.
    o	**Endpoint**: Enter a valid email address that you can access.





    <img width="624" height="170" alt="image" src="https://github.com/user-attachments/assets/b91b7390-9471-4e9f-834c-8e3ee24cbe31" />



 
* 1.6	Choose **Create subscription**. 

    In the Details section, the **Status** should be **Pending confirmation**. You should have received an **AWS Notification - Subscription Confirmation** email message at the email address that you provided in the previous step.


* 1.7	Open the email that you received with the Amazon SNS subscription notification, and choose **Confirm subscription**.





    <img width="624" height="180" alt="image" src="https://github.com/user-attachments/assets/133adc9c-105e-46ae-af34-2978950f17fd" />












     <img width="561" height="259" alt="image" src="https://github.com/user-attachments/assets/6492074a-4198-407d-9da9-9a93563ecc21" />











**STEP 2: CREATE A CLOUDWATCH ALARM**

We initiate and send an email to your SNS topic if the Stress Test EC2 instance increases to more than 60 percent CPU utilization.


* 2.1	In the AWS Management Console, enter **Cloudwatch** in the search bar, and then choose it.
* 2.2	In the left navigation pane, choose the **Metrics** dropdown list, and then choose **All metrics**.

NB : CloudWatch usually takes 5-10 minutes after the creation of an EC2 instance to start fetching metric details.

* 2.3	On the **Metrics** page, choose **EC2**, and choose **Per-Instance Metrics**.

You are able view all the metrics being logged and the specific EC2 instance for the metrics.

* 2.4	Select the check box with **CPUUtilization** as the **Metric name** for the **Stress Test** EC2 instance.







     <img width="624" height="247" alt="image" src="https://github.com/user-attachments/assets/8a0778d6-68f5-4cdd-b03b-678da6130a20" />





* 2.5	In the left navigation pane, choose the **Alarms** dropdown list, and then choose **All alarms**.


You now create a metric alarm. A metric alarm watches a single CloudWatch metric or the result of a math expression based on CloudWatch metrics. The alarm performs one or more actions based on the value of the metric or expression relative to a threshold over a number of time periods. The action then sends a notification to the SNS topic that you created earlier.

* 2.6	Choose **Create alarm > Select metric > EC2 > Per-Instance Metrics**.


* 2.7	Select the check box with **CPUUtilization** as the **Metric name** for the **Stress Test** instance name > Choose **Select metric**.


* 2.8 On the **Specify metric and conditions** page, configure the following options:
  
   **Metric :**
  
      *	Metric name: Enter CPUUtilization
      *	InstanceId: Leave the default option selected.
      *	Statistic: Enter Average
      *	Period: From the dropdown list, choose 1 minute.





     <img width="624" height="226" alt="image" src="https://github.com/user-attachments/assets/5d9fbb53-7886-4585-b6d4-5e6978303aad" />






  **Conditions :**
  
      * Threshold type: Choose Static.
      * Whenever CPUUtilization is...
      * Choose Greater > threshold.
      * Define the threshold value: Enter 60






  <img width="624" height="175" alt="image" src="https://github.com/user-attachments/assets/c3cd6033-220c-4c21-9238-46992a213615" />





* 2.9	Choose **Next** > On the **Configure actions** page, configure the following options:

  **Notification :**
  
      * Alarm state trigger: Choose  > In alarm.
      * Select an SNS topic: Choose > Select an existing SNS topic.

  
* 2.10 Send a notification to...: Choose the text box, and then choose MyCwAlarm > Choose Next, and then configure the following options:

   **Name and description :**
  
      * Alarm name: Enter LabCPUUtilizationAlarm
      * Alarm description - optional: Enter CloudWatch alarm for Stress Test EC2 instance CPUUtilization


* 2.11 Choose **Next** > Review the **Preview and create** page, and then choose **Create alarm**.


**STEP 3: CREATE A CLOUDWATCH DASHBOARD**

* 3.1 Choose **Next** > Review the **Preview and create** page, and then choose **Create alarm**.

* 3.2 Next to **EC2InstanceURL**, there is a link. Copy and paste this link into a new browser tab.

     This link connects you to the Stress Test EC2 instance. 
     To manually increase the CPU load of the EC2 instance, run the following command:





     <img width="612" height="41" alt="image" src="https://github.com/user-attachments/assets/dde4c57d-e73a-4605-ac07-48cdd57e5368" />









  <img width="563" height="546" alt="image" src="https://github.com/user-attachments/assets/bbdf77e0-c321-477f-a6f3-8fcae364709c" />








_This command runs for 400 seconds, loads the CPU to 100 percent, and then decreases the CPU to 0 percent after the allotted time._


* 3.3 Copy and paste the URL text next to EC2InstanceURL into another new browser tab to open a second terminal for the Stress Test instance.


* 3.4 In the new terminal, run the following command  >  **top**

* 3.5 This command shows the live CPU usage.






  <img width="603" height="670" alt="image" src="https://github.com/user-attachments/assets/3f1095a4-b6ff-4ca9-b999-960a6db1f319" />






* 3.6 Navigate back to the AWS console where you have the CloudWatch **Alarms** page open > Choose **LabCPUUtilizationAlarm**.

* 3.7 Monitor the graph while selecting the **refresh** button every 1 minute until the alarm status is **In alarm**.





  <img width="624" height="219" alt="image" src="https://github.com/user-attachments/assets/72fa178f-1204-49e7-b15e-1cccc0dcdd69" />





**STEP 4: CREATE A CLOUDWATCH DASHBOARD**

* 4.1 Go to the CloudWatch section in the AWS console. In the left navigation pane, choose **Dashboards**.
* 4.2 Choose **Create dashboard** > For **Dashboard name** > enter LabEC2Dashboard and then choose **Create dashboard** > Choose **Line** > Choose **Metrics**.






   <img width="624" height="396" alt="image" src="https://github.com/user-attachments/assets/3bc00d87-9626-4257-b214-77fa2518dc06" />





* 4.3 Choose **EC2**, and then choose **Per-Instance Metrics**.
* 4.4 Select the check box with **Stress Test** for the **Instance name** and **CPUUtilization** for the **Metric name** > Choose Create widget > Choose Save dashboard.






  <img width="411" height="368" alt="image" src="https://github.com/user-attachments/assets/16d12f43-86cf-4f87-a462-5bf473a9faa6" />





_Now you have created a quick access shortcut to view the CPUUtilization metric for the Stress Test instance. All steps have been followed in Monitor an EC2 Instance_
  
