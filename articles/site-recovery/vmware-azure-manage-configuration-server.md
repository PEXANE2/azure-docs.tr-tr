---
title: Azure Site Kurtarma ile olağanüstü durum kurtarma için yapılandırma sunucusunu yönetme
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 04/15/2019
ms.author: ramamill
ms.openlocfilehash: 692834903899448707200b24a955301e29e14f90
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80478459"
---
# <a name="manage-the-configuration-server-for-vmware-vmphysical-server-disaster-recovery"></a>VMware VM/physical server olağanüstü durum kurtarma yapılandırma sunucusunu yönetme

VMware VM'lerin ve fiziksel sunucuların Azure'da olağanüstü kurtarma durumu için [Azure Site Kurtarma'yı](site-recovery-overview.md) kullandığınızda şirket içi bir yapılandırma sunucusu ayarlarsınız. Yapılandırma sunucusu şirket içi VMware ve Azure arasındaki iletişimi koordine eder ve veri çoğaltmaişlemini yönetir. Bu makalede, dağıtıldıktan sonra yapılandırma sunucusunu yönetmek için yaygın görevleri özetler.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="update-windows-license"></a>Windows lisansını sız

OVF şablonu ile sağlanan lisans 180 gün için geçerli bir değerlendirme lisansıdır. Kesintisiz kullanım için, Windows'u satın alınmış bir lisansla etkinleştirmeniz gerekir. Lisans güncelleştirmesi tek başına bir anahtar veya KMS standart tuşu ile yapılabilir. Yönlendirme işletim [sistemi çalıştırmak için DISM Windows komut satırında](https://docs.microsoft.com/windows-hardware/manufacture/desktop/dism-windows-edition-servicing-command-line-options)kullanılabilir. Anahtarları almak için [KMS istemcikurulumuna](https://docs.microsoft.com/windows-server/get-started/kmsclientkeys)bakın.

## <a name="access-configuration-server"></a>Erişim yapılandırma sunucusu

Yapılandırma sunucusuna aşağıdaki gibi erişebilirsiniz:

* Dağıtıldığı VM'de oturum açın ve masaüstü kısayolundan **Azure Site Kurtarma Yapılandırma Yöneticisi'ni** başlatın.
* Alternatif olarak,*configurationServerName*/:44315/ https://'dan yapılandırma sunucusuna uzaktan erişebilirsiniz. Yönetici kimlik bilgileriyle oturum açın.

## <a name="modify-vmware-server-settings"></a>VMware sunucu ayarlarını değiştirme

1. Farklı bir VMware sunucusunu yapılandırma sunucusuyla ilişkilendirmek için oturum [açmadan](#access-configuration-server)sonra **vCenter Server/vSphere ESXi sunucusu ekle'yi**seçin.
2. Ayrıntıları girin ve sonra **Tamam'ı**seçin.

## <a name="modify-credentials-for-automatic-discovery"></a>Otomatik bulma için kimlik bilgilerini değiştirme

1. VMware VM'lerin otomatik olarak keşfi için VMware sunucusuna bağlanmak için kullanılan kimlik bilgilerini güncelleştirmek için oturum [açtıktan](#access-configuration-server)sonra hesabı seçin ve **Edit'i**tıklatın.
2. Yeni kimlik bilgilerini girin ve sonra **Tamam'ı**seçin.

    ![VMware'i değiştir](./media/vmware-azure-manage-configuration-server/modify-vmware-server.png)

Ayrıca CSPSConfigtool.exe üzerinden kimlik bilgilerini değiştirebilirsiniz.

1. Yapılandırma sunucusuna giriş yapın ve CSPSConfigtool.exe'yi başlatın
2. Değiştirmek istediğiniz hesabı seçin ve **Edit'i**tıklatın.
3. Değiştirilen kimlik bilgilerini girin ve **Tamam'ı** tıklatın

## <a name="modify-credentials-for-mobility-service-installation"></a>Mobilite Hizmeti yüklemesi için kimlik bilgilerini değiştirme

Çoğaltma için etkinleştirdiğiniz VMware VM'lere Mobilite Hizmeti'ni otomatik olarak yüklemek için kullanılan kimlik bilgilerini değiştirin.

1. [Oturum açtıktan](#access-configuration-server)sonra sanal **makine kimlik bilgilerini yönet'i** seçin
2. Değiştirmek istediğiniz hesabı seçin ve **Edit'i** tıklatın
3. Yeni kimlik bilgilerini girin ve sonra **Tamam'ı**seçin.

    ![Mobilite Hizmeti kimlik bilgilerini değiştirme](./media/vmware-azure-manage-configuration-server/modify-mobility-credentials.png)

Ayrıca CSPSConfigtool.exe üzerinden kimlik bilgilerini değiştirebilirsiniz.

1. Yapılandırma sunucusuna giriş yapın ve CSPSConfigtool.exe'yi başlatın
2. Değiştirmek istediğiniz hesabı seçin ve **Edit'i** tıklatın
3. Yeni kimlik bilgilerini girin ve **Tamam'ı**tıklatın.

## <a name="add-credentials-for-mobility-service-installation"></a>Mobilite hizmeti yüklemesi için kimlik bilgileri ekleme

Yapılandırma sunucusunun OVF dağıtımı sırasında kimlik bilgileri eklemeyi kaçırdıysanız,

1. [Oturum açtıktan](#access-configuration-server)sonra sanal **makine kimlik bilgilerini yönet'i**seçin.
2. Sanal **makine kimlik bilgilerini ekle'ye**tıklayın.
    ![add-mobility-credentials](media/vmware-azure-manage-configuration-server/add-mobility-credentials.png)
3. Yeni kimlik bilgilerini girin ve **Ekle'ye**tıklayın.

Ayrıca CSPSConfigtool.exe üzerinden kimlik bilgileri ekleyebilirsiniz.

1. Yapılandırma sunucusuna giriş yapın ve CSPSConfigtool.exe'yi başlatın
2. **Ekle'yi**tıklatın, yeni kimlik bilgilerini girin ve **Tamam'ı**tıklatın.

## <a name="modify-proxy-settings"></a>Proxy ayarlarını değiştirme

Azure'a internet erişimi için yapılandırma sunucusu makinesi tarafından kullanılan proxy ayarlarını değiştirin. Yapılandırma sunucusu makinesinde çalışan varsayılan işlem sunucusuna ek olarak bir işlem sunucusu makineniz varsa, her iki makinedeki ayarları değiştirin.

1. Yapılandırma sunucusunda [oturum açmadan](#access-configuration-server) sonra **bağlantıyı yönet'i**seçin.
2. Proxy değerlerini güncelleştirin. Ardından ayarları güncelleştirmek için **Kaydet'i** seçin.

## <a name="add-a-network-adapter"></a>Ağ bağdaştırıcısı ekleme

Açık Sanallaştırma Biçimi (OVF) şablonu yapılandırma sunucusu VM'yi tek bir ağ bağdaştırıcısıyla dağıtır.

- [VM'ye ek bir bağdaştırıcı ekleyebilirsiniz,](vmware-azure-deploy-configuration-server.md#add-an-additional-adapter)ancak yapılandırma sunucusunu kasaya kaydettirmeden önce eklemeniz gerekir.
- Yapılandırma sunucusunu kasaya kaydettirdikten sonra bağdaştırıcı eklemek için bağdaştırıcıyı VM özelliklerine ekleyin. Sonra kasadaki sunucuyu [yeniden kaydetmeniz](#reregister-a-configuration-server-in-the-same-vault) gerekir.


## <a name="reregister-a-configuration-server-in-the-same-vault"></a>Yapılandırma sunucusunu aynı kasada yeniden kaydetme

Gerekirse yapılandırma sunucusunu aynı kasada yeniden kaydedebilirsiniz. Yapılandırma sunucusu makinesinde çalışan varsayılan işlem sunucusuna ek olarak ek bir işlem sunucusu makineniz varsa, her iki makineyi de yeniden kaydedin.


1. Kasada Site**Kurtarma Altyapı** > **Yapılandırma Sunucularını** **Yönet'i** > açın.
2. **Sunucularda,** kasa kimlik bilgilerini indirmek için **kayıt anahtarını** indir'i seçin.
3. Yapılandırma sunucusu makinesinde oturum açın.
4. **%ProgramData%\ASR\home\svsystems\bin,** açık **cspsconfigtool.exe**olarak .
5. Vault **Registration** sekmesinde **Gözat'ı**seçin ve indirdiğiniz kasa kimlik bilgilerini bulun.
6. Gerekirse, proxy sunucu ayrıntıları sağlayın. Ardından **Kaydet**’i seçin.
7. Yönetici PowerShell komut penceresini açın ve aşağıdaki komutu çalıştırın:
   ```
    $pwd = ConvertTo-SecureString -String MyProxyUserPassword
    Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
   ```

    >[!NOTE]
    >Yapılandırma sunucusundan ölçeklendirme işlem sunucusuna **en son sertifikaları çekmek** için " *\<Yükleme Sürücüsü\Microsoft Azure Site Kurtarma\aracısı\cdpcli.exe>" --registermt* komutunu çalıştırın

8. Son olarak, aşağıdaki komutu çalıştırarak obengine yeniden başlatın.
   ```
        net stop obengine
        net start obengine
   ```


## <a name="register-a-configuration-server-with-a-different-vault"></a>Yapılandırma sunucusunda farklı bir kasaya sahip kayıt

> [!WARNING]
> Aşağıdaki adım, yapılandırma sunucusunu geçerli kasadan uzaklaştırıyor ve yapılandırma sunucusu nun altındaki tüm korumalı sanal makinelerin çoğaltılması durdurulur.

1. Yapılandırma sunucusunda oturum açın.
2. Yönetici PowerShell komut penceresini açın ve aşağıdaki komutu çalıştırın:

    ```
    reg delete "HKLM\Software\Microsoft\Azure Site Recovery\Registration"
    net stop dra
    ```
3. Masaüstünüzdeki kısayolu kullanarak configuration server appliance tarayıcı portalını başlatın.
4. Yeni yapılandırma sunucusu [kaydına](vmware-azure-tutorial.md#register-the-configuration-server)benzer kayıt adımlarını gerçekleştirin.

## <a name="upgrade-the-configuration-server"></a>Yapılandırma sunucusunu yükseltme

Yapılandırma sunucusunu güncelleştirmek için güncelleştirme toplamaları çalıştırın. Güncelleştirmeler N-4'e kadar sürümler için uygulanabilir. Örnek:

- 9.7, 9.8, 9.9 veya 9.10 çalıştırın, doğrudan 9.11'e yükseltebilirsiniz.
- 9.6 veya daha erken çalıştırın ve 9.11'e yükseltmek istiyorsanız, önce sürüm 9.7'ye yükseltmeniz gerekir. 9.11'den önce.

Azure Site Kurtarma bileşenleri destek bildirimi hakkında ayrıntılı bilgi için [buraya](https://aka.ms/asr_support_statement)bakın.
Yapılandırma sunucusunun tüm sürümlerini yükseltmek için güncelleştirme toplama bağlantıları [na buradan](https://aka.ms/asr_update_rollups)ulaşabilirsiniz.

> [!IMPORTANT]
> Bir Azure Site Kurtarma bileşeninin yayımlanan her yeni sürümü 'N' ile 'N-4' altındaki tüm sürümler desteksiz olarak kabul edilir. Her zaman mevcut en son sürümlerine yükseltmek için tavsiye edilir.</br>
> Azure Site Kurtarma bileşenleri destek bildirimi hakkında ayrıntılı bilgi için [buraya](https://aka.ms/asr_support_statement)bakın.

Sunucuyu aşağıdaki gibi yükseltin:

1. Kasada,**Site Kurtarma Altyapı** > **Yapılandırma Sunucularını** **Yönet'e** > gidin.
2. Bir güncelleştirme varsa, **Aracı Sürümü** > sütununda bir bağlantı görüntülenir.
    ![Güncelleştir](./media/vmware-azure-manage-configuration-server/update2.png)
3. Güncelleştirme yükleyici dosyasını yapılandırma sunucusuna indirin.

    ![Güncelleştirme](./media/vmware-azure-manage-configuration-server/update1.png)

4. Yükleyiciyi çalıştırmak için çift tıklatın.
5. Yükleyici, makinede çalışan geçerli sürümü algılar. Yükseltmeyi başlatmak için **Evet'i** tıklatın.
6. Yükseltme tamamlandığında sunucu yapılandırması doğrulanır.

    ![Güncelleştirme](./media/vmware-azure-manage-configuration-server/update3.png)

7. Yükleyiciyi kapatmak için **Bitir'i** tıklatın.
8. Site Kurtarma bileşenlerinin geri kalanını yükseltmek için [yükseltme kılavuzumuza](https://aka.ms/asr_vmware_upgrades)bakın.

## <a name="upgrade-configuration-serverprocess-server-from-the-command-line"></a>Komut satırından yapılandırma sunucusu/işlem sunucusuyükseltme

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
MySQLRootPassword = "Password>"
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

## <a name="delete-or-unregister-a-configuration-server"></a>Yapılandırma sunucususilme veya kaydını kaldırın

1. Yapılandırma sunucusu altındaki tüm VM'ler için [korumayı devre dışı kaldırın.](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure)
2. Yapılandırma sunucusundaki tüm çoğaltma ilkelerini [ilişkilendirin](vmware-azure-set-up-replication.md#disassociate-or-delete-a-replication-policy) ve [silin.](vmware-azure-set-up-replication.md#disassociate-or-delete-a-replication-policy)
3. Yapılandırma sunucusuyla ilişkili tüm vCenter sunucularını/vSphere ana bilgisayarlarını [silin.](vmware-azure-manage-vcenter.md#delete-a-vcenter-server)
4. Kasada, **Site Kurtarma Altyapı** > **Yapılandırma Sunucularını**açın.
5. Kaldırmak istediğiniz yapılandırma sunucusunu seçin. Ardından, **Ayrıntılar** sayfasında **Sil'i**seçin.

    ![Yapılandırma sunucususilme](./media/vmware-azure-manage-configuration-server/delete-configuration-server.png)


### <a name="delete-with-powershell"></a>PowerShell ile silme

PowerShell'i kullanarak yapılandırma sunucusunu isteğe bağlı olarak silebilirsiniz.

1. Azure PowerShell modüllerini [yükleyin.](https://docs.microsoft.com/powershell/azure/install-Az-ps)
2. Bu komutu kullanarak Azure hesabınızda oturum açın:

    `Connect-AzAccount`
3. Kasa aboneliğini seçin.

     `Get-AzSubscription –SubscriptionName <your subscription name> | Select-AzSubscription`
3.  Kasa bağlamını ayarlayın.

    ```
    $vault = Get-AzRecoveryServicesVault -Name <name of your vault>
    Set-AzSiteRecoveryVaultSettings -ARSVault $vault
    ```
4. Yapılandırma sunucusunu alın.

    `$fabric = Get-AzSiteRecoveryFabric -FriendlyName <name of your configuration server>`
6. Yapılandırma sunucusunu silin.

    `Remove-AzSiteRecoveryFabric -Fabric $fabric [-Force]`

> [!NOTE]
> Yapılandırma sunucusunun zorla silinmesi için Remove-AzSiteRecoveryFabric'deki **-Force** seçeneğini kullanabilirsiniz.

## <a name="generate-configuration-server-passphrase"></a>Yapılandırma sunucusu Passphrase oluşturma

1. Yapılandırma sunucunuzda oturum açın ve ardından yönetici olarak komut istemi penceresini açın.
2. Dizini depo kutusu klasörüne değiştirmek için **cd %ProgramData%\ASR\home\svsystems\bin** komutunu çalıştırın
3. Passphrase dosyasını oluşturmak için **genpassphrase.exe -v > MobSvc.passphrase'i**uygulayın.
4. Parolanız **%ProgramData%\ASR\home\svsystems\bin\MobSvc.passphrase**adresinde bulunan dosyada depolanır.

## <a name="renew-tlsssl-certificates"></a>TLS/SSL sertifikalarını yenileme

Yapılandırma sunucusu, Mobilite Hizmetinin etkinliklerini, işlem sunucularını ve ona bağlı ana hedef sunucuları düzenleyen dahili bir web sunucusuna sahiptir. Web sunucusu, istemcilerin kimliğini doğrulamak için bir TLS/SSL sertifikası kullanır. Sertifika üç yıl sonra sona erer ve herhangi bir zamanda yenilenebilir.

### <a name="check-expiry"></a>Son kullanma tarihini kontrol edin

Yapılandırma sunucusu dağıtımları için Mayıs 2016'dan önce sertifika son kullanma süresi bir yıla ayarlandı. Süresi dolacak bir sertifikanız varsa, aşağıdakiler oluşur:

- Son kullanma tarihi iki ay veya daha az olduğunda, hizmet portalda ve e-posta yla (Site Kurtarma bildirimlerine abone olduysanız) bildirim göndermeye başlar.
- Kasa kaynak sayfasında bir bildirim başlığı görüntülenir. Daha fazla bilgi için banner'ı seçin.
- **Şimdi Yükseltme** düğmesini görürseniz, ortamınızdaki bazı bileşenlerin 9.4.xxxx.x veya daha yüksek sürümlere yükseltilmediğini gösterir. Sertifikayı yenilemeden önce bileşenleri yükseltin. Eski sürümlerinde yenileyemezsiniz.

### <a name="renew-the-certificate"></a>Sertifikayı yenileme

1. Kasada Site **Kurtarma Altyapı** > **Yapılandırma Sunucusu'nu**açın. Gerekli yapılandırma sunucusunu seçin.
2. Son kullanma tarihi **Configuration Server health**altında görünür.
3. **Sertifikaları Yenile'yi**seçin.

## <a name="refresh-configuration-server"></a>Yapılandırma sunucusuyenileme

1. Azure portalında, Kurtarma **Hizmetleri Vault** > **Manage** > **Site Kurtarma Altyapısını** > **VMware & Fiziksel makineler** > **Yapılandırma Sunucuları** için gidin
2. Yenilemek istediğiniz yapılandırma sunucusuna tıklayın.
3. Seçilen yapılandırma sunucusunun ayrıntılarını içeren bıçakta Daha **Fazla** > **SunucuYu Yenile'yi**tıklatın.
4. **Kurtarma Hizmetleri Vault** > **İzleme** > **Sitesi Kurtarma işleri**altında işin ilerlemesini izleyin.

## <a name="failback-requirements"></a>Yeniden çalışma gereksinimleri

Yeniden koruma ve geri koruma sırasında, şirket içi yapılandırma sunucusu çalışıyor ve bağlı durumda olmalıdır. Başarılı bir geri dönüş için, başarısız olan sanal makinenin yapılandırma sunucusu veritabanında bulunması gerekir.

Yapılandırma sunucunuzun düzenli zamanlanmış yedeklerini aldığınızdan emin olun. Bir felaket oluşursa ve yapılandırma sunucusu kaybolursa, önce yapılandırma sunucusunu yedek bir kopyadan geri yüklemeniz ve geri yüklenen yapılandırma sunucusunun kasaya kayıtlı olduğu IP adresiyle aynı IP adresine sahip olduğundan emin olmalısınız. Geri yüklenen yapılandırma sunucusu için farklı bir IP adresi kullanılırsa failback çalışmaz.

## <a name="next-steps"></a>Sonraki adımlar

[VMware VM'lerin](vmware-azure-tutorial.md) olağanüstü durum kurtarmasını Azure'a ayarlama yla ilgili öğreticileri gözden geçirin.
