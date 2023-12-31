# CMPE-283-Virtual Technologies_Assignment-1

## Assignment 1 - Discovering VMX Features

Student names: <a href="https://www.linkedin.com/in/upasana-kumar/"> Upasana Kumar</a>, <a href="https://www.linkedin.com/in/shireesh-vennamaneni-9b906914a/">Shireesh Vennamaneni</a>

University Name: <a href="https://www.sjsu.edu/">San Jose State University</a>

Prerequisites:-
You require a machine that supports Linux and has VMX virtualization features available. Depending on your hardware and software setup, it might be possible to run this within a virtual machine, but this is not guaranteed.

Assignment Details:-
We reviewed the Intel SDM supplied by our professor and the lecture related to this assignment.
Our goal is to identify specific features on our GCP VM.

## Steps to run the code

Step-1. Clone this repository  
Step-2. Setup the VM and run the code using the steps mentioned in the answer to Question 2 in the Questions section below.  

Pushed the code to the forked linux repository: https://github.com/shireesh20/linux

## Questions:-  

### 1. For each member in your team, provide 1 paragraph detailing what parts of the lab that member implemented/researched

&emsp; Shireesh was responsible for configuring the Google Cloud Platform virtual machine, ensuring the correct settings for nested virtualization were enabled, a foundational step for the project. After setting up the environment, he created a dedicated project directory and proceeded to download the essential cmpe283-1.c source file and Makefile from Canvas. He tackled the implementation of primary and secondary MSR controls within the cmpe283.c file. This task required careful adherence to the professor's instructional video and a deep understanding of the Intel SDM volume 3 documentation. His work was critical in laying the groundwork for the kernel module development, ensuring the initial codebase was robust and well-structured.

&emsp; Upasana focused on the latter stages of the assignment, building upon the groundwork established by Shireesh. Her primary responsibility was to enhance the kernel module by adding functionality for querying exit, entry, and tertiary-based MSR controls. This task required a comprehensive understanding of the Intel SDM volume 3 and the ability to interpret and apply complex technical guidelines into practical code, as detailed in the professor's video lecture. She  managed the compilation process, resulting in the creation of .o and .ko files, and played a key role in testing the module through the use of insmod and rmmod tools. Used the dmesg command to review system log messages, was crucial in verifying the successful implementation and functionality of the kernel module.

### 2. Describe in detail the steps you used to complete the assignment 


Step-1. Create a GCP account. You'll get 300$ credit if you signup with student account  

Step-2. Create a project in GCP and create a VM in the project using the command  

```
gcloud compute instances create cmpe283-vm-us --enable-nested-virtualization \
--zone=us-central1-a --machine-type=n2-standard-8 --network-interface=network-tier=PREMIUM,subnet=default \
--create-disk=auto-delete=yes,boot=yes,device-name=instance-1,image=projects/ubuntu-os-cloud/global/images/ubuntu-2004-focal-v20220204,mode=rw,size=200,type=projects/cmpe-283-f23/zones/us-central1-a/diskTypes/pd-ssd \
--metadata=ssh-keys="<key>"
```

Step-3. Add the ssh keys to the VM  

Step-4. Verify if nested virtualization is enabled or not using the command  

   ```cat /proc/cpuinfo | grep vmx```  
   
   This should return vmx flags  
   
Step-5. Run the following commands to install requirements  

```
	sudo apt-get update
        sudo apt-get install bison
        sudo apt-get install make
        sudo apt-get install flex
        sudo apt-get install gcc
        sudo apt-get install libssl-dev
        sudo apt-get install libelf-dev
```

Step-6. Fork the linux repo : ```https://github.com/torvalds/linux``` and clone it into your vm  

Step-7. ```cp /boot/config-5.11.0-1029-gcp ~/linux/.config```  

Step-8. Create a directory cmpe-283 inside the cloned repo  

Step-9. Copy the files(cmpe283-1.c & Makefile) provided on canvas to the directory  

Step-10. Run the command ```make```. If it errors with invalid kernel configuration, then run ```make oldconfig```. Keep pressing enter to finish setting different options to default value.  
 
Step-11. Try to run make again. For us, make was successful without any error after configuration setting even though our kernel version was 5.11.0. However, we installed the kernel later during next assignments. 

Step-12. In case, make fails again. This is because of version mismatch. In such case, run:  

```
	make clean

	make -j 8 modules

	make -j 8

	sudo make -j 8 INSTALL_MOD_STRIP=1 modules_install

	sudo make -j 4 install

	sudo reboot
```

Step-13. Run ```uname -a``` to verify the version again to match the version of linux cloned from git  

<img width="462" alt="uname" src="https://github.com/upasanakr/CMPE-283-VirtualTechnologies_Assignment-1/assets/37268397/bf5d4a63-817e-4fd6-a218-8bb282eb0132">  

(This is done during the second assignment as make was successful for us during the first assignment)

Step-14. Run make to verify if it is successful  

Step-15. Run ```sudo insmod cmpe281-1.ko```  

Step-16. Run ```dmesg```<br />
	You can now see the output of the code given by the professor. Modify the C file accordingly, then run sudo rmmod cmpe283-1 and continue from make command.  



 ## Output Screenshots

1. Pinbased (provided by professor Mike as codebase)

<img width="644" alt="image" src="https://github.com/upasanakr/CMPE-283-VirtualTechnologies_Assignment-1/assets/144417727/a5b8fdc2-0bcb-428b-a2ba-eb209090e9e0">

2. Primary ProcBased Controls

<img width="719" alt="image" src="https://github.com/upasanakr/CMPE-283-VirtualTechnologies_Assignment-1/assets/144417727/d2f79b38-e127-41b6-aec6-730f7b9d528c">


3. Secondary ProcBased Controls

<img width="719" alt="image" src="https://github.com/upasanakr/CMPE-283-VirtualTechnologies_Assignment-1/assets/144417727/4dfe6364-2c7a-42bb-96bc-de12954585e5">



4. Exit Controls

<img width="719" alt="image" src="https://github.com/upasanakr/CMPE-283-VirtualTechnologies_Assignment-1/assets/144417727/cd7238e5-587f-4757-a9b5-569d4adc997d">


5. Entry Controls and Tertiary

<img width="719" alt="image" src="https://github.com/upasanakr/CMPE-283-VirtualTechnologies_Assignment-1/assets/144417727/fb9bdf3f-2d00-4768-9886-f3674f8b45d9">









