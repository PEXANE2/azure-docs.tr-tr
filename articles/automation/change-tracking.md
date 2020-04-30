---
title: Azure Otomasyonu 'nda Değişiklik İzleme ve envantere genel bakış
description: Değişiklik İzleme ve envanter, ortamınızda gerçekleşen yazılım ve Microsoft hizmet değişikliklerini belirlemenize yardımcı olur.
services: automation
ms.subservice: change-inventory-management
ms.date: 01/28/2019
ms.topic: conceptual
ms.openlocfilehash: 1208e08f7b85e893ba754bdbdf71a2da4f68c90a
ms.sourcegitcommit: eaec2e7482fc05f0cac8597665bfceb94f7e390f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82509083"
---
# <a name="overview-of-change-tracking-and-inventory"></a>Değişiklik İzleme ve stoğa genel bakış

Bu makalede, Azure Otomasyonu 'nda Değişiklik İzleme ve envanter sunulmaktadır. Bu özellik, dağıtım paketi Yöneticisi tarafından yönetilen yazılımlarla işletimsel ve çevresel sorunları belirlemenize yardımcı olması için sanal makinelerde ve sunucu altyapısında yapılan değişiklikleri izler. Değişiklik İzleme ve envanter tarafından izlenen öğeler şunları içerir: 

- Windows yazılımı
- Linux yazılımı (paket)
- Windows ve Linux dosyaları
- Windows kayıt defteri anahtarları
- Microsoft Hizmetleri
- Linux Daemon 'ları

Değişiklik İzleme ve envanter, verileri bulutta Azure Izleyici hizmetinden alır. Azure, izlenen sunuculardaki yüklü yazılım, Microsoft Hizmetleri, Windows kayıt defteri ve dosyalar ve Linux Daemon 'ları değişikliklerini işlenmek üzere Azure Izleyici 'ye gönderir. Bulut hizmeti alınan verilere yönelik mantığı uygular, kaydeder ve kullanılabilir hale getirir. 

> [!NOTE]
> Azure Resource Manager özellik değişikliklerini izlemek için bkz. Azure Kaynak Grafiği [değişiklik geçmişi](../governance/resource-graph/how-to/get-resource-changes.md).

Değişiklik İzleme ve Inventory Şu anda aşağıdaki öğeleri desteklemiyor:

* Windows kayıt defteri izleme için özyineleme
* Ağ dosya sistemleri
* Farklı yükleme yöntemleri
* *Windows için **. exe** dosyaları

Diğer sınırlamalar:

* **En büyük dosya boyutu** sütunu ve değerleri geçerli uygulamada kullanılmıyor.
* 30 dakikalık bir koleksiyon döngüsüne 2500 'den fazla dosya topladıysanız çözüm performansı düşebilir.
* Ağ trafiği yüksekse, değişiklik kayıtlarının görüntülenmesi altı saate kadar sürebilir.
* Bir bilgisayar kapatılırken bir yapılandırmayı değiştirirseniz, bilgisayar önceki yapılandırmaya ait değişiklikleri gönderebilir.

Değişiklik İzleme ve envanter Şu anda aşağıdaki sorunları yaşıyor:

* Düzeltme güncelleştirmeleri Windows Server 2016 Core RS3 makinelerinde toplanmaz.
* Linux Daemon 'ları, hiçbir değişiklik gerçekleşmese de değiştirilmiş durumu gösterebilir. Bu sorun, Azure Izleyici [ConfigurationChange](https://docs.microsoft.com/azure/azure-monitor/reference/tables/configurationchange) günlüğündeki `SvcRunLevels` verilerin yakalandığı şekilde ortaya çıkar.

## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

Değişiklik İzleme ve envanter ve Azure Izleyici Log Analytics aracıları hem Windows hem de Linux işletim sistemlerinde desteklenir.

### <a name="windows-operating-systems"></a>Windows işletim sistemleri

Resmi olarak desteklenen Windows işletim sistemi sürümü Windows Server 2008 R2 veya üzeri bir sürümdür.

### <a name="linux-operating-systems"></a>Linux işletim sistemleri

