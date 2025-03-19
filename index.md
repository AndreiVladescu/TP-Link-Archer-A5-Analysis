
<link  rel="shortcut icon"  type="image/x-icon"  href="icon.ico">

  
  

<figure>  <img

  

src="https://raw.githubusercontent.com/AndreiVladescu/TP-Link-Archer-A5-Analysis/refs/heads/main/images/banner.jpeg"

  

alt="Exploiting the router"

  

>  </figure>

  
  

# Introduction

The TP-Link Archer A5 is a cheap router from 2020. According to [OpenWrt Wiki](https://openwrt.org/toh/tp-link/archer_a5_v5), it is identical to the TP-Link C50 v5.

  

<img  src="https://raw.githubusercontent.com/AndreiVladescu/TP-Link-Archer-A5-Analysis/refs/heads/main/images/case.jpeg"  width="60%">

  

# External Photos

  The exterior reveals basic stuff, the MAC of the router, default SSID and password, as well as the version of the router, which is v4.2.
  
  <img  src="https://raw.githubusercontent.com/AndreiVladescu/TP-Link-Archer-A5-Analysis/refs/heads/main/images/backcase.jpeg"  width="70%">
  

# Internal Photos

External photos are nice, but beauty comes from within.

The top of the board reveals the main SoC Mediatek MT7628AN, the ESMT M14D5121632A-2.5BBG2A 400MHz RAM IC, and the secondary Mediatek MT7612EN.

The secondary IC handles the 5GHz band, while the primary SoC handles the 2.4GHz band.

<img  src="https://raw.githubusercontent.com/AndreiVladescu/TP-Link-Archer-A5-Analysis/refs/heads/main/images/frontboard.jpeg"  width="60%">
	
The router came without the EMFI shields soldered on the PCB.

<div style="display: flex; flex-direction: row; align-items: center;">
  <img src="https://raw.githubusercontent.com/AndreiVladescu/TP-Link-Archer-A5-Analysis/refs/heads/main/images/ram_and_cpu.jpeg" width="40%" style="margin-right: 50px;">
  <img src="https://raw.githubusercontent.com/AndreiVladescu/TP-Link-Archer-A5-Analysis/refs/heads/main/images/secondary_mediatek.jpeg" width="28%">
</div>
  
 The backside of the board reveals an 8MB eFeon QH64A flash IC.

<img  src="https://raw.githubusercontent.com/AndreiVladescu/TP-Link-Archer-A5-Analysis/refs/heads/main/images/backboard.jpeg"  width="50%">

  <img  src="https://raw.githubusercontent.com/AndreiVladescu/TP-Link-Archer-A5-Analysis/refs/heads/main/images/flash.jpeg"  width="20%">



# Analyzing the firmware

Luckily, the firmware is not encrypted, and can be obtained easily with a Flash reader. Using `binwalk`, a typical TP-Link `squash-fs` partition can be found.
The `passwd.bak` file is an interesting file, since it contains the users and passwords for this embedded system.

`admin:$1$$iC.dUsGpxNNJGeOm1dFio/:0:0:root:/:/bin/sh`
`dropbear:x:500:500:dropbear:/var/dropbear:/bin/sh`
`nobody:*:0:0:nobody:/:/bin/sh`

Cracking the MD5 hash of the `admin` user's password reveals `1234`.

While the password can be easily revealed, there are 2 encrypted xml files inside the `/etc` folder, the `reduced_data_model.xml`  and `default_config.xml` files. When searching for the names of these files for binaries that use them, `libcmm.so` comes up. 

<img  src="https://raw.githubusercontent.com/AndreiVladescu/TP-Link-Archer-A5-Analysis/refs/heads/main/images/libcmm_dm_decryptFile.png"  width="60%">

Analyzing this library using Ghidra, we find a function `dm_decryptFile`, that makes use of another function caled `cen_desMinDo`.  The `memcpy` function call gives the 8 byte key away, hinting at a possible DES key.

<img  src="https://raw.githubusercontent.com/AndreiVladescu/TP-Link-Archer-A5-Analysis/refs/heads/main/images/des_key.png"  width="40%">

If decrypting this using DES, we find that it decrypts using ECB encryption scheme. In the decrypted `reduced_data_model.xml` file can be found the configuration for the network, firewall, Wi-Fi, WAN and ACL, and in the `default_config.xml`, you guessed it, the default configuration for the router.

<img  src="https://raw.githubusercontent.com/AndreiVladescu/TP-Link-Archer-A5-Analysis/refs/heads/main/images/cyberchef_ecb_des_decrypt.png"  width="80%">
