<h1 align="center">Summary Diagram</h1>


<p align="center">
<br/>
<img width="950" alt="Portfolio" src="https://i.imgur.com/YunCnHZ.png">
<br />
</p>


<br />
<br />

<h1 align="center">Project walk-through</h1>

<br />
<br />

Additionally to the diagram above, we will practice Desynchronization between the domain controller and the Azure AD.


---
<a name="toc"></a>
**Table of Contents:**

- [Creating a New Resource Group](#creating-a-new-resource-group)
- [Creating a Virtual Network](#creating-a-virtual-network)
- [Create a First Domain Controller VM](#create-a-first-domain-controller)
- [Connect to DC01 via RDP](#connect-to-dc01-via-rdp)
- [DC01 Disk Configuration](#dc01-disk-configuration)
- [Installing Active Directory Domain Service](#installing-active-directory-domain-service)
- [Setting up Domain](#setting-up-domain)
- [Setting up Custom DNS Server viz Azure](#setting-up-custom-dns-server)
- [Setting up the second Domain Controller VM](#setting-up-the-second-domain-controller-vm)
- [DC02 Network Configuration](#dc02-network-configuration)
- [DC01 and DC02 syncing via ADSS](#dc01-and-dc02-syncing-via-adss)
- [DC01 and DC02 syncing via ADUC](#dc01-and-dc02-syncing-via-aduc)
- [Downloading Microsoft Azure AD Connect](#downloading-microsoft-azure-ad-connect)
- [Installing Azure AD connect to DC01](#installing-azure-ad-connect-to-dc01)
- [Checking sync status via Azure Portal](#checking-sync-status-via-azure-portal)
- [Force syncing with PowerShell](#force-syncing-with-powershell)
- [Disabling Synchronization](#disabling-synchronization)
- [Verifying Desynchronization](#verifying-desynchronization)
- [Uninstalling Azure AD connect](#uninstalling-azure-ad-connect)




---


<h4>Tip: Any configuration options not mentioned in the walkthrough can be left at their default settings</h4>




---

[back to top](#toc)
## Creating a New Resource Group

<br />
<br />

 - Create a new resource group named "AD-LAB".
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/Pbhwlqj.png">
<br />
<br />
<br />
<br />

---

[back to top](#toc)
## Creating a Virtual Network

<br />
<br />


 - Create a new virtual network called "OnPremNetwork" to simulate on-premises network.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/W0XN2BF.png">
<br />
<br />
<br />
<br />



 - Disable all security features. Set IP addresses to 10.0.0.0/16 and subnet to 10.0.1.0/24.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/dsxbZRB.png">
<br />
<br />
<br />
<br />



 - Name the virtual machine "DC01". Create a new Availability Set with default settings.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/OwsoDqd.png">
<br />
<br />
<br />
<br />

---

[back to top](#toc)
## Create a First Domain Controller VM

<br />
<br />

 - Choose Windows Server 2022 Hot Patched with 2 vcpus & 8 GiB memory. Set username: dc01admin, password: Passwordforlab!1
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/Qwyw7vf.png">
<br />
<br />
<br />
<br />


 - Use HDD Standard default setting for OS disk.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/oczX9m2.png">
<br />
<br />
<br />
<br />



 - Create a new 10 GiB Standard HDD data disk for AD installation.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/sOoMvez.png">
<br />
<br />
<br />
<br />


 - Leave networking settings as default.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/95bxGOK.png">
<br />
<br />
<br />
<br />



 - Disable Boot diagnostic. Review and create VM.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/N88TgOf.png">
<br />
<br />
<br />
<br />

---

[back to top](#toc)
## Connect to DC01 via RDP

<br />
<br />


 - After deployment, go to resource.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/OsN8cFO.png">
<br />
<br />
<br />
<br />



 - Connect and download RDP client. Open file and connect using your credentials.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/S26OUlD.png">
<br />
<br />
<br />
<br />


 - Right-click client and enable Smart Sizing.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/CtRmkdx.png">
<br />
<br />
<br />
<br />

---

[back to top](#toc)
## DC01 Disk Configuration

<br />
<br />

 - In Server Manager, go to Tools - Computer Management.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/NlRaF1H.png">
<br />
<br />
<br />
<br />


 - Initialize disk as MBR when prompted in Disk Management.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/qY2Kj20.png">
<br />
<br />
<br />
<br />





 - Quick format Disk 2. Create new simple volume.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/GoaSY5C.png">
<br />
<br />
<br />
<br />


 - Assign drive letter F:.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/Gd6M8mH.png">
<br />
<br />
<br />
<br />



 - Complete formatting.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/r2rresX.png">
<br />
<br />
<br />
<br />

---

[back to top](#toc)
## Installing Active Directory Domain Service

<br />
<br />


 - Add roles and features. Select role-based or feature-based installation.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/kMtkTkI.png">
<br />
<br />
<br />
<br />




 - Choose DC01 server.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/dnE0gbo.png">
<br />
<br />
<br />
<br />


 - Select ADDS. Install
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/xSPFLvC.png">
<br />
<br />
<br />
<br />

---

[back to top](#toc)
## Setting up Domain

<br />
<br />


 - Set up domain.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/W2wbF20.png">
<br />
<br />
<br />
<br />






 - Add forest and set domain name.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/VAwZmBq.png">
<br />
<br />
<br />
<br />






 - Set DSRM password.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/NXVb0QF.png">
<br />
<br />
<br />
<br />






 - Change default drive from C to F.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/kz4GxHd.png">
<br />
<br />
<br />
<br />






 - Install and reboot VM.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/twQkyXf.png">
<br />
<br />
<br />
<br />






 - Verify domain installation in Active Directory Users and Computers.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/qnuVfiW.png">
<br />
<br />
<br />
<br />


---

[back to top](#toc)
## Setting up Custom DNS Server viz Azure

<br />
<br />


 - Go to DC01 Network setting. Select Network adapter.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/s5GPDxQ.png">
<br />
<br />
<br />
<br />


 - In IP configurations, change [ipconfig1] to static IP.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/UhckuwA.png">
<br />
<br />
<br />
<br />


 - In OnPremNetwork, set custom DNS server to DC01's private IP.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/DrGjcML.png">
<br />
<br />
<br />
<br />


---

[back to top](#toc)
## Setting up the second Domain Controller VM



 - Create second domain controller following first DC's settings.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/bNaIeb6.png">
<br />
<br />
<br />
<br />



 - Create VM.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/ooCkZiW.png">
<br />
<br />
<br />
<br />


 - Connect to DC02 via RDP. Set up Disk Management (MBR, Simple volume, F:).
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/HFXyAMs.png">
<br />
<br />
<br />
<br />



 - Install Active Directory.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/23wvvlT.png">
<br />
<br />
<br />
<br />





 - Use default settings unless specified.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/kUCx1HZ.png">
<br />
<br />
<br />
<br />





 - Add domain controller to existing domain. Provide admin credentials (Domain\User).
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/lVYz6Od.png">
<br />
<br />
<br />
<br />






 - Set DSRM password.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/5c8CKqU.png">
<br />
<br />
<br />
<br />






 - Duplicate additional options from DC01.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/DvEtdPJ.png">
<br />
<br />
<br />
<br />






 - Change paths from C: to F: drive.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/X6Mcvm5.png">
<br />
<br />
<br />
<br />






 - Install and reboot VM.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/12sFuMd.png">
<br />
<br />
<br />
<br />






 - Log in to DC02 with DC01 admin account. Verify both DCs in Domain Controllers OU.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/GadtmoA.png">
<br />
<br />
<br />
<br />


---

[back to top](#toc)
## DC02 Network Configuration

<br />
<br />

 - Go to DC02 Network setting and Network adapter.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/8bBNb4z.png">
<br />
<br />
<br />
<br />



 - Change [ipconfig1] to static IP.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/J7GF0Er.png">
<br />
<br />
<br />
<br />




 - Add DC02's private IP to OnPremNetwork DNS settings.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/cudub4I.png">
<br />
<br />
<br />
<br />


---

[back to top](#toc)
## DC01 and DC02 syncing via ADSS

<br />
<br />

 - In DC01, rename [Default-First-Site-Name] to [OnPrem] in Active Directory Sites and Services.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/qL1xOOC.png">
<br />
<br />
<br />
<br />



 - Create new subnet.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/T7s5Har.png">
<br />
<br />
<br />
<br />


 - Copy Virtual Network subnet from Azure portal.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/UlxTgQk.png">
<br />
<br />
<br />
<br />


 - Paste subnet prefix and associate with OnPrem network.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/128pqr8.png">
<br />
<br />
<br />
<br />


 - Verify configuration in left pane.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/rZi4c5D.png">
<br />
<br />
<br />
<br />


---

[back to top](#toc)
## DC01 and DC02 syncing via ADUC



 - Check DC01 and DC02 sync in Active Directory Users and Computers. Set directory server to DC01.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/dOlhUpS.png">
<br />
<br />
<br />
<br />




 - Create new OU "_USERS". Add new User.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/FN1NILr.png">
<br />
<br />
<br />
<br />



 - Name user "testuser1".
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/7b8h8ol.png">
<br />
<br />
<br />
<br />



 - Verify "testuser1" in "_USER" OU.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/7js2Mz0.png">
<br />
<br />
<br />
<br />


 - On DC02 client, open Active Directory Users and Computers. Set Directory server to DC01.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/w483mqI.png">
<br />
<br />
<br />
<br />



 - Verify OU and User creation.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/xFSzUss.png">
<br />
<br />
<br />
<br />


 - Change directory server to DC02.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/WCLNpLo.png">
<br />
<br />
<br />
<br />


 - Confirm OU and user update. Dual domain controller deployment complete.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/vSlVqWj.png">
<br />
<br />
<br />
<br />


---

[back to top](#toc)
## Downloading Microsoft Azure AD Connect

<br />
<br />


 - Sync DC01 VM with Azure AD using Entra Connect. Download from Azure portal.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/C8XgZ21.png">
<br />
<br />
<br />
<br />



 - Open downloaded file on DC01 VM.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/2uoICBS.png">
<br />
<br />
<br />
<br />


---

[back to top](#toc)
## Installing Azure AD connect to DC01

<br />
<br />



 - Install Azure AD connect.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/DQy9yxm.png">
<br />
<br />
<br />
<br />



 - Use express settings for lab environment.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/sGKD8WN.png">
<br />
<br />
<br />
<br />



 - Provide Azure Global Admin credentials. Fix error if encountered.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/EMy4hxh.png">
<br />
<br />
<br />
<br />


 - (only if error encountered) Run Command Prompt as admin. Navigate to Microsoft Azure Active Directory Connect folder. Execute `AzureADConnect.exe /InteractiveAuth`.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/mk0ozUK.png">
<br />
<br />
<br />
<br />


 - Sign in with your Azure Global Admin account.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/dFZ7FoT.png">
<br />
<br />
<br />
<br />


 - Provide on-premise local AD admin credentials.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/NMOF9Nx.png">
<br />
<br />
<br />
<br />



 - Skip UPN Suffix addition and domain verification in Azure.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/vdzpZLK.png">
<br />
<br />
<br />
<br />



 - Complete installation.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/tPFPgaf.png">
<br />
<br />
<br />
<br />

---

[back to top](#toc)
## Checking sync status via Azure Portal

<br />
<br />


 - Check Users in Azure AD for synchronization status.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/YA1uW2z.png">
<br />
<br />
<br />
<br />


 - Verify sync status in Microsoft Entra Connect.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/ybsJvfT.png">
<br />
<br />
<br />
<br />

---

[back to top](#toc)
## Force syncing with PowerShell

<br />
<br />




 - AD connect syncs every 30 minutes. Force sync with PowerShell:

<br />

```
Import-Module ADSync
Start-ADSyncSyncCycle -PolicyType Delta
```

<br />

<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/9x5awPT.png">
<br />
<br />
<br />
<br />


---

[back to top](#toc)
## Disabling Synchronization

<br />
<br />



 - Install MS online Module via PowerShell: `Install-Module MSonline`
 - Connect the PowerShell with Azure AD: `Connect-MsolService`

<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/WsuSuU0.png">
<br />
<br />
<br />
<br />


 - Check sync status: `(Get-MsolCompanyInformation).Directorysynchronizationenabled`
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/LWA5ewN.png">
<br />
<br />
<br />
<br />



 - Disable sync: `Set-MsolDirSyncEnabled -EnableDirSync $false`
 - Confirm status: `(Get-MsolCompanyInformation).Directorysynchronizationenabled`
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/Xu1hNYg.png">
<br />
<br />
<br />
<br />

---

[back to top](#toc)
## Verifying Desynchronization

<br />
<br />


 - Verify in Azure AD.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/4Iix5v8.png">
<br />
<br />
<br />
<br />


 - Users convert to cloud-only accounts. Delete "On-Premises Directory Synchronization Service Account".
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/AknMc3x.png">
<br />
<br />
<br />
<br />

---

[back to top](#toc)
## Uninstalling Azure AD connect

<br />
<br />


 - Uninstall Microsoft AD connect from domain controller.
<p align="center">
<br/>
<img width="750" alt="Portfolio" src="https://i.imgur.com/OXWTDo2.png">
<br />
<br />
<br />
<br />






\<end\>

[back to top](#toc)

<br />
<br />
<br />


