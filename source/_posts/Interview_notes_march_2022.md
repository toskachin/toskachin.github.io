---
title: Interview notes March 2022
date: 2022-03-10
categories: ideas,interview
tags: 
 - ideas,interview,面试
---

## Application support 

### Macbook support
how to reset Macbook
> reset SMC, Shift-Control-Option
> reset PRAM,Command-Option-P-R
how to reinstall MacOS
> Command-R
how to install package on MacOS
> MDM,mobile device management
> dmg,pkg(DMG like ISO,not installer; pkg is installer package)
> apple store
how to manage softwares on MacOS
> munki 
> MDM
how to deploy printer on MacOS
> MDM
Macbook device management software - MDM

### OKTA intergation 
SSO context
> three types: secure web authentiation(SWA),SMAL2.0(Security Assertion Markup Language),OpenID connect
> Authentiation and authorization 
okta three patry intergation process
1. okta - application - add new application - create new app
2. platform - signon method - SMAL2.0 
3. general setting - app name - sso url 
4. identity provider metadata - open file to find ID and add back to sso url
   
okta advanced workflows
### Google admin
manage endusers license
manage intern and contractor account
forward leaver's email,driver,calaner 

## Network tech
VPN types
* IP security IPsesc
* SSL secure socket layer
* PPTP  point to point tunneling protocol
* L2TP layer 2 tunneling protocol 
  
how to configure IPsesc VPN
* site A
1. Phase 1 proposal (Authentication)
   1. Authentication method Using a Pre-shared Key
   2. My identifier
   3. Peer identifier
   4. Pre-Shared Key
2. Phase 1 proposal (Algorithms)
   1. Encryption algorithm AES
   2. Hash algoritm
3. Phase 2
   1. Local Network - Route the local LAN subnet
   2. remote network - The remote LAN subnet
4. Phase 2 proposal (SA/Key Exchange)
   1. Protocol
   2. Encryption algorithms
   3. Hash algortihms
   4. PFS Key group
   5. Lifetime
* site B
do the same as site A

how to block loop in LAN
> enable STP 
how to secure lan interface 
> enable 802.1x + radius
how to initize Cisco devices/APs
## Script and coding

how to rollback git add locally
difference of git merge and git rebase
Shell script

## Cloud tech

AWS Load balance type
Introduce AWS feature you familiar with

## IT process and Project management

How to initize project for office movement
Vendor management
Project schedule management