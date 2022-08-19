```
#!/bin/sh

source /opt/ros/noetic/setup.bash

USERNAME="cona"
PASSWARD="cona"
PC_IP="192.168.2.2"
BungP_path="/home/cona/git/BungP_v2/"
CoNA_Navi_path="/home/cona/CoNA_Navi/"
CoNA_install_path="/home/cona/CoNA_Navi/install/lib/cona/"

echo  "\033[30;41m"**************************************************************"\033[0m"
echo  "\033[30;41m"**************************************************************"\033[0m"
echo  "\033[41m"*****************Welcome Coga-Robotics World******************"\033[0m"
echo  "\033[30;41m"**************************************************************"\033[0m"
echo  "\033[30;41m"**************************************************************"\033[0m"
echo  "********maintainer email=ytam1014@coga-robotics.com**********"
echo "\n"

echo  "*************************[Mode table]*************************"
echo  "*                  [1]Connect SSH vinggo                     *"
echo  "*          [2]Connect SSH vinggo and cona_all start          *"
echo  "*          [3]Connect SSH vinggo and vinggo_GUI start        *"
echo  "*             [4]PC BungP_v2 build and SCP vinggo            *"
echo  "**************************************************************"
echo  "-------------------------------------------------------------"
echo  "-                     Mode Definition                       -"
echo  "-------------------------------------------------------------"
echo  "|[1] PC to Servinggo ssh connect                            |"
echo  "|[2] PC to Servinggo ssh connect & roslaunch cona_all       |"
echo  "|[3] PC to Servinggo ssh connect & Servinggo_GUI_start      |"
echo  "|[4] PC BungP_v2 cmake install and SCP lib file to vinggo   |"
echo  "-------------------------------------------------------------"


echo "Enter the Mode(q: quit)"

SSH_MODE="1"
read SSH_MODE

echo  "\033[46m"User name: $USERNAME"\033[0m"
echo  "\033[46m"IP: $PC_IP"\033[0m"
echo  "SSH_MODE = $SSH_MODE"
today=$(date)
echo  "\033[45m"$today"\033[0m"

MODE=$SSH_MODE

if [ $MODE = "1" ] ;
then
        sshpass -p $PASSWARD ssh $USERNAME@$PC_IP -XC
elif [ $MODE = "2" ] ;
then
        sshpass -p $PASSWARD ssh $USERNAME@$PC_IP -XC '
                source /opt/ros/noetic/setup.bash

                export ROS_MASTER_URI=http://192.168.2.2:11311

                export ROS_IP=192.168.2.2

                source /home/cona/CoNA_Navi/install/setup.bash

                roslaunch cona cona_all.launch
        '
elif [ $MODE = "3" ] ;
then
        sshpass -p $PASSWARD ssh $USERNAME@$PC_IP -XC '
                export ROS_MASTER_URI=http://192.168.2.2:11311/
                export ROS_IP=192.168.2.2

                source /opt/ros/noetic/setup.bash
                source /home/cona/CoNA_Navi/install/setup.bash

                cd /home/cona/Servinggo
                npm start
        '
elif [ $MODE = "4" ] ;
then
        echo "System environment variable initalize...."

        echo "----------------Done--------------------!"
        echo "###########Build BungP_v2################"
        cd ~
        cd $BungP_path
        catkin_make -j4 && catkin_make install

        echo "############Install Navi##################"
        cd ~
        cd $CoNA_Navi_path
        catkin_make install
        cd $CoNA_install_path
        echo "----------------Done--------------------!"
        echo "SCP file pss..."
        sshpass -pcona scp -o StrictHostKeyChecking=no cona_node $USERNAME@$PC_IP:$CoNA_install_path

        echo "-------------Process time---------------!"
        today=$(date)
        echo $today

elif [ $MODE = "q" ] ;
then
        echo Out!
else
        echo "What?"
        sshpass -p $PASSWARD ssh $USERNAME@$PC_IP -XC
fi
```

