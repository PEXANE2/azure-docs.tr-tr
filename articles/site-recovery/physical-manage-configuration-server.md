---
title: Azure Site Kurtarma'da fiziksel sunucular için yapılandırma sunucusunu yönetme
description: Bu makalede, Azure'da fiziksel sunucu olağanüstü durum kurtarma için Azure Site Kurtarma yapılandırma sunucusunun nasıl yönetilenbir şekilde yönetilen.
services: site-recovery
author: mayurigupta13
ms.service: site-recovery
ms.topic: article
ms.date: 02/28/2019
ms.author: mayg
ms.openlocfilehash: 25be48e9caed446be3a86a11143ce3040808065a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80294303"
---
# <a name="manage-the-configuration-server-for-physical-server-disaster-recovery"></a>Fiziksel sunucu olağanüstü durum kurtarma için yapılandırma sunucusunu yönetme

Azure [Site Kurtarma](site-recovery-overview.md) hizmetini fiziksel sunucuların Azure'da olağanüstü kurtarma hizmeti için kullandığınızda şirket içi bir yapılandırma sunucusu ayarlarsınız. Yapılandırma sunucusu şirket içi makineler le Azure arasındaki iletişimi koordine eder ve veri çoğaltmaişlemini yönetir. Bu makalede, dağıtıldıktan sonra yapılandırma sunucusunu yönetmek için yaygın görevleri özetler.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar

Tablo, şirket içi yapılandırma sunucusu makinesini dağıtmak için ön koşulları özetler.

