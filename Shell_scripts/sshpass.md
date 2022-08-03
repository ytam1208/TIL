#!/bin/bash

```
1. Bash environment init
2. ros catkin_make & make install
3. SCP file throw A to B
```

```
echo "System environment variable initalize...."
source ~/.bashrc
echo "----------------Done--------------------!"
echo "###########Build BungP_v2################"
cd ~/BungP_v2
catkin_make
catkin_make install

echo "############Install Navi##################"
cd ~/CoNA_Navi
catkin_make install
cd ~/CoNA_Navi/install/lib/cona
echo "----------------Done--------------------!"

echo "SCP file pss..."
sshpass -pcona scp -o StrictHostKeyChecking=no file_name passwd@192.168.2.2:~/output_file_path

echo "-------------Process time---------------!"
today=$(date)
echo $today
```