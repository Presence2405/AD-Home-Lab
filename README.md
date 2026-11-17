# AD-Home-Lab
In this lab I will use Microsoft Azure to host a windows server 2022 VM and promote it to a DC. I will then add another VM to the domain and configure AD to practice making users, OUs, and configuring group policy.
I started with making the optimal VM for my DC
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/7d98daab24534d85970f0421d31a1c1274e5f295/Screenshot%202026-11-14%20210328.png)
After this I setup a local admin account to log into, and disabled RDP (temp) for security reasons and I will be using Azure Bastion to connect
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/2eaccd4153f764e04e4b37bf3308ae032ce4ac2d/Screenshot%202026-11-14%20210451.png)
I then configured the virtual network and subnet my VM would run on, as well as the public ip
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/cf27a0792c3897643686369edd9145257a01f439/Screenshot%202026-11-14%20210729.png)
Configured the VM disk to be an SSD for better performance and made sure to check "delete with VM" (though I will be deleting the entire RG at the end of the lab)
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/96313d66c4157b0aa48438537137323c0133c605/Screenshot%202026-11-14%20210751.png)
That concluded my configuration for the VM so I spung it up.
Once it was up, I went to network settings to now allow RDP connection to only my IP
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/912858153350fe70fe148e456c7fd6fe740b4f4d/Screenshot%202026-11-14%20191201.png)
As this server is going to be my DC, DHCP, and DNS server, I gave it a static IP to avoid it potentially changing and breaking the lab
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/e389f1241dcdbd339520d4c40cbcecd0375276fe/Screenshot%202026-11-14%20191434.png)
I then used Azure Bastion and RDP'd into my windows server VM
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/8788be729ce5a227d0ba7bc0c91070df2e644a50/Screenshot%202026-11-16%20204417.png)

Upon getting into the windows server VM, server manager was opened automatically 
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/6da2f4f58ee58e456eba04d7abafd2678d866a60/Screenshot%202026-11-16%20204731.png)

I then added all the roles and features needed to complete this lab including AD domain services, DNS, DHCP, Print server, Group Policy management, and Web server(IIS)

![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/aa70dd053ff53045e82ba05ee63e8e228779f347/Screenshot%202026-11-14%20192329.png)

After this I rebooted the server as needed for these changes to take effect, used Azure Bastion to RDP in like last time
I then went to server manager and into AD DS to turn my server into my DC
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/f646701129f6b684f3b5ff79f35b7f1be5aba610/Screenshot%202026-11-14%20193912.png)

I created a new forest with the root domain name "lab.local"
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/f4a90565cea74f0f03dd7fc0d738a953bb16b8bc/Screenshot%202026-11-14%20193949.png)

Configured my domain controller options making sure to use a strong password
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/980d3dc89e3a252cb223292156d9f94d996ac59d/Screenshot%202026-11-14%20194028.png)

I left the rest of the setting default and finished the install which rebooted my VM. I connected via bastion again and went to active directory users and computers and created a new OU called "branch 1"
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/f56e4db64ca16ef33a10f42e84c6110eaa0c17b7/Screenshot%202026-11-14%20195142.png)

Within "branch 1" I created 3 more OU's "users" "computers" "groups" to further organize my lab
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/769bf1583f47c19d730a4e3e7db9da45482fea78/Screenshot%202026-11-14%20195402.png)

I then added my first user to the "users" OU and created the naming scheme for the domain (firstname.lastname@lab.local)
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/6cc7d8e5d24c2ade1f24e9815194124aade4f32c/Screenshot%202026-11-14%20195617.png)

I gave my user his temp password and set it so he must change it on next logon
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/3bd10f06fecfe6ada9017a439d57dae3890f53f0/Screenshot%202026-11-14%20195651.png)

I then confirmed my user was in the correct OU
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/24b77c877fb714116744d85a0341393cc4401b16/Screenshot%202026-11-14%20195824.png)

After this I created another user and practiced resetting passwords/unlocking accounts 
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/4484c60dbc326f29ce7d7c74c4b4c06ef895d082/Screenshot%202026-11-14%20200427.png)

I then made my first group called "IT Workers"
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/6dda617722b74efa1283d6bf2015068389bbde87/Screenshot%202026-11-14%20200535.png)

Added "Tom Brady" to the group
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/2a6c5f15fb8e6926623774de3b564e4e8f59ad40/Screenshot%202026-11-14%20200814.png)

Confirmed Tom was in the group 
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/01cdf7df715bc1376852f51adceb3268319a12f7/Screenshot%202026-11-14%20200826.png)

I then selected Tom Brady and chose copy to make another user just like him name "Tom smith"
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/f090bd30d85cd40a6f330efb018b0a96c7c7b4bf/Screenshot%202026-11-14%20201216.png)

Selected Tom Smith and confirmed he was a copy of Tom Brady as he was automatically added into the "IT workers" group
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/5649da213777f61d849494b910bf257f5dff634c/Screenshot%202026-11-14%20201237.png)