Aşağıda ele alınan Linux dağıtımları, Linux için Log Analytics Aracısı için resmi olarak desteklenir. Ancak, Linux Aracısı listelenmeyen diğer dağıtımlar üzerinde de çalıştırılabilir. Aksi belirtilmediği takdirde, listelenen her ana sürüm için tüm küçük yayınlar desteklenir.

#### <a name="64-bit-linux-operating-systems"></a>64-bit Linux işletim sistemleri

* CentOS 6 ve 7
* Amazon Linux 2017,09
* Oracle Linux 6 ve 7
* Red Hat Enterprise Linux Server 6 ve 7
* Borçlu GNU/Linux 8 ve 9
* Ubuntu Linux 14,04 LTS, 16,04 LTS ve 18,04 LTS
* SUSE Linux Enterprise Server 12

#### <a name="32-bit-linux-operating-systems"></a>32-bit Linux işletim sistemleri

* CentOS 6
* Oracle Linux 6
* Red Hat Enterprise Linux Server 6
* Borçlu GNU/Linux 8 ve 9
* Ubuntu Linux 14,04 LTS ve 16,04 LTS

## <a name="network-requirements"></a>Ağ gereksinimleri

Değişiklik İzleme ve envanteri özellikle bir sonraki tabloda listelenen ağ adreslerini gerektirir. Bu adreslere yönelik iletişimler 443 numaralı bağlantı noktasını kullanır.

|Azure Genel  |Azure Kamu  |
|---------|---------|
|*.ods.opinsights.azure.com     |*. ods.opinsights.azure.us         |
|*.oms.opinsights.azure.com     | *. oms.opinsights.azure.us        |
|*.blob.core.windows.net|*. blob.core.usgovcloudapi.net|
|*.azure-automation.net|*. azure-automation.us|

## <a name="change-tracking-and-inventory-user-interface"></a>Değişiklik İzleme ve envanter Kullanıcı arabirimi

İzlenen bilgisayarların değişikliklerinin özetini görüntülemek için Azure portal Değişiklik İzleme ve envanterini kullanın. Bu özellik, Otomasyon hesabınızda **yapılandırma yönetimi** altında **değişiklik izleme** seçilerek kullanılabilir. 

![Değişiklik İzleme panosu](./media/change-tracking/change-tracking-dash01.png)

Değişiklik izleme grafiğini ve değişiklik türü ve zaman aralıklarına göre ayrıntılı bilgileri sınırlamak için panonun üst kısmında açılır listeler bulunur. Ayrıca, özel bir zaman aralığı seçmek için grafiğe tıklayıp sürükleyebilirsiniz. 

Ayrıntılarını görüntülemek için bir değişikliğe veya olaya tıklayabilirsiniz. Kullanılabilir değişiklik türleri şunlardır:

* Olaylar
* Daemon 'ları
* Dosyalar
* Kayıt Defteri
* Yazılım
* Microsoft Hizmetleri

Her değişikliği ekleyebilir, değiştirebilir veya kaldırabilirsiniz. Aşağıdaki örnekte, bir hizmetin başlangıç türünde bir değişikliği el Ile ve otomatik olarak görebilirsiniz.

![Değişiklik İzleme ayrıntıları](./media/change-tracking/change-tracking-details.png)

## <a name="tracking-of-file-changes"></a>Dosya değişikliklerinin izlenmesi

Hem Windows hem de Linux 'ta dosyalardaki değişiklikleri izlemek için, Değişiklik İzleme ve stok dosyaların MD5 karma değerlerini kullanır. Bu özellik, son stoktan bu yana değişikliklerin yapılmış olup olmadığını algılamak için karmaları kullanır.

## <a name="tracking-of-file-content-changes"></a>Dosya içeriği değişikliklerinin izlenmesi

Değişiklik İzleme ve envanter, bir Windows veya Linux dosyasının içeriğini bir dosya değişikliğinden önce ve sonra görüntülemenize olanak sağlar. Bir dosyada yapılan her değişiklik için Değişiklik İzleme ve stok, dosyanın içeriğini bir [Azure depolama hesabına](../storage/common/storage-create-storage-account.md)depolar. Dosyayı izlerken, bir değişiklikten önce veya sonra içeriğini görüntüleyebilirsiniz. İçeriği satır içi veya yan yana görüntüleyebilirsiniz. 

![Dosyadaki değişiklikleri görüntüleme](./media/change-tracking/view-file-changes.png)

