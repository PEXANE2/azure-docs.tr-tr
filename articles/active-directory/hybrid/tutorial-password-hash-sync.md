---
title: "Öğretici: PHS kullanarak tek bir AD ormanını Azure' a entegre etme"
description: Parola karma eşitlemeyi kullanarak karma kimlik ortamının nasıl kurulabildiğini gösterir.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/31/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: b17300fa69b61c7713c860e2a35e63fcb6584bc4
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "66474007"
---
# <a name="tutorial--integrate-a-single-ad-forest-using-password-hash-sync-phs"></a>Öğretici: Parola karma eşitleme (PHS) kullanarak tek bir AD ormanLarını tümleştir

![Oluşturma](media/tutorial-password-hash-sync/diagram.png)

Aşağıdaki öğretici, parola karma eşitlemeyi kullanarak karma bir kimlik ortamı oluşturmada size yol gösterir.  Bu ortam daha sonra sınamak veya karma kimliğin nasıl çalıştığını daha iyi tanımak için kullanılabilir.

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticinin tamamlanması için gerekli ön koşullar aşağıda dır.
- [Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-technology-overview) yüklü bir bilgisayar.  Bunu bir Windows 10 veya [Windows Server 2016](https://docs.microsoft.com/windows-server/virtualization/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows) bilgisayarında yapmanız önerilir. [Windows 10](https://docs.microsoft.com/virtualization/hyper-v-on-windows/about/supported-guest-os)
- Sanal makinenin internetle iletişim kurmasını sağlamak için harici ağ [bağdaştırıcısı.](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/connect-to-network)
- [Azure aboneliği](https://azure.microsoft.com/free)
- Windows Server 2016'nın bir kopyası

> [!NOTE]
> Bu öğretici, en kısa sürede öğretici ortamı oluşturabilmeniz için PowerShell komut dosyalarını kullanır.  Komut dosyalarının her biri, komut dosyalarının başında bildirilen değişkenleri kullanır.  Değişkenleri ortamınızı yansıtacak şekilde değiştirebilirsiniz ve değiştirmelisiniz.
>
>Kullanılan komut dosyaları, Azure AD Connect'i yüklemeden önce genel bir Etkin Dizin ortamı oluşturur.  Tüm öğreticiler için geçerlidir.
>
> Bu eğitimde kullanılan PowerShell komut dosyalarının kopyalarını [Burada](https://github.com/billmath/tutorial-phs)GitHub'da bulabilirsiniz.

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma
Hibrid kimlik ortamımızı çalışır hale getirmek için yapmamız gereken ilk şey, şirket içi Active Directory sunucumuz olarak kullanılacak sanal bir makine oluşturmaktır.  Şunları yapın:

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
Artık sanal bir makinemiz olduğuna göre, Active Directory'yi yüklemeden önce birkaç şey yapmamız gerekiyor.  Diğer bir de sanal makinenin adını yeniden adlandırmamız, statik bir IP adresi ve DNS bilgileri ayarlamamız ve Uzaktan Sunucu Yönetimi araçlarını yüklememiz gerekir.   Şunları yapın:

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
Şimdi VM oluşturulan ve yeniden adlandırıldı ve statik bir IP adresi vardır, biz devam edin ve yüklemek ve Aktif Dizin Etki Alanı Hizmetleri yapılandırmak.  Şunları yapın:

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
Artık Aktif Dizin ortamımız olduğuna göre, bir test hesabına ihtiyacımız var.  Bu hesap şirket içi REKLAM ortamımızda oluşturulur ve azure AD ile senkronize edilir.  Şunları yapın:

1. PowerShell ISE'yi Yönetici olarak açın.
2. Aşağıdaki komut dosyasını çalıştırın.

```powershell 
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
Şimdi, kullanıcılarımızı bulutla senkronize edebilmemiz için bir Azure AD kiracıoluşturmamız gerekiyor.  Yeni bir Azure AD kiracısı oluşturmak için aşağıdaki adımları uygulayın.

1. [Azure portalına](https://portal.azure.com) gidip Azure aboneliği olan bir hesapla oturum açın.
2. **Artı simgesini (+)** seçip **Azure Active Directory** terimini aratın.
3. Arama sonuçlarında **Azure Active Directory** girişini seçin.
4. **Oluştur'u**seçin.</br>
![Oluşturma](media/tutorial-password-hash-sync/create1.png)</br>
5. **Kuruluş için bir ad** ve **ilk etki alanı adı** girin. Ardından **Oluştur**’u seçin. Dizininiz oluşturulur.
6. Bu işlem tamamlandıktan sonra, dizini yönetmek için **buradaki** bağlantıyı tıklatın.

## <a name="create-a-global-administrator-in-azure-ad"></a>Azure AD'de genel bir yönetici oluşturma
Artık bir Azure AD kiracımız olduğuna göre, genel bir yönetici hesabı oluşturacağız.  Bu hesap, Azure AD Connect yüklemesi sırasında Azure AD Bağlayıcısı hesabını oluşturmak için kullanılır.  Azure AD Bağlayıcısı hesabı, Azure AD'ye bilgi yazmak için kullanılır.   Genel yönetici hesabını oluşturmak için aşağıdakileri yapın.

1.  **Yönet** bölümünde **Kullanıcılar**’ı seçin.</br>
![Oluşturma](media/tutorial-password-hash-sync/gadmin1.png)</br>
2.  **Tüm kullanıcılar**'ı ve ardından **+ Yeni kullanıcı**'yı seçin.
3.  Bu kullanıcı için bir ad ve kullanıcı adı girin. Bu kullanıcı kiracınızın Genel Yöneticisi olacak. **Dizin rolünü** Global yönetici olarak da değiştirmek isteyeceksiniz. **Global administrator.** İsterseniz geçici parolayı da gösterebilirsiniz. İşiniz bittiğinde **Oluştur**'u seçin.</br>
![Oluşturma](media/tutorial-password-hash-sync/gadmin2.png)</br>
4. Bu işlem tamamlandıktan sonra, yeni bir web tarayıcısı açın ve yeni genel yönetici hesabını ve geçici parolayı kullanarak myapps.microsoft.com oturum açın.
5. Genel yöneticinin parolasını hatırlayacağınız bir şeyle değiştirin.

## <a name="download-and-install-azure-ad-connect"></a>Azure AD Connect'i indirin ve yükleyin
Şimdi Azure AD Connect'i indirme ve yükleme zamanı.  Kurulduktan sonra ekspres kurulum üzerinden çalışacaktır.  Şunları yapın:

1. [Azure AD Connect'i](https://www.microsoft.com/download/details.aspx?id=47594) İndir
2. **AzureADConnect.msi** öğesine gidin ve çift tıklayın.
3. Hoş Geldiniz ekranında, lisans koşullarını kabul ettiğinizi belirten kutuyu seçin ve **Devam**'a tıklayın.  
4. Hızlı ayarlar ekranında **Hızlı ayarları kullan**'a tıklayın.</br>  
![Oluşturma](media/tutorial-password-hash-sync/express1.png)</br>
5. Azure AD'ye Bağlan ekranına, kullanıcı adını girin ve Azure AD'nin genel yöneticisini parolayla şifrelayın. **İleri**'ye tıklayın.  
6. AD DS'ye Bağlanma ekranında kuruluş yöneticisi hesabına ilişkin kullanıcı adını ve parolayı girin. **İleri**'ye tıklayın.  
7. Yapılandırma için hazır ekranında **Yükle**'ye tıklayın.
8. Yükleme tamamlandığında **Çıkış**'a tıklayın.
9. Yükleme tamamlandıktan sonra, Eşitleme Hizmet Yöneticisi veya Eşitleme Kuralı Düzenleyicisi'ni kullanmadan önce oturum açın ve yeniden oturum açın.


## <a name="verify-users-are-created-and-synchronization-is-occurring"></a>Kullanıcıların oluşturulduğunu ve eşitlemenin oluştuğunu doğrulayın
Şimdi şirket içi dizinimizde bulunan kullanıcıların senkronize edildiğini ve artık Azure AD kiracıdışında var olduğunu doğrulayabiliyoruz.  Bunun tamamlanmasının birkaç saat sürebileceğini unutmayın.  Kullanıcıların senkronize olduğunu doğrulamak için aşağıdakileri yapın.


1. [Azure portalına](https://portal.azure.com) gidip Azure aboneliği olan bir hesapla oturum açın.
2. Solda Azure **Etkin Dizini'ni** seçin
3. **Yönet** bölümünde **Kullanıcılar**’ı seçin.
4. Kiracımızda yeni kullanıcıları gördüğünüzden doğrulama</br>
![Synch](media/tutorial-password-hash-sync/synch1.png)</br>

## <a name="test-signing-in-with-one-of-our-users"></a>Kullanıcılarımızdan biriyle oturum açma testini test edin

1. Göz atın[https://myapps.microsoft.com](https://myapps.microsoft.com)
2. Yeni kiracımızda oluşturulan bir kullanıcı hesabıyla oturum açın.  Aşağıdaki biçimi kullanarak oturum açmanız gerekir:user@domain.onmicrosoft.com( ). Kullanıcının şirket içinde oturum açmada kullandığı parolayı kullanın.</br>
   ![Doğrulama](media/tutorial-password-hash-sync/verify1.png)</br>

Azure'un sunduklarını sınamak ve tanımak için kullanabileceğiniz karma bir kimlik ortamını başarıyla kurabilirsiniz.

## <a name="next-steps"></a>Sonraki Adımlar


- [Donanım ve önkoşullar](how-to-connect-install-prerequisites.md) 
- [Ekspres ayarlar](how-to-connect-install-express.md)
- [Parola karma senkronizasyonu](how-to-connect-password-hash-synchronization.md)|
