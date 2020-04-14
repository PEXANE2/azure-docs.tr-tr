---
title: FSLogix profil kapsayıcısı oluşturma Azure Dosyaları Etkin Dizin Etki Alanı Hizmetleri - Azure
description: Bu makalede, Azure Dosyaları ve Azure Etkin Dizin Etki Alanı Hizmetleri içeren bir FSLogix profil kapsayıcısı nasıl oluşturulacak açıklanmaktadır.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 04/10/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: dd01b950435fadb96a961b6bb1c6b28ff436907a
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81265778"
---
# <a name="create-an-fslogix-profile-container-with-azure-files"></a>Azure Dosyaları ile Bir FSLogix profil kapsayıcısı oluşturma

Bu makalede, Azure Dosyaları ve Azure Etkin Dizin Etki Alanı Hizmetleri (AD DS) içeren bir FSLogix profil kapsayıcısı nasıl oluşturulacağı gösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar

Bu makalede, zaten bir Azure AD DS örneği ayarladınız varsayar. Henüz bir tane yoksa, önce temel [yönetilen etki alanı oluştur'daki](../active-directory-domain-services/tutorial-create-instance.md) yönergeleri izleyin, ardından buraya dönün.

## <a name="add-azure-ad-ds-admins"></a>Azure AD DS yöneticileri ekleme

Ek yöneticiler eklemek için yeni bir kullanıcı oluşturun ve onlara izin ler verirsiniz.

Yönetici eklemek için:

1. Kenar çubuğundan **Azure Etkin Dizini'ni** seçin, ardından Tüm **kullanıcılar'ı**seçin ve ardından Yeni **kullanıcıyı**seçin.

2.  Alanlarına kullanıcı ayrıntılarını girin.

3. Ekranın sol tarafındaki Azure Etkin Dizin bölmesinde **Gruplar'ı**seçin.

4. **AAD DC Yöneticileri** grubunu seçin.

5. Sol bölmede **Üyeler'i**seçin ve ardından ana bölmeye **üye ekle'yi** seçin. Bu, Azure AD'de kullanılabilen tüm kullanıcıların listesini gösterir. Oluşturduğunuz kullanıcı profilinin adını seçin.

## <a name="set-up-an-azure-storage-account"></a>Azure Depolama hesabı ayarlama

Şimdi Sunucu İleti Bloğu (SMB) üzerinden Azure AD DS kimlik doğrulamasını etkinleştirme zamanı. 

Kimlik doğrulamayı etkinleştirmek için:

1. Azure [Depolama hesabı oluştur'daki](../storage/common/storage-account-create.md)yönergeleri izleyerek genel amaçlı bir v2 Azure Depolama hesabı ayarlayın ve dağıtın.

2. Hesabınızı ayarlamayı bitirdikten sonra **kaynağa git'i**seçin.

3. Ekranın sol tarafındaki bölmeden **Yapılandırma'yı** seçin ve ardından ana bölmedeki **Azure Dosyaları için Azure Etkin Dizin kimlik doğrulamasını** etkinleştirin. İşiniz bittiğinde **Kaydet**'i seçin.

4. Ekranın sol tarafındaki bölmede **Genel Bakış'ı** seçin ve ardından ana bölmedeki **Dosyalar'ı** seçin.

5. **Dosya paylaşımı'nı** seçin ve ekranın sağ tarafında görünen alanlara **Ad** ve **Kota** girin.

## <a name="assign-access-permissions-to-an-identity"></a>Bir kimliğe erişim izinleri atama

Diğer kullanıcıların dosya paylaşımınıza erişmek için erişim izinlerine ihtiyacı vardır. Bunu yapmak için, her kullanıcıya uygun erişim izinlerine sahip bir rol atamanız gerekir.

Kullanıcılara erişim izinleri atamak için:

1. Azure portalından, Bir Azure Depolama [hesabı oluşturma'da](#set-up-an-azure-storage-account)oluşturduğunuz dosya paylaşımını açın.

2. **Erişim Denetimi 'ni (IAM)** seçin.

3. **Rol ataması ekle'yi**seçin.

4. Rol **atamaekle** sekmesinde, rol listesinden uygun yerleşik rolü seçin. Uygun izinleri almak için hesap için en azından **Depolama Dosyası Veri SMB Share Contributor'ı** seçmeniz gerekir.

5. **Access'e erişim atamak**için **Azure Active Directory kullanıcısını, grubunu veya hizmet yöneticisini**seçin.

6. Hedef Azure Etkin Dizin kimliği için bir ad veya e-posta adresi seçin.

7. **Kaydet'i**seçin.

## <a name="get-the-storage-account-access-key"></a>Depolama Hesabı erişim anahtarını alın

Ardından, Depolama Hesabınız için erişim anahtarını almanız gerekir.

Depolama Hesabı erişim anahtarını almak için:

1. Azure portalı kenar çubuğundan **Depolama hesapları'nı**seçin.

2. Depolama hesapları listesinden, Azure AD DS'yi etkinleştirdiğiniz ve yukarıdaki adımlarda özel rolleri oluşturduğunuz hesabı seçin.

3. **Ayarlar** **altında, Access tuşlarını** seçin ve **tuşu kopyalayın1**.

4. **Sanal Makineler** sekmesine gidin ve ana bilgisayar havuzunuzun bir parçası olacak herhangi bir VM'yi bulun.

5. **Sanal Makineler (adVM)** altında sanal makinenin (VM) adını seçin ve **Bağlan'ı** seçin

    Bu, VM'de kendi kimlik bilgileriyle oturum açmanızı sağlayan bir RDP dosyasını karşıdan yükler.

    ![Sanal makineye Bağlan penceresinin RDP sekmesinin ekran görüntüsü.](media/rdp-tab.png)

6. VM'de oturum açtığınızda, yönetici olarak bir komut istemi çalıştırın.

7. Şu komutu çalıştırın:

     ```cmd
     net use <desired-drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name> <storage-account-key> /user:Azure\<storage-account-name>
     ```

    - Seçtiğiniz `<desired-drive-letter>` bir sürücü harfiyle değiştirin `y:`(örneğin, ).
    - Tüm örnekleri `<storage-account-name>` daha önce belirttiğiniz depolama hesabının adıyla değiştirin.
    - Daha `<share-name>` önce oluşturduğunuz paylaşımın adıyla değiştirin.
    - Azure'daki depolama hesabı anahtarıyla değiştirin. `<storage-account-key>`

    Örneğin:  
  
     ```cmd
     net use y: \\fsprofile.file.core.windows.net\share HDZQRoFP2BBmoYQ=(truncated)= /user:Azure\fsprofile)
     ```

8. Kullanıcıya Azure Dosyaları paylaşımına tam erişim hakkı vermek için aşağıdaki komutu çalıştırın.

     ```cmd
     icacls <mounted-drive-letter>: /grant <user-email>:(f)
     ```

    - Kullanıcının kullanmasını istediğiniz sürücünün harfiyle değiştirin. `<mounted-drive-letter>`
    - Oturum `<user-email>` ana bilgisayar VM'lerine erişmek için bu profili kullanacak kullanıcının UPN'i ile değiştirin.

    Örneğin:
     
     ```cmd
     icacls y: /grant john.doe@contoso.com:(f)
     ```

## <a name="create-a-profile-container"></a>Profil kapsayıcısı oluşturma

Artık profilleriniz kullanıma hazır olduğuna göre, bir FSLogix profil konteyneri oluşturalım.

Bir FSLogix profil kapsayıcısını yapılandırmak için:

1. Bu makalenin başında yapılandırdığınız oturum ana bilgisayar VM oturum, sonra [indirin ve FSLogix aracısını yükleyin.](/fslogix/install-ht/)

2. İndirdiğiniz FSLogix aracı dosyasıun zip'ini açın ve **x64** > **Bültenleri'ne**gidin, ardından **FSLogixAppsSetup.exe'yi**açın.

3. Yükleyici başlattıktan sonra, **ben lisans şart ve koşullarını kabul seçin.** Varsa, yeni bir anahtar sağlayın.

4. **Yükle**’yi seçin.

5. **Sürücü C'yi**açın, ardından FSLogix aracısının düzgün bir şekilde yüklendiğinden emin olmak için **Program Files** > **FSLogix** > **Apps'a** gidin.

     >[!NOTE]
     > Ana bilgisayar havuzunda birden çok VM varsa, her VM için 1'den 5'e kadar olan adımları yinelemeniz gerekir.

6. **Kayıt Defteri Düzenleyicisi'ni** (RegEdit) yönetici olarak çalıştırın.

7. **Bilgisayar** > **HKEY_LOCAL_MACHINE** > **yazılımı** > **FSLogix**gidin , **FSLogix**sağ tıklayın , **Yeni**seçin , ve sonra **Anahtar**seçin .

8. **Profiller**adlı yeni bir anahtar oluşturun.

9.  **Profiller'e**sağ tıklayın, **Yeni'yi**seçin ve ardından **DWORD (32 bit) Değeri'ni seçin.** **Etkin** değeri adlandırın ve **Veri** değerini **1**olarak ayarlayın.

    ![Profiller anahtarının ekran görüntüsü. REG_DWORD dosyası vurgulanır ve Veri değeri 1 olarak ayarlanır.](media/dword-value.png)

10. **Profiller'e**sağ tıklayın, **Yeni'yi**seçin ve ardından **Multi-String Değeri'ni**seçin. **VHDLocations** değerini adlandırın ve Veri değeri olarak `\\fsprofile.file.core.windows.net\share` Azure Dosyaları paylaşımı için URI'yi ayarlayın.

    ![VHDLocations dosyasını gösteren Profiller anahtarının ekran görüntüsü. Veri değeri, Azure Dosyaları paylaşımı için URI'yi gösterir.](media/multi-string-value.png)

## <a name="assign-users-to-a-session-host"></a>Kullanıcıları oturum ana bilgisayara atama

Artık kullanıcıları oturum ana bilgisayarınıza atamanız gerekir.

Kullanıcıları atamak için:

1. Windows PowerShell'i yönetici olarak çalıştırın ve ardından PowerShell ile Windows Sanal Masaüstünde oturum açmaiçin aşağıdaki cmdlet'i çalıştırın:

   ```powershell
   Import-Module Microsoft.RdInfra.RdPowershell

   #Optional
   Install-Module Microsoft.RdInfra.RdPowershell

   $brokerurl = "https://rdbroker.wvd.microsoft.com"

   Add-RdsAccount -DeploymentUrl $brokerurl
   ```

   Kimlik bilgileri istendiğinde, Windows Sanal Masaüstü kiracısına TenantCreator, RDS Owner veya RDS Katılımcı sıfatı verilen kullanıcıyı girin.

2. Kullanıcıyı uzak masaüstü grubuna atamak için aşağıdaki cmdlets'i çalıştırın:

     ```powershell
     $tenant = "<your-wvd-tenant>"

     $pool1 = "<wvd-pool>"

     $appgroup = "Desktop Application Group"

     $user1 = "<user-principal>"

     Add-RdsAppGroupUser $tenant $pool1 $appgroup $user1
     ```

    Önceki cmdlets gibi, değiştirmek `<your-wvd-tenant>`için `<wvd-pool>`emin `<user-principal>` olun , , ve ilgili değerleri ile.

    Örneğin:

     ```powershell
     $pool1 = "contoso"
     
     $tenant = "contoso"
     
     $appgroup = "Desktop Application Group"
     
     $user1 = "jane.doe@contoso.com"
     
     Add-RdsAppGroupUser $tenant $pool1 $appgroup $user1
     ```

## <a name="make-sure-your-profile-works"></a>Profilinizin çalıştığından emin olun

Şimdi tek yapmanız gereken oluşturduğunuz profilin var olduğundan ve beklendiği gibi çalıştığından emin olmaktır.

Profilinizi doğrulamak için:

1. Bir tarayıcı açın ve [Windows Sanal Masaüstü web istemcisine](https://rdweb.wvd.microsoft.com/webclient/index.html)gidin.

2. Uzak Masaüstü grubuna atanan kullanıcı hesabıyla oturum açın.

3. Kullanıcı oturumu oluşturulduktan sonra Azure portalını açın ve yönetim hesabıyla oturum açın.

4. Kenar çubuğundan **Depolama hesapları'nı**seçin.

5. Oturum ana bilgisayar havuzunuz için dosya paylaşımı olarak yapılandırdığınız ve Azure AD DS ile etkinleştirdiğiniz depolama hesabını seçin.

6. **Dosyalar** simgesini seçin ve ardından paylaşımınızı genişletin.

    Her şey doğru ayarlanmışsa, şu şekilde biçimlendirilmiş bir adiçeren bir **Dizin** görmeniz gerekir: `<user SID>-<username>`.

## <a name="next-steps"></a>Sonraki adımlar

FSLogix profil kapsayıcıları oluşturmak için alternatif yollar arıyorsanız, aşağıdaki makalelere göz atın:

- [Dosya paylaşımını kullanarak ana bilgisayar havuzu için profil kapsayıcısı oluşturun.](create-host-pools-user-profile.md)
- [Azure NetApp Dosyalarını kullanarak ana bilgisayar havuzu için FSLogix profil kapsayıcısı oluşturma](create-fslogix-profile-container.md)

[FSLogix profil kapsayıcıları ve Azure dosyaları](fslogix-containers-azure-files.md)için FSlogix kapsayıcıları ile ilgili kavramlar hakkında daha ayrıntılı bilgi bulabilirsiniz.