## <a name="tracking-of-registry-keys"></a>Kayıt defteri anahtarlarının izlenmesi

Değişiklik İzleme ve envanter, kayıt defteri anahtarlarındaki değişikliklerin izlenmesini sağlar. İzleme, üçüncü taraf kod ve kötü amaçlı yazılımların etkinleştirebileceği genişletilebilirlik noktalarını belirlemenize olanak sağlar. Aşağıdaki tabloda önceden yapılandırılmış (ancak etkinleştirilmemiş) kayıt defteri anahtarları listelenmektedir. Bu anahtarları izlemek için her birini etkinleştirmeniz gerekir.

> [!div class="mx-tdBreakAll"]
> |Kayıt Defteri Anahtarı | Amaç |
> | --- | --- |
> |`HKEY\LOCAL\MACHINE\Software\Classes\Directory\ShellEx\ContextMenuHandlers` | Doğrudan Windows Gezgini 'nde bulunan ve genellikle **Explorer. exe**ile işlem içinde çalıştırılan ortak autostart girdilerini izler.
> |`HKEY\LOCAL\MACHINE\Software\Microsoft\Windows\CurrentVersion\Group Policy\Scripts\Startup` | Başlangıçta çalıştırılan betikleri izler.
> |`HKEY\LOCAL\MACHINE\Software\Microsoft\Windows\CurrentVersion\Group Policy\Scripts\Shutdown` | Kapatılırken çalıştırılan betikleri izler.
> |`HKEY\LOCAL\MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Run` | Kullanıcı Windows hesabında oturum açmadan önce yüklenen anahtarları izler. Anahtar, 64 bit bilgisayarlarda çalışan 32 bitlik uygulamalar için kullanılır.
> |`HKEY\LOCAL\MACHINE\SOFTWARE\Microsoft\Active Setup\Installed Components` | Uygulama ayarlarındaki değişiklikleri izler.
> |`HKEY\LOCAL\MACHINE\Software\Classes\Directory\ShellEx\ContextMenuHandlers` | Doğrudan Windows Gezgini 'nde bulunan ve genellikle **Explorer. exe**ile işlem içinde çalıştırılan ortak autostart girdilerini izler.
> |`HKEY\LOCAL\MACHINE\Software\Classes\Directory\Shellex\CopyHookHandlers` | Doğrudan Windows Gezgini 'nde bulunan ve genellikle **Explorer. exe**ile işlem içinde çalıştırılan ortak autostart girdilerini izler.
> |`HKEY\LOCAL\MACHINE\Software\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers` | Simge kaplama işleyicisi kaydını izler.
> |`HKEY\LOCAL\MACHINE\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers` | 64 bit bilgisayarlarda çalışan 32 bitlik uygulamalar için simge kaplama işleyici kaydını izler.
> |`HKEY\LOCAL\MACHINE\Software\Microsoft\Windows\CurrentVersion\Explorer\Browser Helper Objects` | Internet Explorer için yeni tarayıcı yardımcısı nesne eklentilerini izler. Geçerli sayfanın Belge Nesne Modeli (DOM) erişmek ve gezintiyi denetlemek için kullanılır.
> |`HKEY\LOCAL\MACHINE\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Explorer\Browser Helper Objects` | Internet Explorer için yeni tarayıcı yardımcısı nesne eklentilerini izler. Geçerli sayfanın Belge Nesne Modeli (DOM) öğesine erişmek ve 64 bit bilgisayarlarda çalışan 32 bitlik uygulamalar için gezintiyi denetlemek için kullanılır.
> |`HKEY\LOCAL\MACHINE\Software\Microsoft\Internet Explorer\Extensions` | Özel araç menüleri ve özel araç çubuğu düğmeleri gibi yeni Internet Explorer uzantılarını izler.
> |`HKEY\LOCAL\MACHINE\Software\Wow6432Node\Microsoft\Internet Explorer\Extensions` | 64 bit bilgisayarlarda çalışan 32 bitlik uygulamalar için özel araç menüleri ve özel araç çubuğu düğmeleri gibi yeni Internet Explorer uzantılarına yönelik izleyiciler.
> |`HKEY\LOCAL\MACHINE\Software\Microsoft\Windows NT\CurrentVersion\Drivers32` | WaveMapper, wave1 ve Wave2, msacm. ımaadpcm,. msadpcm,. msgsm610 ve vidc ile ilişkili 32 bitlik sürücüleri izler. **System. ini** dosyasındaki [Drivers] bölümüne benzer.
> |`HKEY\LOCAL\MACHINE\Software\Wow6432Node\Microsoft\Windows NT\CurrentVersion\Drivers32` | , 64 bit bilgisayarlarda çalışan 32 bit uygulamalar için WaveMapper, wave1 ve Wave2, msacm. ımaadpcm,. msadpcm,. msgsm610 ve vidc ile ilişkili 32 bitlik sürücüleri izler. **System. ini** dosyasındaki [Drivers] bölümüne benzer.
> |`HKEY\LOCAL\MACHINE\System\CurrentControlSet\Control\Session Manager\KnownDlls` | Bilinen veya yaygın olarak kullanılan sistem dll 'lerinin listesini izler. Bu sistem, sistem dll 'lerinin Truva atı sürümlerini bırakarak kullanıcıların zayıf uygulama dizini izinleriyle yararlanmasını engeller.
> |`HKEY\LOCAL\MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon\Notify` | Windows için etkileşimli oturum açma desteği modeli olan **Winlogon. exe**' den olay bildirimleri alabilen paketlerin listesini izler.

