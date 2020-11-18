# MicroTik
<img src="https://github.com/Abdolraouf-KIE/MicroTik/blob/main/imgs/kit.png" alt="MicroTik Lora Kit" width="300" height="300">
<img src="https://github.com/Abdolraouf-KIE/MicroTik/blob/main/imgs/loramodule.jpg" alt="MicroTik Lora Module" width="300" height="300">

## Quickstart 
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
10. Find your LR Gateway ID on the label within the product and register it in your Network Server. #the label is at the back of kit given by "GW ID: 323XXXXXXXX" and this has to be added to the cloud.loranet.my for gateway information.
11. To make the device connect to the LR Network Server. #To do this go to configure section after finishing this section
12. Click the "Check for updates" button and update your RouterOS software to the latest version, the device needs to have an active Internet connection. #To allow the router to have internet, connet an ethernet cable to it. If router has internet the wifi provided by microtik also has internet.
13. After update set your country, to apply country regulation settings. #This is just for wifi regulations
14. Set your WiFi password. #Only important for security
15. Set the router password. #Only important for security
<br>

## configuration(basic)
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
5. Select the frequency band #since Malaysia uses AS1 and there is no option for this we must manually set this using the console. refere to "Advance Frequency Plan Configuration"
6. Click OK to save.
7. The configuration is done.

## Advance Frequency Plan Configuration (for AS1 2020)
This section refers to https://help.mikrotik.com/docs/display/ROS/General+Properties. For guidlines on how to use commands refer to https://wiki.mikrotik.com/wiki/Manual:Console. <br>

Before sending out commands we must setup ssh to the router and send command using PC termianl instead of web based terminal <br>
### Setting up ssh with MicroTik
1. Connect to the Microtik wifi
2. go to the router IP using browser-192.168.88.1 
3. click on WebFig tab located at top right of webpage.
4. go to "System" and then "Users" found in the tab menue to the right
5. Here you can add new user and set it's password.
6. After setting up a user you can ssh as that user with it's password from the PC connected to Microtik wifi. for example i use the command below for user "loranet":<br>
<code> ssh loranet@192.168.88.1 </code>

### Configuring Frequency Plan
To do this we need to change the radio specifications to our desired specs which is found in the [excel file!](https://github.com/Abdolraouf-KIE/MicroTik/blob/main/LORA%20Frequency%20Plan%202019%262020.xlsx).
1. First we need to go to the "Lora" layer by command below:<br>
<code>lora</code>
2. To see the list of commands in this layer use the code below:
<code>?</code>
This will print out all the commmands available, to know more about these commands go to https://wiki.mikrotik.com/wiki/Manual:Console. 
3. We would like to see list of parameters so then we can change them. to see list of parameters use: <br>
<code>print</code>
<br>e.g. output: <br>
<pre><code>Flags: X - disabled 
 0   name="gateway-0" status="Enabled" hardware-id="323531321C002F00" 
     gateway-id="323531321C002F00" firmware-id="5c00745" servers=loranet 
     channel-plan=as-923 antenna-gain=0dB forward=crc-valid,crc-error 
     network=private lbt-enabled=no listen-time=5000us rssi-threshold=-65dBm 
     band=902-928 locks=""</pre></code>
<br> 
4. We would like to change the parameter "channel-plan" and set it to "custom". <br>
<code>set channel-plan=custom</code><br>
5. Select "Numbers" as 0 since the parameter is at columb 0-refer to preveious output.<br>
<code>Numebrs: 0</code> <br>
* to see full list of values possible for a parameter you can double tab or use "?" after typing "set channel-plan=" <br>
6. for AS1 as of 2020 we need to obtain the output below when we print paramters at "lora/channels" layer
<pre><code>[loranet@MikroTik] /lora channels> print
Flags: X - disabled
 #   NAME       TYPE RADIO    FREQ-OFF BANDW...   FREQ SP..   DATARATE
 0   gateway-0  MSF  radio0     200000 125_kHz   923.2
 1   gateway-0  MSF  radio0     400000 125_kHz   923.4
 2   gateway-0  MSF  radio1     200000 125_kHz   922.2
 3   gateway-0  MSF  radio1     400000 125_kHz   922.4
 4   gateway-0  MSF  radio0    -400000 125_kHz   922.6
 5   gateway-0  MSF  radio0    -200000 125_kHz   922.8
 6   gateway-0  MSF  radio0          0 125_kHz     923
 7   gateway-0  MSF  radio1          0 125_kHz     922
 8   gateway-0  LoRa radio1     100000 250_kHz   922.1 SF7
 9   gateway-0  FSK  radio1    -200000 125_kHz   921.8           50000
</pre></code>
7. To do this first you need to go to "lora/radios" layer and set the centeral frequency as given in excel after which set the radio, frequency-off, and bandwidth. you can set pramter of multiple columb by:<br>
<code>set 0,1,2,3,4,5,6,7 bandwidth=125</code> <br>

## References 
### command line references: <br>
1. https://wiki.mikrotik.com/wiki/Manual:Console <br>
2. https://wiki.mikrotik.com/wiki/Manual:Interface/Lora#Channels <br>
3. https://help.mikrotik.com/docs/display/ROS/General+Properties <br>
4. https://help.mikrotik.com/docs/display/ROS/Environment <br>
5. https://wiki.mikrotik.com/wiki/Manual:System/SSH_client <br>
6. https://wiki.mikrotik.com/wiki/Manual:IP/SSH <br>
7. https://lora-developers.semtech.com/library/tech-papers-and-guides/lora-and-lorawan/ <br>

### Setup instructions references: <br>
Lora module manule: https://mikrotik.com/product/r11e_lr9 <br>
wAP LR9 kit: https://mikrotik.com/product/wap_lr9_kit#fndtn-downloads <br>

