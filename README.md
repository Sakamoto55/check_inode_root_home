# check_inodes_root_home

#!/bin/bash
# All Rights Reserved.
# $Author: Steven Sakamoto<steven@care2team.com>
# Nagios plugin script for OPS team checking inode percentage of home and root directories.

# Nagios return codes
STATE_OK=0
STATE_WARNING=1
STATE_CRITICAL=2
STATE_UNKNOWN=3

root_inode=$(df -i|xargs -n1|grep -xB1 '/'|head -1|tr -d %)
root_inode_status_info=$(df -i|xargs -n1|grep -xB1 '/'|tail -1)
home_inode=$(df -i|xargs -n1|grep -B1 "/home"|head -1|tr -d %)
home_inode_status_info=$(df -i|xargs -n1|grep -B1 "/home"|tail -1)
check_for_home_directory=$(df -i|xargs -n1|grep -B1 "/home"|tail -1)

#this if statement checks to see if there is a home directory. If so, it will check the inode status of both the root and home directory.
	if [ "$check_for_home_directory" == "/home" ]

	then

	        if [ "$root_inode" -le 84 ] && [ "$home_inode" -le 84 ]

      	        then

		echo inode% OK
      	        echo $root_inode"% "$root_inode_status_info
                echo $home_inode"% "$home_inode_status_info

                exit 0

       	        elif [ "$root_inode" -ge 85 ] && [ "$root_inode" -le 94 ] || [ "$home_inode" -ge 85 ] && [ "$home_inode" -le 94 ] && [ "$root_inode" -le 94 ] && [ "$home_inode" -le 94 ]

                then

                echo inode% WARNING
        	echo $root_inode"% "$root_inode_status_info
                echo $home_inode"% "$home_inode_status_info

                exit 1

                elif [ "$root_inode" -ge 95 ] || [ "$home_inode" -ge 95 ]

                then

                echo inode% CRITICAL
	        echo $root_inode"% "$root_inode_status_info
                echo $home_inode"% "$home_inode_status_info

                exit 2

                else 

	        exit 3

	        fi

	else
#If there is no home directory, it will only check the inode status of the root directory.
	        if [ "$root_inode" -le 84 ]

	        then

	        echo inode% OK
	        echo $root_inode"% "$root_inode_status_info

	        exit 0

          	elif [ "$root_inode" -ge 85 ] && [ "$root_inode" -le 94 ]

	        then

	        echo inode% WARNING
	        echo $root_inode"% "$root_inode_status_info

	        exit 1

	        elif [ "$root_inode" -ge 95 ]

	        then

	        echo inode% CRITICAL
	        echo $root_inode"% "$root_inode_status_info

	        exit 2
	
	        else

	        exit 3

	        fi
	
	fi
