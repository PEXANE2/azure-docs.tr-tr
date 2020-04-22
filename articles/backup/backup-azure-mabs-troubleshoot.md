---
title: Azure Backup Sunucusu sorunlarını giderme
description: Sorun giderme yükleme, Azure Yedekleme Sunucusu kaydı ve uygulama iş yüklerinin yedeklenmesi ve geri yüklenmesi.
ms.reviewer: srinathv
ms.topic: troubleshooting
ms.date: 07/05/2019
ms.openlocfilehash: 7a1cac63ba6497b8580c83fe2b666b020701283a
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81688043"
---
# <a name="troubleshoot-azure-backup-server"></a>Azure Backup Sunucusu sorunlarını giderme

Azure Yedekleme Sunucusu'nu kullanırken karşılaştığınız hataları gidermek için aşağıdaki tablolardaki bilgileri kullanın.

## <a name="basic-troubleshooting"></a>Temel sorun giderme

Microsoft Azure Yedekleme Sunucusu (MABS) sorun giderme başlatmaya başlamadan önce aşağıdaki doğrulamayı gerçekleştirmenizi öneririz:

- [Microsoft Azure Kurtarma Hizmetleri (MARS) Aracısının güncel olduğundan emin olun](https://go.microsoft.com/fwlink/?linkid=229525&clcid=0x409)
- [MARS aracısı ile Azure arasında ağ bağlantısı sağlandığından emin olun](https://docs.microsoft.com/azure/backup/backup-azure-mars-troubleshoot#the-microsoft-azure-recovery-service-agent-was-unable-to-connect-to-microsoft-azure-backup)
- Microsoft Azure Kurtarma Hizmetleri'nin çalıştığından emin olun (Hizmet konsolunda). Gerekirse, işlemi yeniden başlatın ve yeniden deneyin
- [Boş klasör konumunda %5-10 oranında kullanılabilir alan olduğundan emin olun](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq#whats-the-minimum-size-requirement-for-the-cache-folder)
- Kayıt başarısız oluyorsa, Azure Yedekleme Sunucusu'nu yüklemeye çalıştığınız sunucunun başka bir kasaya kayıtlı olmadığından emin olun
- Göndermeli yükleme başarısız olursa DPM aracısının zaten mevcut olup olmadığını kontrol edin. Varsa aracıyı kaldırıp tekrar yüklemeyi deneyin
- [Azure Backup ile çakışan başka bir işlem veya virüsten koruma yazılımı olmadığından emin olun](https://docs.microsoft.com/azure/backup/backup-azure-troubleshoot-slow-backup-performance-issue#cause-another-process-or-antivirus-software-interfering-with-azure-backup)<br>
- SQL Agent hizmetinin çalıştığını ve MABS sunucusunda otomatik olarak ayarlandığından emin olun<br>

## <a name="invalid-vault-credentials-provided"></a>Sağlanan kasa kimlik bilgileri geçersiz

| İşlem | Hata ayrıntıları | Geçici çözüm |
| --- | --- | --- |
| Kasaya kaydolmak | Sağlanan kasa kimlik bilgileri geçersiz. Dosya bozuk veya kurtarma hizmetiyle ilişkili en son kimlik bilgilerine sahip değil. | Önerilen eylem: <br> <ul><li> Kasadan en son kimlik bilgilerini indirin ve yeniden deneyin. <br>(VEYA)</li> <li> Önceki eylem işe yaramadıysa, kimlik bilgilerini farklı bir yerel dizine indirmeyi deneyin veya yeni bir kasa oluşturun. <br>(VEYA)</li> <li> [Bu makalede](https://docs.microsoft.com/azure/backup/backup-azure-mars-troubleshoot#invalid-vault-credentials-provided)açıklandığı gibi tarih ve saat ayarlarını güncelleştirmeyi deneyin. <br>(VEYA)</li> <li> c:\windows\temp'in 65000'den fazla dosyası olup olmadığını kontrol edin. Eski dosyaları başka bir konuma taşıyın veya Geçici klasördeki öğeleri silin. <br>(VEYA)</li> <li> Sertifikaların durumunu kontrol edin. <br> a. **Bilgisayar Sertifikalarını Aç** (Denetim Masası'nda). <br> b. **Kişisel** düğüm ve alt düğüm **Sertifikalarını**genişletin.<br> c.  **Windows Azure Araçları**sertifikasını kaldırın. <br> d. Azure Yedekleme istemcisinde kaydı yeniden deneyin. <br> (VEYA) </li> <li> Herhangi bir grup ilkesinin yerinde olup olmadığını kontrol edin. </li></ul> |

## <a name="replica-is-inconsistent"></a>Yineleme tutarsız

| İşlem | Hata ayrıntıları | Geçici çözüm |
| --- | --- | --- |
| Backup | Yineleme tutarsız | Koruma Grubu Sihirbazı'ndaki otomatik tutarlılık denetimi seçeneğinin açık olduğunu doğrulayın. Yineleme tutarsızlık ve ilgili önerilerin nedenleri hakkında daha fazla bilgi için, yineleme [tutarsız makalebakın.](https://docs.microsoft.com/previous-versions/system-center/data-protection-manager-2006/cc161593(v=technet.10))<br> <ol><li> System State/BMR yedeklemesi durumunda, Windows Server Yedeklemesinin korumalı sunucuda yüklü olduğunu doğrulayın.</li><li> DPM/Microsoft Azure Yedekleme Sunucusu'ndaki DPM depolama havuzunda alanla ilgili sorunları denetleyin ve gerektiğinde depolama alanı ayırın.</li><li> Korumalı sunucudaki Birim Gölge Kopyalama Hizmeti'nin durumunu denetleyin. Devre dışı bırakılmış durumdaysa, el ile başlayacak şekilde ayarlayın. Hizmeti sunucuda başlatın. Ardından DPM/Microsoft Azure Yedekleme Sunucusu konsoluna geri dön ve tutarlılık denetimi işiyle eşitlemeye başlayın.</li></ol>|

## <a name="online-recovery-point-creation-failed"></a>Çevrimiçi kurtarma noktası oluşturma başarısız oldu

| İşlem | Hata ayrıntıları | Geçici çözüm |
| --- | --- | --- |
| Backup | Çevrimiçi kurtarma noktası oluşturma başarısız oldu | **Hata İletisi**: Windows Azure Yedekleme Aracısı seçili birimin anlık görüntüsünü oluşturamadı. <br> **Geçici Çözüm**: Çoğaltma ve kurtarma noktası hacmindeki alanı artırmayı deneyin.<br> <br> **Hata İletisi**: Windows Azure Yedekleme Aracısı OBEngine hizmetine bağlanamıyor <br> **Geçici Çözüm**: OBEngine'in bilgisayardaki çalışan hizmetler listesinde olduğunu doğrulayın. OBEngine hizmeti çalışmıyorsa, OBEngine hizmetini başlatmak için "net start OBEngine" komutunu kullanın. <br> <br> **Hata İletisi**: Bu sunucunun şifreleme parolası ayarlanmaz. Lütfen bir şifreleme parolası yapılandırın. <br> **Geçici Çözüm**: Şifreleme parolasını yapılandırmayı deneyin. Başarısız olursa, aşağıdaki adımları izleyin: <br> <ol><li>Çizilme konumunun var olduğunu doğrulayın. Bu, kayıt defterinde belirtilen **konumHKEY_LOCAL_MACHINE\Software\Microsoft\Windows Azure Yedekleme\Config**, **ScratchLocation** adı ile olmalıdır.</li><li> Scratch konumu varsa, eski parolayı kullanarak yeniden kaydetmeyi deneyin. *Bir şifreleme parolasını yapılandırdığınızda, güvenli bir konuma kaydedin.*</li><ol>|

## <a name="the-original-and-external-dpm-servers-must-be-registered-to-the-same-vault"></a>Orijinal ve harici DPM sunucuları aynı kasaya kaydedilmelidir

| İşlem | Hata ayrıntıları | Geçici çözüm |
| --- | --- | --- |
| Geri Yükleme | **Hata kodu**: CBPServerRegisteredVaultDontMatchWithCurrent/Vault Credentials Hatası: 100110 <br/> <br/>**Hata iletisi**: Orijinal ve harici DPM sunucuları aynı kasaya kaydedilmelidir | **Neden**: Bu sorun, Harici DPM kurtarma seçeneğini kullanarak dosyaları özgün sunucudan alternatif sunucuya geri yüklemeye çalışırken ve kurtarılan sunucu ve verilerin yedeklendiği orijinal sunucu aynı Kurtarma Hizmeti kasasıyla ilişkilendirilmezse oluşur.<br/> <br/>**Geçici Çözüm** Bu sorunu gidermek için hem özgün hem de alternatif sunucunun aynı kasaya kayıtlı olduğundan emin olun.|

## <a name="online-recovery-point-creation-jobs-for-vmware-vm-fail"></a>VMware VM için çevrimiçi kurtarma noktası oluşturma işleri başarısız

| İşlem | Hata ayrıntıları | Geçici çözüm |
| --- | --- | --- |
| Backup | VMware VM için çevrimiçi kurtarma noktası oluşturma işleri başarısız oldu. DPM ChangeTracking bilgi almak için çalışırken VMware bir hata karşılaştı. Hata Kodu - FileFaultFault (ID 33621) |  <ol><li> Etkilenen VM'ler için VMware'deki CTK'yi sıfırla.</li> <li>VMware'de bağımsız diskin yerinde olmadığını kontrol edin.</li> <li>Etkilenen VM'ler için korumayı durdurun ve **Yenile** düğmesiyle yeniden koruyun. </li><li>Etkilenen VM'ler için bir CC çalıştırın.</li></ol>|

## <a name="the-agent-operation-failed-because-of-a-communication-error-with-the-dpm-agent-coordinator-service-on-the-server"></a>Aracı işlemi, sunucudaki DPM aracı koordinatörü hizmetiyle ilgili bir iletişim hatası nedeniyle başarısız oldu

| İşlem | Hata ayrıntıları | Geçici çözüm |
| --- | --- | --- |
| Aracıları korumalı sunuculara itme | Aracı işlemi, ServerName>'ndeki \<DPM Agent Coordinator hizmetinde yapılan bir iletişim hatası nedeniyle başarısız oldu. | **Üründe gösterilen önerilen eylem işe yaramazsa, aşağıdaki adımları gerçekleştirin:** <ul><li> Güvenilmeyen bir etki alanından bilgisayar ekiyorsanız, [aşağıdaki adımları](https://docs.microsoft.com/system-center/dpm/back-up-machines-in-workgroups-and-untrusted-domains?view=sc-dpm-2019)izleyin. <br> (VEYA) </li><li> Güvenilir bir etki alanından bir bilgisayar ekiyorsanız, bu [blogda](https://techcommunity.microsoft.com/t5/system-center-blog/data-protection-manager-agent-network-troubleshooting/ba-p/344726)özetlenen adımları kullanarak sorun giderme. <br>(VEYA)</li><li> Sorun giderme adımı olarak virüsten korumayı devre dışı bırakmayı deneyin. Sorunu giderirse, [bu makalede](https://docs.microsoft.com/system-center/dpm/run-antivirus-server?view=sc-dpm-2019)önerilen virüsten koruma ayarlarını değiştirin.</li></ul> |

## <a name="setup-could-not-update-registry-metadata"></a>Kurulum, kayıt defteri meta verilerini güncelleştiremedi

| İşlem | Hata ayrıntıları | Geçici çözüm |
|-----------|---------------|------------|
|Yükleme | Kurulum kayıt defteri meta verilerini güncelleştiremedi. Bu güncelleştirme hatası depolama tüketiminin aşırı kullanımına neden olabilir. Bu güncelleştirmeyi önlemek için ReFS Kırpma kayıt defteri girişi. | Kayıt defteri anahtarını ayarlayın **SYSTEM\CurrentControlSet\Control\FileSystem\RefsEnableInlineTrim**. Dword değerini 1 olarak ayarlayın. |
|Yükleme | Kurulum kayıt defteri meta verilerini güncelleştiremedi. Bu güncelleştirme hatası depolama tüketiminin aşırı kullanımına neden olabilir. Bunu önlemek için, Birim SnapOptimization kayıt defteri girişini güncelleştirin. | Boş bir dize değeri ile kayıt defteri anahtarı **SOFTWARE\Microsoft Veri Koruma Yöneticisi\Configuration\VolSnapOptimization\WriteIds** oluşturun. |

## <a name="registration-and-agent-related-issues"></a>Kayıt ve aracıyla ilgili sorunlar

| İşlem | Hata ayrıntıları | Geçici çözüm |
| --- | --- | --- |
| Aracıları korumalı sunuculara itme | Sunucu için belirtilen kimlik bilgileri geçersizdir. | **Üründe gösterilen önerilen eylem işe yaramazsa aşağıdaki adımları izleyin:** <br> Koruma aracısını [bu makalede](https://docs.microsoft.com/system-center/dpm/deploy-dpm-protection-agent?view=sc-dpm-2019)belirtildiği gibi üretim sunucusuna el ile yüklemeyi deneyin.|
| Azure Yedekleme Aracısı Azure Yedekleme hizmetine bağlanamadı (ID: 100050) | Azure Yedekleme Aracısı Azure Yedekleme hizmetine bağlanamadı. | **Üründe gösterilen önerilen eylem işe yaramazsa aşağıdaki adımları izleyin:** <br>1. Aşağıdaki komutu yükseltilmiş bir komuttan çalıştırın: **psexec -i -s "c:\Program Files\Internet Explorer\iexplore.exe**. Bu, Internet Explorer penceresini açar. <br/> 2. **Araçlar** > **Internet Seçenekleri** > **Bağlantıları** > LAN**ayarları**gidin. <br/> 3. Proxy sunucusu kullanmak için ayarları değiştirin. Ardından proxy sunucusu ayrıntılarını sağlayın.<br/> 4. Makinenizin internet erişimi sınırlıysa, makine veya proxy üzerindeki güvenlik duvarı ayarlarının bu [URL'lere](install-mars-agent.md#verify-internet-access) ve [IP adresine](install-mars-agent.md#verify-internet-access)izin verdiğinden emin olun.|
| Azure Yedekleme Aracısı yüklemesi başarısız oldu | Microsoft Azure Kurtarma Hizmetleri yüklemesi başarısız oldu. Microsoft Azure Kurtarma Hizmetleri yüklemesi tarafından sistemde yapılan tüm değişiklikler geri alındı. (Kimlik numarası: 4024) | Azure Aracısı'nı el ile yükleyin.

## <a name="configuring-protection-group"></a>Koruma grubunu yapılandırma

| İşlem | Hata ayrıntıları | Geçici çözüm |
| --- | --- | --- |
| Koruma gruplarını yapılandırma | DPM, korumalı bilgisayardaki uygulama bileşenini (korumalı bilgisayar adı) sayamadı. | İlgili veri kaynağı/bileşen düzeyinde yapılandırılan koruma grubu Kullanıcı Arabirimi ekranında **Yenile'yi** seçin. |
| Koruma gruplarını yapılandırma | Koruma yapılandırılamıyor | Korumalı sunucu bir SQL sunucusuysa, sysadmin rol izinlerinin [bu makalede](https://docs.microsoft.com/previous-versions/system-center/system-center-2012-R2/hh757977(v=sc.12))açıklandığı şekilde korunan bilgisayardaki sistem hesabına (NTAuthority\System) sağlandığını doğrulayın.
| Koruma gruplarını yapılandırma | Depolama havuzunda bu koruma grubu için yeterli boş alan yoktur. | Depolama havuzuna eklenen diskler [bir bölüm içermemelidir.](https://docs.microsoft.com/previous-versions/system-center/system-center-2012-R2/hh758075(v=sc.12)) Disklerde varolan birimleri silin. Sonra bunları depolama havuzuna ekleyin.|
| İlke değişikliği |Yedekleme ilkesi değiştirilemedi. Hata: Geçerli işlem bir iç hizmet hatası nedeniyle başarısız oldu [0x29834]. Lütfen bir süre geçtikten sonra işlemi yeniden deneyin. Sorun devam ederse, Microsoft desteğine başvurun. | **Neden:**<br/>Bu hata üç koşul altında oluşur: güvenlik ayarları etkinleştirildiğinde, bekletme aralığını daha önce belirtilen minimum değerlerin altına düşürmeye çalıştığınızda ve desteklenmeyen bir sürümdeyken. (Desteklenmeyen sürümler Microsoft Azure Yedekleme Sunucusu sürümü 2.0.9052 ve Azure Backup Server güncelleştirmesi 1 altındadır.) <br/>**Önerilen eylem:**<br/> İlkeyle ilgili güncelleştirmelere devam etmek için bekletme süresini belirtilen minimum bekletme süresinin üzerine ayarlayın. (Minimum saklama süresi günlük için yedi gün, haftalık dört hafta, aylık için üç hafta veya yıllık bir yıl içindir.) <br><br>İsteğe bağlı olarak, tercih edilen bir diğer yaklaşım da, tüm güvenlik güncelleştirmelerini kullanabilmek için yedekleme aracısını ve Azure Yedekleme Sunucusu'nu güncelleştirmektir. |

## <a name="backup"></a>Backup

| İşlem | Hata ayrıntıları | Geçici çözüm |
| --- | --- | --- |
| Backup | İş çalışırken beklenmeyen bir hata oluştu. Cihaz hazır değil. | **Üründe gösterilen önerilen eylem işe yaramazsa aşağıdaki adımları izleyin:** <br> <ul><li>Gölge Kopya Depolama alanını koruma grubundaki öğelerüzerinde sınırsız olacak şekilde ayarlayın ve ardından tutarlılık denetimini çalıştırın.<br></li> (VEYA) <li>Varolan koruma grubunu silmeyi ve birden çok yeni grup oluşturmayı deneyin. Her yeni koruma grubunda ayrı bir öğe olmalıdır.</li></ul> |
| Backup | Yalnızca sistem durumunu yedeklederseniz, korumalı bilgisayarda sistem durumu yedeklemesini depolamak için yeterli boş alan olduğunu doğrulayın. | <ol><li>Windows Server Backup'ın korumalı makineye yüklü olduğunu doğrulayın.</li><li>Korumalı bilgisayarda sistem durumu için yeterli alan olduğunu doğrulayın. Bunu doğrulamanın en kolay yolu, korumalı bilgisayara gitmek, Windows Server Yedekleme'yi açmak, seçimleri tıklatmak ve ardından BMR'yi seçmektir. UI daha sonra ne kadar alan gerektiğini söyler. AÇ **WSB** > **Yerel Yedekleme** > **Yedekleme çizelgesi** > **Seç Yedek Yapılandırma** > **Tam sunucu** (boyut görüntülenir). Doğrulama için bu boyutu kullanın.</li></ol>
| Backup | BMR için yedekleme hatası | BMR boyutu büyükse, bazı uygulama dosyalarını işletim sistemi sürücüsüne taşıyın ve yeniden deneyin. |
| Backup | Yeni bir Microsoft Azure Yedekleme Sunucusu'nda bir VMware VM'yi yeniden koruma seçeneği eklenecek şekilde gösterilmez. | VMware özellikleri, Microsoft Azure Yedekleme Sunucusu'nun eski ve kullanımdan kaldırılmış bir örneğine işaret edilir. Bu sorunu çözmek için:<br><ol><li>VCenter'da (SC-VMM eşdeğeri), **Özet** sekmesine gidin ve ardından **Özel Özniteliklere**gidin.</li>  <li>Eski Microsoft Azure Yedekleme Sunucusu adını **DPMServer** değerinden silin.</li>  <li>Yeni Microsoft Azure Yedekleme Sunucusu'na geri dön ve PG'yi değiştirin.  **Yenile** düğmesini seçtikten sonra, VM koruma eklemek için kullanılabilir olarak bir onay kutusu ile görünür.</li></ol> |
| Backup | Dosyalara/paylaşılan klasörlere erişirken hata | Bu makalede önerilen virüsten koruma ayarlarını değiştirmeyi deneyin [DPM sunucusunda virüsten koruma yazılımı çalıştırın.](https://docs.microsoft.com/system-center/dpm/run-antivirus-server?view=sc-dpm-2019)|

## <a name="change-passphrase"></a>Parola sözcükünü değiştir

| İşlem | Hata ayrıntıları | Geçici çözüm |
| --- | --- | --- |
| Parola sözcükünü değiştir |Girilen güvenlik PINi yanlış. Bu işlemi tamamlamak için doğru güvenlik PIN'ini sağlayın. |**Neden:**<br/> Bu hata, kritik bir işlem gerçekleştirirken (parola değiştirme gibi) geçersiz veya süresi dolmuş bir güvenlik PIN'i girdiğinizde oluşur. <br/>**Önerilen eylem:**<br/> İşlemi tamamlamak için geçerli bir güvenlik PIN'i girmeniz gerekir. PIN'i almak için Azure portalında oturum açın ve Kurtarma Hizmetleri kasasına gidin. Ardından **Ayarlar** > **Özellikleri** > **Güvenlik PINi Oluştur'a**gidin. Parolayı değiştirmek için bu PIN'i kullanın. |
| Parola sözcükünü değiştir |İşlem başarısız oldu. Kimlik Numarası: 120002 |**Neden:**<br/>Bu hata, güvenlik ayarları etkinleştirildiğinde veya desteklenmeyen bir sürüm kullanırken parolayı değiştirmeye çalıştığınızda oluşur.<br/>**Önerilen eylem:**<br/> Parolayı değiştirmek için öncelikle yedekleme aracısını 2.0.9052 olan minimum sürüme güncelleştirmeniz gerekir. Ayrıca Azure Yedekleme Sunucusu'nu en az güncelleştirme 1'e güncelleştirmeniz ve ardından geçerli bir güvenlik PIN'i girmeniz gerekir. PIN'i almak için Azure portalında oturum açın ve Kurtarma Hizmetleri kasasına gidin. Ardından **Ayarlar** > **Özellikleri** > **Güvenlik PINi Oluştur'a**gidin. Parolayı değiştirmek için bu PIN'i kullanın. |

## <a name="configure-email-notifications"></a>E-posta bildirimlerini yapılandırma

| İşlem | Hata ayrıntıları | Geçici çözüm |
| --- | --- | --- |
| Office 365 hesabını kullanarak e-posta bildirimleri ayarlama |Hata Kimliği: 2013| **Neden:**<br> Office 365 hesabını kullanmaya çalışıyorum <br>**Önerilen eylem:**<ol><li> Emin olmak için ilk şey, DPM sunucunuz için "Receive Connector'da Anonim Geçişe İzin Ver"in Exchange'de ayarlandığıdır. Bunu nasıl yapılandırılabilen ler hakkında daha fazla bilgi [için](https://docs.microsoft.com/exchange/mail-flow/connectors/allow-anonymous-relay?view=exchserver-2019)bkz.</li> <li> Dahili bir SMTP rölesi kullanamıyorsanız ve Office 365 sunucunuzu kullanarak ayarlamanız gerekiyorsa, IIS'yi bir röle olarak ayarlayabilirsiniz. [SMTP'yi IIS kullanarak O365'e aktarmak için](https://docs.microsoft.com/exchange/mail-flow/test-smtp-with-telnet?view=exchserver-2019)DPM sunucusunu yapılandırın).<br><br>  Kullanıcıyı\@ *domain.com* biçimi nden değil, etki alanı\kullanıcıyı kullandığınızdan emin olun.<br><br><li>Yerel sunucu adını SMTP sunucusu, bağlantı noktası 587 olarak kullanmak için DPM noktasını işaretleyin. Ardından, e-postaların gelmesi gerektiğini kullanıcı e-postasına yönlendirin.<li> DPM SMTP kurulum sayfasındaki kullanıcı adı ve parola, DPM'nin açık olduğu etki alanında bir etki alanı hesabı için olmalıdır. </li><br> SMTP sunucu adresini değiştirirken, yeni ayarlarda değişiklik yapın, ayarlar kutusunu kapatın ve yeni değeri yansıttığından emin olmak için yeniden açın.  Yalnızca değiştirme ve sınama her zaman yeni ayarların etkili olmasına neden olmayabilir, bu nedenle bu şekilde test etmek en iyi yöntemdir.<br><br>Bu işlem sırasında herhangi bir zamanda, DPM konsolu kapatarak ve aşağıdaki kayıt defteri tuşlarını düzenleyerek bu ayarları temizleyebilirsiniz: **HKLM\SOFTWARE\Microsoft\Microsoft Veri Koruma Yöneticisi\Bildirim\ <br/> SMTPPassword ve SMTPUserName anahtarlarını sil**. Yeniden başlattığınızda bunları ui'ye geri ekleyebilirsiniz.

## <a name="common-issues"></a>Genel sorunlar

Bu bölüm, Azure Yedekleme Sunucusu'nu kullanırken karşılaşabileceğiniz sık karşılaşılan hataları kapsar.

### <a name="cbpsourcesnapshotfailedreplicamissingorinvalid"></a>CBPSourceSnapshotFailedReplicaMissingOrInvalid

Hata iletisi | Önerilen eylem |
-- | --
Disk yedekleme kopyası geçersiz ya da eksik olduğundan yedekleme başarısız oldu. | Bu sorunu gidermek için aşağıdaki adımları doğrulayın ve işlemi yeniden deneyin: <br/> 1. Disk kurtarma noktası oluşturma<br/> 2. Veri kaynağında tutarlılık denetimi çalıştırma <br/> 3. Veri kaynağının korunmasını durdurun ve bu veri kaynağı için korumayı yeniden yapılandırın

### <a name="cbpsourcesnapshotfailedreplicametadatainvalid"></a>CBPSourceSnapshotFailedReplicaMetadataInvalid

Hata iletisi | Önerilen eylem |
-- | --
Yinelemedeki meta veriler geçersiz olduğundan kaynak birim anlık görüntüsü başarısız oldu. | Bu veri kaynağının bir disk kurtarma noktası oluşturun ve çevrimiçi yedeklemeyi yeniden deneyin

### <a name="cbpsourcesnapshotfailedreplicainconsistent"></a>CBPSourceSnapshotFailedReplicaInconsistent

Hata iletisi | Önerilen eylem |
-- | --
Tutarsız veri kaynağı çoğaltma nedeniyle kaynak birim anlık görüntü başarısız oldu. | Bu veri kaynağında tutarlılık denetimi çalıştırın ve yeniden deneyin

### <a name="cbpsourcesnapshotfailedreplicacloningissue"></a>CBPSourceSnapshotFailedReplicaCloningIssue

Hata iletisi | Önerilen eylem |
-- | --
Disk yedekleme çoğaltması kopyalanamadığı için yedekleme başarısız oldu.| Önceki tüm disk yedekleme çoğaltma dosyalarının (.vhdx) monte edilmiş olduğundan ve çevrimiçi yedeklemesırasında disk yedeklemesi yapılmadığından emin olun
