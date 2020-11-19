# Ros local à l'AIP
Chemin local  => ~ = /home/etudiant
Chemin docker => root 

env | grep ROS
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
# Vérif des dépendances : 
rospack depends1 beginner_tutorials

# Modif var env :
echo $ROS_PACKAGE_PATH
export ROS_PACKAGE_PATH=/opt/ros/melodic/share:/home/etudiant/catkin_ws/src
# ou export ROS_PACKAGE_PATH=$ROS_PACKAGE_PATH:/home/etudiant/catkin_ws/src
rosdep update

roscd beginner_tutorials
# Compilation à la racine (dans le workspace catkin_ws)
catkin_make
catkin_make install


