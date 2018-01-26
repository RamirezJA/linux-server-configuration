# linux-server-configuration
Secure linux server on AWS

Your README.md file should include all of the following:
i. The IP address and SSH port so your server can be accessed by the reviewer.
ii. The complete URL to your hosted web application.
iii. A summary of software you installed and configuration changes made.
iv. A list of any third-party resources you made use of to complete this project.

Locate the SSH key you created for the grader user.

During the submission process, paste the contents of the grader user's SSH key into the "Notes to Reviewer" field

# Server Details

# Configuration changes

# Steps
1. Are the applications up-to-date?
sudo apt-get update - latest infor stored in repos
sudo apt-get upgrade - upgrades installed packages
All system packages have been updated to most recent versions.
sudo apt-get install finger
sudo adduser grader
finger grader to see if created
2. Added user grader as sudo
3. Set up SSH keys
