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

I then made my first group called "IT Workers" and set the group type to security group and made it a global group 
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/6dda617722b74efa1283d6bf2015068389bbde87/Screenshot%202026-11-14%20200535.png)

Added "Tom Brady" to the group
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/2a6c5f15fb8e6926623774de3b564e4e8f59ad40/Screenshot%202026-11-14%20200814.png)

Confirmed Tom was in the group 
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/01cdf7df715bc1376852f51adceb3268319a12f7/Screenshot%202026-11-14%20200826.png)

I then selected Tom Brady and chose copy to make another user just like him name "Tom smith"
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/f090bd30d85cd40a6f330efb018b0a96c7c7b4bf/Screenshot%202026-11-14%20201216.png)

Selected Tom Smith and confirmed he was a copy of Tom Brady as he was automatically added into the "IT workers" group
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/5649da213777f61d849494b910bf257f5dff634c/Screenshot%202026-11-14%20201237.png)

Confirmed both Tom Brady and Tom Smith are in the "IT workers" group 
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/b5053b4fbc67b52c83c61710e5b4915b6d9fa873/Screenshot%202026-11-14%20201315.png)

Next I went to one of my existing user accounts and practiced resetting a password/unlocking the account for an existing user
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/878f7edf377bdc18ab03cb10aa45ffec5b8f4b2f/Screenshot%202026-11-14%20201444.png)

I then created a new OU called "branch 2" and filled it with the same OUs in it as branch one "users" "computers" "groups
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/6ae3e439b7a8d572359e2ccadbcf3910398b01a8/Screenshot%202026-11-14%20202205.png)

I then used Tom Brady to practice moving a user from one branch to another by moving him from users in branch one to users in branch 2
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/588ba49243197d45395c1e9594589b837e69a86f/Screenshot%202026-11-14%20202505.png)

Confirmed that Tom Brady was now in the "users" OU in branch 2
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/b0e38bee17311fb0986e109b7b4b54c8d6c5d7d4/Screenshot%202026-11-14%20202524.png)

Next I gave Tom Smith a description and confirmed it went through
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/7f9b6c05a61f288f1ef5ef5fbc88fd401cccc639/Screenshot%202026-11-14%20202755.png)
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/7f9b6c05a61f288f1ef5ef5fbc88fd401cccc639/Screenshot%202026-11-14%20202806.png)

I then added his phone number and his manager

![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/770416073614004b969940faa5aef74e028e918f/Screenshot%202026-11-14%20202848.png)
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/770416073614004b969940faa5aef74e028e918f/Screenshot%202026-11-14%20203101.png)

I then create a new group called "HR" and added user "Logan Heck" to it

![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/745b76a323be9f5d8ca27d5a8dd73628f1ed2430/Screenshot%202026-11-14%20203918.png)
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/745b76a323be9f5d8ca27d5a8dd73628f1ed2430/Screenshot%202026-11-14%20204147.png)

I then turned on "advanced views" to see more options
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/c96e4a2167ab168f1f4cb4143c6f91d5897ac3f0/Screenshot%202026-11-14%20204319.png)

After this I practiced using the "find users, contacts, and groups" feature to find user "Logan Heck"
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/918d929d5dfd400c7dd8e20bdb3cc5bedf01944e/Screenshot%202026-11-14%20204407.png)

Now that I found "Logan Heck" with advanced view turned on a went to attribute editor and added a proxy address for him

![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/685096ab32d86239834508289cdf5ee2e3de3823/Screenshot%202026-11-14%20204803.png)
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/685096ab32d86239834508289cdf5ee2e3de3823/Screenshot%202026-11-14%20204921.png)

I then went to the "member of" section and added him to "HR" and confirmed it went through 

![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/7185fdd6e8b41aa89820ed99c4abe6a8d0710a5c/Screenshot%202026-11-14%20205100.png)
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/7185fdd6e8b41aa89820ed99c4abe6a8d0710a5c/Screenshot%202026-11-14%20205114.png)
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/7185fdd6e8b41aa89820ed99c4abe6a8d0710a5c/Screenshot%202026-11-14%20205208.png)

After this I spun up another VM to join my domain. I went to the VNIC settings of my new VM and configured the DNS to use the static IP I configured prior
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/628dcdfce199317d9124cead0f57ccf5f42d715d/Screenshot%202026-11-14%20211156.png)

I then set up a port rule to allow only my IP to RDP into the VM
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/dc92061c56265ad70395ecfca5c5e2b9e2035dc8/Screenshot%202026-11-14%20212759.png)

I then used Azure Bastion to log into my new VM and join it to my domain
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/981b12a8e6e6d0e750c137a2584dc6ba0a88cc20/Screenshot%202026-11-14%20214943.png)

When I tried to connect my new VM to the DC I got an error saying it could not be contacted. I opened cmd and did ipconfig /all and saw my DNS server was not set to what I had preconfigured it in azure. I then went to the windows search bar and typed in ncpa.cpl to pull up my network settings and manually configured my DNS

![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/334c1eb3d7e22f588e18dfad0e0059ccbfa58693/Screenshot%202026-11-14%20214958.png)
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/334c1eb3d7e22f588e18dfad0e0059ccbfa58693/Screenshot%202026-11-14%20215419.png)
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/334c1eb3d7e22f588e18dfad0e0059ccbfa58693/Screenshot%202026-11-14%20215027.png)
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/334c1eb3d7e22f588e18dfad0e0059ccbfa58693/Screenshot%202026-11-14%20215059.png)
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/334c1eb3d7e22f588e18dfad0e0059ccbfa58693/Screenshot%202026-11-14%20215253.png)

After this I was finally able to join my domain and ping my DC
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/f421f9cc43195d01b4b636af7ab4a4818e26b756/Screenshot%202026-11-14%20215532.png)
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/f421f9cc43195d01b4b636af7ab4a4818e26b756/Screenshot%202026-11-14%20215603.png)
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/f421f9cc43195d01b4b636af7ab4a4818e26b756/Screenshot%202026-11-14%20215848.png)

Now that I joined this VM to the DC I went back to my DC VM and moved the new computer from the default "computer" group to the "computer" group in branch one

![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/44d5712dadcfc3009fbd251442aaced8a1925bbf/Screenshot%202026-11-14%20215914.png)
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/44d5712dadcfc3009fbd251442aaced8a1925bbf/Screenshot%202026-11-14%20220000.pn)

I then went into "group policy management" and configured the password policy of my domain

![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/483e162311485d24dba38755355f1269b703a5c2/Screenshot%202026-11-14%20221000.png)
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/483e162311485d24dba38755355f1269b703a5c2/Screenshot%202026-11-14%20221113.png)
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/483e162311485d24dba38755355f1269b703a5c2/Screenshot%202026-11-14%20221216.png)

Lastly I delegated the task of resetting passwords and forcing a password change at next login to the "HR" group
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/8800ff787a9e7aaa73e1c7d3b5c73cab7d589107/Screenshot%202026-11-14%20221745.png)
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/8800ff787a9e7aaa73e1c7d3b5c73cab7d589107/Screenshot%202026-11-14%20221757.png)
![image alt](https://github.com/Presence2405/AD-Home-Lab/blob/8800ff787a9e7aaa73e1c7d3b5c73cab7d589107/Screenshot%202026-11-14%20221808.png)

After this I completed my lab by deleting my Azure RG with everything in it to confirm I would not miss anything
