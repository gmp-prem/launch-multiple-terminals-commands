# Launch multiple terminals with a specific commands
The tutorial for launch multiple terminals and commands right after the startup of Linux

To launch multiple terminals with commands at once when system startup, the idea is to use the bash script file to execute a specific commands. There are many ways to perform the launching terminal with a specific command automatically during the startup of the Linux system.

`This tutorial will guide through how to execute the ROS command.`

## Steps (Information)
1. Create the bash script files for a specific command, the number of bash script files will be 1 + (no. of specific program)
    - The first one is the bash script that will execute the rest of the program
    - The rest of the bash script will be the script that launch a specific program (in this case, the ROS node or a set of ROS nodes — so called the launch file)

2. After create the set of bash scripts, we will make the Linux to execute the bash script at the startup using the Startup Application
    - Open the startup application
    - Add new startup service
    - Type in the necessary information for launching the bash script, this includes
        - The name of startup service
        - the commmand (this is very important since it will run the bash script, if this command is wrong, the program you wish to start may not be executed)
    - Save the startup service and try logout or reboot the device.

### Step 1

1. Create the bash script that will execute the entire ROS nodes — I will call this bash script as `robot_bringup`. You can create the bash script by hidden file or normal, but this will be a hidden file
```
touch .robot_bringup
```

2. For example, if you want to execute 2 ROS launch files (typically, the roslaunch will initiate the ros master, but it is better to run ros master separately, so when the system is down, everything will not be blown up)
```
touch .start_rosmaster
```

Then, create the bash script for the specific roslaunch file. The command below will create two bash scripts
```
touch .ros_node1 .ros_node2
```

3. After all the bash scripts are created, make them executable using `chmod` command or change in the property of the file
```
sudo chmod +x .robot_bringup .start_rosmaster .ros_node1 .ros_node2
```

4. Check that all the created bash scripts' name are correct and executable using your `ls -al` command 😎

### Step 2

1. Put some code in the bash script, this is the example bash script from the `.robot_bringup` file
```
#!/bin/bash

gnome-terminal \
  --tab -e "bash -c '/home/user/.start_rosmaster; bash'" \
  --tab -e "bash -c 'sleep 1; /home/user/.ros_node1; bash'" \
  --tab -e "bash -c 'sleep 1; /home/user/.ros_node2; bash'" \
```

The first line `#!/bin/bash` is the shebang to make the created text file as a bash script, so whenever the customized bash script is created, this is the first essential code that must be put in the first line of that bash script.

The next command is the `gnome-terminal`, this command will open up the default terminal from linux (you can use others if you wish to!). The `--tab` is the argument that will open the terminal in new tab and execute the specific command. To execute the command, use the following `-e` argument and put your command inside the double-quotes. For example, the below code,

```
gnome-terminal --tab -e "bash -c '/home/user/.start_rosmaster; bash'" \
```
The `bash -c` will execute the bash script you create, the bash script file is in the single-quote (it is recommend to append the path to the file), after that, the `bash` at the end of the command will make the command stays when it is executed, otherwise if you dont place the bash at the end, the terminal will shutdown after the command is executed, this is important for ROS master, and nodes, since they needs to stay running and perform your task stuffs.

You can download the example files in this repository and put them in the home directory, and change the command in that file to a specific command for your ROS application.

2. After that, we will add the bash script `.robot_bringup` in the `Startup Application`, you can open the startup application in the linux menu application.

The first image will show you the looks of the startup application app

<p align="center">
  <img src="https://github.com/gmp-prem/launch-multiple-terminals-commands/blob/main/images/1.png" width="500" height="300">
</p>

Next, add the startup service and type in the neccessary information, the second picture here will show you the command that needs to be in.

<p align="center">
  <img src="https://github.com/gmp-prem/launch-multiple-terminals-commands/blob/main/images/2.png" width="500" height="300">
</p>

`Name` : name of your startup service
`Command` : the command to execute your bash script
`Comment` : the detail of the startup service

<p align="center">
  <img src="https://github.com/gmp-prem/launch-multiple-terminals-commands/blob/main/images/3.png" width="500" height="600">
</p>

The last image will show you the command to launch the `.robot_bringup`, so, you can just follow my commands

For the `Name`
```
Terminal
```

For the `Command`, in here, do not forget to change the name of the user !
```
gnome-terminal -e "bash -c '/home/user/.robot_bringup; bash'"
```

For the `Comment`
```
start the terminal and ROS master automatically
```
After that, save and close the startup application, try reboot or logout the device and see that your ROS application will start automatically when the device boots. 😎