## <a name="support-for-file-integrity-monitoring-in-azure-security-center"></a>Azure Güvenlik Merkezi 'nde dosya bütünlüğü Izleme desteği

Değişiklik İzleme ve envanter, [Azure Güvenlik Merkezi dosya bütünlüğü izleme (FIM)](https://docs.microsoft.com/azure/security-center/security-center-file-integrity-monitoring)kullanımını sağlar. FIM yalnızca dosyaları ve kayıt defterlerini izlerken, tam Değişiklik İzleme ve envanter özelliği için izleme de içerir:

- Yazılım değişiklikleri
- Microsoft Hizmetleri
- Linux Daemon 'ları

## <a name="recursion-support"></a>Özyineleme desteği

Değişiklik İzleme ve envanter özyineleme destekler, bu da dizinler genelinde izlemeyi basitleştirmek için joker karakterler belirtmenize olanak tanır. Özyineleme Ayrıca, birden çok veya dinamik sürücü adına sahip ortamlarda dosyaları izlemenize olanak tanımak için ortam değişkenleri sağlar. Aşağıdaki listede, özyineleme yapılandırılırken bilmeniz gereken yaygın bilgiler yer almaktadır:

* Birden çok dosyayı izlemek için joker karakterler gereklidir.
* Joker karakterler yalnızca bir yolun son kesiminde kullanılabilir; Örneğin, **\\c:\klasör dosyası*** veya **/etc/*. conf**.
* Bir ortam değişkeni geçersiz bir yol içeriyorsa, doğrulama başarılı olur ancak yürütme sırasında yol başarısız olur.
* Yolu ayarlarken genel yol adlarından kaçının, bu tür bir ayar, çok fazla klasör alınmasına neden olabilir.

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

Değişiklik İzleme ve envanter kullanan bir makineye yönelik ortalama Log Analytics veri kullanımı ayda yaklaşık 40 MB 'dir. Bu değer yalnızca bir yaklaşık değerdir ve ortamınıza bağlı olarak değişebilir. Sahip olduğunuz tam kullanımı görmek için ortamınızı izlemeniz önerilir.

### <a name="microsoft-service-data"></a>Microsoft hizmet verileri

Microsoft Hizmetleri için varsayılan toplama sıklığı 30 dakikadır. **Ayarları Düzenle**' nin altındaki **Microsoft Hizmetleri** sekmesinde bir kaydırıcıyı kullanarak sıklığı yapılandırabilirsiniz. 

![Microsoft Hizmetleri kaydırıcısı](./media/change-tracking/windowservices.png)

Log Analytics Aracısı, performansı iyileştirmek için yalnızca değişiklikleri izler. Hizmetin özgün durumuna geri dönmesi durumunda yüksek eşikte değişiklikler kaçırılması gerekebilir. Sıklığı daha küçük bir değere ayarlamak, aksi takdirde kaçırılmış olabilecek değişiklikleri yakalamadığınızı sağlar.

> [!NOTE]
> Aracı değişiklikleri 10 saniyelik bir aralığa göre izlebilirken, verilerin Azure portal görüntülenmesi birkaç dakika sürer. Portalda görüntülenmesi sırasında oluşan değişiklikler hala izlenir ve günlüğe kaydedilir.

## <a name="support-for-alerts-on-configuration-state"></a>Yapılandırma durumunda uyarılar için destek

Değişiklik İzleme ve envanterin önemli bir özelliği, karma ortamınızın yapılandırma durumundaki değişiklikler hakkında uyarı verebilir. Birçok yararlı eylem, uyarılara yanıt olarak tetiklenebilir. Örneğin, Azure işlevleri, Otomasyon Runbook 'ları, Web kancaları ve benzeri eylemler. Bir makine için **C:\Windows\system32\drivers\etc\hosts** dosyasında yapılan değişikliklerle ilgili uyarı verme, değişiklik izleme ve envanter verileri için tek iyi bir uyarı uygulamasıdır. Sonraki tabloda tanımlanan sorgu senaryoları da dahil olmak üzere, uyarı için çok daha fazla senaryo vardır. 

|Sorgu  |Açıklama  |
|---------|---------|
|ConfigurationChange <br>ConfigChangeType = = "Files" ve Filesystemmpath "c\\: Windows\\system32\\Drivers\\" içerdiğinde &#124;|Sistem açısından kritik dosyalarda yapılan değişiklikleri izlemek için faydalıdır.|
|ConfigurationChange <br>Alanlardaki "FileContentChecksum" ve Filesystemmpath = = "\\c: Windows\\system32\\Drivers\\ve\\Konakları" içeren &#124;|Anahtar yapılandırma dosyalarında yapılan değişiklikleri izlemek için faydalıdır.|
|ConfigurationChange <br>ConfigChangeType = = "Microsoft Services" ve SvcName "W3SVC" ve SvcState = = "durduruldu" içerdiğinde &#124;|Sistem açısından kritik hizmetlerde yapılan değişiklikleri izlemek için faydalıdır.|
|ConfigurationChange <br>&#124; ConfigChangeType = = "Daemon 'ları" ve SvcName "SSH" ve SvcState! = "Running" içerir|Sistem açısından kritik hizmetlerde yapılan değişiklikleri izlemek için faydalıdır.|
|ConfigurationChange <br>&#124; ConfigChangeType = = "Software" ve ChangeCategory = = "Added"|Kilitli yazılım yapılandırmalarına ihtiyaç duyulan ortamlar için faydalıdır.|
|ConfigurationData <br>SoftwareName, "Izleme Aracısı" ve CurrentVersion! = "8.0.11081.0" içerdiğinde &#124;|Güncel olmayan veya uyumsuz yazılım sürümünün yüklü olduğu makineleri görmek için faydalıdır. Bu sorgu, son bildirilen yapılandırma durumunu bildirir, ancak değişiklikleri rapor etmez.|
|ConfigurationChange <br>&#124; RegistryKey = = @\\"HKEY_LOCAL_MACHINE software\\Microsoft\\Windows\\CurrentVersion\\QualityCompat"| Önemli virüsten koruma anahtarlarındaki değişiklikleri izlemek için faydalıdır.|
|ConfigurationChange <br>RegistryKey 'ın @\\"HKEY_LOCAL_MACHINE System\\CurrentControlSet\\Services\\SharedAccess\\parametreleri\\firewallpolicy" içerdiğini &#124;| Güvenlik Duvarı ayarlarında yapılan değişiklikleri izlemek için faydalıdır.|

## <a name="next-steps"></a>Sonraki adımlar

* Runbook 'larınızda Değişiklik İzleme ve envanterle çalışmak için bkz. [Manage değişiklik izleme and Inventory](change-tracking-file-contents.md).
* Değişiklik İzleme ve envanterle ilgili hataları gidermek için bkz. [değişiklik izleme ve envanter sorunlarını giderme](automation-tutorial-troubleshoot-changes.md).
* Ayrıntılı değişiklik izleme verilerini görüntülemek için [Azure izleyici günlüklerinde günlük aramalarını](../log-analytics/log-analytics-log-searches.md) kullanın.
