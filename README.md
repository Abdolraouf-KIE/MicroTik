# MicroTik
### command line references: <br>
1. https://wiki.mikrotik.com/wiki/Manual:Console <br>
2. https://wiki.mikrotik.com/wiki/Manual:Interface/Lora#Channels <br>
3. https://help.mikrotik.com/docs/display/ROS/General+Properties <br>
4. https://help.mikrotik.com/docs/display/ROS/Environment <br>
5. https://wiki.mikrotik.com/wiki/Manual:System/SSH_client <br>
6. https://wiki.mikrotik.com/wiki/Manual:IP/SSH <br>
7. https://lora-developers.semtech.com/library/tech-papers-and-guides/lora-and-lorawan/ <br>

### Setup instructions: <br>
![alt text](https://github.com/[username]/[reponame]/blob/[branch]/image.jpg?raw=true)
Lora module manule: https://mikrotik.com/product/r11e_lr9 <br>

![alt text](https://github.com/[username]/[reponame]/blob/[branch]/image.jpg?raw=true)
wAP LR9 kit: https://mikrotik.com/product/wap_lr9_kit#fndtn-downloads <br>


# Quickstart 
The quick setup below is according to https://help.mikrotik.com/docs/display/UM/wAP+LR9+kit. Additional notes will be added after '#'. <br> <br>

1. Make sure your ISP is allowing hardware change and will issue an automatic IP address. #This is to allow the router to be connected to internet. forget this for now, further information will be given at step 12
2. Open the bottom lid. 
3. Connect an external antenna to the SMA connector. #alternativly you can use the PCB antenna by the sid of the box
4. Connect the device to the power source. 
5. Connect with your device to the MikroTik wireless network #for me it took a while for the wifi ssid to to showup, so after power up give it few minutes. Standard ssid of wifi is "MicroTkiXXX".
6. The configuration has to be done through the wireless network using a web browser or mobile app. Alternatively, you can use the WinBox configuration tool https://mt.lv/winbox. By default, Ethernet port access is blocked by a firewall. #to configure i used the web brower, but the app provides a more intuitive and faster quick setup-for full configuration DONT use mobile.
7. Once connected to the wireless network, open https://192.168.88.1 in your web browser to start the configuration. #The URL didnt work for me and instead "192.168.88.1" worked for me.
8. user name: admin and there is no password by default. #by defualt this credential is automaticlly keyed and you simply enter the front page
9. When using a mobile application choose Quick setup and it will guide you through all necessary configuration in six easy steps. #Easy and quick setup with mobile, but it has limited ability (i didnt fully explore though)
10. Find your LR Gateway ID on the label within the product and register it in your Network Server. #the label is at the back of kit given by "GW ID: 323XXXXXXXX"
11. To make the device connect to the LR Network Server. #To do this go to configure section after finishing this section
12. Click the "Check for updates" button and update your RouterOS software to the latest version, the device needs to have an active Internet connection. #To allow the router to have internet, connet an ethernet cable to it. If router has internet the wifi provided by microtik also has internet.
13. After update set your country, to apply country regulation settings. #This is just for wifi regulations
14. Set your WiFi password. #Only important for security
15. Set the router password. #Only important for security
<br>

# configuration(basic)
This section refers to the configuration section from https://help.mikrotik.com/docs/display/UM/wAP+LR9+kit#heading-Configuration <br> <br>
To set the configuration for LR please connect to the device and log in with your web browser or use a mobile application. Two easy steps to follow:<br>

### First_step:

1. Once logged in, Quick Set will be selected, please switch to WebFig on the right side of the screen. If the configuration is done through mobile application then click on the gear symbol on the right side of the screen to open up an advanced menu.
2. On the left side menu please find and select the section "Lora".
3. On the newly opened window select the Servers tab.
4. Click + to add a new server configuration.
5. A new window will appear and you will have to enter:
* Name: (Server name)
* Address: (Server address)
* Up port: (Usually it's 1700)
* Down port: (Usually it's 1700)
6. Click OK to save.

### Second step:
1. Select the Device tab on the previous window.
2. Double-click or tap on the line to configure.
3. Choose the previously entered network on the drop-down menu.
4. Click on the button Enable to enable the gateway.
5. Select the frequency band (since Malaysia uses AS1 and there is no option for this we must manually set this using the console. refere to "Advance Band Configuration"
6. Click OK to save.
7. The configuration is done.

# Advance Band Configuration (for AS1 2020)
