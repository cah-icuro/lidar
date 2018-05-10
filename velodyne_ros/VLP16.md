# Velodyne on ROS #

Using the [Velodyne driver](https://github.com/ros-drivers/velodyne) by Josh Whitley from AutonomouStuff

## Basic instructions - VLP16 ##

The VLP-16's default IP address is 192.168.1.201

Connect power to interface box, ethernet from computer to interface box.

Check if you can connect to it by entering this IP address in a web browser's URL field.

If you cannot yet connect, follow these steps:
 - Disable WiFi
 - Open "Network Connections"
   - Select connection & click "edit" (and rename it to velodyne_connection or something similar if you desire)
   - Go to "IPV4 Settings"
   - Change Method to "Manual"
   - Click "Add"
     - Set IP Address to 192.168.1.X, where X is in the range [1, 254] and not 201
     - Set Netmask to 255.255.255.0
     - Set Gateway to 0.0.0.0
     - Click "Save"
 - Statically assign an IP in the 192.168.3._ range (?) (I forget if this actually worked, or I used 1 instead of 3)
```bash
sudo ifconfig eth0 192.168.3.X
````
 - Add a static route
 ```bash
 sudo route add 192.168.1.201 eth0
 ```
 
 Once you can view the browser interface at 192.168.1.201, proceed to setting up ROS drivers.  Replace 'kinetic' with your distribution if you are not using ros-kinetic.
 
 ```bash
 sudo apt-get install ros-kinetic-velodyne
 cd your_workspace_dir/src
 git clone https://github.com/ros-drivers/velodyne.git
 cd ..
 rosdep install --from-paths src --ignore-src --rosdistro kinetic -y
 catkin_make
 ```
 
 If you're getting errors above, try creating a fresh workspace just for this package.
 
 To run, from within your workspace:
 ```bash
 roslaunch velodyne_pointcloud VLP16_points.launch
 # verify pointcloud is being published to velodyne_points
 rostopic echo velodyne_points
 ```
 
