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