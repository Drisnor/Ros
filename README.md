# TP robotique mobile : 
http://wiki.ros.org/fr/ROS/Tutorials/UnderstandingTopics
# Tuto install local Ubuntu 18.04 : 
# http://wiki.ros.org/melodic/Installation/Ubuntu
# + sudo apt install ros-melodic-rospy
# + sudo apt install ros-melodic-roslaunch
# + sudo apt install ros-melodic-turtlesim
# + sudo apt install ros-melodic-rosbash

Chemin local AIP => ~ = /home/etudiant
Chemin docker => root 

env | grep ROS
gedit ~/.bashrc
=> Add : source /opt/ros/melodic/setup.bash  
=> Redémarrer un terminal

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
export ROS_PACKAGE_PATH=/opt/ros/melodic/share:$HOME/catkin_ws/src  # TODO check $HOME en local AIP
# ou export ROS_PACKAGE_PATH=$ROS_PACKAGE_PATH:/home/etudiant/catkin_ws/src
rosdep update

roscd beginner_tutorials
# Compilation à la racine (dans le workspace catkin_ws !)
catkin_make
catkin_make install

# TODO ADD : source /root/catkin_ws/devel/setup.bash dans .bashrc
#                   ou /home/etudiant/...
roscore
# Dans un second terminal
rosrun turtlesim turtlesim_node

# Ouvrir 2 turtle :
rosrun turtlesim turtlesim_node __name:=turtle
rosrun turtlesim turtlesim_node __name:=turtle2

# Contrôle des 2 tortues avec les flèches
rosrun turtlesim turtle_teleop_key

# Affichage du graphe
rosrun rqt_graph rqt_graph
# Tout suppr et add /turtle1/pose, puis enlever les vitesses linéaire et angulaire


rostopic pub /turtle1/cmd_vel * tab * * tab * pour complétion
# Donne => rostopic pub /turtle1/cmd_vel geometry_msgs/Twist "linear:
  x: 0.0
  y: 0.0
  z: 0.0
angular:
  x: 0.0
  y: 0.0
  z: 0.0
  
# On ajoute des vitesses : 
# Position sur la grille en (x,y,z)
  x: 0.0
  y: 1000.0
  z: 0.0
# Vitesse angulaire
angular:
  x: 0.0
  y: 0.0
  z: 1000.5" -r 1 

# Console
rosrun rqt_console rqt_console

# Contrôle de l'affichage console : 
rosrun rqt_logger_level rqt_logger_level
# Puis choisir les infos que l'on veut : Info, Debug, Error only ...

roscd beginner_tutorials
mkdir launch
# Copier le fichier xml du poly dans launch1.txt
roslaunch beginner_tutorials launch1.txt

# Rosbag : Enregistre une séquence d'action teleop_key
rosbag record -a
rosbag play 2020-12-02-09-44-50.bag # nom du fic créé au même endroit

# Subset sur ce qui nous intéresse
rosbag record -O subset /turtlesim1/turtle1/cmd_vel
rosbag play subset.bag

# Rosservice
rosservice list
rosservice call /turtlesim1/spawn *tab* *tab* # complétion

# Ajoute une tortue
rosservice call /turtlesim1/spawn "x: 7.0
y: 7.0
theta: 0.0
name: ''" 

rosservice call /turtlesim1/spawn "x: 7.0
y: 7.0
theta: 0.0
name: 'my_turtle'" 

# Déplacement
rosservice call /turtlesim1/my_turtle/teleport_relative "linear: 0.0
angular: 0.0" 

# Changer la couleur de la traînée 
rosservice call /turtlesim1/my_turtle/set_pen "{r: 255, g: 0, b: 0, width: 0, 'off': 0}" 

# Rosparam
rosparam list

rosparam get turtlesim1/sim
{background_b: 255, background_g: 86, background_r: 69}

rosparam set turtlesim1/sim/background_r 255 
rosparam get turtlesim1/sim {background_b: 255, background_g: 86, background_r: 255}
# Il faut mettre à jour pour prendre en compte les modifs
rosservice call /turtlesim1/clear
# On peut charger ces instructions avec un fichier YAML

# Création de messages et services
Config suivant le poly...

# Puis recompiler
catkin_make

# Rosmsg
rosmsg show beginner_tutorials/Num
rosmsg show *tab*

# Services
# sudo apt install ros-melodic-rospy-tutorials
mkdir srv
roscp rospy_tutorials AddTwoInts.srv ./srv

# Dans catkin_ws
mkdir scripts
http://wiki.ros.org/ROS/Tutorials/WritingPublisherSubscriber%28python%29

roscd beginner_tutorials/scripts/
wget https://raw.github.com/ros/ros_tutorials/kinetic-devel/rospy_tutorials/001_talker_listener/talker.py
chmod +x talker.py

roscd beginner_tutorials/scripts/
wget https://raw.github.com/ros/ros_tutorials/kinetic-devel/rospy_tutorials/001_talker_listener/listener.py
chmod +x listener.py

# Lancement des services
roscore
rosrun beginner_tutorials listener.py
rosrun beginner_tutorials talker.py

..... TUTO du wiki dessus
