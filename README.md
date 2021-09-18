# MEAN Stack Deployment to Ubuntu in AWS

**MEAN** Stack is a combination of following components:
1. MongoDB (Document database) - Stores and allows to retrieve data.
2. Express (Back-end application framework) - Makes requests to Database for Reads and Writes.
3. Angular (Front-end application framework) - Handles Client and Server Requests
4. Node.js (JavaScript runtime environment) - Accepts requests and displays results to end user

# **Step 0 - Preparing prerequisites**

In order to complete this project you will need an AWS account and a virtual server with Ubuntu Server OS.

If you do not have an AWS account - go back to **[Project 1 Step 0**](https://www.notion.so/PROJECT-1-WEB-STACK-IMPLEMENTATION-6bdce943581e4743a5f1a65b331e567f)** to sign in to AWS free tier account ans create a new EC2 Instance of t2.nano family with Ubuntu Server 20.04 LTS (HVM) image. Remember, you can have multiple EC2 instances, but make sure you **STOP** the ones you are not working with at the moment to save available free hours.

# **Task**

In this assignment you are going to implement a simple Book Register web form using MEAN stack.

# **Step 1: Install NodeJs**

`Node.js` is a JavaScript runtime built on Chrome’s V8 JavaScript engine. `Node.js` is used in this tutorial to set up the Express routes and AngularJS controllers.

Update ubuntu

`sudo apt update`

Upgrade ubuntu

`sudo apt upgrade`

Install NodeJS

`sudo apt install -y nodejs`