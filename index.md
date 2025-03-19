
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

The top of the board reveals the main SoC Mediatek MT7628AN, the ESMT M14D5121632A-2.5BBG2A 400MHz RAM IC, and the secondary Mediatek MT7612EN SoC.

The secondary SoC handles the 5GHz band, while the primary SoC handles the 2.4GHz band.

<img  src="https://raw.githubusercontent.com/AndreiVladescu/TP-Link-Archer-A5-Analysis/refs/heads/main/images/frontboard.jpeg"  width="60%">
	
The router came without the EMFI shields soldered on the PCB.

<div style="display: flex; flex-direction: row; align-items: center;">
  <img src="https://raw.githubusercontent.com/AndreiVladescu/TP-Link-Archer-A5-Analysis/refs/heads/main/images/ram_and_cpu.jpeg" width="40%" style="margin-right: 50px;">
  <img src="https://raw.githubusercontent.com/AndreiVladescu/TP-Link-Archer-A5-Analysis/refs/heads/main/images/secondary_mediatek.jpeg" width="28%">
</div>
  
 The backside of the board reveals an 8MB eFeon QH64A flash IC.

<img  src="https://raw.githubusercontent.com/AndreiVladescu/TP-Link-Archer-A5-Analysis/refs/heads/main/images/backboard	.jpeg"  width="50%">

  <img  src="https://raw.githubusercontent.com/AndreiVladescu/TP-Link-Archer-A5-Analysis/refs/heads/main/images/flash.jpeg"  width="20%">
