---
title: Öğretici - Temel Etkin Dizin şirket içi ve Azure REKLAM ortamı.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/02/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 356a05d4d92f17ceb66ff0208153ec3eac736757
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74793904"
---
# <a name="tutorial-basic-active-directory-environment"></a>Öğretici: Temel Aktif Dizin ortamı

Bu öğretici, temel bir Active Directory ortamı oluşturma da size yol göstermektedir. 

![Oluşturma](media/tutorial-single-forest/diagram1.png)

Öğreticide oluşturduğunuz ortamı, karma kimlik senaryolarının çeşitli yönlerini test etmek için kullanabilirsiniz ve bazı öğreticiler için ön koşul olacaktır.  Zaten varolan bir Active Directory ortamınız varsa, bunu yerine olarak kullanabilirsiniz.  Bu bilgiler benim hiçbir şeyden başlayarak olan bireyler için sağlanmaktadır.

Bu öğretici oluşur
## <a name="prerequisites"></a>Ön koşullar
Bu öğreticinin tamamlanması için gerekli ön koşullar aşağıda dır.
- [Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-technology-overview) yüklü bir bilgisayar.  Bunu bir Windows 10 veya [Windows Server 2016](https://docs.microsoft.com/windows-server/virtualization/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows) bilgisayarında yapmanız önerilir. [Windows 10](https://docs.microsoft.com/virtualization/hyper-v-on-windows/about/supported-guest-os)
- Sanal makinenin internetle iletişim kurmasını sağlamak için harici ağ [bağdaştırıcısı.](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/connect-to-network)
- [Azure aboneliği](https://azure.microsoft.com/free)
- Windows Server 2016'nın bir kopyası
- [Microsoft .NET çerçevesi 4.7.1](https://www.microsoft.com/download/details.aspx?id=56115)

> [!NOTE]
> Bu öğretici, en kısa sürede öğretici ortamı oluşturabilmeniz için PowerShell komut dosyalarını kullanır.  Komut dosyalarının her biri, komut dosyalarının başında bildirilen değişkenleri kullanır.  Değişkenleri ortamınızı yansıtacak şekilde değiştirebilirsiniz ve değiştirmelisiniz.
>
>Kullanılan komut dosyaları, Azure AD Connect bulut sağlama aracısını yüklemeden önce genel bir Active Directory ortamı oluşturur.  Tüm öğreticiler için geçerlidir.
>
> Bu eğitimde kullanılan PowerShell komut dosyalarının kopyalarını [Burada](https://github.com/billmath/tutorial-phs)GitHub'da bulabilirsiniz.

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma
Hibrid kimlik ortamımızı çalışır hale getirmek için yapmanız gereken ilk şey, şirket içi Active Directory sunucumuz olarak kullanılacak sanal bir makine oluşturmaktır.  Şunları yapın:

1. PowerShell ISE'yi Yönetici olarak açın.
2. Aşağıdaki komut dosyasını çalıştırın.

    ```powershell
    #Declare variables
    $VMName = 'DC1'
    $Switch = 'External'
    $InstallMedia = 'D:\ISO\en_windows_server_2016_updated_feb_2018_x64_dvd_11636692.iso'
    $Path = 'D:\VM'
    $VHDPath = 'D:\VM\DC1\DC1.vhdx'
    $VHDSize = '64424509440'

    #Create New Virtual Machine
    New-VM -Name $VMName -MemoryStartupBytes 16GB -BootDevice VHD -Path $Path -NewVHDPath $VHDPath -NewVHDSizeBytes $VHDSize  -Generation 2 -Switch $Switch  

    #Set the memory to be non-dynamic
    Set-VMMemory $VMName -DynamicMemoryEnabled $false

    #Add DVD Drive to Virtual Machine
    Add-VMDvdDrive -VMName $VMName -ControllerNumber 0 -ControllerLocation 1 -Path $InstallMedia

    #Mount Installation Media
    $DVDDrive = Get-VMDvdDrive -VMName $VMName

    #Configure Virtual Machine to Boot from DVD
    Set-VMFirmware -VMName $VMName -FirstBootDevice $DVDDrive 
    ```

## <a name="complete-the-operating-system-deployment"></a>İşletim sistemi dağıtımını tamamlama
Sanal makine bina bitirmek için, işletim sistemi yükleme bitirmek gerekir.

1. Hyper-V Manager, sanal makineye çift tıklayın
2. Başlat düğmesine tıklayın.
3. 'CD veya DVD'den önyükleme için herhangi bir tuşa basın' sizden istenir. Devam et ve yap.
4. Windows Server başlangıç ekranında dilinizi seçin ve **İleri'yi**tıklatın.
5. **Şimdi Yükle'yi**tıklatın.
6. Lisans anahtarınızı girin ve **İleri'yi**tıklatın.
7. Kontrol **Lisans koşullarını kabul ediyorum ve **İleri'yi**tıklatın.
8. **Özel'i seçin: Yalnızca Windows'u Yükle (Gelişmiş)**
9. **İleri'yi** tıklatın
10. Yükleme tamamlandıktan sonra sanal makineyi yeniden başlatın, VM'nin en güncel olduğundan emin olmak için Windows güncelleştirmelerini oturum açın ve çalıştırın.  En son güncelleştirmeleri yükleyin.

## <a name="install-active-directory-prerequisites"></a>Etkin Dizin önkoşulları yükleyin
Artık sanal bir makineniz olduğuna göre, Active Directory'yi yüklemeden önce birkaç şey yapmanız gerekir.  Diğer bir süre, sanal makineyi yeniden adlandırmanız, statik bir IP adresi ve DNS bilgileri ayarlamanız ve Uzaktan Sunucu Yönetimi araçlarını yüklemeniz gerekir.   Şunları yapın:

1. PowerShell ISE'yi Yönetici olarak açın.
2. Aşağıdaki komut dosyasını çalıştırın.

    ```powershell
    #Declare variables
    $ipaddress = "10.0.1.117" 
    $ipprefix = "24" 
    $ipgw = "10.0.1.1" 
    $ipdns = "10.0.1.117"
    $ipdns2 = "8.8.8.8" 
    $ipif = (Get-NetAdapter).ifIndex 
    $featureLogPath = "c:\poshlog\featurelog.txt" 
    $newname = "DC1"
    $addsTools = "RSAT-AD-Tools" 

    #Set static IP address
    New-NetIPAddress -IPAddress $ipaddress -PrefixLength $ipprefix -InterfaceIndex $ipif -DefaultGateway $ipgw 

    # Set the DNS servers
    Set-DnsClientServerAddress -InterfaceIndex $ipif -ServerAddresses ($ipdns, $ipdns2)

    #Rename the computer 
    Rename-Computer -NewName $newname -force 

    #Install features 
    New-Item $featureLogPath -ItemType file -Force 
    Add-WindowsFeature $addsTools 
    Get-WindowsFeature | Where installed >>$featureLogPath 

    #Restart the computer 
    Restart-Computer
    ```

## <a name="create-a-windows-server-ad-environment"></a>Windows Server AD ortamı oluşturma
Artık VM oluşturulduve yeniden adlandırıldı ve statik bir IP adresi var, devam edin ve yüklemek ve Active Directory Etki Alanı Hizmetleri yapılandırmak.  Şunları yapın:

1. PowerShell ISE'yi Yönetici olarak açın.
2. Aşağıdaki komut dosyasını çalıştırın.

    ```powershell 
    #Declare variables
    $DatabasePath = "c:\windows\NTDS"
    $DomainMode = "WinThreshold"
    $DomainName = "contoso.com"
    $DomaninNetBIOSName = "CONTOSO"
    $ForestMode = "WinThreshold"
    $LogPath = "c:\windows\NTDS"
    $SysVolPath = "c:\windows\SYSVOL"
    $featureLogPath = "c:\poshlog\featurelog.txt" 
    $Password = "Pass1w0rd"
    $SecureString = ConvertTo-SecureString $Password -AsPlainText -Force

    #Install AD DS, DNS and GPMC 
    start-job -Name addFeature -ScriptBlock { 
    Add-WindowsFeature -Name "ad-domain-services" -IncludeAllSubFeature -IncludeManagementTools 
    Add-WindowsFeature -Name "dns" -IncludeAllSubFeature -IncludeManagementTools 
    Add-WindowsFeature -Name "gpmc" -IncludeAllSubFeature -IncludeManagementTools } 
    Wait-Job -Name addFeature 
    Get-WindowsFeature | Where installed >>$featureLogPath

    #Create New AD Forest
    Install-ADDSForest -CreateDnsDelegation:$false -DatabasePath $DatabasePath -DomainMode $DomainMode -DomainName $DomainName -SafeModeAdministratorPassword $SecureString -DomainNetbiosName $DomainNetBIOSName -ForestMode $ForestMode -InstallDns:$true -LogPath $LogPath -NoRebootOnCompletion:$false -SysvolPath $SysVolPath -Force:$true
    ```

## <a name="create-a-windows-server-ad-user"></a>Windows Server AD kullanıcısı oluşturma
Artık Aktif Dizin ortamımıza sahip olduğunuza göre, bir test hesabına ihtiyacınız var.  Bu hesap şirket içi REKLAM ortamımızda oluşturulur ve azure AD ile senkronize edilir.  Şunları yapın:

1. PowerShell ISE'yi Yönetici olarak açın.
2. Aşağıdaki komut dosyasını çalıştırın.

    ```powershell 
    # Filename:    4_CreateUser.ps1
    # Description: Creates a user in Active Directory.  This is part of
    #              the Azure AD Connect password hash sync tutorial.
    #
    # DISCLAIMER:
    # Copyright (c) Microsoft Corporation. All rights reserved. This 
    # script is made available to you without any express, implied or 
    # statutory warranty, not even the implied warranty of 
    # merchantability or fitness for a particular purpose, or the 
    # warranty of title or non-infringement. The entire risk of the 
    # use or the results from the use of this script remains with you.
    #
    #
    #
    #
    #Declare variables
    $Givenname = "Allie"
    $Surname = "McCray"
    $Displayname = "Allie McCray"
    $Name = "amccray"
    $Password = "Pass1w0rd"
    $Identity = "CN=ammccray,CN=Users,DC=contoso,DC=com"
    $SecureString = ConvertTo-SecureString $Password -AsPlainText -Force


    #Create the user
    New-ADUser -Name $Name -GivenName $Givenname -Surname $Surname -DisplayName $Displayname -AccountPassword $SecureString

    #Set the password to never expire
    Set-ADUser -Identity $Identity -PasswordNeverExpires $true -ChangePasswordAtLogon $false -Enabled $true
    ```


## <a name="create-an-azure-ad-tenant"></a>Azure AD kiracısı oluşturma
Artık kullanıcılarımızı bulutla senkronize edebilmeniz için bir Azure AD kiracıoluşturmanız gerekiyor.  Yeni bir Azure AD kiracısı oluşturmak için aşağıdaki adımları uygulayın.

1. [Azure portalına](https://portal.azure.com) gidip Azure aboneliği olan bir hesapla oturum açın.
2. **Artı simgesini (+)** seçip **Azure Active Directory** terimini aratın.
3. Arama sonuçlarında **Azure Active Directory** girişini seçin.
4. **Oluştur'u**seçin.</br>
![Oluşturma](media/tutorial-single-forest/create1.png)</br>
5. **Kuruluş için bir ad** ve **ilk etki alanı adı** girin. Ardından **Oluştur**’u seçin. Dizininiz oluşturulur.
6. Bu işlem tamamlandıktan sonra, dizini yönetmek için **buradaki** bağlantıyı tıklatın.

## <a name="create-a-global-administrator-in-azure-ad"></a>Azure AD'de genel bir yönetici oluşturma
Artık bir Azure AD kiracınız olduğuna göre, genel bir yönetici hesabı oluşturursunuz.  Genel yönetici hesabını oluşturmak için aşağıdakileri yapın.

1.  **Yönet** bölümünde **Kullanıcılar**’ı seçin.</br>
![Oluşturma](media/tutorial-single-forest/administrator1.png)</br>
2.  **Tüm kullanıcılar**'ı ve ardından **+ Yeni kullanıcı**'yı seçin.
3.  Bu kullanıcı için bir ad ve kullanıcı adı girin. Bu kullanıcı kiracınızın Genel Yöneticisi olacak. **Dizin rolünü** Global yönetici olarak da değiştirmek isteyeceksiniz. **Global administrator.** İsterseniz geçici parolayı da gösterebilirsiniz. İşiniz bittiğinde **Oluştur**'u seçin.</br>
![Oluşturma](media/tutorial-single-forest/administrator2.png)</br>
4. Bu işlem tamamlandıktan sonra, yeni bir web tarayıcısı açın ve yeni genel yönetici hesabını ve geçici parolayı kullanarak myapps.microsoft.com oturum açın.
5. Genel yöneticinin parolasını hatırlayacağınız bir şeyle değiştirin.

## <a name="optional--additional-server-and-forest"></a>İsteğe bağlı: Ek sunucu ve orman
Aşağıda, ek bir sunucu ve orman oluşturmak için adımlar sağlayan isteğe bağlı bir bölüm veorman.  Bu, Bulut sağlama için [Azure AD Connect için Pilot](tutorial-pilot-aadc-aadccp.md)gibi daha gelişmiş öğreticilerin bazılarında kullanılabilir.

Yalnızca ek bir sunucuya ihtiyacınız varsa, sonra durdurabilirsiniz - **Sanal makine** adımı oluşturun ve yukarıda oluşturulan varolan etki alanına sunucu katılmak.  

### <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

1. PowerShell ISE'yi Yönetici olarak açın.
2. Aşağıdaki komut dosyasını çalıştırın.

    ```powershell
    # Filename:    1_CreateVM_CP.ps1
    # Description: Creates a VM to be used in the tutorial.
    #
    # DISCLAIMER:
    # Copyright (c) Microsoft Corporation. All rights reserved. #This script is made available to you without any express, implied or statutory warranty, not even the implied warranty of merchantability or fitness for a particular purpose, or the warranty of title or non-infringement. The entire risk of the use or the results from the use of this script remains with you.
    #
    #
    #
    #
    #Declare variables
    $VMName = 'CP1'
    $Switch = 'External'
    $InstallMedia = 'D:\ISO\en_windows_server_2016_updated_feb_2018_x64_dvd_11636692.iso'
    $Path = 'D:\VM'
    $VHDPath = 'D:\VM\CP1\CP1.vhdx'
    $VHDSize = '64424509440'

    #Create New Virtual Machine
    New-VM -Name $VMName -MemoryStartupBytes 16GB -BootDevice VHD -Path $Path -NewVHDPath $VHDPath -NewVHDSizeBytes $VHDSize  -Generation 2 -Switch $Switch  

    #Set the memory to be non-dynamic
    Set-VMMemory $VMName -DynamicMemoryEnabled $false

    #Add DVD Drive to Virtual Machine
    Add-VMDvdDrive -VMName $VMName -ControllerNumber 0 -ControllerLocation 1 -Path $InstallMedia

    #Mount Installation Media
    $DVDDrive = Get-VMDvdDrive -VMName $VMName

    #Configure Virtual Machine to Boot from DVD
    Set-VMFirmware -VMName $VMName -FirstBootDevice $DVDDrive
    ```

### <a name="complete-the-operating-system-deployment"></a>İşletim sistemi dağıtımını tamamlama
Sanal makine bina bitirmek için, işletim sistemi yükleme bitirmek gerekir.

1. Hyper-V Manager, sanal makineye çift tıklayın
2. Başlat düğmesine tıklayın.
3. 'CD veya DVD'den önyükleme için herhangi bir tuşa basın' sizden istenir. Devam et ve yap.
4. Windows Server başlangıç ekranında dilinizi seçin ve **İleri'yi**tıklatın.
5. **Şimdi Yükle'yi**tıklatın.
6. Lisans anahtarınızı girin ve **İleri'yi**tıklatın.
7. Kontrol **Lisans koşullarını kabul ediyorum ve **İleri'yi**tıklatın.
8. **Özel'i seçin: Yalnızca Windows'u Yükle (Gelişmiş)**
9. **İleri'yi** tıklatın
10. Yükleme tamamlandıktan sonra sanal makineyi yeniden başlatın, VM'nin en güncel olduğundan emin olmak için Windows güncelleştirmelerini oturum açın ve çalıştırın.  En son güncelleştirmeleri yükleyin.

### <a name="install-active-directory-prerequisites"></a>Etkin Dizin önkoşulları yükleyin
Artık sanal bir makineniz olduğuna göre, Active Directory'yi yüklemeden önce birkaç şey yapmanız gerekir.  Diğer bir süre, sanal makineyi yeniden adlandırmanız, statik bir IP adresi ve DNS bilgileri ayarlamanız ve Uzaktan Sunucu Yönetimi araçlarını yüklemeniz gerekir.   Şunları yapın:

1. PowerShell ISE'yi Yönetici olarak açın.
2. Aşağıdaki komut dosyasını çalıştırın.

    ```powershell
    # Filename:    2_ADPrep_CP.ps1
    # Description: Prepares your environment for Active Directory.  This is part of
    #              the Azure AD Connect password hash sync tutorial.
    #
    # DISCLAIMER:
    # Copyright (c) Microsoft Corporation. All rights reserved. This 
    # script is made available to you without any express, implied or 
    # statutory warranty, not even the implied warranty of 
    # merchantability or fitness for a particular purpose, or the 
    # warranty of title or non-infringement. The entire risk of the 
    # use or the results from the use of this script remains with you.
    #
    #
    #
    #
    #Declare variables
    $ipaddress = "10.0.1.118" 
    $ipprefix = "24" 
    $ipgw = "10.0.1.1" 
    $ipdns = "10.0.1.118"
    $ipdns2 = "8.8.8.8" 
    $ipif = (Get-NetAdapter).ifIndex 
    $featureLogPath = "c:\poshlog\featurelog.txt" 
    $newname = "CP1"
    $addsTools = "RSAT-AD-Tools" 

    #Set static IP address
    New-NetIPAddress -IPAddress $ipaddress -PrefixLength $ipprefix -InterfaceIndex $ipif -DefaultGateway $ipgw 

    #Set the DNS servers
    Set-DnsClientServerAddress -InterfaceIndex $ipif -ServerAddresses ($ipdns, $ipdns2)

    #Rename the computer 
    Rename-Computer -NewName $newname -force 

    #Install features 
    New-Item $featureLogPath -ItemType file -Force 
    Add-WindowsFeature $addsTools 
    Get-WindowsFeature | Where installed >>$featureLogPath 

    #Restart the computer 
    Restart-Computer
    ```
### <a name="create-a-windows-server-ad-environment"></a>Windows Server AD ortamı oluşturma
Artık VM oluşturulduve yeniden adlandırıldı ve statik bir IP adresi var, devam edin ve yüklemek ve Active Directory Etki Alanı Hizmetleri yapılandırmak.  Şunları yapın:

1. PowerShell ISE'yi Yönetici olarak açın.
2. Aşağıdaki komut dosyasını çalıştırın.

    ```powershell
    # Filename:    3_InstallAD_CP.ps1
    # Description: Creates an on-premises AD envrionment.  This is part of
    #              the Azure AD Connect password hash sync tutorial.
    #
    # DISCLAIMER:
    # Copyright (c) Microsoft Corporation. All rights reserved. This 
    # script is made available to you without any express, implied or 
    # statutory warranty, not even the implied warranty of 
    # merchantability or fitness for a particular purpose, or the 
    # warranty of title or non-infringement. The entire risk of the 
    # use or the results from the use of this script remains with you.
    #
    #
    #
    #
    #Declare variables
    $DatabasePath = "c:\windows\NTDS"
    $DomainMode = "WinThreshold"
    $DomainName = "fabrikam.com"
    $DomaninNetBIOSName = "FABRIKAM"
    $ForestMode = "WinThreshold"
    $LogPath = "c:\windows\NTDS"
    $SysVolPath = "c:\windows\SYSVOL"
    $featureLogPath = "c:\poshlog\featurelog.txt" 
    $Password = "Pass1w0rd"
    $SecureString = ConvertTo-SecureString $Password -AsPlainText -Force

    #Install AD DS, DNS and GPMC 
    start-job -Name addFeature -ScriptBlock { 
    Add-WindowsFeature -Name "ad-domain-services" -IncludeAllSubFeature -IncludeManagementTools 
    Add-WindowsFeature -Name "dns" -IncludeAllSubFeature -IncludeManagementTools 
    Add-WindowsFeature -Name "gpmc" -IncludeAllSubFeature -IncludeManagementTools } 
    Wait-Job -Name addFeature 
    Get-WindowsFeature | Where installed >>$featureLogPath

    #Create New AD Forest
    Install-ADDSForest -CreateDnsDelegation:$false -DatabasePath $DatabasePath -DomainMode $DomainMode -DomainName $DomainName -SafeModeAdministratorPassword $SecureString -DomainNetbiosName $DomainNetBIOSName -ForestMode $ForestMode -InstallDns:$true -LogPath $LogPath -NoRebootOnCompletion:$false -SysvolPath $SysVolPath -Force:$true
    ```

### <a name="create-a-windows-server-ad-user"></a>Windows Server AD kullanıcısı oluşturma
Artık Aktif Dizin ortamımıza sahip olduğunuza göre, bir test hesabına ihtiyacınız var.  Bu hesap şirket içi REKLAM ortamımızda oluşturulur ve azure AD ile senkronize edilir.  Şunları yapın:

1. PowerShell ISE'yi Yönetici olarak açın.
2. Aşağıdaki komut dosyasını çalıştırın.

    ```powershell 
    # Filename:    4_CreateUser_CP.ps1
    # Description: Creates a user in Active Directory.  This is part of
    #              the Azure AD Connect password hash sync tutorial.
    #
    # DISCLAIMER:
    # Copyright (c) Microsoft Corporation. All rights reserved. This 
    # script is made available to you without any express, implied or 
    # statutory warranty, not even the implied warranty of 
    # merchantability or fitness for a particular purpose, or the 
    # warranty of title or non-infringement. The entire risk of the 
    # use or the results from the use of this script remains with you.
    #
    #
    #
    #
    #Declare variables
    $Givenname = "Anna"
    $Surname = "Ringdal"
    $Displayname = "Anna Ringdal"
    $Name = "aringdal"
    $Password = "Pass1w0rd"
    $Identity = "CN=aringdal,CN=Users,DC=fabrikam,DC=com"
    $SecureString = ConvertTo-SecureString $Password -AsPlainText -Force


    #Create the user
    New-ADUser -Name $Name -GivenName $Givenname -Surname $Surname -DisplayName $Displayname -AccountPassword $SecureString

    #Set the password to never expire
    Set-ADUser -Identity $Identity -PasswordNeverExpires $true -ChangePasswordAtLogon $false -Enabled $true
    ```

## <a name="conclusion"></a>Sonuç
Artık varolan öğreticiler için kullanılabilecek ve bulut sağlamanın sağladığı ek özellikleri sınamak için kullanılabilen bir ortama sahipsiniz.

## <a name="next-steps"></a>Sonraki adımlar 

- [Sağlama nedir?](what-is-provisioning.md)
- [Azure AD Connect bulut sağlama nedir?](what-is-cloud-provisioning.md)
