---
title: FSLogix profili kapsayıcısı Azure dosyaları oluşturma Active Directory Domain Services-Azure
description: Bu makalede, Azure dosyaları ve Azure Active Directory Domain Services ile FSLogix profil kapsayıcısının nasıl oluşturulacağı açıklanır.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 04/10/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 916d34abfaf8223e3cf29977e13dfddf15a3fbf9
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2020
ms.locfileid: "82607291"
---
# <a name="create-an-fslogix-profile-container-with-azure-files"></a>Azure dosyaları ile FSLogix profil kapsayıcısı oluşturma

Bu makalede, Azure dosyaları ve Azure Active Directory Domain Services (AD DS) ile bir FSLogix profil kapsayıcısı oluşturma gösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar

Bu makalede, zaten bir Azure AD DS örneği ayarlamış olduğunuz varsayılmaktadır. Henüz bir tane yoksa, önce [temel yönetilen etki alanı oluşturma](../active-directory-domain-services/tutorial-create-instance.md) bölümündeki yönergeleri izleyin, sonra buraya geri dönün.

## <a name="add-azure-ad-ds-admins"></a>Azure AD DS yöneticileri ekleme

Ek yöneticiler eklemek için yeni bir kullanıcı oluşturur ve bunlara izinler verirsiniz.

Yönetici eklemek için:

1. Kenar çubuğundan **Azure Active Directory** seçin, ardından **tüm kullanıcılar**' ı seçin ve ardından **Yeni Kullanıcı**' yı seçin.

2.  Alanlara Kullanıcı ayrıntılarını girin.

3. Ekranın sol tarafındaki Azure Active Directory bölmesinde, **gruplar**' ı seçin.

4. **AAD DC Yöneticiler** grubunu seçin.

5. Sol bölmede **Üyeler**' i ve ardından ana bölmedeki **üye Ekle** ' yi seçin. Bu, Azure AD 'de mevcut olan tüm kullanıcıların listesini gösterir. Yeni oluşturduğunuz Kullanıcı profilinin adını seçin.

## <a name="set-up-an-azure-storage-account"></a>Azure depolama hesabı ayarlama

Artık, sunucu Ileti bloğu (SMB) üzerinden Azure AD DS kimlik doğrulamasını etkinleştirme zamanı. 

Kimlik doğrulamasını etkinleştirmek için:

1. Henüz yapmadıysanız, [Azure Storage hesabı oluşturma](../storage/common/storage-account-create.md)' daki yönergeleri izleyerek genel amaçlı v2 Azure Storage hesabı ayarlayıp dağıtın.

2. Hesabınızı ayarlamayı tamamladıktan sonra **Kaynağa Git**' i seçin.

3. Ekranın sol tarafındaki bölmeden **yapılandırma** ' yı seçin, sonra ana bölmedeki **Azure dosyaları için Azure Active Directory kimlik doğrulamasını** etkinleştirin. İşiniz bittiğinde **Kaydet**'i seçin.

4. Ekranın sol tarafındaki bölmede **genel bakış** ' ı seçin, sonra ana bölmedeki **dosyalar** ' ı seçin.

5. **Dosya paylaşma** ' yı seçin ve ekranın sağ tarafında görüntülenen alanlara **adı** ve **kotayı** girin.

## <a name="assign-access-permissions-to-an-identity"></a>Kimliğe erişim izinleri atama

Diğer kullanıcılara, dosya paylaşımınıza erişmek için erişim izinleri gerekecektir. Bunu yapmak için, her kullanıcıya uygun erişim izinlerine sahip bir rol atamanız gerekir.

Kullanıcılara erişim izinleri atamak için:

