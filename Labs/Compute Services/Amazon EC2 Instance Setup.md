# Amazon EC2 Instance Setup - Documentation Journey

## Executive Summary

This documentation captures the journey of completing an introductory Amazon EC2 lab that covered the fundamental operations of launching, configuring, monitoring, and managing an EC2 instance on AWS. Over approximately 45 minutes, the lab provided hands-on experience with core AWS compute services and instance lifecycle management.

---

## Introduction to Amazon EC2

### Overview

The lab was structured to provide a foundational understanding of Amazon Elastic Compute Cloud (Amazon EC2), which was explored as a web service providing resizable compute capacity in the cloud. The primary objective was to demonstrate how developers could leverage EC2's simple web service interface to obtain and configure computing resources with minimal friction.

#### Key Concepts Covered:

- **Amazon EC2** was learned as the core service that reduces the time required to obtain and boot new server instances to minutes
- **Elastic Scalability** was demonstrated as the ability to quickly scale capacity up or down based on computing requirements
- **Cost Efficiency** was highlighted as the pay-as-you-go model that charged only for capacity actually used
- **Resilience** was emphasized as the capability to build failure-resistant applications

**[Screenshot: AWS Management Console EC2 Dashboard Overview]**
> *Place architectural diagram showing EC2 service within AWS infrastructure here*

---

## Lab Objectives Achieved

By the conclusion of this lab, the following objectives were successfully accomplished:

1. **Launched** a web server instance with termination protection enabled
2. **Monitored** the EC2 instance using CloudWatch metrics and status checks
3. **Modified** security group rules to allow HTTP access
4. **Resized** the instance by changing instance type and EBS volume size
5. **Tested** termination protection functionality
6. **Terminated** the EC2 instance gracefully

---

## Task 1: Launching the EC2 Instance

### Initial Setup Phase

The journey began by accessing the AWS Management Console and navigating to the EC2 service. The instance was configured through a comprehensive multi-step launch wizard that addressed naming, image selection, instance type, networking, storage, and advanced settings.

#### Step 1: Instance Naming

The instance was named "Web Server" through the Name and tags pane. This action created a key-value pair in AWS that allowed for easy identification and management of the resource.

**[Screenshot: EC2 Launch Wizard - Naming Step]**
> *Showing the Name and tags pane with "Web Server" entered in the text box*

#### Step 2: Amazon Machine Image (AMI) Selection

The Amazon Linux 2023 AMI was selected from the Quick Start list. This image was chosen as it provided a pre-configured operating system template suitable for web server deployment.

**[Screenshot: AMI Selection Screen]**
> *Displaying the Application and OS Images pane with Amazon Linux 2023 highlighted*

#### Step 3: Instance Type Configuration

A **t3.micro** instance type was selected for this lab. This instance type was noted to include:
- 2 virtual CPUs
- 1 GiB of memory
- Suitable for low-traffic applications and testing scenarios

**[Screenshot: Instance Type Selection - t3.micro]**
> *Showing the instance type selection dropdown with t3.micro highlighted*

#### Step 4: Key Pair Configuration

For this lab environment, the instance was configured to proceed without a key pair, as SSH access was not required for the demonstrations.

#### Step 5: Network and Security Configuration

The network settings were configured as follows:

- **VPC Selected:** Lab VPC
- **Security Group Name:** Web Server security group
- **Description:** Security group for my web server
- **Initial Inbound Rules:** The default SSH rule was removed to improve security posture

**[Screenshot: Network Settings Configuration]**
> *Displaying the VPC selection, security group creation, and inbound rules configuration*

#### Step 6: Storage Configuration

The default storage configuration was maintained:
- **Root Volume Size:** 8 GiB
- **Volume Type:** Default EBS configuration

#### Step 7: Advanced Details and User Data

Termination protection was **enabled** in the Advanced details pane to prevent accidental instance termination.

The following User Data script was deployed to configure the web server automatically upon instance launch:

```bash
#!/bin/bash
yum -y install httpd
systemctl enable httpd
systemctl start httpd
echo '<html><h1>Hello From Your Web Server!</h1></html>' > /var/www/html/index.html
```

**Purpose of the script:**
- Installed Apache HTTP Server (httpd)
- Enabled httpd to start automatically on system boot
- Started the httpd service immediately
- Created a simple HTML test page

**[Screenshot: Advanced Details - User Data Script Entry]**
> *Showing the User Data text box with the bash script visible*

#### Step 8: Instance Launch

The instance was launched successfully and transitioned through the following states:

1. **Pending** → Instance was being provisioned
2. **Running** → Instance had started booting

The instance was assigned a public DNS name and public IPv4 address for external connectivity.

**[Screenshot: EC2 Instances Dashboard - Web Server Running]**
> *Showing the Web Server instance in Running state with status checks at 2/2 passed*

---

## Task 2: Instance Monitoring

### Status Checks and CloudWatch Metrics

Once the instance was fully operational, the monitoring capabilities were explored. The instance was observed to have successfully passed both automated status checks:

- **System Reachability Check:** Verified that the underlying hardware was functioning properly
- **Instance Reachability Check:** Confirmed that the instance's operating system was operational

**[Screenshot: Status Checks Tab]**
> *Displaying the System reachability and Instance reachability checks both in "passed" status*

### Monitoring Tab Review

The Monitoring tab was accessed to view CloudWatch metrics being collected for the instance. Although minimal data was present due to the recent launch, the tab demonstrated the following metrics collection:

- CPU utilization
- Network traffic (in/out)
- Instance status
- Detailed (one-minute) monitoring availability

**[Screenshot: CloudWatch Metrics for EC2 Instance]**
> *Showing the monitoring graphs and available metrics dashboard*

