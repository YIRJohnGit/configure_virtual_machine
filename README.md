# Troubleshooting of Oracle VM VirtualBox #

## TS:01 - Screen Resize Issue ##

**Step 1 - Run the Following Command**
```
sudo apt update && sudo apt upgrade -y
sudo apt install -y linux-headers-$(uname -r) build-essential dkms
```
_**Note:** If you get any error, re run the the above command_

**_Result:-_**
![image](https://user-images.githubusercontent.com/111234771/194798887-06fa75b3-0a8a-47a0-a5bd-34614186941f.png)

**Step 2 -Setting Up Additional cd Image**
![image](https://user-images.githubusercontent.com/111234771/194798972-1a90d727-485a-4771-b48c-e1d50c2512b1.png)

_If you are not able to see the Menu Option (Refer the above image) in the VB, Please press the right side button <ctrl+f> ***(^f)*** and follow on screen instruction_

  Step 2.1 - Go to <Devices> Menu => Select the Menu <Insert Guest Additional CD Image> (Refer the below Image)
    ![image](https://user-images.githubusercontent.com/111234771/194799121-218869db-788d-4313-9675-7323922c6359.png)

  Step 2.2 - Go to the Terminal and find the media drive
  ```
  df
  ```
  _Verify the Folder Structure_
  
  ![image](https://user-images.githubusercontent.com/111234771/198995283-46ea5212-ba26-46ce-bce9-63e1878a2652.png)

```
cd /media/ubuntu/VBox_GAs_6.1.40/
```
```
./autorun.sh
```
_**Note:** Please follow on screen instruction to complete the activity and restart the machine and enter the password when prompted_
    ![image](https://user-images.githubusercontent.com/111234771/194799459-c2521a83-1fe5-479e-a40e-9e62c47c4f60.png)


  _Once Completed you will get the screen as shown below image_
  ![image](https://user-images.githubusercontent.com/111234771/199002887-524c471a-cdad-4b57-bbb5-23a04da45847.png)
  ![image](https://user-images.githubusercontent.com/111234771/194807750-7421ab4e-b902-45c9-8191-37e288117711.png)

  ## TS:02 - Mouse Pointer Issue / Copy Paste Issues ##

  #### Step 1 - Make Sure you have set the Mouse to Bidirectional ####
  ![image](https://user-images.githubusercontent.com/111234771/194800357-2f231130-ca9a-45da-92a6-d6eae379306c.png)
  
  #### Step 2 - Make Sure you have set the Pointing Device to <USB Multi-Touch Tablet> ####
  ![image](https://user-images.githubusercontent.com/111234771/194800445-0f8443c8-c732-4ff1-a068-19350e2cd84a.png)
  
  #### Step 3 - Changes in the grub File ####
  ```
  sudo nano /etc/default/grub
  ```
  _add the below to the line in the file and save (Refer Below Image)_
  ```
  i8042.nomux i8024.noloop
  ```
  Change this line <this is default>
  ![image](https://user-images.githubusercontent.com/111234771/194801063-99d1174a-185d-43ca-9c26-3c5c9b51dd18.png)

  Make the change in the specified Line (Changes in Line No. 10)
 ![image](https://user-images.githubusercontent.com/111234771/209621957-88fa99fd-9748-4a9e-a98d-eecea50419ab.png)

***Or Inline Command***
```
sudo sed -i '/GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"/ s/"$/ i8042.nomux i8024.noloop"/' /etc/default/grub
sudo sed -i '/GRUB_CMDLINE_LINUX=""/d' /etc/default/grub
```
  
  ```
  sudo update-grub
  ```
  _Result_
    ![image](https://user-images.githubusercontent.com/111234771/194801191-0a4d7317-ea9a-4fd3-82fd-b255e8bca28e.png)
  ```
  sudo shutdown now	
  ```

  #### Changing View Resolution ####
  - The dafult (May be)
![image](https://github.com/user-attachments/assets/c59fdf65-3973-4b9a-849b-0816f3d06853)
  - Change the setting
![image](https://github.com/user-attachments/assets/39c8a2c9-8c8e-44ea-8a1f-861045e4e670)

  
  # Troubleshooting on Hostname Issue #
  
  ```
  hostnamectl
  sudo sysctl kernel.hostname=localhost # Change the transient hostname
  hostnamectl
  ```
  
  **OR**
```
  hostnamectl
  sudo hostnamectl set-hostname localhost
  hostname
  exec bash
```

**OR**
```
# Get the local IP address
local_ip=$(hostname -I | awk '{print $2}')

# Replace dot with dash in local IP
local_ip=${local_ip//./-}

# Set the hostname with the local IP address
sudo hostnamectl set-hostname "$local_ip"

# Update the /etc/hosts file with the new hostname
sudo cp /etc/hosts /etc/hosts_bak_$(date +"%Y%m%d-%H%M%S")
sudo sed -i "s/127.0.1.1.*/127.0.1.1\t$local_ip/g" /etc/hosts
hostname
exec bash
```
