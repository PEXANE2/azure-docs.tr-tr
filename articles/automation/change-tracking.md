---
title: Azure Otomasyonu Değişiklik İzleme ve envantere genel bakış
description: Bu makalede, ortamınızdaki yazılım ve Microsoft hizmet değişikliklerini belirlemenize yardımcı olan Değişiklik İzleme ve envanter özelliği açıklanır.
services: automation
ms.subservice: change-inventory-management
ms.date: 06/08/2020
ms.topic: conceptual
ms.openlocfilehash: 2aab90b12cd3844b94b0b7e6e94582d403db2efe
ms.sourcegitcommit: 964af22b530263bb17fff94fd859321d37745d13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84555044"
---
# <a name="change-tracking-and-inventory-overview"></a>Değişiklik İzleme ve envantere genel bakış

Bu makalede, Azure Otomasyonu 'nda Değişiklik İzleme ve envanter sunulmaktadır. Bu özellik, dağıtım paketi Yöneticisi tarafından yönetilen yazılımlarla işletimsel ve çevresel sorunları belirlemenize yardımcı olması için sanal makinelerde ve sunucu altyapısında yapılan değişiklikleri izler. Değişiklik İzleme ve envanter tarafından izlenen öğeler şunları içerir: 

- Windows yazılımı
- Linux yazılımı (paket)
- Windows ve Linux dosyaları
- Windows kayıt defteri anahtarları
- Microsoft Hizmetleri
- Linux Daemon 'ları

> [!NOTE]
> Azure Resource Manager özellik değişikliklerini izlemek için bkz. Azure Kaynak Grafiği [değişiklik geçmişi](../governance/resource-graph/how-to/get-resource-changes.md).

Değişiklik İzleme ve envanter, verileri Azure Izleyici 'den alır. Log Analytics çalışma alanlarına bağlı sanal makineler, izlenen sunuculardaki yüklü yazılım, Microsoft Hizmetleri, Windows kayıt defteri ve dosyalar ve Linux Daemon 'ları değişikliklerle ilgili verileri toplamak için Log Analytics aracılarını kullanır. Veriler kullanılabilir olduğunda, aracılar işlenmek üzere Azure Izleyici 'ye gönderir. Azure Izleyici alınan verilere yönelik mantığı uygular, kaydeder ve kullanılabilir hale getirir. 

> [!NOTE]
> Değişiklik İzleme ve envanter özelliğini kullanmak için, tüm VM 'lerinizi Otomasyon hesabının aynı abonelik ve bölgesine bulmanız gerekir.

Değişiklik İzleme ve Inventory Şu anda aşağıdaki öğeleri desteklemiyor:

- Windows kayıt defteri izleme için özyineleme
- Ağ dosya sistemleri
- Farklı yükleme yöntemleri
- *Windows için **. exe** dosyaları

Diğer sınırlamalar:

- **En büyük dosya boyutu** sütunu ve değerleri geçerli uygulamada kullanılmıyor.
- 30 dakikalık bir toplama döngüsüne 2500 'den fazla dosya topladıysanız, Değişiklik İzleme ve stok performansı düşebilir.
- Ağ trafiği yüksekse, değişiklik kayıtlarının görüntülenmesi altı saate kadar sürebilir.
- Bir bilgisayar kapatılırken bir yapılandırmayı değiştirirseniz, bilgisayar önceki yapılandırmaya ait değişiklikleri gönderebilir.

Değişiklik İzleme ve envanter Şu anda aşağıdaki sorunları yaşıyor:

- Düzeltme güncelleştirmeleri Windows Server 2016 Core RS3 makinelerinde toplanmaz.

- Linux Daemon 'ları, hiçbir değişiklik gerçekleşmese de değiştirilmiş durumu gösterebilir. Bu sorun, `SvcRunLevels` Azure Izleyici [ConfigurationChange](https://docs.microsoft.com/azure/azure-monitor/reference/tables/configurationchange) günlüğündeki verilerin yakalandığı şekilde ortaya çıkar.

## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

Değişiklik İzleme ve envanter, Log Analytics Aracısı gereksinimlerini karşılayan tüm işletim sistemlerinde desteklenir. Resmi işletim sistemi sürümleri Windows Server 2008 SP1 veya üzeri ve Windows 7 SP1 veya sonraki bir sürümü. Özelliği, bir dizi Linux işletim sisteminde de desteklenir. Log Analytics destekleyen işletim sistemleri için bkz. [Log Analytics aracısına genel bakış](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent).