1. Azure portal, [Azure depolama hesabı ayarlama](#set-up-an-azure-storage-account)bölümünde oluşturduğunuz dosya paylaşımından açın.

2. **Access Control (IAM)** seçeneğini belirleyin.

3. **Rol ataması Ekle**' yi seçin.

4. **Rol ataması Ekle** sekmesinde rol listesinden uygun yerleşik rolü seçin. Uygun izinleri almak için hesap için en az **depolama dosya VERI SMB paylaşımında katkıda bulunan** ' ı seçmeniz gerekir.

5. **Erişim atama**için **Kullanıcı, Grup veya hizmet sorumlusu Azure Active Directory**seçin.

6. Hedef Azure Active Directory kimliği için bir ad veya e-posta adresi seçin.

7. **Kaydet**’i seçin.

## <a name="get-the-storage-account-access-key"></a>Depolama hesabı erişim anahtarını al

Daha sonra, depolama hesabınızın erişim anahtarını almanız gerekir.

Depolama hesabı erişim anahtarını almak için:

1. Azure portal kenar çubuğundan **depolama hesapları**' nı seçin.

2. Depolama hesapları listesinden, Azure AD DS 'yi etkinleştirdiğiniz hesabı seçin ve yukarıdaki adımlarda özel rolleri oluşturdunuz.

3. **Ayarlar**altında **erişim anahtarları** ' nı seçin ve anahtarı **KEY1**' dan kopyalayın.

4. **Sanal makineler** sekmesine gidin ve konak havuzunuzun parçası olacak tüm VM 'leri bulun.

5. **Sanal makineler (adVM)** altında sanal makınenın (VM) adını seçin ve **Bağlan** ' ı seçin.

    Bu işlem, VM 'de kendi kimlik bilgileriyle oturum açmanıza olanak sağlayan bir RDP dosyası indirir.

    ![Sanal makineye bağlan penceresinin RDP sekmesinin ekran görüntüsü.](media/rdp-tab.png)

6. VM 'de oturum açtıktan sonra yönetici olarak bir komut istemi çalıştırın.

7. Şu komutu çalıştırın:

     ```cmd
     net use <desired-drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name> <storage-account-key> /user:Azure\<storage-account-name>
     ```

    - Tercih `<desired-drive-letter>` ettiğiniz bir sürücü harfiyle değiştirin (örneğin, `y:`).
    - Öğesinin tüm örneklerini, `<storage-account-name>` daha önce belirttiğiniz depolama hesabının adıyla değiştirin.
    - Daha `<share-name>` önce oluşturduğunuz paylaşımın adıyla değiştirin.
    - Azure `<storage-account-key>` 'daki depolama hesabı anahtarıyla değiştirin.

    Örneğin:  
  
     ```cmd
     net use y: \\fsprofile.file.core.windows.net\share HDZQRoFP2BBmoYQ=(truncated)= /user:Azure\fsprofile)
     ```

8. Kullanıcıya Azure dosya paylaşımının tam erişimini sağlamak için aşağıdaki komutu çalıştırın.

     ```cmd
     icacls <mounted-drive-letter>: /grant <user-email>:(f)
     ```

    - Kullanıcının `<mounted-drive-letter>` kullanmasını istediğiniz sürücünün harfiyle değiştirin.
    - Oturum `<user-email>` ana bilgisayar VM 'lerine erişmek için bu profili kullanacak olan kullanıcının UPN 'si ile değiştirin.

    Örneğin:
     
     ```cmd
     icacls y: /grant john.doe@contoso.com:(f)
     ```

## <a name="create-a-profile-container"></a>Profil kapsayıcısı oluşturma

Artık profilleriniz çalışmaya hazır olduğuna göre, bir FSLogix profil kapsayıcısı oluşturalım.

FSLogix profil kapsayıcısını yapılandırmak için:

1. Bu makalenin başlangıcında yapılandırdığınız oturum ana bilgisayarında oturum açın, ardından [FSLogix aracısını indirip yükleyin](/fslogix/install-ht/).

2. İndirdiğiniz fslogix aracı dosyasını açın ve **x64** > **sürümleri**' ne gidin ve ardından **fslogixappssetup. exe**' yi açın.

3. Yükleyici başlatıldıktan sonra **Lisans hüküm ve koşullarını kabul** ediyorum ' u seçin. Uygulanabiliyorsa, yeni bir anahtar sağlayın.

4. **Yükle**’yi seçin.

5. Fslogix aracısının düzgün yüklendiğinden emin olmak için **sürücü C**'yi açın ve **Program Files** > **fslogix** > **uygulamalarına** gidin.

     >[!NOTE]
     > Konak havuzunda birden çok VM varsa, her VM için 1 ile 5 arasındaki adımları tekrarlamanız gerekir.

6. **Kayıt defteri Düzenleyicisi 'ni** (regedit) yönetici olarak çalıştırın.

7. **Bilgisayar** > **HKEY_LOCAL_MACHINE**HKEY_LOCAL_MACHINE > **software**yazılım > **fslogix**' e gidin, **fslogix**öğesine sağ tıklayın, **Yeni**' yi seçin ve ardından **anahtar**' ı seçin.

8. **Profiller**adlı yeni bir anahtar oluşturun.

9.  **Profiller**' e sağ tıklayın, **Yeni**' yi ve ardından **DWORD (32-bit) değeri** ' ni seçin. Değeri **etkin** olarak adlandırın ve **veri** değerini **1**olarak ayarlayın.

    ![Profiller anahtarının ekran görüntüsü. REG_DWORD dosyası vurgulanır ve veri değeri 1 olarak ayarlanır.](media/dword-value.png)

10. **Profiller**' e sağ tıklayın, **Yeni**' yi seçin ve sonra **çok dizeli değer**' i seçin. Değer **Vhdkonumlarını** adlandırın ve veri değeri olarak Azure dosya paylaşımının `\\fsprofile.file.core.windows.net\share` URI 'sini girin.

    ![VHDLocations dosyasını gösteren profiller anahtarının ekran görüntüsü. Veri değeri, Azure dosya paylaşımının URI 'sini gösterir.](media/multi-string-value.png)

## <a name="assign-users-to-a-session-host"></a>Kullanıcıları bir oturum konağına atama

Artık oturum ana bilgisayarınıza Kullanıcı atamanız gerekir.

Kullanıcıları atamak için:

1. Windows PowerShell 'i yönetici olarak çalıştırın ve ardından PowerShell ile Windows sanal masaüstü 'nde oturum açmak için aşağıdaki cmdlet 'i çalıştırın:

   ```powershell
   Import-Module Microsoft.RdInfra.RdPowershell

   #Optional
   Install-Module Microsoft.RdInfra.RdPowershell

   $brokerurl = "https://rdbroker.wvd.microsoft.com"

   Add-RdsAccount -DeploymentUrl $brokerurl
   ```

   Kimlik bilgileri istendiğinde, Windows sanal masaüstü kiracısında, TenantCreator, RDS Owner veya RDS katılımcısı rolüne verilen aynı kullanıcıyı girin.

2. Kullanıcıyı uzak masaüstü grubuna atamak için aşağıdaki cmdlet 'leri çalıştırın:

     ```powershell
     $tenant = "<your-wvd-tenant>"

     $pool1 = "<wvd-pool>"

     $appgroup = "Desktop Application Group"

     $user1 = "<user-principal>"

     Add-RdsAppGroupUser $tenant $pool1 $appgroup $user1
     ```

    Önceki cmdlet 'ler gibi,, ve `<your-wvd-tenant>` `<wvd-pool>` `<user-principal>` değerlerini de ilgili değerlerle değiştirdiğinizden emin olun.

    Örneğin:

     ```powershell
     $pool1 = "contoso"
     
     $tenant = "contoso"
     
     $appgroup = "Desktop Application Group"
     
     $user1 = "jane.doe@contoso.com"
     
     Add-RdsAppGroupUser $tenant $pool1 $appgroup $user1
     ```

## <a name="make-sure-your-profile-works"></a>Profilinizin çalıştığından emin olun

Şimdi tüm yapmanız gerekir, oluşturduğunuz profilin varolduğundan ve hedeflenen şekilde çalıştığından emin olun.

Profilinizi doğrulamak için:

1. Bir tarayıcı açın ve [Windows Sanal Masaüstü Web istemcisine](https://rdweb.wvd.microsoft.com/arm/webclient)gidin.

2. Uzak Masaüstü grubuna atanan kullanıcı hesabıyla oturum açın.

3. Kullanıcı oturumu kurulduktan sonra, Azure portal açın ve bir yönetici hesabıyla oturum açın.

4. Kenar çubuğundan **depolama hesapları**' nı seçin.

5. Oturum Ana bilgisayar havuzunuz için dosya paylaşma olarak yapılandırdığınız ve Azure AD DS etkinleştirilmiş depolama hesabını seçin.

6. **Dosyalar** simgesini seçin, sonra paylaşımınızı genişletin.

    Her şey doğru şekilde ayarlandıysa, şöyle biçimlendirilen bir ada sahip bir **Dizin** görmeniz gerekir: `<user SID>-<username>`.

## <a name="next-steps"></a>Sonraki adımlar

FSLogix profil kapsayıcıları oluşturmanın farklı yollarını arıyorsanız, aşağıdaki makalelere göz atın:

- Bir [dosya paylaşımının kullanıldığı konak havuzu için bir profil kapsayıcısı oluşturun](create-host-pools-user-profile.md).
- [Azure NetApp Files kullanarak bir konak havuzu için FSLogix profil kapsayıcısı oluşturma](create-fslogix-profile-container.md)

[Fslogix profil kapsayıcılarındaki ve Azure dosyalarındaki](fslogix-containers-azure-files.md)Azure dosyaları Için fslogix kapsayıcılarıyla ilgili daha ayrıntılı bilgi edinebilirsiniz.
