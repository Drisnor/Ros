# Ros local à l'AIP
Chemin local  => ~ = /home/etudiant
Chemin docker => root 

gedit ~/.bashrc
=> Add : source /opt/ros/melodic/setup.bash  
=> Redémarré un terminal

mkdir -p ~/catkin_ws/src
cd ~/catkin_ws/src
catkin_init_workspace

# On a pas encore le dossier devel pour cette ligne : source $HOME/catkin_ws/devel/setup.bash

rospack find roscpp
roscd roscpp
roscd log
rosls roscpp_tutorials

# Création d'un package
cd ~/catkin_ws/src
catkin_create_pkg beginner_tutorials std_msgs rospy roscpp