### Instance Screenshot Feature

The instance screenshot capability was tested through the Actions menu (Monitor and troubleshoot → Get Instance Screenshot). This feature was noted as valuable for troubleshooting when SSH or RDP connections were unavailable, as it displayed the actual console output of the instance.

**[Screenshot: Instance Console Screenshot]**
> *Showing the EC2 instance console display as captured by the screenshot feature*

---

## Task 3: Security Group Configuration and Web Server Access

### Initial Access Attempt

An attempt was made to access the web server using the instance's public IPv4 address in a web browser. This attempt failed, as the security group had not yet been configured to allow inbound HTTP traffic.

**Key Learning:** Security groups functioned as virtual firewalls, controlling all ingress and egress traffic to instances.

### Security Group Modification

The security group was updated to permit HTTP traffic:

1. Navigated to Security Groups in the EC2 console
2. Selected the "Web Server security group"
3. Added a new inbound rule with the following configuration:
   - **Type:** HTTP
   - **Protocol:** TCP
   - **Port Range:** 80
   - **Source:** Anywhere-IPv4 (0.0.0.0/0)

**[Screenshot: Inbound Rules Configuration - HTTP Rule Addition]**
> *Showing the Edit inbound rules interface with the HTTP rule being added*

### Successful Web Server Access

After applying the security group changes, the web browser was refreshed and successfully displayed:

```
Hello From Your Web Server!
```

**[Screenshot: Web Server Successfully Accessed]**
> *Showing the browser window displaying the "Hello From Your Web Server!" message*

---

## Task 4: Instance Resizing

### Stopping the Instance

The instance was stopped through the Instance state menu before resizing operations could be performed. The instance transitioned to a **stopped** state, during which:
- No compute charges were incurred
- Storage charges for EBS volumes remained active

**[Screenshot: Instance State - Stopped]**
> *Showing the Web Server instance in stopped state*

### Instance Type Upgrade

The instance type was upgraded from **t3.micro** to **t3.small** using the Instance Settings menu:

- **Change:** t3.micro (2 vCPU, 1 GiB RAM) → t3.small (2 vCPU, 2 GiB RAM)
- **Benefit:** Doubled available memory for improved application performance

**[Screenshot: Instance Type Change Dialog]**
> *Showing the change instance type interface with t3.small selected*

### EBS Volume Resizing

The root EBS volume was resized through the Volumes interface:

- **Original Size:** 8 GiB
- **New Size:** 10 GiB
- **Storage Increase:** 2 GiB additional capacity

**[Screenshot: EBS Volume Modification]**
> *Displaying the Modify volume dialog with size change from 8 GiB to 10 GiB*

### Instance Restart

The resized instance was restarted, bringing it back to a **Running** state with the upgraded configuration:
- Increased memory (2 GiB)
- Larger root volume (10 GiB)

**[Screenshot: Instance Restarted with New Configuration]**
> *Showing the Web Server instance running with updated instance type and volume size*

---

## Task 5: Termination Protection Testing

### Testing Protection Mechanism

An attempt was made to terminate the instance through the Instance state menu. The termination operation failed with an error message:

```
Failed to terminate an instance: The instance may not be terminated.
```

This failure occurred because termination protection was enabled during the initial instance configuration.

**[Screenshot: Termination Protection Error]**
> *Showing the error message when attempting to terminate a protected instance*

### Disabling Protection

Termination protection was disabled through the Instance settings menu:

1. Accessed Actions → Instance settings → Change termination protection
2. Unchecked the "Enable" option
3. Saved the changes

**[Screenshot: Termination Protection Disabled]**
> *Displaying the Instance settings dialog with termination protection unchecked*

### Instance Termination

With termination protection disabled, the instance was successfully terminated:

1. Selected Instance state → Terminate instance
2. Confirmed the termination action
3. Instance transitioned through **shutting-down** state and reached **terminated** state

**[Screenshot: Instance Terminated]**
> *Showing the Web Server instance in terminated state*

---

## Key Learnings and Best Practices

### Security Group Management

The lab demonstrated that security groups served as critical components of AWS security architecture:
- Default-deny posture required explicit rules for traffic allowance
- Rules could be modified without instance restart
- Changes applied immediately to all associated instances

### Instance Lifecycle Management

The complete instance lifecycle was explored:
- **Launch:** Configuration through the wizard interface
- **Monitor:** CloudWatch metrics and status checks
- **Modify:** Instance type and storage resizing
- **Terminate:** Graceful shutdown with protection mechanisms

### Cost Optimization

Key cost considerations were highlighted:
- Stopped instances incurred no compute charges
- Storage charges persisted for stopped instances
- Right-sizing instances reduced unnecessary spending

### Monitoring and Troubleshooting

Multiple monitoring capabilities were demonstrated:
- Real-time status checks for instance health
- CloudWatch metrics for performance analysis
- Console screenshots for deep troubleshooting

---

## Conclusion

The completion of this lab provided a comprehensive introduction to Amazon EC2 operations. The journey covered the entire instance lifecycle from creation through termination, with emphasis on practical AWS management skills including security configuration, performance optimization, and operational monitoring. These foundational skills were established as essential for any AWS practitioner working with compute services.

**Lab Duration:** Approximately 45 minutes  
**Status:** Successfully Completed  
**Key Achievement:** Demonstrated proficiency in EC2 instance management across all core operational areas.

---

## Appendix: Important AWS Resources

- **EC2 Dashboard:** Central hub for instance management and monitoring
- **Security Groups:** Virtual firewalls for network access control
- **CloudWatch:** Comprehensive monitoring and logging service
- **EBS Volumes:** Persistent block storage for instances
- **Instance Types:** Various configurations optimized for different workloads
