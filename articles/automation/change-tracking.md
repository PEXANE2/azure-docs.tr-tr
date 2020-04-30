---
title: Azure Otomasyonu ile değişiklikleri izleme
description: Değişiklik İzleme çözümü, ortamınızda gerçekleşen yazılım ve Windows hizmeti değişikliklerini belirlemenize yardımcı olur.
services: automation
ms.subservice: change-inventory-management
ms.date: 01/28/2019
ms.topic: conceptual
ms.openlocfilehash: d84566c7680081561f60d4825f25a9ce19e02b24
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81682984"
---
# <a name="track-environment-changes-with-change-tracking"></a>Değişiklik İzleme ile ortam değişikliklerini izleyin

Bu makale, ortamınızdaki değişiklikleri kolayca belirlemek için Değişiklik İzleme çözümünü kullanmanıza yardımcı olur. Çözüm, işlemsel sorunları belirlemenize yardımcı olması için aşağıdaki yapılandırma değişikliklerini izler:

- Windows yazılımı
- Linux yazılımı (paket)
    >[!NOTE]
    >Değişiklik İzleme yalnızca dağıtım paketi yöneticisiyle yönetilen yazılımları izler.

- Windows ve Linux dosyaları
- Windows kayıt defteri anahtarları
- Windows hizmetleri
- Linux Daemon 'ları

Çözüm etkinleştirildikten sonra, Otomasyon hesabınızda **yapılandırma yönetimi** altında **değişiklik izleme** ' yi seçerek izlenen bilgisayarlarınızın değişikliklerinin özetini görüntüleyebilirsiniz.

> [!NOTE]
> Azure Otomasyonu Değişiklik İzleme sanal makinelerdeki değişiklikleri izler. Azure Resource Manager özellik değişikliklerini izlemek için bkz. Azure Kaynak grafiğinin [değişiklik geçmişi](../governance/resource-graph/how-to/get-resource-changes.md).

Bilgisayarlarınızdaki değişiklikleri görüntüleyebilir ve sonra her bir olayla ilgili ayrıntılara gidebilirsiniz. Grafiği ve değişiklik türü ve zaman aralıklarına göre ayrıntılı bilgileri sınırlamak için grafiğin en üstünde yer alan bırakma listeleri kullanılabilir. Ayrıca, özel bir zaman aralığı seçmek için grafiğe tıklayıp sürükleyebilirsiniz. **Değişiklik türü** , aşağıdaki değerlerden biri olan **Olaylar**, **Daemon 'ları**, **dosyalar**, **kayıt defteri**, **yazılım**, **Windows Hizmetleri**olacaktır. Kategori, size değişiklik türünü gösterir ve **eklenebilir**, **değiştirilebilir**veya **Kaldırılabilir**.

![Değişiklik İzleme panosunun görüntüsü](./media/change-tracking/change-tracking-dash01.png)

Bir değişikliği veya olayı tıklatmak, bu değişiklik hakkındaki ayrıntılı bilgileri getirir. Örnekte görebileceğiniz gibi, hizmetin başlangıç türü el Ile otomatik olarak değiştirilmiştir.

![değişiklik izleme ayrıntılarının görüntüsü](./media/change-tracking/change-tracking-details.png)

İzlenen sunuculardaki yüklü yazılım, Windows Hizmetleri, Windows kayıt defteri ve dosyalar ve Linux Daemon 'ları üzerinde yapılan değişiklikler, işlenmek üzere buluttaki Azure Izleyici hizmetine gönderilir. Alınan verilere mantık uygulanır ve bulut hizmeti verileri kaydeder. Değişiklik İzleme panosundaki bilgileri kullanarak, sunucu altyapınızda yapılan değişiklikleri kolayca görebilirsiniz.

## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

### <a name="windows-operating-systems"></a>Windows işletim sistemleri

Windows işletim sisteminin aşağıdaki sürümleri resmi olarak Windows Agent için desteklenir:

* Windows Server 2008 R2 veya sonraki sürümü

### <a name="linux-operating-systems"></a>Linux işletim sistemleri

Aşağıdaki Linux dağıtımları resmi olarak desteklenmektedir. Ancak, Linux Aracısı listelenmeyen diğer dağıtımlar üzerinde de çalıştırılabilir. Aksi belirtilmediği takdirde, listelenen her ana sürüm için tüm küçük yayınlar desteklenir.