TLS 1,2 için istemci gereksinimlerini anlamak üzere bkz. [Azure Otomasyonu Için tls 1,2 zorlaması](automation-managing-data.md#tls-12-enforcement-for-azure-automation).

## <a name="network-requirements"></a>Ağ gereksinimleri

Değişiklik İzleme ve envanteri özellikle bir sonraki tabloda listelenen ağ adreslerini gerektirir. Bu adreslere yönelik iletişimler 443 numaralı bağlantı noktasını kullanır.

|Azure Genel  |Azure Kamu  |
|---------|---------|
|*.ods.opinsights.azure.com    | *. ods.opinsights.azure.us         |
|*.oms.opinsights.azure.com     | *. oms.opinsights.azure.us        |
|*.blob.core.windows.net | *. blob.core.usgovcloudapi.net|
|*.azure-automation.net | *. azure-automation.us|

## <a name="change-tracking-and-inventory-user-interface"></a>Değişiklik İzleme ve envanter Kullanıcı arabirimi

İzlenen bilgisayarların değişikliklerinin özetini görüntülemek için Azure portal Değişiklik İzleme ve envanterini kullanın. Otomasyon hesabınızda **yapılandırma yönetimi** altındaki **değişiklik izleme** veya **Stok** için VM ekle seçeneklerinden birini seçerek özelliği kullanabilirsiniz.  

![Değişiklik İzleme panosu](./media/change-tracking/change-tracking-dash01.png)

Değişiklik izleme grafiğini ve değişiklik türü ve zaman aralıklarına göre ayrıntılı bilgileri sınırlamak için panonun üst kısmında açılır listeler bulunur. Ayrıca, özel bir zaman aralığı seçmek için grafiğe tıklayıp sürükleyebilirsiniz. 

Ayrıntılarını görüntülemek için bir değişikliğe veya olaya tıklayabilirsiniz. Kullanılabilir değişiklik türleri şunlardır:

- Ekinlikler
- Daemon 'ları
- Dosyalar
- Kayıt Defteri
- Yazılım
- Microsoft Hizmetleri

Her değişikliği ekleyebilir, değiştirebilir veya kaldırabilirsiniz. Aşağıdaki örnekte, bir hizmetin başlangıç türünde El Ile otomatik olarak bir değişiklik gösterilmektedir.

![Değişiklik İzleme ve envanter ayrıntıları](./media/change-tracking/change-tracking-details.png)

## <a name="fim-support-in-azure-security-center"></a>Azure Güvenlik Merkezi 'nde FIM desteği

Değişiklik İzleme ve envanter, [Azure Güvenlik Merkezi dosya bütünlüğü izleme (FIM)](https://docs.microsoft.com/azure/security-center/security-center-file-integrity-monitoring)kullanımını sağlar. FIM yalnızca dosyaları ve kayıt defterlerini izlerken, tam Değişiklik İzleme ve envanter özelliği için izleme de içerir:

- Yazılım değişiklikleri
- Microsoft Hizmetleri
- Linux Daemon 'ları

> [!NOTE]
> Tam Değişiklik İzleme ve envanter özelliğinin etkinleştirilmesi ek ücretlere neden olabilir. Bkz. [Otomasyon Fiyatlandırması](https://azure.microsoft.com/pricing/details/automation/). FIM 'yi, Azure portal bulunan [yüklü izleme çözümleri listesinden](../azure-monitor/insights/solutions.md#list-installed-monitoring-solutions) silmek mümkündür. Bkz. [izleme çözümünü kaldırma](../azure-monitor/insights/solutions.md#remove-a-monitoring-solution).

## <a name="tracking-of-file-changes"></a>Dosya değişikliklerinin izlenmesi

Hem Windows hem de Linux 'ta dosyalardaki değişiklikleri izlemek için, Değişiklik İzleme ve stok dosyaların MD5 karma değerlerini kullanır. Bu özellik, son stoktan bu yana değişikliklerin yapılmış olup olmadığını algılamak için karmaları kullanır.

## <a name="tracking-of-file-content-changes"></a>Dosya içeriği değişikliklerinin izlenmesi

Değişiklik İzleme ve envanter, bir Windows veya Linux dosyasının içeriğini görüntülemenizi sağlar. Bir dosyada yapılan her değişiklik için Değişiklik İzleme ve stok, dosyanın içeriğini bir [Azure depolama hesabına](../storage/common/storage-create-storage-account.md)depolar. Bir dosyayı izlerken, bir değişiklikten önce veya sonra içeriğini görüntüleyebilirsiniz. Dosya içeriği satır içi veya yan yana görüntülenebilir. 

![Dosyadaki değişiklikleri görüntüleme](./media/change-tracking/view-file-changes.png)

## <a name="tracking-of-registry-keys"></a>Kayıt defteri anahtarlarının izlenmesi

Değişiklik İzleme ve envanter, Windows kayıt defteri anahtarlarının değişikliklerinin izlenmesini sağlar. İzleme, üçüncü taraf kod ve kötü amaçlı yazılımların etkinleştirebileceği genişletilebilirlik noktalarını belirlemenize olanak sağlar. Aşağıdaki tabloda önceden yapılandırılmış (ancak etkinleştirilmemiş) kayıt defteri anahtarları listelenmektedir. Bu anahtarları izlemek için her birini etkinleştirmeniz gerekir.

> [!div class="mx-tdBreakAll"]
> |Kayıt Defteri Anahtarı | Amaç |
> | --- | --- |
> |`HKEY\LOCAL\MACHINE\Software\Microsoft\Windows\CurrentVersion\Group Policy\Scripts\Startup` | Başlangıçta çalıştırılan betikleri izler.
> |`HKEY\LOCAL\MACHINE\Software\Microsoft\Windows\CurrentVersion\Group Policy\Scripts\Shutdown` | Kapatılırken çalıştırılan betikleri izler.
> |`HKEY\LOCAL\MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Run` | Kullanıcı Windows hesabında oturum açmadan önce yüklenen anahtarları izler. Anahtar, 64 bit bilgisayarlarda çalışan 32 bitlik uygulamalar için kullanılır.
> |`HKEY\LOCAL\MACHINE\SOFTWARE\Microsoft\Active Setup\Installed Components` | Uygulama ayarlarındaki değişiklikleri izler.
> |`HKEY\LOCAL\MACHINE\Software\Classes\Directory\ShellEx\ContextMenuHandlers` | Doğrudan Windows Gezgini 'nde bulunan ve genellikle **Explorer. exe**ile işlem içinde çalıştırılan bağlam menüsü işleyicilerini izler.
> |`HKEY\LOCAL\MACHINE\Software\Classes\Directory\Shellex\CopyHookHandlers` | Doğrudan Windows Gezgini 'ne bağlanan ve genellikle **Explorer. exe**ile işlem içinde çalışan kopya kanca işleyicilerini izler.
> |`HKEY\LOCAL\MACHINE\Software\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers` | Simge kaplama işleyicisi kaydını izler.
> |`HKEY\LOCAL\MACHINE\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers` | 64 bit bilgisayarlarda çalışan 32 bitlik uygulamalar için simge kaplama işleyici kaydını izler.
> |`HKEY\LOCAL\MACHINE\Software\Microsoft\Windows\CurrentVersion\Explorer\Browser Helper Objects` | Internet Explorer için yeni tarayıcı yardımcısı nesne eklentilerini izler. Geçerli sayfanın Belge Nesne Modeli (DOM) erişmek ve gezintiyi denetlemek için kullanılır.
> |`HKEY\LOCAL\MACHINE\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Explorer\Browser Helper Objects` | Internet Explorer için yeni tarayıcı yardımcısı nesne eklentilerini izler. Geçerli sayfanın Belge Nesne Modeli (DOM) öğesine erişmek ve 64 bit bilgisayarlarda çalışan 32 bitlik uygulamalar için gezintiyi denetlemek için kullanılır.
> |`HKEY\LOCAL\MACHINE\Software\Microsoft\Internet Explorer\Extensions` | Özel araç menüleri ve özel araç çubuğu düğmeleri gibi yeni Internet Explorer uzantılarını izler.
> |`HKEY\LOCAL\MACHINE\Software\Wow6432Node\Microsoft\Internet Explorer\Extensions` | 64 bit bilgisayarlarda çalışan 32 bitlik uygulamalar için özel araç menüleri ve özel araç çubuğu düğmeleri gibi yeni Internet Explorer uzantılarına yönelik izleyiciler.
> |`HKEY\LOCAL\MACHINE\Software\Microsoft\Windows NT\CurrentVersion\Drivers32` | WaveMapper, wave1 ve Wave2, msacm. ımaadpcm,. msadpcm,. msgsm610 ve vidc ile ilişkili 32 bitlik sürücüleri izler. **System. ini** dosyasındaki [Drivers] bölümüne benzer.
> |`HKEY\LOCAL\MACHINE\Software\Wow6432Node\Microsoft\Windows NT\CurrentVersion\Drivers32` | , 64 bit bilgisayarlarda çalışan 32 bit uygulamalar için WaveMapper, wave1 ve Wave2, msacm. ımaadpcm,. msadpcm,. msgsm610 ve vidc ile ilişkili 32 bitlik sürücüleri izler. **System. ini** dosyasındaki [Drivers] bölümüne benzer.
> |`HKEY\LOCAL\MACHINE\System\CurrentControlSet\Control\Session Manager\KnownDlls` | Bilinen veya yaygın olarak kullanılan sistem dll 'lerinin listesini izler. İzleme, kullanıcıların, sistem dll 'lerinin Truva atı sürümlerini bırakarak zayıf uygulama dizini izinleriyle yararlanmasını önler.
> |`HKEY\LOCAL\MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon\Notify` | Windows için etkileşimli oturum açma desteği modeli olan **Winlogon. exe**' den olay bildirimleri alabilen paketlerin listesini izler.

## <a name="recursion-support"></a>Özyineleme desteği

Değişiklik İzleme ve envanter özyineleme destekler, bu da dizinler genelinde izlemeyi basitleştirmek için joker karakterler belirtmenize olanak tanır. Özyineleme Ayrıca, birden çok veya dinamik sürücü adına sahip ortamlarda dosyaları izlemenize olanak tanımak için ortam değişkenleri sağlar. Aşağıdaki listede, özyineleme yapılandırılırken bilmeniz gereken yaygın bilgiler yer almaktadır:

- Birden çok dosyayı izlemek için joker karakterler gereklidir.

- Joker karakterleri yalnızca bir dosya yolunun son kesiminde (örneğin, **arayacak \\ dosyası*** veya **/etc/*. conf**) kullanabilirsiniz.

- Bir ortam değişkeni geçersiz bir yol içeriyorsa, doğrulama başarılı olur ancak yürütme sırasında yol başarısız olur.

- Yolu ayarlarken genel yol adlarından kaçının. bu tür bir ayar, çok fazla klasör alınmasına neden olabilir.

## <a name="change-tracking-and-inventory-data-collection"></a>Değişiklik İzleme ve envanter verileri toplama

Sonraki tabloda, Değişiklik İzleme ve envanter tarafından desteklenen değişiklik türleri için veri toplama sıklığı gösterilmektedir. Her tür için, geçerli durumun veri anlık görüntüsü en az 24 saatte bir yenilenir.

| **Türü Değiştir** | **Sıklık** |
| --- | --- |
| Windows kayıt defteri | 50 dakika |
| Windows dosyası | 30 dakika |
| Linux dosyası | 15 dakika |
| Microsoft Hizmetleri | 10 saniye ila 30 dakika</br> Varsayılan: 30 dakika |
| Linux Daemon 'ları | 5 dakika |
| Windows yazılımı | 30 dakika |
| Linux yazılımı | 5 dakika |

Aşağıdaki tabloda, Değişiklik İzleme ve envanter için makine başına izlenen öğe sınırları gösterilmektedir.

| **Kaynak** | **Sınırlı** |
|---|---|---|
|Dosya|500|
|Kayıt Defteri|250|
|Windows yazılımı (düzeltmeler dahil değil) |250|
|Linux paketleri|1250|
|Hizmetler|250|
|Daemon 'ları|250|

Değişiklik İzleme ve envanter kullanan bir makineye yönelik ortalama Log Analytics veri kullanımı ortamınıza bağlı olarak ayda yaklaşık 40 MB 'dir. Log Analytics çalışma alanının kullanım ve tahmini maliyetler özelliği sayesinde, kullanım grafiğinde Değişiklik İzleme ve envanterle alınan verileri görüntüleyebilirsiniz. Veri kullanımınızı değerlendirmek ve faturanızı nasıl etkileyeceğini öğrenmek için bu veri görünümünü kullanın. Bkz. [kullanımınızı anlayın ve maliyetleri tahmin edin](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#understand-your-usage-and-estimate-costs).

### <a name="microsoft-service-data"></a>Microsoft hizmet verileri

Microsoft Hizmetleri için varsayılan toplama sıklığı 30 dakikadır. **Ayarları Düzenle**' nin altındaki **Microsoft Hizmetleri** sekmesinde bir kaydırıcıyı kullanarak sıklığı yapılandırabilirsiniz.

![Microsoft Hizmetleri kaydırıcısı](./media/change-tracking/windowservices.png)

Log Analytics Aracısı, performansı iyileştirmek için yalnızca değişiklikleri izler. Hizmetin özgün durumuna geri dönmesi durumunda yüksek eşikte değişiklikler kaçırılması gerekebilir. Sıklığı daha küçük bir değere ayarlamak, aksi takdirde kaçırılmış olabilecek değişiklikleri yakalamadığınızı sağlar.

> [!NOTE]
> Aracı değişiklikleri 10 saniyelik bir aralığa göre izlebilirken, verilerin Azure portal görüntülenmesi birkaç dakika sürer. Portalda görüntülenmesi sırasında oluşan değişiklikler hala izlenir ve günlüğe kaydedilir.

## <a name="support-for-alerts-on-configuration-state"></a>Yapılandırma durumunda uyarılar için destek

Değişiklik İzleme ve envanterin önemli bir özelliği, karma ortamınızın yapılandırma durumundaki değişiklikler hakkında uyarı verebilir. Birçok yararlı eylem, uyarılara yanıt olarak tetiklenebilir. Örneğin, Azure işlevleri, Otomasyon Runbook 'ları, Web kancaları ve benzeri eylemler. Bir makine için **c:\Windows\system32\drivers\etc\hosts** dosyasında yapılan değişikliklerle ilgili uyarı verme, değişiklik izleme ve envanter verileri için tek iyi bir uyarı uygulamasıdır. Sonraki tabloda tanımlanan sorgu senaryoları da dahil olmak üzere, uyarı için çok daha fazla senaryo vardır.

|Sorgu  |Description  |
|---------|---------|
|ConfigurationChange <br>ConfigChangeType = = "Files" ve Filesystemmpath "c: \\ Windows \\ system32 \\ drivers \\ " içerdiğinde &#124;|Sistem açısından kritik dosyalarda yapılan değişiklikleri izlemek için faydalıdır.|
|ConfigurationChange <br>Alanlardaki "FileContentChecksum" ve Filesystemmpath = = "c: \\ Windows \\ system32 \\ Drivers ve \\ \\ Konakları" içeren &#124;|Anahtar yapılandırma dosyalarında yapılan değişiklikleri izlemek için faydalıdır.|
|ConfigurationChange <br>ConfigChangeType = = "WindowsServices" ve SvcName "W3SVC" ve SvcState = = "durduruldu" içerdiğinde &#124;|Sistem açısından kritik hizmetlerde yapılan değişiklikleri izlemek için faydalıdır.|
|ConfigurationChange <br>&#124; ConfigChangeType = = "Daemon 'ları" ve SvcName "SSH" ve SvcState! = "Running" içerir|Sistem açısından kritik hizmetlerde yapılan değişiklikleri izlemek için faydalıdır.|
|ConfigurationChange <br>&#124; ConfigChangeType = = "Software" ve ChangeCategory = = "Added"|Kilitli yazılım yapılandırmalarına ihtiyaç duyulan ortamlar için faydalıdır.|
|ConfigurationData <br>SoftwareName, "Izleme Aracısı" ve CurrentVersion! = "8.0.11081.0" içerdiğinde &#124;|Güncel olmayan veya uyumsuz yazılım sürümünün yüklü olduğu makineleri görmek için faydalıdır. Bu sorgu, son bildirilen yapılandırma durumunu bildirir, ancak değişiklikleri rapor etmez.|
|ConfigurationChange <br>&#124; RegistryKey = = @ "HKEY_LOCAL_MACHINE \\ Software \\ Microsoft \\ Windows \\ CurrentVersion \\ QualityCompat"| Önemli virüsten koruma anahtarlarındaki değişiklikleri izlemek için faydalıdır.|
|ConfigurationChange <br>RegistryKey 'ın @ "HKEY_LOCAL_MACHINE \\ System \\ CurrentControlSet \\ Services \\ SharedAccess \\ parametreleri \\ firewallpolicy" içerdiğini &#124;| Güvenlik Duvarı ayarlarında yapılan değişiklikleri izlemek için faydalıdır.|

## <a name="next-steps"></a>Sonraki adımlar

- Otomasyon hesabından özelliği etkinleştirmek için bkz. [Otomasyon hesabından değişiklik izleme ve envanteri etkinleştirme](automation-enable-changes-from-auto-acct.md).

- Azure portal göz atarak özelliği etkinleştirmek için, bkz. [Azure portal değişiklik izleme ve envanteri etkinleştirme](automation-onboard-solutions-from-browse.md).

- Bir runbook 'tan özelliği etkinleştirmek için, bkz. [runbook 'tan değişiklik izleme ve envanteri etkinleştirme](automation-enable-changes-from-runbook.md).

- Özelliği bir Azure VM 'den etkinleştirmek için bkz. [Azure VM 'den değişiklik izleme ve envanteri etkinleştirme](automation-enable-changes-from-vm.md).