| **Bileşen** | **Gereksinim** |
| --- |---|
| CPU çekirdekleri| 8 |
| RAM | 16 GB|
| Disk sayısı | 3, işletim sistemi diski, işlem sunucusu önbellek diski ve failback için saklama sürücüsü dahil |
| Boş disk alanı (işlem sunucusu önbelleği) | 600 GB
| Boş disk alanı (bekletme diski) | 600 GB|
| İşletim sistemi  | Windows Server 2012 R2 <br> Windows Server 2016 |
| İşletim sistemi yerel ayarı | İngilizce (ABD)|
| VMware vSphere PowerCLI sürümü | Gerekli değil|
| Windows Server rolleri | Bu rolleri etkinleştirme: <br> - Active Directory Domain Services <br>- İnternet Bilgi Hizmetleri <br> - Hyper-V |
| Grup ilkeleri| Bu grup ilkelerini etkinleştirme: <br> - Komut istemine erişimi engelleyin <br> - Kayıt defteri düzenleme araçlarına erişimi engelleme <br> - Dosya ekleri için güven mantığı <br> - Komut Dosyası Yürütme'yi aç <br> [Daha fazlasını öğrenin](https://technet.microsoft.com/library/gg176671(v=ws.10).aspx)|
| IIS | - Önceden varolan varsayılan web sitesi yok <br> - [Anonim Kimlik Doğrulamayı Etkinleştirme](https://technet.microsoft.com/library/cc731244(v=ws.10).aspx) <br> - [FastCGI](https://technet.microsoft.com/library/cc753077(v=ws.10).aspx) ayarını etkinleştirin  <br> - 443 nolu bağlantı noktası üzerinde önceden var olan web sitesi/uygulama dinleme<br>|
| NIC türü | VMXNET3 (VMware VM olarak dağıtıldığında) |
| IP adresi türü | Statik |
| İnternet erişimi | Sunucunun bu URL'lere erişmesi gerekir: <br> - \*.accesscontrol.windows.net<br> - \*.backup.windowsazure.com <br>- \*.store.core.windows.net<br> - \*.blob.core.windows.net<br> - \*.hypervrecoverymanager.windowsazure.com <br> - `https://management.azure.com` <br> - *.services.visualstudio.com <br> - https://dev.mysql.com/get/Downloads/MySQLInstaller/mysql-installer-community-5.7.20.0.msi(Ölçeklendirme İşlem Sunucuları için gerekli değildir) <br> - time.nist.gov <br> - time.windows.com |
| Bağlantı Noktaları | 443 (Denetim kanalı düzenleme)<br>9443 (Veri aktarımı)|

## <a name="download-the-latest-installation-file"></a>En son yükleme dosyasını indirin

Yapılandırma sunucusu yükleme dosyasının en son sürümü Site Kurtarma portalında mevcuttur. Ayrıca, doğrudan [Microsoft Download Center'dan](https://aka.ms/unifiedsetup)indirilebilir.

1. Azure portalında oturum açın ve Kurtarma Hizmetleri Kasanıza göz atın.
2. **Site Kurtarma Altyapısı** > **Yapılandırma Sunucularına** göz atın (VMware & Physical Machines için altında).
3. **+Sunucular** düğmesini tıklatın.
4. Sunucu **Ekle** sayfasında, Kayıt tuşunu indirmek için İndir düğmesini tıklatın. Azure Site Kurtarma hizmetine kaydetmek için Configuration Server yüklemesi sırasında bu anahtara ihtiyacınız var.
5. Configuration **Server'ın** en son sürümünü indirmek için Microsoft Azure Sitesi Kurtarma Birleşik Kurulum bağlantısını indir'i tıklatın.

   ![İndirme Sayfası](./media/physical-manage-configuration-server/downloadcs.png)


## <a name="install-and-register-the-server"></a>Sunucuyu yükleme ve kaydetme

1. Birleşik Kurulum yükleme dosyasını çalıştırın.
2. **Başlamadan Önce,** **yapılandırma sunucusu ve işlem sunucusu yükleyin**seçin.

    ![Başlamadan önce](./media/physical-manage-configuration-server/combined-wiz1.png)

3. MySQL indirip yüklemek için **Üçüncü Taraf Yazılım Lisansı** bölümünde **Kabul Ediyorum**’a tıklayın.
4. **İnternet Ayarları** alanında, yapılandırma sunucusunda çalışan Sağlayıcının Azure Site Recovery'ye İnternet üzerinden nasıl bağlanacağını belirtin. Gerekli URL'lere izin verdiğınızdan emin olun.

    - Şu anda makinede ayarlanmış olan proxy ile bağlanmak istiyorsanız, **proxy sunucusu kullanarak Azure Site Kurtarma'ya Bağlan'ı**seçin.
    - Sağlayıcının doğrudan bağlanmasını istiyorsanız, **proxy sunucusu olmadan doğrudan Azure Site Kurtarma'ya bağlan'ı**seçin.
    - Varolan proxy kimlik doğrulaması gerektiriyorsa veya Sağlayıcı bağlantısı için özel bir proxy kullanmak istiyorsanız, **özel proxy ayarlarıyla bağlan'ı**seçin ve adresi, bağlantı noktasını ve kimlik bilgilerini belirtin.
     ![Güvenlik duvarı](./media/physical-manage-configuration-server/combined-wiz4.png)
6. **Önkoşul Denetimi** menüsünde Kurulum, yüklemenin çalışabildiğinden emin olmak üzere bir denetim gerçekleştirir. **Genel saat eşitleme denetimi** hakkında bir uyarı görünürse, sistem saatindeki zamanın (**Tarih ve Saat** ayarları) saat dilimiyle aynı olduğunu doğrulayın.

    ![Ön koşullar](./media/physical-manage-configuration-server/combined-wiz5.png)
7. **MySQL Yapılandırması** menüsünde, yüklü MySQL sunucu örneğinde oturum açmak için kimlik bilgileri oluşturun.

    ![MySQL](./media/physical-manage-configuration-server/combined-wiz6.png)
8. **Ortam Ayrıntıları**’nda VMware sanal makinelerini çoğaltıp çoğaltmayacağınızı seçin. Eğer öyleyse, Kurulum PowerCLI 6.0 yüklü olduğunu denetler.
9. **Yükleme Konumu** alanında ikili dosyaları yüklemek ve önbelleği depolamak istediğiniz konumu seçin. Seçtiğiniz sürücü en az 5 GB kullanılabilir disk alanına sahip olmalıdır, ancak en az 600 GB boş alanı olan bir önbellek sürücüsü seçmeniz önerilir.

    ![Yükleme konumu](./media/physical-manage-configuration-server/combined-wiz8.png)
10. **Ağ**Seçimi'nde, önce yerleşik işlem sunucusunun kaynak makinelerde mobilite hizmetinin keşfi ve itme yüklemesi için kullandığı NIC'i seçin ve ardından Configuration Server'ın Azure ile bağlantı için kullandığı NIC'i seçin. Bağlantı noktası 9443, çoğaltma trafiğini gönderip almak için kullanılan varsayılan bağlantı noktasıdır, ancak bu bağlantı noktası numarasını ortamınızın gereksinimlerine uyacak şekilde değiştirebilirsiniz. Bağlantı noktası 9443’e ek olarak, çoğaltma işlemlerini düzenlemek için web sunucusu tarafından kullanılan bağlantı noktası 443 de açılır. Çoğaltma trafiği göndermek veya almak için 443 bağlantı noktasını kullanmayın.

    ![Ağ seçimi](./media/physical-manage-configuration-server/combined-wiz9.png)


11. **Özet** alanındaki bilgileri gözden geçirin ve **Yükle**’ye tıklayın. Yükleme tamamlandığında bir parola oluşturulur. Çoğaltmayı etkinleştirdiğinizde bu parola gerekli olacaktır; bu yüzden kopyalayıp güvenli bir yerde saklayın.


Kayıt bittikten sonra sunucu kasadaki **Ayarlar** > **Sunucuları** belinde görüntülenir.


## <a name="install-from-the-command-line"></a>Komut satırından yükleme

Yükleme dosyasını aşağıdaki gibi çalıştırın:

  ```
  UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address to be used for data transfer] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]
  ```

### <a name="sample-usage"></a>Örnek kullanımı
  ```
  MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /x:C:\Temp\Extracted
  cd C:\Temp\Extracted
  UNIFIEDSETUP.EXE /AcceptThirdpartyEULA /servermode "CS" /InstallLocation "D:\" /MySQLCredsFilePath "C:\Temp\MySQLCredentialsfile.txt" /VaultCredsFilePath "C:\Temp\MyVault.vaultcredentials" /EnvType "VMWare"
  ```


### <a name="parameters"></a>Parametreler

|Parametre Adı| Tür | Açıklama| Değerler|
|-|-|-|-|
| /ServerMode|Gerekli|Hem yapılandırma hem de işlem sunucusunun mu yoksa yalnızca işlem sunucusunun mu yükleneceğini belirtir|CS<br>PS|
|/InstallLocation|Gerekli|Bileşenlerin yüklendiği klasör| Bilgisayardaki herhangi bir klasör|
|/MySQLCredsFilePath|Gerekli|MySQL sunucusu kimlik bilgilerinin depolandığı dosya yolu|Dosya aşağıda belirtilen biçimde olmalıdır|
|/VaultCredsFilePath|Gerekli|Kasa kimlik bilgileri dosyasının yolu|Geçerli dosya yolu|
|/EnvType|Gerekli|Korumak istediğiniz ortam türü |VMware<br>NonVMware|
|/PSIP|Gerekli|Çoğaltma veri aktarımı için kullanılacak NIC’nin IP adresi| Herhangi bir geçerli IP adresi|
|/CSIP|Gerekli|Yapılandırma sunucusunun dinleme yaptığı NIC’nin IP adresi| Herhangi bir geçerli IP adresi|
|/PassphraseFilePath|Gerekli|Parola dosyası konumunun tam yolu|Geçerli dosya yolu|
|/BypassProxy|İsteğe bağlı|Yapılandırma sunucusunun Azure'a bir ara sunucu olmadan bağlandığını belirtir|Yapmak için bu değeri Venu’den alın|
|/ProxySettingsFilePath|İsteğe bağlı|Ara sunucu ayarları (Varsayılan ara sunucu kimlik doğrulaması gerektirir ya da özel bir ara sunucu kullanılır)|Dosya aşağıda belirtilen biçimde olmalıdır|
|DataTransferSecurePort|İsteğe bağlı|Çoğaltma verileri için kullanılacak PSIP’deki bağlantı noktası numarası| Geçerli Bağlantı Noktası Numarası (varsayılan değer: 9433)|
|/SkipSpaceCheck|İsteğe bağlı|Önbellek diski için alan denetimini atlama| |
|/AcceptThirdpartyEULA|Gerekli|Bayrak, üçüncü taraf EULA'nın kabul edildiğini gösterir| |
|/ShowThirdpartyEULA|İsteğe bağlı|Üçüncü taraf EULA belgesini görüntüler. Giriş olarak sağlanırsa, diğer tüm parametreler yoksayılır| |



### <a name="create-file-input-for-mysqlcredsfilepath"></a>MYSQLCredsFilePath için dosya girişi oluşturma

MySQLCredsFilePath parametresi bir dosyayı giriş olarak alır. Aşağıdaki biçimi kullanarak dosyayı oluşturun ve MySQLCredsFilePath parametresini girdi olarak geçirin.
```ini
[MySQLCredentials]
MySQLRootPassword = "Password"
MySQLUserPassword = "Password"
```
### <a name="create-file-input-for-proxysettingsfilepath"></a>ProxySettingsFilePath için dosya girişi oluşturma
ProxySettingsFilePath parametresi bir dosyayı giriş olarak alır. Aşağıdaki biçimi kullanarak dosyayı oluşturun ve proxySettingsFilePath parametresi olarak geçirin.

```ini
[ProxySettings]
ProxyAuthentication = "Yes/No"
Proxy IP = "IP Address"
ProxyPort = "Port"
ProxyUserName="UserName"
ProxyPassword="Password"
```
## <a name="modify-proxy-settings"></a>Proxy ayarlarını değiştirme

Yapılandırma sunucusu makinesi için proxy ayarlarını aşağıdaki gibi değiştirebilirsiniz:

1. Yapılandırma sunucusunda oturum açın.
2. Masaüstünüzdeki kısayolu kullanarak cspsconfigtool.exe'yi başlatın.
3. Vault **Kayıt** sekmesini tıklatın.
4. Portaldan yeni bir kasa kayıt dosyası indirin ve araca giriş olarak sağlayın.

   ![register-configuration-server](./media/physical-manage-configuration-server/register-csconfiguration-server.png)
5. Yeni proxy ayrıntılarını sağlayın ve **Kaydol** düğmesini tıklayın.
6. Yönetici PowerShell komut pencereni açın.
7. Şu komutu çalıştırın:

   ```powershell
   $Pwd = ConvertTo-SecureString -String MyProxyUserPassword
   Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber –ProxyUserName domain\username -ProxyPassword $Pwd
   net stop obengine
   net start obengine
   ```

   > [!WARNING]
   > Yapılandırma sunucusuna ek işlem sunucularınız varsa, dağıtımınızdaki [tüm ölçeklendirme işlem sunucularında proxy ayarlarını düzeltmeniz](vmware-azure-manage-process-server.md#modify-proxy-settings-for-an-on-premises-process-server) gerekir.

## <a name="reregister-a-configuration-server-with-the-same-vault"></a>Aynı kasayla bir yapılandırma sunucusunu yeniden kaydetme
1. Configuration Server'ınızda oturum açın.
2. Masaüstünüzdeki kısayolu kullanarak cspsconfigtool.exe'yi başlatın.
3. Vault **Kayıt** sekmesini tıklatın.
4. Portaldan yeni bir kayıt dosyası indirin ve araca giriş olarak sağlayın.
      ![register-configuration-server](./media/physical-manage-configuration-server/register-csconfiguration-server.png)
5. Proxy Server ayrıntılarını sağlayın ve **Kaydol** düğmesini tıklayın.  
6. Yönetici PowerShell komut pencereni açın.
7. Aşağıdaki komutu çalıştırın

    ```powershell
    $Pwd = ConvertTo-SecureString -String MyProxyUserPassword
    Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber –ProxyUserName domain\username -ProxyPassword $Pwd
    net stop obengine
    net start obengine
    ```

   > [!WARNING]
   > Birden çok işlem sunucunuz varsa, [bunları yeniden kaydetmeniz](vmware-azure-manage-process-server.md#reregister-a-process-server)gerekir.

## <a name="register-a-configuration-server-with-a-different-vault"></a>Yapılandırma sunucusunda farklı bir kasaya sahip kayıt

> [!WARNING]
> Aşağıdaki adım, yapılandırma sunucusunu geçerli kasadan uzaklaştırıyor ve yapılandırma sunucusu nun altındaki tüm korumalı sanal makinelerin çoğaltılması durdurulur.

1. Yapılandırma sunucusunda oturum açma
2. yönetici komut isteminden komutu çalıştırın:

    ```
    reg delete HKLM\Software\Microsoft\Azure Site Recovery\Registration
    net stop dra
    ```
3. Masaüstünüzdeki kısayolu kullanarak cspsconfigtool.exe'yi başlatın.
4. Vault **Kayıt** sekmesini tıklatın.
5. Portaldan yeni bir kayıt dosyası indirin ve araca giriş olarak sağlayın.
6. Proxy Server ayrıntılarını sağlayın ve **Kaydol** düğmesini tıklayın.  
7. Yönetici PowerShell komut pencereni açın.
8. Aşağıdaki komutu çalıştırın
    ```powershell
    $pwd = ConvertTo-SecureString -String MyProxyUserPassword
    Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber –ProxyUserName domain\username -ProxyPassword $pwd
    net stop obengine
    net start obengine
    ```

## <a name="upgrade-a-configuration-server"></a>Yapılandırma sunucusuyükseltme

Yapılandırma sunucusunu güncelleştirmek için güncelleştirme toplamaları çalıştırın. Güncelleştirmeler N-4'e kadar sürümler için uygulanabilir. Örnek:

- 9.7, 9.8, 9.9 veya 9.10 çalıştırıyorsanız doğrudan 9.11'e yükseltebilirsiniz.
- 9.6 veya daha erken çalıştırıyorsanız ve 9.11'e yükseltmek istiyorsanız, önce sürüm 9.7'ye yükseltmeniz gerekir. 9.11'den önce.

Yapılandırma sunucusunun tüm sürümlerine yükseltme için güncelleştirme toplama bağlantıları [wiki güncellemeleri sayfasında](https://social.technet.microsoft.com/wiki/contents/articles/38544.azure-site-recovery-service-updates.aspx)mevcuttur.

Sunucuyu aşağıdaki gibi yükseltin:

1. Güncelleştirme yükleyici dosyasını yapılandırma sunucusuna indirin.
2. Yükleyiciyi çalıştırmak için çift tıklatın.
3. Yükleyici, makinede çalışan geçerli sürümü algılar.
4. Onaylamak ve yükseltmeyi çalıştırmak için **Tamam'ı** tıklatın. 


## <a name="delete-or-unregister-a-configuration-server"></a>Yapılandırma sunucususilme veya kaydını kaldırın

> [!WARNING]
> Configuration Server'ınızı devre dışı bırakmaya başlamadan önce aşağıdakileri doğru lanın.
> 1. Bu Configuration Server kapsamındaki tüm sanal makineler için [korumayı devre dışı kaldırın.](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure)
> 2. Configuration Server'daki tüm Çoğaltma ilkelerini [ilişkilendirin](vmware-azure-set-up-replication.md#disassociate-or-delete-a-replication-policy) ve [silin.](vmware-azure-set-up-replication.md#disassociate-or-delete-a-replication-policy)
> 3. Configuration Server ile ilişkili tüm vCenters sunucularını/vSphere ana bilgisayarlarını [silin.](vmware-azure-manage-vcenter.md#delete-a-vcenter-server)


### <a name="delete-the-configuration-server-from-azure-portal"></a>Yapılandırma Sunucusunu Azure portalından silme
1. Azure portalında Vault menüsünden **Site Kurtarma Altyapısı** > **Yapılandırma Sunucularına** göz atın.
2. Devre dışı bırakmak istediğiniz yapılandırma sunucusunu tıklatın.
3. Configuration Server'ın ayrıntılar sayfasında **Sil** düğmesini tıklatın.
4. Sunucunun silinmesini onaylamak için **Evet'i** tıklatın.

### <a name="uninstall-the-configuration-server-and-its-dependencies"></a>Yapılandırma sunucusunu ve bağımlılıklarını kaldırma
> [!TIP]
>   Yapılandırma Sunucusunu Azure Site Kurtarma ile yeniden kullanmayı planlıyorsanız, doğrudan 4 adıma atlayabilirsiniz

1. Configuration Server'da Yönetici olarak oturum açın.
2. Programları Kaldırma > Denetim Paneli > Programı'nı açın
3. Programları aşağıdaki sırayla kaldırın:
   * Microsoft Azure Kurtarma Hizmetleri Aracısı
   * Microsoft Azure Site Kurtarma Mobilite Hizmeti/Ana Hedef sunucusu
   * Microsoft Azure Site Kurtarma Sağlayıcısı
   * Microsoft Azure Site Kurtarma Yapılandırma Sunucusu/İşlem Sunucusu
   * Microsoft Azure Site Kurtarma Yapılandırma Sunucusu Bağımlılıkları
   * MySQL Server 5.5
4. Aşağıdaki komutu çalıştırın ve yönetici komut istemi.
   ```
   reg delete HKLM\Software\Microsoft\Azure Site Recovery\Registration
   ```

## <a name="delete-or-unregister-a-configuration-server-powershell"></a>Yapılandırma sunucusunu silme veya silme (PowerShell)

1. [Yükle](https://docs.microsoft.com/powershell/azure/install-Az-ps) Azure PowerShell modülü
2. Komutu kullanarak Azure hesabınıza giriş yapın
    
    `Connect-AzAccount`
3. Kasanın bulunduğu aboneliği seçin

     `Get-AzSubscription –SubscriptionName <your subscription name> | Select-AzSubscription`
3.  Şimdi kasa bağlamınızı ayarlayın
    
    ```powershell
    $Vault = Get-AzRecoveryServicesVault -Name <name of your vault>
    Set-AzSiteRecoveryVaultSettings -ARSVault $Vault
    ```
4. Yapılandırma sunucunuzu seçin

    `$Fabric = Get-AzSiteRecoveryFabric -FriendlyName <name of your configuration server>`
6. Yapılandırma Sunucusunu Silme

    `Remove-AzSiteRecoveryFabric -Fabric $Fabric [-Force]`

> [!NOTE]
> Remove-AzSiteRecoveryFabric'deki **-Force** seçeneği, Yapılandırma sunucusunun kaldırılmasını/silinmesini zorlamak için kullanılabilir.

## <a name="renew-ssl-certificates"></a>SSL sertifikalarını yenileme
Yapılandırma sunucusu, Mobilite hizmetinin etkinliklerini, işlem sunucularını ve ona bağlı ana hedef sunucuları düzenleyen dahili bir web sunucusuna sahiptir. Web sunucusu, istemcilerin kimliğini doğrulamak için bir SSL sertifikası kullanır. Sertifika üç yıl sonra sona erer ve herhangi bir zamanda yenilenebilir.

### <a name="check-expiry"></a>Son kullanma tarihini kontrol edin

Yapılandırma sunucusu dağıtımları için Mayıs 2016'dan önce sertifika son kullanma süresi bir yıla ayarlandı. Bir sertifikanız sona erecekse, aşağıdakiler oluşur:

- Son kullanma tarihi iki ay veya daha az olduğunda, hizmet portalda ve e-posta yla (Azure Site Kurtarma bildirimlerine abone olduysanız) bildirim göndermeye başlar.
- Kasa kaynak sayfasında bir bildirim başlığı görüntülenir. Daha fazla bilgi için banner'ı tıklatın.
- **Şimdi Yükseltme** düğmesini görürseniz, bu, ortamınızda 9.4.xxxx.x veya daha yüksek sürümlere yükseltilmeyen bazı bileşenler olduğunu gösterir. Sertifikayı yenilemeden önce bileşenleri yükseltin. Eski sürümlerinde yenileyemezsiniz.

### <a name="renew-the-certificate"></a>Sertifikayı yenileme

1. Kasada, **Site Kurtarma Altyapı** > **Yapılandırma Sunucusu'nu**açın ve gerekli yapılandırma sunucusunu tıklatın.
2. Son kullanma tarihi **Configuration Server sistem durumu** altında görünür
3. **Sertifikaları Yenile'yi**tıklatın. 




## <a name="common-issues"></a>Genel sorunlar
[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]

## <a name="next-steps"></a>Sonraki adımlar

[Fiziksel sunucuların](tutorial-physical-to-azure.md) Azure'da olağanüstü durum kurtarmasını ayarlamaya yönelik öğreticileri gözden geçirin.