#### <a name="64-bit"></a>64 bit

* CentOS 6 ve 7
* Amazon Linux 2017,09
* Oracle Linux 6 ve 7
* Red Hat Enterprise Linux Server 6 ve 7
* Borçlu GNU/Linux 8 ve 9
* Ubuntu Linux 14,04 LTS, 16,04 LTS ve 18,04 LTS
* SUSE Linux Enterprise Server 12

#### <a name="32-bit"></a>32 bit

* CentOS 6
* Oracle Linux 6
* Red Hat Enterprise Linux Server 6
* Borçlu GNU/Linux 8 ve 9
* Ubuntu Linux 14,04 LTS ve 16,04 LTS

## <a name="limitations"></a>Sınırlamalar

Değişiklik İzleme çözümü Şu anda aşağıdaki öğeleri desteklemez:

* Windows kayıt defteri izleme için özyineleme
* Ağ dosya sistemleri
* Farklı yükleme yöntemleri
* *Windows için **. exe** dosyaları

Diğer sınırlamalar:

* **En büyük dosya boyutu** sütunu ve değerleri geçerli uygulamada kullanılmıyor.
* 30 dakikalık bir koleksiyon döngüsüne 2500 'den fazla dosya topladıysanız çözüm performansı düşebilir.
* Ağ trafiği yüksekse, değişiklik kayıtlarının görüntülenmesi altı saate kadar sürebilir.
* Bir bilgisayar kapatılırken yapılandırmayı değiştirirseniz, bilgisayar önceki yapılandırmaya ait değişiklikleri gönderebilir.

## <a name="known-issues"></a>Bilinen sorunlar

Değişiklik İzleme çözümü Şu anda aşağıdaki sorunları yaşıyor:

* Düzeltme güncelleştirmeleri Windows Server 2016 Core RS3 makinelerinde toplanmaz.
* Linux Daemon 'ları, değişiklik olmamasına rağmen değiştirilmiş bir durumu gösterebilir. Bunun nedeni `SvcRunLevels` alanın yakalanmasına neden olur.

## <a name="network-requirements"></a>Ağ gereksinimleri

Değişiklik İzleme, özellikle aşağıdaki adresleri gerektirir. Bu adreslere yönelik iletişimler 443 numaralı bağlantı noktasını kullanır.

|Azure Genel  |Azure Kamu  |
|---------|---------|
|*.ods.opinsights.azure.com     |*. ods.opinsights.azure.us         |
|*.oms.opinsights.azure.com     | *. oms.opinsights.azure.us        |
|*.blob.core.windows.net|*. blob.core.usgovcloudapi.net|
|*.azure-automation.net|*. azure-automation.us|

## <a name="wildcard-recursion-and-environment-settings"></a>Joker karakter, özyineleme ve ortam ayarları

Özyineleme, dizinler genelinde izlemeyi basitleştirecek ve ortam değişkenlerinin birden çok veya dinamik sürücü adına sahip ortamlarda dosya izlemenize imkan tanıyan joker karakterler belirtmenize olanak tanır. Aşağıdaki listede, özyineleme yapılandırılırken bilmeniz gereken yaygın bilgiler gösterilmektedir:

* Birden çok dosyayı izlemek için joker karakterler gereklidir.
* Joker karakterler yalnızca bir yolun son kesiminde kullanılır; Örneğin, c:\klasör\\dosyası * veya/etc/*. conf.
* Bir ortam değişkeni geçersiz bir yol içeriyorsa, doğrulama başarılı olur, ancak envanter çalıştırıldığında bu yol başarısız olur.
* Yolu ayarlarken genel yollardan kaçının. bu tür bir ayar, çok fazla klasör alınmasına neden olabilir.

## <a name="change-tracking-data-collection-details"></a>Veri toplama ayrıntılarını Değişiklik İzleme

Aşağıdaki tabloda değişiklik türleri için veri toplama sıklığı gösterilmektedir. Her tür için, geçerli durumun veri anlık görüntüsü en az 24 saatte bir yenilenir:

| **Türü Değiştir** | **Sıklık** |
| --- | --- |
| Windows kayıt defteri | 50 dakika |
| Windows dosyası | 30 dakika |
| Linux dosyası | 15 dakika |
| Windows hizmetleri | 10 saniye ila 30 dakika</br> Varsayılan: 30 dakika |
| Linux Daemon 'ları | 5 dakika |
| Windows yazılımı | 30 dakika |
| Linux yazılımı | 5 dakika |

Aşağıdaki tabloda, Değişiklik İzleme makine başına izlenen öğe sınırları gösterilmektedir.

| **Kaynak** | **Sınırlı**| **Notlar** |
|---|---|---|
|Dosya|500||
|Kayıt Defteri|250||
|Windows yazılımı|250|Yazılım düzeltmelerini içermez|
|Linux paketleri|1250||
|Hizmetler|250||
|İnin|250||

Değişiklik İzleme kullanarak bir makineye yönelik ortalama Log Analytics veri kullanımı ayda yaklaşık 40MB 'tır. Bu değer yalnızca bir yaklaşık değerdir ve ortamınıza bağlı olarak değişebilir. Sahip olduğunuz tam kullanımı görmek için ortamınızı izlemeniz önerilir.

### <a name="windows-service-tracking"></a>Windows hizmeti izleme

Windows Hizmetleri için varsayılan toplama sıklığı 30 dakikadır. Sıklığı yapılandırmak için **değişiklik izleme**gidin. **Windows Hizmetleri** sekmesindeki **Ayarları Düzenle** ' nin altında, Windows Hizmetleri için toplama sıklığını 30 dakika kadar olan 10 saniye kadar hızla değiştirmenize olanak tanıyan bir kaydırıcı vardır. Kaydırıcı çubuğunu istediğiniz sıklık kadar taşıyın ve otomatik olarak kaydeder.

![Windows Hizmetleri kaydırıcısı](./media/change-tracking/windowservices.png)

Aracı yalnızca değişiklikleri izler, bu, aracının performansını iyileştirir. Hizmetin özgün durumlarına geri döndürülmesi durumunda yüksek bir eşiğin ayarlanması değişiklikleri kaçırmayabilir. Sıklığı daha küçük bir değere ayarlamak, aksi takdirde eksik olabilecek değişiklikleri yakalamadığınızı sağlar.

> [!NOTE]
> Aracı değişiklikleri 10 saniyelik bir aralığa göre izlebilirken, verilerin portalda görüntülenmesi hala birkaç dakika sürer. Portalda görüntülenecek zaman içindeki değişiklikler hala izlenir ve günlüğe kaydedilir.

### <a name="registry-key-change-tracking"></a>Kayıt defteri anahtarı değişiklik izleme

Kayıt defteri anahtarlarındaki değişiklikleri izlemenin amacı, üçüncü taraf kod ve kötü amaçlı yazılımların etkinleştirebileceği genişletilebilirlik noktalarını bulmaktır. Aşağıdaki listede önceden yapılandırılmış kayıt defteri anahtarlarının listesi gösterilmektedir. Bu anahtarlar yapılandırıldı ancak etkin değil. Bu kayıt defteri anahtarlarını izlemek için her birini etkinleştirmeniz gerekir.

> [!div class="mx-tdBreakAll"]
> |Kayıt Defteri Anahtarı | Amaç |
> |---------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> |`HKEY\LOCAL\MACHINE\Software\Classes\Directory\ShellEx\ContextMenuHandlers` | Doğrudan Windows Gezgini 'nde bulunan ve genellikle Explorer. exe ile işlem içinde çalıştırılan ortak autostart girdilerini izler.
> |`HKEY\LOCAL\MACHINE\Software\Microsoft\Windows\CurrentVersion\Group Policy\Scripts\Startup` | Başlangıçta çalıştırılan betikleri izler.
> |`HKEY\LOCAL\MACHINE\Software\Microsoft\Windows\CurrentVersion\Group Policy\Scripts\Shutdown` | Kapatılırken çalıştırılan betikleri izler.
> |`HKEY\LOCAL\MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Run` | Kullanıcı Windows hesabında oturum açmadan önce yüklenen anahtarları izler. Anahtar, 64 bit bilgisayarlarda çalışan 32 bitlik programlar için kullanılır.
> |`HKEY\LOCAL\MACHINE\SOFTWARE\Microsoft\Active Setup\Installed Components` | Uygulama ayarlarındaki değişiklikleri izler.
> |`HKEY\LOCAL\MACHINE\Software\Classes\Directory\ShellEx\ContextMenuHandlers` | Doğrudan Windows Gezgini 'nde bulunan ve genellikle Explorer. exe ile işlem içinde çalıştırılan ortak autostart girdilerini izler.
> |`HKEY\LOCAL\MACHINE\Software\Classes\Directory\Shellex\CopyHookHandlers` | Doğrudan Windows Gezgini 'nde bulunan ve genellikle Explorer. exe ile işlem içinde çalıştırılan ortak autostart girdilerini izler.
> |`HKEY\LOCAL\MACHINE\Software\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers` | Simge kaplama işleyicisi kaydını izler.
> |`HKEY\LOCAL\MACHINE\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers` | 64 bit bilgisayarlarda çalışan 32 bitlik programlar için simge kaplama işleyici kaydını izler.
> |`HKEY\LOCAL\MACHINE\Software\Microsoft\Windows\CurrentVersion\Explorer\Browser Helper Objects` | Internet Explorer için yeni tarayıcı yardımcısı nesne eklentilerini izler. Geçerli sayfanın Belge Nesne Modeli (DOM) erişmek ve gezintiyi denetlemek için kullanılır.
> |`HKEY\LOCAL\MACHINE\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Explorer\Browser Helper Objects` | Internet Explorer için yeni tarayıcı yardımcısı nesne eklentilerini izler. Geçerli sayfanın Belge Nesne Modeli (DOM) ve 64 bit bilgisayarlarda çalışan 32 bitlik programların gezintisini denetlemek için kullanılır.
> |`HKEY\LOCAL\MACHINE\Software\Microsoft\Internet Explorer\Extensions` | Özel araç menüleri ve özel araç çubuğu düğmeleri gibi yeni Internet Explorer uzantılarını izler.
> |`HKEY\LOCAL\MACHINE\Software\Wow6432Node\Microsoft\Internet Explorer\Extensions` | 64 bit bilgisayarlarda çalışan 32 bitlik programlar için özel araç menüleri ve özel araç çubuğu düğmeleri gibi yeni Internet Explorer uzantılarına yönelik izleyiciler.
> |`HKEY\LOCAL\MACHINE\Software\Microsoft\Windows NT\CurrentVersion\Drivers32` | WaveMapper, wave1 ve Wave2, msacm. ımaadpcm,. msadpcm,. msgsm610 ve vidc ile ilişkili 32 bitlik sürücüleri izler. SISTEMDEKI [Drivers] bölümüne benzer. INı dosyası.
> |`HKEY\LOCAL\MACHINE\Software\Wow6432Node\Microsoft\Windows NT\CurrentVersion\Drivers32` | , 64 bit bilgisayarlarda çalışan 32 bit programlar için WaveMapper, wave1 ve Wave2, msacm. ımaadpcm,. msadpcm,. msgsm610 ve vidc ile ilişkili 32 bitlik sürücüleri izler. SISTEMDEKI [Drivers] bölümüne benzer. INı dosyası.
> |`HKEY\LOCAL\MACHINE\System\CurrentControlSet\Control\Session Manager\KnownDlls` | Bilinen veya yaygın olarak kullanılan sistem dll 'lerinin listesini izler; Bu sistem, sistem dll 'lerinin Truva atı sürümlerini bırakarak kullanıcıların zayıf uygulama dizini izinleriyle yararlanmasını engeller.
> |`HKEY\LOCAL\MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon\Notify` | Windows işletim sistemi için etkileşimli oturum açma desteği modeli olan Winlogon 'dan olay bildirimleri alabilen paketlerin listesini izler.

## <a name="enable-change-tracking"></a><a name="onboard"></a>Değişiklik İzleme etkinleştir

Değişiklikleri izlemeye başlamak için Değişiklik İzleme çözümünü etkinleştirmeniz gerekir. Değişiklik İzleme makineleri eklemek için birçok yol vardır. Çözümü eklemek için önerilen ve desteklenen yollar aşağıda verilmiştir.

* [Bir sanal makineden](automation-onboard-solutions-from-vm.md)
* [Birden çok makineye göz atmaya](automation-onboard-solutions-from-browse.md)
* [Otomasyon hesabınızdan](automation-onboard-solutions-from-automation-account.md)
* [Bir Azure Otomasyonu runbook 'u ile](automation-onboard-solutions.md)

## <a name="configure-change-tracking"></a>Değişiklik İzleme Yapılandır

Bilgisayarlara nasıl bilgisayar ekleneceğini öğrenmek için bkz. [otomasyon çözümlerini ekleme](automation-onboard-solutions-from-automation-account.md). Değişiklik İzleme çözümü ile makine ekleme yaptıktan sonra, izlenecek öğeleri yapılandırabilirsiniz. İzlemek üzere yeni bir dosya veya kayıt defteri anahtarı etkinleştirdiğinizde, her iki Değişiklik İzleme de etkinleştirilir.

Hem Windows hem de Linux dosyalarındaki dosyalardaki değişiklikleri izlemek için dosyaların MD5 karmaları kullanılır. Daha sonra, son envanterden bu yana bir değişikliğin yapıldığını algılamak için kullanılır.

## <a name="enable-file-integrity-monitoring-in-azure-security-center"></a>Azure Güvenlik Merkezi 'nde dosya bütünlüğü Izlemeyi etkinleştirme

Azure Güvenlik Merkezi, Azure Değişiklik İzleme 'da oluşturulan dosya bütünlüğü Izleme (FIM) ekledi. FIM yalnızca dosyaları ve kayıt defterlerini izlerken, tam Değişiklik İzleme çözümü de şunları içerir:

- Yazılım değişiklikleri
- Windows Hizmetleri
- Linux Daemon 'ları

FIM 'yi zaten etkinleştirdiyseniz ve tam Değişiklik İzleme çözümünü denemek istiyorsanız, aşağıdaki adımları gerçekleştirmeniz gerekir. Bu işlem, ayarlarınız kaldırılmaz.

> [!NOTE]
> Tam Değişiklik İzleme çözümünü etkinleştirmek ek ücretler oluşmasına neden olabilir. daha fazla bilgi için bkz. [Otomasyon Fiyatlandırması](https://azure.microsoft.com/pricing/details/automation/).

1. Çalışma alanına giderek ve bunları [yüklü izleme çözümleri listesinde](../azure-monitor/insights/solutions.md#list-installed-monitoring-solutions)bularak izleme çözümünü kaldırın.
2. Çözüm adına tıklayarak Özet sayfasını açın ve [izleme çözümünü kaldırma](../azure-monitor/insights/solutions.md#remove-a-monitoring-solution)bölümünde açıklandığı gibi Sil ' e tıklayın.
3. Otomasyon hesabına gidip **yapılandırma yönetimi**altındaki **değişiklik izleme** ' i seçerek çözümü yeniden etkinleştirin.
4. Çalışma alanı ayarı ayrıntılarınızı doğrulayın ve **Etkinleştir**' e tıklayın.

## <a name="configure-file-content-change-tracking"></a>Dosya içeriği değişiklik izlemeyi yapılandırma

Dosya içeriği değişiklik izleme ile dosya değişikliğinden önce ve sonra içerikleri görüntüleyebilirsiniz. Bu özellik Windows ve Linux dosyaları için kullanılabilir. Bir dosyada yapılan her değişiklik için, dosyanın içeriği bir depolama hesabında depolanır. Dosya değişiklikten önce ve sonra, satır içi veya yan yana gösterilir. Daha fazla bilgi için bkz. [izlenen bir dosyanın Içeriğini görüntüleme](change-tracking-file-contents.md).

![dosyadaki değişiklikleri görüntüleme](./media/change-tracking-file-contents/view-file-changes.png)

## <a name="configure-windows-registry-keys-to-track"></a>İzlemek için Windows kayıt defteri anahtarlarını yapılandırma

Windows bilgisayarlarda kayıt defteri anahtarı izlemeyi yapılandırmak için aşağıdaki adımları kullanın:

1. Otomasyon hesabınızda **yapılandırma yönetimi**altında **değişiklik izleme** ' yi seçin. **Ayarları Düzenle** (dişli simgesi) seçeneğine tıklayın.
2. Değişiklik İzleme sayfasında **Windows kayıt defteri**' ni seçin ve ardından **+ Ekle** ' ye tıklayarak izlenecek yeni bir kayıt defteri anahtarı ekleyin.
3. **Değişiklik izleme Için Windows kayıt defteri Ekle**' de, izlenecek anahtarın bilgilerini girin ve **Kaydet**' e tıklayın.

|Özellik  |Açıklama  |
|---------|---------|
|Etkin     | Ayarın uygulandığını belirler.        |
|Öğe Adı     | İzlenecek kayıt defteri anahtarının kolay adı.        |
|Grup     | Kayıt defteri anahtarlarını mantıksal olarak gruplandırmak için bir grup adı.        |
|Windows Kayıt Defteri Anahtarı   | Kayıt defteri anahtarını denetlemek için yol. Örneğin: "HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\User Shell Folders\Common Startup"      |

## <a name="configure-file-tracking-on-windows"></a>Windows 'da dosya izlemeyi yapılandırma

Windows bilgisayarlarda dosya izlemeyi yapılandırmak için aşağıdaki adımları kullanın:

1. Otomasyon hesabınızda **yapılandırma yönetimi**altında **değişiklik izleme** ' yi seçin. **Ayarları Düzenle** (dişli simgesi) seçeneğine tıklayın.
2. Değişiklik İzleme sayfasında, **Windows dosyaları**' nı seçin ve ardından **+ Ekle** ' ye tıklayarak izlenecek yeni bir dosya ekleyin.
3. **Değişiklik izleme Için Windows dosyası Ekle**' de, izlenecek dosyanın bilgilerini girin ve **Kaydet**' e tıklayın.

|Özellik  |Açıklama  |
|---------|---------|
|Etkin     | Ayar uygulanmışsa true, aksi takdirde false.        |
|Öğe Adı     | İzlenecek dosyanın kolay adı.        |
|Grup     | Dosyaları mantıksal olarak gruplandırmak için bir grup adı.        |
|Yolu girin     | Dosyanın denetlenecek yol (örneğin, **\\\*c:\Temp. txt** )<br>Ayrıca, gibi ortam değişkenlerini de kullanabilirsiniz `%winDir%\System32\\\*.*`.       |
|Özyineleme     | Bu öğeyi izlenecek öğe ararken, aksi takdirde false ise true.        |
|Dosya içeriğini tüm ayarlar için karşıya yükleme| İzlenen değişikliklerle dosya içeriğini karşıya yüklemek için true, aksi durumda false.|

## <a name="configure-file-tracking-on-linux"></a>Linux 'ta dosya izlemeyi yapılandırma

Linux bilgisayarlarda dosya izlemeyi yapılandırmak için aşağıdaki adımları kullanın:

1. Otomasyon hesabınızda **yapılandırma yönetimi**altında **değişiklik izleme** ' yi seçin. **Ayarları Düzenle** (dişli simgesi) seçeneğine tıklayın.
2. Değişiklik İzleme sayfasında, **Linux dosyaları**' nı seçin ve ardından **+ Ekle** ' ye tıklayarak izlenecek yeni bir dosya ekleyin.
3. **Değişiklik izleme Için Linux dosyası Ekle**' de, izlenecek dosya veya dizinin bilgilerini girin ve **Kaydet**' e tıklayın.

|Özellik  |Açıklama  |
|---------|---------|
|Etkin     | Ayarın uygulandığını belirler.        |
|Öğe Adı     | İzlenecek dosyanın kolay adı.        |
|Grup     | Dosyaları mantıksal olarak gruplandırmak için bir grup adı.        |
|Yolu girin     | Dosyanın denetlenecek yol. Örneğin: "/etc/*. conf"       |
|Yol Türü     | İzlenecek öğenin türü, olası değerler dosya ve dizindir.        |
|Özyineleme     | İzlenecek öğe aranırken özyinelemenin kullanılıp kullanılmadığını belirler.        |
|Sudo Kullan     | Bu ayar, öğe denetlenirken sudonun kullanılıp kullanılmadığını belirler.         |
|Bağlantılar     | Bu ayar, dizinleri dolaşırken sembolik bağlantıların nasıl ele alındığını belirler.<br> **Ignore** -sembolik bağlantıları yoksayar ve başvurulan dosya/dizinleri içermez.<br>**Takip et** -özyineleme sırasında sembolik bağlantıları izler ve başvurulan dosya/dizinleri de içerir.<br>**Yönet** -sembolik bağlantıları izler ve döndürülen içeriğin değiştirilmesine izin verir.     |
|Dosya içeriğini tüm ayarlar için karşıya yükleme| İzlenen değişikliklerin dosya içeriği karşıya yükleme işlemini açar veya kapatır. Kullanılabilir seçenekler: **True** veya **False**.|

> [!NOTE]
> “Yönet” bağlantıları seçeneği önerilmez. Dosya içeriğini alma desteklenmiyor.

## <a name="search-logs"></a>Günlüklerde ara

Değişiklik kayıtları için günlüklere göre çeşitli aramalar yapabilirsiniz. Değişiklik İzleme sayfası açıkken **Log Analytics**' a tıklayın. Bu, Günlükler sayfasını açar. Aşağıdaki tabloda, bu çözüm tarafından toplanan değişiklik kayıtları için örnek günlük aramaları verilmiştir:

|Sorgu  |Açıklama  |
|---------|---------|
|ConfigurationData<br>&#124; config DataType = = "WindowsServices" ve SvcStartupType = = "Auto"<br>&#124; SvcState = = "durduruldu"<br>SoftwareName, bilgisayar tarafından arg_max (TimeGenerated, *) özetleme &#124;         | Otomatik olarak ayarlanan ancak durdurulma olarak bildirilen Windows Hizmetleri için en son envanter kayıtlarını gösterir<br>Sonuçlar, bu SoftwareName ve bilgisayar için en son kayıtla sınırlıdır      |
|ConfigurationChange<br>&#124; ConfigChangeType = = "Software" ve ChangeCategory = = "removed"<br>&#124; zaman tarafından üretilen DESC 'e göre sırala|Kaldırılan yazılım için değişiklik kayıtlarını gösterir|

## <a name="alert-on-changes"></a>Değişikliklerle ilgili uyarı

Değişiklik İzleme temel bir özelliği, yapılandırma durumunda ve karma ortamınızın yapılandırma durumunda yapılan değişikliklerle ilgili olarak uyarır. Aşağıdaki örnek, bir makinede **C:\Windows\system32\drivers\etc\hosts** dosyasının değiştirildiğini gösterir. Windows bu dosyayı, ana bilgisayar adlarını IP adreslerine çözümlemek üzere kullandığından, bu dosya önemlidir. Bu işlem DNS 'den önceliklidir ve bağlantı sorunları ya da trafiğin kötü amaçlı ya da başka bir şekilde tehlikeli Web sitelerine yönlendirilmesini sağlayabilir.

![Hosts dosyası değişikliğini gösteren bir grafik](./media/change-tracking/changes.png)

Bu değişikliği daha fazla analiz etmek için **Log Analytics**tıklamasından günlük araması bölümüne gidin. Günlük araması ' nda, ana bilgisayar dosyasına sorgu `ConfigurationChange | where FieldsChanged contains "FileContentChecksum" and FileSystemPath contains "hosts"`ile içerik değişikliklerini arayın. Bu sorgu, tam yolu "konaklar" sözcüğünü içeren dosyalar için dosya içeriği değişikliğini içeren değişiklikleri arar. Ayrıca, yol bölümünü tam nitelikli biçimde (gibi `FileSystemPath == "c:\windows\system32\drivers\etc\hosts"`) değiştirerek belirli bir dosya için sorun da yapabilirsiniz.

Sorgu istenen sonuçları döndürdüğünden, uyarı oluşturma sayfasını açmak için günlük aramasında **Yeni uyarı kuralı** ' na tıklayın. Ayrıca, Azure portal **Azure izleyici** aracılığıyla bu deneyime gidebilirsiniz. 

Sorgunuzu yeniden denetleyin ve uyarı mantığını değiştirin. Bu durumda, ortamdaki tüm makinelerde tek bir değişiklik algılanırsa uyarının tetiklenmesi istersiniz.

![Hosts dosyasında değişiklikleri izlemek için değişiklik sorgusunu gösteren resim](./media/change-tracking/change-query.png)

Koşul mantığı ayarlandıktan sonra, tetiklenen uyarıya yanıt olarak eylemler gerçekleştirmek için eylem grupları atayın. Bu durumda, gönderilecek e-postaları ve oluşturulacak bir ıTSM bileti ayarladım.  Azure Işlevi, Otomasyon Runbook 'u, Web kancası veya mantıksal uygulama tetiklenmesi gibi diğer birçok yararlı eylem de alınabilir.

![Değişiklik üzerinde uyarı vermek için bir eylem grubunu yapılandıran bir görüntü](./media/change-tracking/action-groups.png)

Tüm parametreler ve Logic ayarlandıktan sonra, bu uyarıyı ortama uygulayabiliriz.

### <a name="alert-suggestions"></a>Uyarı önerileri

Hosts dosyasında yapılan değişikliklerle ilgili uyarı, Değişiklik İzleme veya envanter verileri için bir dizi iyi uygulamadır, uyarı için aşağıdaki bölümde yer alan örnek sorgularıyla birlikte tanımlanan durumlar da dahil olmak üzere çok sayıda daha fazla senaryo vardır.

|Sorgu  |Açıklama  |
|---------|---------|
|ConfigurationChange <br>ConfigChangeType = = "Files" ve Filesystemmpath "c\\: Windows\\system32\\Drivers\\" içerdiğinde &#124;|Sistem açısından kritik dosyalardaki değişiklikleri izlemek için faydalıdır|
|ConfigurationChange <br>Alanlardaki "FileContentChecksum" ve Filesystemmpath = = "\\c: Windows\\system32\\Drivers\\ve\\Konakları" içeren &#124;|Anahtar yapılandırma dosyalarında yapılan değişiklikleri izlemek için faydalıdır|
|ConfigurationChange <br>ConfigChangeType = = "WindowsServices" ve SvcName "W3SVC" ve SvcState = = "durduruldu" içerdiğinde &#124;|Sistem açısından kritik hizmetlerde yapılan değişiklikleri izlemek için faydalıdır|
|ConfigurationChange <br>&#124; ConfigChangeType = = "Daemon 'ları" ve SvcName "SSH" ve SvcState! = "Running" içerir|Sistem açısından kritik hizmetlerde yapılan değişiklikleri izlemek için faydalıdır|
|ConfigurationChange <br>&#124; ConfigChangeType = = "Software" ve ChangeCategory = = "Added"|Kilitleme yazılım yapılandırmalarına ihtiyaç duyulan ortamlar için faydalıdır|
|ConfigurationData <br>SoftwareName, "Izleme Aracısı" ve CurrentVersion! = "8.0.11081.0" içerdiğinde &#124;|Güncel olmayan veya uyumsuz bir yazılım sürümünün yüklü olduğu makineleri görmek için faydalıdır. Son bildirilen yapılandırma durumunu bildirir, değişiklikler değildir.|
|ConfigurationChange <br>&#124; RegistryKey = = @\\"HKEY_LOCAL_MACHINE software\\Microsoft\\Windows\\CurrentVersion\\QualityCompat"| Kritik Anti-Virus anahtarlarındaki değişiklikleri izlemek için faydalıdır|
|ConfigurationChange <br>RegistryKey 'ın @\\"HKEY_LOCAL_MACHINE System\\CurrentControlSet\\Services\\SharedAccess\\parametreleri\\firewallpolicy" içerdiğini &#124;| Güvenlik Duvarı ayarlarında yapılan değişiklikleri izlemek için yararlıdır|

## <a name="next-steps"></a>Sonraki adımlar

Çözümü kullanma hakkında daha fazla bilgi edinmek için Değişiklik İzleme öğreticisini ziyaret edin:

> [!div class="nextstepaction"]
> [Ortamınızdaki değişikliklerle ilgili sorunları giderme](automation-tutorial-troubleshoot-changes.md)

* Ayrıntılı değişiklik izleme verilerini görüntülemek için [Azure izleyici günlüklerinde günlük aramalarını](../log-analytics/log-analytics-log-searches.md) kullanın.
