# AutomatedCitrix

Some could say CVAD (Citrix Virtual Apps and Desktops), never the less I will stick with the first string from the product name only, as you'll neve know when the current name become the relict of the past. Where the word Citrix is still with us since 20y.

Okay so you decided to spend some time with this product, but you do not want spend too much of your precious time with setting up the vm's at first place..
1. Think of the hypervisor vendor, you'll bring into your lab. Your option may be xcp-ng (https://xcp-ng.org/). It's free, it's based on Xen.
2. Consider 10G network connectivity for your storage, then the provisioning should be much faster. Unless you got it virtualized already, and you dont' have to take care about this aspect.
3. Consider splitting your regular network for the VM traffic, that you end up with different possible scenarios for deployment (especially if you are interested with ending up with more than one site), and some specific use cases.
4. Think about the PKI infrastructure, then you can get some hands on experience with FAS (Federated Authentication Services), TLS 1.x - and you'll gain some hands on experience with openSSL, and you can take the benefit of that piece for the regular RDS (Remote Desktop Infrastructure).
5. Check the internet against the possibility of spinnig up Remote Desktop Services Session Hosts, with the regular PVS (Citrix Provisioning Services) infrastructure. Please check the licensing constrains, which will help you assessing which version of the PVS you may be utilizing for the usecase. There are/were some differences..
6. Consider the Freemium version of the Citrix Netscaler (Citrix ADC), starting version 13.0 of the product, the freemium version equals with the features previous standard releases, so it should at least give you the possibilities to get some hands on experience with SSL offloading, Load Balancing, Context switching, and some other interesting things, without a chance to set the Gateway for your CVAD infrastructure... Let's see what the 2022 bring for home labbers as Citrix is promissing some improvements in that area, and some special licenses / options.
7. Consider preparing the unattended iso's for the Server and Desktop OS'es - it will help you spinning up new vm's.

(1) Xcp-ng is based on RedHat, like regular XenServer is. In case you are still using windows desktop as your management station, there is an option called (https://github.com/xcp-ng/xenadmin/releases). Bare in mind that there are no toos available for windows VM which runs on top of xcp-ng, but in case you are so fortunate and have a mycitrix account, there is an option for you, to get rid of that issue, unless this is prevented. Apart from that if you decide to stick with the original stack, and you won't mix vmware, nutanix, or hyper-v with CVAD's, then you can take the benefit from the Citrix Hypervisor SDK (available in PowerShell) which will allow you automating the provisioning of the VM's, so you don't have to click within the XenCenter / XenAdmin Centre. The Xen API does not belong to the easiest one, especially if you decide to perform something a bit more complicated. Thankfully there are github projects like ALX wchich may help you constructing your own functions which are hopefully idempotent.

(2) If you decide to go with physical route for your network infrastructure, there are great second hand product available, like aruba switches (10G switches ethernet based) and not yet that cheap in 2021. I'm not a big fan of having plenty of different vendors which compose your infrastructure, as it causes life even more interesting, so maybe it is a good idea to stick with mikrotik products with their series of CRS-3XX. With RouterOS which give you possibiluty to manage via winbox or ssh, those can act as L2 on the bridge level if you configure vlans, and only the logical interface of the device itself, which allows you managing it will reside in L3. The configuration from ground zero is a bit tricky for w newbe, never the less the overall performance for the price is not too bad for a home lab.

(5) There is a great book from Freek Berson (https://github.com/fberson) and Claudio Rodrigues - RDS - The Complete Guide: Everything you need to know about RDS. And more. - (https://www.amazon.com/RDS-Complete-Guide-Everything-about-ebook/dp/B07C6849WD/ref=sr_1_1?ie=UTF8&qid=1525462416&sr=8-1&keywords=rds+complete+guide). Installation of the RDS was also automated with AutomatedRDS and some other projects in the internet. On top of that the configuration piece is left for you. Configuring the windows 10 for the SSO (Single Sing On) experience on top of RDS, will make you busy. As it was said, you may let the cogel mogel begins, by provisioning the RDSH (Remote Session Hosts) by PVS (Citrix Provisioning Services) which is not a bad idea, especially for RDS infrastructures which have plenty of Collections.

(6) With Fremium you'll gain some hands on experience with the Citrix ADC which is FreeBSD based. Even if you give up with CVAD, at least you may have some interesting time with certificates, perform a lot of binding, and HTTP/HTTPS protocol itself.

(7) Unattended file, if this is home lab, you'll find plenty of examples in the internet with the GVLK keys (https://docs.microsoft.com/en-us/windows-server/get-started/kms-client-activation-keys), please do not follow them. If you decide constructing your xml answer file, leave this parameter blank, and focus on updating the .wim file with OSD from Mr. Segura (https://osdupdate.osdeploy.com/) - who brought this fantastic product which will save your time and make your life easier.

Once you download the image of your LTSC windows release, it will contain Standard and Datacenter versions, in Core (no GUI - Graphical User Interface in windows) and Desktop Experience (GUI which you got used to during all the years). The sources can be downloaded from the Microsoft webpage (https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2019), the downloaded iso will contain the .wim file. If you decide to use another ways, like Windows Media Creation Tool, then you'll end up with the iso or other format, which contain .esd file instead of .wim file. Up to my knowledge, you'll have to convert it to .wim if you'd like to make use of it in Windows ADK. ADK goes hand in hand with releases of windows, there is different verison of ADK for 2022, 2019, 2012R2 etc.

A bit of powershell will give you the possibility to remove the versions of the OS release you are not going to use in your lab. Please consider Core, your skills and OS understanding will raise, and you'll gain a better understanding how does the system work.
Few elements in your citrix home lab may be based on core editions of the windows servers. If I'm not mistaken it is advised to put your PKI on servers with Desktop Based experience, but elements like your domain controllers, citrix license servers may run on core. You may be tempted to use Windows Admin Center - it has great marketing, but still left much to be desired, so please follow the route of combining old and still good mmc's along with Server Manager, anlong with RSAT tools, which may reside on your management machine. The machine may be desktop based.

Good starting point for the automated answer file is this link: https://www.windowscentral.com/how-create-unattended-media-do-automated-installation-windows-10, it will help you creating the correct structure, and give you some bit of understanding, how does those windows popping up during the installation, reflects the xml structure. UEFI needs 4 partitions, BIOS boot 2.
