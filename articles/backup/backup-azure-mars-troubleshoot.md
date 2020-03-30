---
title: Azure Yedekleme aracısını sorun giderme
description: Bu makalede, Azure Yedekleme aracısının yüklenmesini ve kaydedilmesinde nasıl sorun giderilen öğrenin.
ms.reviewer: saurse
ms.topic: troubleshooting
ms.date: 07/15/2019
ms.openlocfilehash: 24169356600c25e664221af397051bb0fec3e459
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78673085"
---
# <a name="troubleshoot-the-microsoft-azure-recovery-services-mars-agent"></a>Microsoft Azure Kurtarma Hizmetleri (MARS) aracısını sorun giderme

Bu makalede, yapılandırma, kayıt, yedekleme ve geri yükleme sırasında görebileceğiniz hataların nasıl çözüleceği açıklanmaktadır.

## <a name="basic-troubleshooting"></a>Temel sorun giderme

Microsoft Azure Kurtarma Hizmetleri (MARS) aracısını sorun gidermeye başlamadan önce aşağıdakileri kontrol etmenizi öneririz:

- [MARS ajanının güncel olduğundan emin olun.](https://go.microsoft.com/fwlink/?linkid=229525&clcid=0x409)
- [MARS aracısı ile Azure arasında ağ bağlantısı olduğundan emin olun.](https://docs.microsoft.com/azure/backup/backup-azure-mars-troubleshoot#the-microsoft-azure-recovery-service-agent-was-unable-to-connect-to-microsoft-azure-backup)
- MARS'ın (Servis konsolunda) çalışırken olduğundan emin olun. Gerekirse işlemi yeniden başlatın ve yeniden deneyin.
- [Scratch klasörü konumunda %5 ila %10 serbest hacim alanı olduğundan emin olun.](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq#whats-the-minimum-size-requirement-for-the-cache-folder)
- [Başka bir işlemin veya virüsten koruma yazılımının Azure Yedekleme'yi mi engellediğini kontrol edin.](https://docs.microsoft.com/azure/backup/backup-azure-troubleshoot-slow-backup-performance-issue#cause-another-process-or-antivirus-software-interfering-with-azure-backup)
- Zamanlanmış yedekleme başarısız olursa, ancak el ile yedekleme [çalışıyorsa,](https://docs.microsoft.com/azure/backup/backup-azure-mars-troubleshoot#backups-dont-run-according-to-schedule)bkz.
- İşletim sisteminizin en son güncelleştirmeleri olduğundan emin olun.
- [Desteklenmeyen sürücülerin ve desteklenmeyen özniteliklere sahip dosyaların yedek dışında olduğundan emin olun.](backup-support-matrix-mars-agent.md#supported-drives-or-volumes-for-backup)
- Korunan sistemdeki saatin doğru saat dilimine yapılandırıldığından emin olun.
- [.NET Framework 4.5.2 veya sonraki sunucuya yüklendiğinden emin olun.](https://www.microsoft.com/download/details.aspx?id=30653)
- Sunucunuzu kasaya yeniden kaydetmeye çalışıyorsanız:
  - Aracının sunucuda kaldırdığından ve portaldan silindiğinden emin olun.
  - Sunucuyu kaydetmek için başlangıçta kullanılan parolayı kullanın.
- Çevrimdışı yedeklemeler için, yedeklemeyi başlatmadan önce Azure PowerShell 3.7.0'ın hem kaynağa hem de kopya bilgisayara yüklendiğinden emin olun.
- Yedekleme aracısı Bir Azure sanal makinesinde çalışıyorsa, [bu makaleye](https://docs.microsoft.com/azure/backup/backup-azure-troubleshoot-slow-backup-performance-issue#cause-backup-agent-running-on-an-azure-virtual-machine)bakın.

## <a name="invalid-vault-credentials-provided"></a>Sağlanan kasa kimlik bilgileri geçersiz

**Hata iletisi**: Geçersiz kasa kimlik bilgileri sağlanmıştır. Dosya bozuk veya kurtarma hizmeti ile ilişkili en son kimlik bilgilerine sahip değil. (Kimlik numarası: 34513)

| Nedeni | Önerilen eylemler |
| ---     | ---    |
| **Kasa kimlik bilgileri geçerli değil** <br/> <br/> Vault kimlik bilgileri dosyaları bozuk olabilir veya süresi dolmuş olabilir. (Örneğin, kayıt saatinden 48 saatten daha önce indirilmiş olabilirler.)| Azure portalındaki Kurtarma Hizmetleri kasasından yeni kimlik bilgilerini indirin. (Mars [aracısını indir](https://docs.microsoft.com/azure/backup/install-mars-agent#download-the-mars-agent) bölümündeki adım 6'ya bakın.) Ardından, uygun şekilde şu adımları atın: <ul><li> MARS'ı zaten yüklediyseniz ve kaydettiyseniz, Microsoft Azure Yedekleme Aracısı MMC konsolunu açın ve ardından yeni kimlik bilgileriyle kaydı tamamlamak için **Eylemler** bölmesinde **Register Server'ı** seçin. <br/> <li> Yeni yükleme başarısız olursa, yeni kimlik bilgileriyle yeniden yüklemeyi deneyin.</ul> **Not**: Birden çok kasa kimlik dosyası indirilmişse, yalnızca en son dosya sonraki 48 saat için geçerlidir. Yeni bir kasa kimlik dosyası indirmenizi öneririz.
| **Proxy sunucusu/güvenlik duvarı kaydı engelliyor** <br/>or <br/>**İnternet bağlantısı yok** <br/><br/> Makinenizin veya proxy sunucunuzun internet bağlantısı sınırlıysa ve gerekli URL'lere erişim sağlamazsanız, kayıt başarısız olur.| Şu adımları atın:<br/> <ul><li> Sistemin internet bağlantısıolduğundan emin olmak için BT ekibinizle birlikte çalışın.<li> Proxy sunucunuz yoksa, aracıyı kaydettirirken proxy seçeneğinin seçilmediğinden emin olun. [Proxy ayarlarınızı kontrol edin.](#verifying-proxy-settings-for-windows)<li> Bir güvenlik duvarınız/proxy sunucunuz varsa, bu URL'lerin ve IP adreslerinin erişebilmesini sağlamak için ağ ekibinizle birlikte çalışın:<br/> <br> **URL'ler**<br> `www.msftncsi.com` <br> .Microsoft.com <br> .WindowsAzure.com <br> .microsoftonline.com <br> .windows.net <br>**IP adresleri**<br>  20.190.128.0/18 <br>  40.126.0.0/18 <br/></ul></ul>Önceki sorun giderme adımlarını tamamladıktan sonra yeniden kaydetmeyi deneyin.<br></br> Bağlantınız Azure ExpressRoute üzerinden gerçekse, ayarların [Azure ExpressRoute desteğinde](backup-support-matrix-mars-agent.md#azure-expressroute-support)açıklandığı şekilde yapılandırıldığından emin olun.
| **Virüsten koruma yazılımı kaydı engelliyor** | Sunucuda virüsten koruma yazılımı yüklüyse, bu dosya ve klasörler için virüsten koruma sına gerekli dışlama kuralları ekleyin: <br/><ul> <li> CBengine.exe <li> Csc.exe<li> Scratch klasörü. Varsayılan konumu C:\Program Files\Microsoft Azure Kurtarma Hizmetleri Aracısı\Scratch'dir. <li> C:\Program Files\Microsoft Azure Kurtarma Hizmetleri Aracısı\Bin'deki çöp kutusu klasörü.

### <a name="additional-recommendations"></a>Ek öneriler

- C:/Windows/Temp'a gidin ve .tmp uzantılı 60.000'den fazla veya 65.000'den fazla dosya olup olmadığını kontrol edin. Varsa, bu dosyaları silin.
- Makinenin tarih ve saatinin yerel saat dilimiyle eşleştirdiğinden emin olun.
- [Bu sitelerin](install-mars-agent.md#verify-internet-access) Internet Explorer'daki güvenilir sitelerinize eklenmediğinden emin olun.

### <a name="verifying-proxy-settings-for-windows"></a>Windows için proxy ayarlarını doğrulama

1. [Sysinternals](https://docs.microsoft.com/sysinternals/downloads/psexec) sayfasından PsExec'i indirin.
1. Yükseltilmiş `psexec -i -s "c:\Program Files\Internet Explorer\iexplore.exe"` bir komut isteminden çalıştırın.

   Bu komut Internet Explorer'ı açar.
1. **Araçlar** > **Internet seçenekleri** > **Bağlantılar** > LAN**ayarları**gidin.
1. Sistem hesabının proxy ayarlarını kontrol edin.
1. Proxy yapılandırılmamışsa ve proxy ayrıntıları sağlanmışsa, ayrıntıları kaldırın.
1. Proxy yapılandırılırsa ve proxy ayrıntıları yanlışsa, **Proxy IP** ve **Bağlantı Noktası** ayrıntılarının doğru olduğundan emin olun.
1. Internet Explorer'ı kapatın.

## <a name="unable-to-download-vault-credential-file"></a>Kasa kimlik bilgileri dosyasını karşıdan yükleyemiyor

| Hata   | Önerilen eylemler |
| ---     | ---    |
|Kasa kimlik dosyasını indiremedi. (Id: 403) | <ul><li> Farklı bir tarayıcı kullanarak kasa kimlik bilgilerini indirmeyi deneyin veya aşağıdaki adımları izleyin: <ul><li> Internet Explorer'ı başlatın. F12'yi seçin. </li><li> **Ağ** sekmesine gidin ve önbelleği ve tanımlama bilgilerini temizleyin. </li> <li> Sayfayı yenileyin.<br></li></ul> <li> Aboneliğin devre dışı bırakıldığını/süresinin dolup dolmadığını kontrol edin.<br></li> <li> Herhangi bir güvenlik duvarı kuralının karşıdan yüklemeyi engelleyip engellemedığını kontrol edin. <br></li> <li> Kasadaki sınırı (kasa başına 50 makine) tüketmediğinden emin olun.<br></li>  <li> Kullanıcının kasa kimlik bilgilerini indirmek ve bir sunucuyu kasaya kaydettirmek için gereken Azure Yedekleme izinlerine sahip olduğundan emin olun. Bkz. [Azure Yedekleme kurtarma noktalarını yönetmek için Rol Tabanlı Erişim Denetimini Kullan.](backup-rbac-rs-vault.md)</li></ul> |

## <a name="the-microsoft-azure-recovery-service-agent-was-unable-to-connect-to-microsoft-azure-backup"></a>Microsoft Azure Kurtarma Hizmeti Aracısı, Microsoft Azure Backup'a bağlanamadı

| Hata  | Olası nedeni | Önerilen eylemler |
| ---     | ---     | ---    |
| <br /><ul><li>Microsoft Azure Kurtarma Hizmeti Aracısı Microsoft Azure Yedekleme'ye bağlanamadı. (Kimlik numarası: 100050) Ağ ayarlarınızı kontrol edin ve internete bağlanabildiğinizden emin olun.<li>(407) Proxy Kimlik Doğrulama Gerekli. |Proxy bağlantıyı engelliyor. |  <ul><li>Internet Explorer'da, **Araçlar** > **Internet seçenekleri** > **Güvenlik** > **İnternet'e**gidin. **Özel Düzey'i** seçin ve **Dosya indirme** bölümüne gidin. **Etkinleştir**’i seçin.<p>Ayrıca, Internet Explorer'daki güvenilir sitelerinize [URL ve IP adresleri](install-mars-agent.md#verify-internet-access) eklemeniz gerekebilir.<li>Proxy sunucusu kullanmak için ayarları değiştirin. Ardından proxy sunucusu ayrıntılarını sağlayın.<li> Makinenizin internet erişimi sınırlıysa, makine veya proxy'deki güvenlik duvarı ayarlarının bu [URL'lere ve IP adreslerine](install-mars-agent.md#verify-internet-access)izin verdiğinden emin olun. <li>Sunucuda virüsten koruma yazılımı yüklüyse, bu dosyaları virüsten koruma tayından hariç tut: <ul><li>CBEngine.exe (dpmra.exe yerine).<li>CSC.exe (.NET Framework ile ilgili). Sunucuda yüklü her .NET Framework sürümü için bir CSC.exe vardır. CSC.exe dosyalarını etkilenen sunucudaki .NET Framework'ün tüm sürümleri için hariçleyin. <li>Scratch klasörü veya önbellek konumu. <br>Kazıyışı klasörü veya önbellek yolu için varsayılan konum C:\Program Files\Microsoft Azure Kurtarma Hizmetleri Aracısı\Scratch'dir.<li>C:\Program Files\Microsoft Azure Kurtarma Hizmetleri Aracısı\Bin'deki çöp kutusu klasörü.

## <a name="failed-to-set-the-encryption-key-for-secure-backups"></a>Güvenli yedekleme işlemleri için şifreleme anahtarı ayarlanamadı

| Hata | Olası nedenler | Önerilen eylemler |
| ---     | ---     | ---    |
| <br />Güvenli yedeklemeler için şifreleme anahtarıayarlayamadı. Etkinleştirme tam olarak başarılı olamadı, ancak şifreleme parolası aşağıdaki dosyaya kaydedildi. |<li>Sunucu zaten başka bir kasaya kayıtlı.<li>Yapılandırma sırasında parola bozuktu.| Sunucuyu kasadan kaldırın ve yeni bir parolayla yeniden kaydedin.

## <a name="the-activation-did-not-complete-successfully"></a>Etkinleştirme başarıyla tamamlanmadı

| Hata  | Olası nedenler | Önerilen eylemler |
|---------|---------|---------|
|<br />Etkinleştirme başarıyla tamamlanmadı. Geçerli işlem bir iç hizmet hatası nedeniyle başarısız oldu [0x1FC07]. Bir süre sonra işlemi yeniden deneyin. Sorun devam ederse, lütfen Microsoft Desteği'ne başvurun.     | <li> Scratch klasörü yeterli alana sahip olmayan bir birim üzerinde yer alır. <li> Scratch klasörü yanlış taşınmış. <li> OnlineBackup.KEK dosyası eksik.         | <li>MARS aracısının [en son sürümüne](https://aka.ms/azurebackup_agent) yükseltin.<li>Scratch klasörünü veya önbellek konumunu, yedekleme verilerinin toplam boyutunun %5 ila %10'u arasında boş alana sahip bir ses birimine taşıyın. Önbellek konumunu doğru taşımak [için, dosya ve klasörleri yedekleme yle ilgili sık sorulan sorulara](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq#manage-the-backup-cache-folder)bakın.<li> OnlineBackup.KEK dosyasının bulunduğundan emin olun. <br>*Scratch klasörü veya önbellek yolu için varsayılan konum C:\Program Files\Microsoft Azure Kurtarma Hizmetleri Aracısı\Scratch' dir.*        |

## <a name="encryption-passphrase-not-correctly-configured"></a>Şifreleme parolası düzgün yapılandırılmadı

| Hata  | Olası nedenler | Önerilen eylemler |
|---------|---------|---------|
| <br />Hata 34506. Bu bilgisayarda depolanan şifreleme parolası doğru şekilde yapılandırılmamıştır.    | <li> Scratch klasörü yeterli alana sahip olmayan bir birim üzerinde yer alır. <li> Scratch klasörü yanlış taşınmış. <li> OnlineBackup.KEK dosyası eksik.        | <li>MARS Aracısı'nın [en son sürümüne](https://aka.ms/azurebackup_agent) yükseltin.<li>Scratch klasörünü veya önbellek konumunu, yedekleme verilerinin toplam boyutunun %5 ila %10'u arasında boş alana sahip bir ses birimine taşıyın. Önbellek konumunu doğru taşımak [için, dosya ve klasörleri yedekleme yle ilgili sık sorulan sorulara](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq#manage-the-backup-cache-folder)bakın.<li> OnlineBackup.KEK dosyasının bulunduğundan emin olun. <br>*Scratch klasörü veya önbellek yolu için varsayılan konum C:\Program Files\Microsoft Azure Kurtarma Hizmetleri Aracısı\Scratch' dir.*         |

## <a name="backups-dont-run-according-to-schedule"></a>Yedeklemeler zamanlamaya göre çalışmaz

Zamanlanmış yedeklemeler otomatik olarak tetiklenmiyorsa ancak el ile yedeklemeler doğru çalışıyorsa, aşağıdaki eylemleri deneyin:

- Windows Server yedekleme zamanlamasının Azure dosyaları ve klasörleri yedekleme zamanlamasıyla çakışmadığından emin olun.

- Çevrimiçi yedekleme durumunun **Etkinleştir'e**ayarlandığından emin olun. Durumu doğrulamak için aşağıdaki adımları izleyin:

  1. Görev Zamanlayıcısı'nda **Microsoft'u** genişletin ve **Çevrimiçi Yedekleme'yi**seçin.
  1. **Microsoft-OnlineBackup'ı** çift tıklatın ve **Tetikleyiciler** sekmesine gidin.
  1. Durumun **Etkin**olarak ayarlıp ayarlamamasını denetleyin. Değilse, **Edit'i**, **Etkin'i**seçin ve ardından **Tamam'ı**seçin.

- Görevi çalıştırmak için seçilen kullanıcı hesabının sunucudaki **System** veya **Local Administrators grubu** olduğundan emin olun. Kullanıcı hesabını doğrulamak için **Genel** sekmesine gidin ve **Güvenlik** seçeneklerini kontrol edin.

- PowerShell 3.0 veya sonraki sunucuda yüklü olduğundan emin olun. PowerShell sürümünü denetlemek için bu komutu `Major` çalıştırın ve sürüm numarasının 3 veya daha sonra olduğunu doğrulayın:

  `$PSVersionTable.PSVersion`

- Bu yolun ortam değişkeninin `PSMODULEPATH` bir parçası olduğundan emin olun:

  `<MARS agent installation path>\Microsoft Azure Recovery Services Agent\bin\Modules\MSOnlineBackup`

- PowerShell yürütme ilkesi `LocalMachine` için ayarlanmışsa `restricted`, yedekleme görevi tetikleyen PowerShell cmdlet başarısız olabilir. Yürütme ilkesini denetlemek ve ayarlamak için bu komutları `Unrestricted` `RemoteSigned`yükseltilmiş modda çalıştırın:

 ```PowerShell
 Get-ExecutionPolicy -List

Set-ExecutionPolicy Unrestricted
```

- Eksik veya bozuk PowerShell modülü MSOnlineBackup dosyaları olmadığından emin olun. Eksik veya bozuk dosyalar varsa, şu adımları izleyin:

  1. Düzgün çalışan bir MARS aracısı olan herhangi bir makineden, MSOnlineBackup klasörünü C:\Program Files\Microsoft Azure Recovery Services Agent\bin\Modules'den kopyalayın.
  1. Sorunlu makinede, kopyalanan dosyaları aynı klasör konumuna yapıştırın (C:\Program Files\Microsoft Azure Kurtarma Hizmetleri Aracısı\bin\Modules).

     Makinede zaten bir MSOnlineBackup klasörü varsa, dosyaları içine yapıştırın veya varolan dosyaları değiştirin.

> [!TIP]
> Değişikliklerin tutarlı bir şekilde uygulandığından emin olmak için, önceki adımları uyguladıktan sonra sunucuyu yeniden başlatın.

## <a name="troubleshoot-restore-problems"></a>Sorun giderme sorunları geri yükleme

Azure Yedekleme, birkaç dakika sonra bile kurtarma hacmini başarıyla monte edemeyebilir. Ve işlem sırasında hata iletileri alabilirsiniz. Normal şekilde kurtarma ya da kurtarma ya da kurtarma başlatmaya başlamak için şu adımları izleyin:

1. Birkaç dakikadır çalışıyorsa montaj işlemini iptal edin.

2. Yedekleme aracısının en son sürümüne sahip olup olmadığını kontrol edin. Mars konsolunun **Eylemler** bölmesindeki sürümü denetlemek için **Microsoft Azure Kurtarma Hizmetleri Aracısı Hakkında'yı**seçin. **Sürüm** numarasının [bu makalede](https://go.microsoft.com/fwlink/?linkid=229525)belirtilen sürüme eşit veya daha yüksek olduğunu onaylayın. En son [sürümü indirmek](https://go.microsoft.com/fwLink/?LinkID=288905)için bu bağlantıyı seçin.

3. Aygıt **Yöneticisi** > **Depolama denetleyicilerine** gidin ve **Microsoft iSCSI Başlatıcısı'nı**bulun. Yerini tespit ederseniz, doğrudan adım 7'ye gidin.

4. Microsoft iSCSI Başlatıcı hizmetini bulamıyorsanız, Donanım Kimliği **ROOT\ISCSIPRT**ile **Bilinmeyen Aygıt** adlı **Aygıt Yöneticisi** > **Depolama denetleyicileri** altında bir giriş bulmaya çalışın.

5. **Bilinmeyen Aygıt'ı** sağ tıklatın ve **Sürücü YazılımLarını Güncelleştir'i**seçin.

6. **Güncelleştirilmiş sürücü yazılımı için otomatik olarak Arama**seçeneğini seçerek sürücüyü güncelleştirin. Bu güncelleştirme, **Bilinmeyen Aygıtı** Microsoft **iSCSI Başlatıcısı**olarak değiştirmeli:

    ![Depolama denetleyicileri vurgulanmış Azure Yedekleme Aygıt Yöneticisi'nin ekran görüntüsü](./media/backup-azure-restore-windows-server/UnknowniSCSIDevice.png)

7. Görev **Yöneticisi** > **Hizmetleri (Yerel)** > **Microsoft iSCSI Başlatıcı Hizmetine**gidin:

    ![Hizmetler (Yerel) vurgulanmış Azure Yedekleme Görev Yöneticisi'nin ekran görüntüsü](./media/backup-azure-restore-windows-server/MicrosoftInitiatorServiceRunning.png)

8. Microsoft iSCSI Başlatıcı hizmetini yeniden başlatın. Bunu yapmak için, hizmeti sağ tıklatın ve **Durdur'u**seçin. Sonra tekrar sağ tıklatın ve **Başlat'ı**seçin.

9. [Anında Geri Yükleme'yi](backup-instant-restore-capability.md)kullanarak kurtarmayı yeniden deneyin.

Kurtarma hala başarısız olursa, sunucunuzu veya istemcinizi yeniden başlatın. Yeniden başlatmak istemiyorsanız veya sunucuyu yeniden başlattıktan sonra bile kurtarma hala başarısız oluyorsa, [başka bir makineden kurtarmayı](backup-azure-restore-windows-server.md#use-instant-restore-to-restore-data-to-an-alternate-machine)deneyin.

## <a name="troubleshoot-cache-problems"></a>Sorun giderme Önbellek sorunları

Önbellek klasörü (scratch klasörü olarak da adlandırılır) yanlış yapılandırılmışsa, ön koşulları kaçırMışsa veya erişimi kısıtlanmışsa yedekleme işlemi başarısız olabilir.

### <a name="prerequisites"></a>Ön koşullar

ÖNbellek klasöründe başarılı olmak için MARS aracı işlemlerinin aşağıdaki gereksinimlere uyması gerekir:

- [Scratch klasörü konumunda %5 ila %10 serbest ses alanı olduğundan emin olun](backup-azure-file-folder-backup-faq.md#whats-the-minimum-size-requirement-for-the-cache-folder)
- [Scratch klasörü konumunun geçerli ve erişilebilir olduğundan emin olun](backup-azure-file-folder-backup-faq.md#how-to-check-if-scratch-folder-is-valid-and-accessible)
- [Önbellek klasöründeki dosya özniteliklerinin desteklenmesini sağlamak](backup-azure-file-folder-backup-faq.md#are-there-any-attributes-of-the-cache-folder-that-arent-supported)
- [Ayrılan gölge kopyalama depolama alanının yedekleme işlemi için yeterli olduğundan emin olun](#increase-shadow-copy-storage)
- [Önbellek klasörüne erişimi kısıtlayan başka işlemlerin (ör. anti-virüs yazılımı) olmadığından emin olun](#another-process-or-antivirus-software-blocking-access-to-cache-folder)

### <a name="increase-shadow-copy-storage"></a>Gölge kopya depolamasını artırma

Veri kaynağını korumak için gereken yetersiz gölge kopyalama depolama alanı varsa yedekleme işlemleri başarısız olabilir. Bu sorunu gidermek için, aşağıda gösterildiği gibi vssadmin kullanarak korunan birimüzerinde gölge kopya depolama alanı artırın:

- Yükseltilmiş komut isteminden geçerli gölge depolama alanını denetleyin:<br/>
  `vssadmin List ShadowStorage /For=[Volume letter]:`
- Aşağıdaki komutu kullanarak gölge depolama alanını artırın:<br/>
  `vssadmin Resize ShadowStorage /On=[Volume letter]: /For=[Volume letter]: /Maxsize=[size]`

### <a name="another-process-or-antivirus-software-blocking-access-to-cache-folder"></a>Önbellek klasörüne erişimi engelleyen başka bir işlem veya virüsten koruma yazılımı

Sunucuda virüsten koruma yazılımı yüklüyse, bu dosya ve klasörler için virüsten koruma sına gerekli dışlama kuralları ekleyin:  

- Scratch klasörü. Varsayılan konumu C:\Program Files\Microsoft Azure Kurtarma Hizmetleri Aracısı\Scratch
- C:\Program Files\Microsoft Azure Kurtarma Hizmetleri Aracısı\Bin'deki çöp kutusu klasörü
- CBengine.exe
- Csc.exe

## <a name="common-issues"></a>Genel sorunlar

Bu bölüm, MARS aracısını kullanırken karşılaştığınız sık karşılaşılan hataları kapsar.

### <a name="salchecksumstoreinitializationfailed"></a>SalChecksumStoreInitializationFailed

Hata iletisi | Önerilen eylem |
-- | --
Microsoft Azure Kurtarma Hizmetleri Aracısı, karalama konumunda depolanmış yedekleme sağlama toplamına erişemedi | Bu sorunu gidermek için aşağıdakileri gerçekleştirin ve sunucuyu yeniden başlatın <br/> - [Çizilme konumu dosyalarını kilitleyen bir virüsten koruma veya başka işlemler olup olmadığını denetleme](#another-process-or-antivirus-software-blocking-access-to-cache-folder)<br/> - [Çizik konumunun geçerli olup olmadığını ve mars ajanı tarafından erişilebilir olup olmadığını kontrol edin.](backup-azure-file-folder-backup-faq.md#how-to-check-if-scratch-folder-is-valid-and-accessible)

### <a name="salvhdinitializationerror"></a>SalVhdInitializationError

Hata iletisi | Önerilen eylem |
-- | --
Microsoft Azure Kurtarma Hizmetleri Aracısı, VHD’yi başlatmak için karalama konumuna erişemedi | Bu sorunu gidermek için aşağıdakileri gerçekleştirin ve sunucuyu yeniden başlatın <br/> - [Çizilme konumu dosyalarını kilitleyen bir virüsten koruma veya başka işlemler olup olmadığını denetleme](#another-process-or-antivirus-software-blocking-access-to-cache-folder)<br/> - [Çizik konumunun geçerli olup olmadığını ve mars ajanı tarafından erişilebilir olup olmadığını kontrol edin.](backup-azure-file-folder-backup-faq.md#how-to-check-if-scratch-folder-is-valid-and-accessible)

### <a name="sallowdiskspace"></a>SallowDiskSpace

Hata iletisi | Önerilen eylem |
-- | --
Yedekleme, karalama klasöründe bulunan birimde yetersiz depolama nedeniyle başarısız oldu | Bu sorunu gidermek için aşağıdaki adımları doğrulayın ve işlemi yeniden deneyin:<br/>- [MARS ajanının en son olduğundan emin olun](https://go.microsoft.com/fwlink/?linkid=229525&clcid=0x409)<br/> - [Yedekleme çizilme alanını etkileyen depolama sorunlarını doğrulama ve çözme](#prerequisites)

### <a name="salbitmaperror"></a>SalBitmapError

Hata iletisi | Önerilen eylem |
-- | --
Dosyadaki değişiklikler bulunamadı. Bunun çeşitli nedenleri olabilir. Lütfen işlemi yeniden deneyin | Bu sorunu gidermek için aşağıdaki adımları doğrulayın ve işlemi yeniden deneyin:<br/> - [MARS ajanının en son olduğundan emin olun](https://go.microsoft.com/fwlink/?linkid=229525&clcid=0x409) <br/> - [Yedekleme çizilme alanını etkileyen depolama sorunlarını doğrulama ve çözme](#prerequisites)

## <a name="next-steps"></a>Sonraki adımlar

- Azure Yedekleme [aracısıyla Windows Server'ı yedekleme hakkında](tutorial-backup-windows-server-to-azure.md)daha fazla bilgi alın.
- Yedeklemegeri yüklemeniz gerekiyorsa, [windows makinesine dosyaları geri yükleme'ye](backup-azure-restore-windows-server.md)bakın.
