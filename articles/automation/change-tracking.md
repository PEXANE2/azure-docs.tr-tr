---
title: Azure Otomasyonu ile değişiklikleri izleme
description: İzlemeyi Değiştir çözümü, ortamınızda meydana gelen yazılım ve Windows Hizmeti değişikliklerini belirlemenize yardımcı olur.
services: automation
ms.subservice: change-inventory-management
ms.date: 01/28/2019
ms.topic: conceptual
ms.openlocfilehash: 83babd65fdf22ab40b0137d93a1cbe7f1fd7ff04
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76844811"
---
# <a name="track-changes-in-your-environment-with-the-change-tracking-solution"></a>Değişiklik İzleme çözümü yle ortamınızdaki değişiklikleri izleme

Bu makale, ortamınızdaki değişiklikleri kolayca tanımlamak için İzlemeyi Değiştir çözümünü kullanmanıza yardımcı olur. Çözüm, operasyonel sorunları belirlemenize yardımcı olmak için aşağıdaki yapılandırma değişikliklerini izler:

- Windows yazılımı
- Linux yazılımı (paketler)

    >[!NOTE]
    >Değişiklik İzleme yalnızca dağıtımın paket yöneticisiyle yönetilen yazılımları izler.

- Windows ve Linux dosyaları
- Windows kayıt defteri anahtarları
- Windows hizmetleri
- Linux daemons

Yüklenen yazılım, Windows hizmetleri, Windows kayıt defteri ve dosyalarında yapılan değişiklikler ve izlenen sunucularda Linux daemon'ları işlenmek üzere buluttaki Azure Monitor hizmetine gönderilir. Alınan verilere mantık uygulanır ve bulut hizmeti verileri kaydeder. İzlemeyi Değiştir panosundaki bilgileri kullanarak, sunucu altyapınızda yapılan değişiklikleri kolayca görebilirsiniz.

> [!NOTE]
> Azure Otomasyon Değişikliği İzleme sanal makinelerdeki değişiklikleri izler. Azure Kaynak Yöneticisi özellik değişikliklerini izlemek için Azure Kaynak Grafiği'nin [Değişiklik geçmişine](../governance/resource-graph/how-to/get-resource-changes.md)bakın.

## <a name="supported-windows-operating-systems"></a>Desteklenen Windows işletim sistemleri

Windows işletim sisteminin aşağıdaki sürümleri resmi olarak Windows aracısı için desteklenir:

* Windows Server 2008 R2 veya sonraki sürümü

## <a name="supported-linux-operating-systems"></a>Desteklenen Linux işletim sistemleri

Aşağıdaki Linux dağıtımları resmi olarak desteklenir. Ancak, Linux aracısı listelenmemiş diğer dağıtımlarda da çalışabilir. Aksi belirtilmedikçe, listelenen her ana sürüm için tüm küçük sürümler desteklenir.

### <a name="64-bit"></a>64 bit

* CentOS 6 ve 7
* Amazon Linux 2017.09
* Oracle Linux 6 ve 7
* Red Hat Enterprise Linux Server 6 ve 7
* Debian GNU/Linux 8 ve 9
* Ubuntu Linux 14.04 LTS, 16.04 LTS ve 18.04 LTS
* SUSE Linux Enterprise Server 12

### <a name="32-bit"></a>32 bit

* CentOS 6
* Oracle Linux 6
* Red Hat Enterprise Linux Server 6
* Debian GNU/Linux 8 ve 9
* Ubuntu Linux 14.04 LTS ve 16.04 LTS

## <a name="enable-change-tracking-and-inventory"></a><a name="onboard"></a>Değişiklik İzlemeyi ve Sayımı Etkinleştirme

Değişiklikleri izlemeye başlamak için İzleme ve Stok çözümünü değiştirme'yi etkinleştirmeniz gerekir. İzleme ve Envanteri Değiştirmek için gemideki makinelere birçok yol vardır. Aşağıdaki ler, çözüme dahil etmek için önerilen ve desteklenen yollardır.

* [Sanal bir makineden](automation-onboard-solutions-from-vm.md)
* [Birden fazla makineye göz atma](automation-onboard-solutions-from-browse.md)
* [Otomasyon hesabınızdan](automation-onboard-solutions-from-automation-account.md)
* [Azure Otomasyon runbook ile](automation-onboard-solutions.md)

## <a name="configuring-change-tracking-and-inventory"></a>Değişim İzleme ve Envanterin Yapılandırılması

Çözüm ekine bilgisayarları nasıl bindireceklerini öğrenmek için: [Onboarding Automation çözümleri.](automation-onboard-solutions-from-automation-account.md) Değişiklik İzleme ve Stok çözümüyle birlikte bir makineye bindikten sonra, öğeleri izleyecek şekilde yapılandırabilirsiniz. İzlemek için yeni bir dosya veya kayıt defteri anahtarı etkinleştirdiğinizde, hem İzleme yi değiştir hem de Stok için etkinleştirilir.

Hem Windows hem de Linux'taki dosyalardaki değişiklikleri izlemek için dosyaların MD5'i kullanılmıştır. Tezler daha sonra son stoktan beri bir değişiklik yapIlip yapılmadığını algılamak için kullanılır.

### <a name="file-integrity-monitoring-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde Dosya Bütünlüğü İzleme

Azure Güvenlik Merkezi, Azure Değişiklik İzleme'ye yerleşik Dosya Bütünlüğü İzleme (FIM) ekledi. FIM yalnızca dosyaları ve kayıt defterlerini izlerken, tam Değişiklik İzleme çözümü de şunları içerir:

- Yazılım değişiklikleri
- Windows Hizmetleri
- Linux Daemons

FIM'i zaten etkinleştirdiyseniz ve tam Değişiklik İzleme çözümünü denemek istiyorsanız, aşağıdaki adımları gerçekleştirmeniz gerekir. Ayarlar bu işlem tarafından kaldırılmaz.

> [!NOTE]
> Tam Değişiklik İzleme çözümünü etkinleştirmek, daha fazla bilgi için ek ücretlere neden [olabilir,](https://azure.microsoft.com/pricing/details/automation/)bkz.

1. Çalışma alanına gidip [yüklü izleme çözümleri listesinde](../azure-monitor/insights/solutions.md#list-installed-monitoring-solutions)bularak izleme çözümünü kaldırın.
2. Özet sayfasını açmak için çözümün adını tıklatın ve ardından [bir izleme çözümünü kaldır'da](../azure-monitor/insights/solutions.md#remove-a-monitoring-solution)ayrıntılı olarak belirtildiği gibi Sil'e tıklayın.
3. Otomasyon hesabına gidip **Configuration Management**altındaki kaynak menüsünden **Değişiklik İzleme'yi** seçerek çözümü yeniden etkinleştirin.
4. Çalışma alanı ayar ayrıntılarınızı onaylayın ve **Etkinleştir'i**tıklatın.

### <a name="configure-linux-files-to-track"></a>Linux dosyalarını izlemek için yapılandırma

Linux bilgisayarlarda dosya izlemeyi yapılandırmak için aşağıdaki adımları kullanın:

1. Otomasyon Hesabınızda CONFIGURATION **MANAGEMENT**altında **İzlemeyi Değiştir'i** seçin. **Ayarları (vites** simgesi) Düzelt'i tıklatın.
2. **İzlemeyi Değiştir** **sayfasında, Linux Dosyalarını**seçin, ardından izlemek için yeni bir dosya eklemek için **+ Ekle'yi** tıklatın.
3. Değişiklik **İzleme için Linux Dosyası**Ekle'ye, **Kaydet'i**izlemek ve tıklatmak için dosya veya dizin bilgilerini girin.

|Özellik  |Açıklama  |
|---------|---------|
|Etkin     | Ayarın uygulanıp uygulanmayacağı belirlenir.        |
|Öğe Adı     | Dosyanın dostane adı izlenecek.        |
|Grup     | Dosyaları mantıksal olarak gruplandırmak için bir grup adı.        |
|Yolu girin     | Dosyayı denetleme yolu. Örneğin: "/etc/*.conf"       |
|Yol Türü     | İzlenecek öğe türü, olası değerler Dosya ve Dizin'dir.        |
|Özyineleme     | İzlenecek öğe aranırken özyinelemenin kullanılıp kullanılmadığını belirler.        |
|Sudo Kullan     | Bu ayar, öğe denetlenirken sudonun kullanılıp kullanılmadığını belirler.         |
|Bağlantılar     | Bu ayar, dizinleri dolaşırken sembolik bağlantıların nasıl ele alındığını belirler.<br> **Yoksay** - Sembolik bağlantıları yoksa ve başvurulan dosyaları/dizinleri içermez.<br>**Takip et** - Özyineleme sırasında sembolik bağlantıları izler ve başvurulan dosyaları/dizinleri de içerir.<br>**Yönet** - Sembolik bağlantıları izler ve iade edilen içeriğin değiştirilmesine izin verir.     |
|Dosya içeriğini tüm ayarlar için karşıya yükleme| İzlenen değişikliklerin dosya içeriği karşıya yükleme işlemini açar veya kapatır. Kullanılabilir seçenekler: **True** veya **False**.|

> [!NOTE]
> “Yönet” bağlantıları seçeneği önerilmez. Dosya içeriğini alma desteklenmiyor.

### <a name="configure-windows-files-to-track"></a>Windows dosyalarını izlemek için yapılandırma

Windows bilgisayarlarda dosya izlemeyi yapılandırmak için aşağıdaki adımları kullanın:

1. Otomasyon Hesabınızda CONFIGURATION **MANAGEMENT**altında **İzlemeyi Değiştir'i** seçin. **Ayarları (vites** simgesi) Düzelt'i tıklatın.
2. **İzlemeyi Değiştir** sayfasında **Windows Dosyaları'nı**seçin ve ardından izlemek için yeni bir dosya eklemek için **+ Ekle'yi** tıklatın.
3. Windows **Dosyasını Değiştir İzleme**Ekle'de, **kaydet'i**izlemek ve tıklatmak için dosyanın bilgilerini girin.

|Özellik  |Açıklama  |
|---------|---------|
|Etkin     | Ayarın uygulanıp uygulanmayacağı belirlenir.        |
|Öğe Adı     | Dosyanın dostane adı izlenecek.        |
|Grup     | Dosyaları mantıksal olarak gruplandırmak için bir grup adı.        |
|Yolu girin     | Dosyayı denetlemek için kullanılacak yol (örneğin, "c:\temp\\\*.txt")<br>"%winDir%\System32\\\*.*" gibi ortam değişkenleri de kullanabilirsiniz       |
|Özyineleme     | İzlenecek öğe aranırken özyinelemenin kullanılıp kullanılmadığını belirler.        |
|Dosya içeriğini tüm ayarlar için karşıya yükleme| İzlenen değişikliklerin dosya içeriği karşıya yükleme işlemini açar veya kapatır. Kullanılabilir seçenekler: **True** veya **False**.|

## <a name="wildcard-recursion-and-environment-settings"></a>Joker karakter, özyineleme ve ortam ayarları

Yineleme, dizinler arasında izlemeyi basitleştirmek için joker karakterler ve birden çok veya dinamik sürücü adlarına sahip ortamlardaki dosyaları izlemenize olanak tanıyan ortam değişkenleri belirtmenize olanak tanır. Aşağıdaki liste, özyinelemeyi yapılandırırken bilmeniz gereken ortak bilgileri gösterir:

* Joker karakterler birden çok dosyayı izlemek için gereklidir
* Joker karakterler kullanıyorsanız, bunlar yalnızca bir yolun son segmentinde kullanılabilir. (gibi `c:\folder\*file*` veya `/etc/*.conf`)
* Bir ortam değişkeninin geçersiz bir yolu varsa, doğrulama başarılı olur, ancak stok çalıştığında bu yol başarısız olur.
* Yolu ayarlarken `c:\*.*` olduğu gibi genel yollardan kaçının, çünkü bu çok fazla klasörün geçmesine neden olur.

## <a name="configure-file-content-tracking"></a>Dosya İçeriği izleme yapılandırma

Dosya İçeriği Değişikliği İzleme ile dosya değişikliğinden önce ve sonra içeriği görüntüleyebilirsiniz. Bu, Windows ve Linux dosyaları için kullanılabilir, dosyadaki her değişiklik için, dosyanın içeriği bir depolama hesabında depolanır ve dosyayı değişiklikten önce ve sonra, satır satır veya yan yana gösterir. Daha fazla bilgi için izlenen [dosyanın içeriğini görüntüleyin.](change-tracking-file-contents.md)

![dosyadaki değişiklikleri görüntüleme](./media/change-tracking-file-contents/view-file-changes.png)

### <a name="configure-windows-registry-keys-to-track"></a>Windows kayıt defteri anahtarlarını izlemek için yapılandırma

Windows bilgisayarlarda kayıt defteri anahtarı izlemeyi yapılandırmak için aşağıdaki adımları kullanın:

1. Otomasyon Hesabınızda CONFIGURATION **MANAGEMENT**altında **İzlemeyi Değiştir'i** seçin. **Ayarları (vites** simgesi) Düzelt'i tıklatın.
2. **İzlemeyi Değiştir** **sayfasında, Windows Kayıt Defteri'ni**seçin , ardından izlemek için yeni bir kayıt defteri anahtarı eklemek için **+ Ekle'yi** tıklatın.
3. Windows **Kayıt Defteri Ni Ekle'de İzleme,** izlemek için anahtara ait bilgileri girin ve **Kaydet'i**tıklatın.

|Özellik  |Açıklama  |
|---------|---------|
|Etkin     | Ayarın uygulanıp uygulanmayacağı belirlenir.        |
|Öğe Adı     | İzlenecek kayıt defteri anahtarının dostu adı.        |
|Grup     | Kayıt defteri anahtarlarını mantıksal olarak gruplandırmak için bir grup adı.        |
|Windows Kayıt Defteri Anahtarı   | Kayıt defteri anahtarını denetleme yolu. Örneğin: "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\User Shell Klasörleri\Ortak Başlangıç"      |

## <a name="limitations"></a>Sınırlamalar

İzlemeyi Değiştir çözümü şu anda aşağıdaki öğeleri desteklemiyor:

* Windows kayıt defteri izleme için yineleme
* Ağ dosya sistemleri
* Farklı yükleme yöntemleri izlenmez
* *.exe dosyaları Windows için izlenmez

Diğer sınırlamalar:

* Geçerli uygulamada **Max Dosya Boyutu** sütunu ve değerleri kullanılmaz.
* 30 dakikalık toplama döngüsünde 2500'den fazla dosya toplarsanız, çözüm performansı düşürülebilir.
* Ağ trafiği yüksek olduğunda, değişiklik kayıtlarının görüntülenmesi altı saat kadar sürebilir.
* Bilgisayar kapatılırken yapılandırmayı değiştirirseniz, bilgisayar önceki yapılandırmaya ait değişiklikleri yayınlayabilir.

## <a name="known-issues"></a>Bilinen Sorunlar

Değişiklik İzleme çözümü şu anda aşağıdaki sorunları yaşıyor:

* Hotfix güncelleştirmeleri Windows Server 2016 Core RS3 makinelerinde toplanmaz.
* Linux Daemons hiçbir değişiklik olmasına rağmen değişen bir durum gösterebilir. Bunun nedeni `SvcRunLevels` alanın nasıl ele geçirildiğidir.

## <a name="change-tracking-data-collection-details"></a>Veri toplama ayrıntılarını değiştirme

Aşağıdaki tabloda değişiklik türleri için veri toplama sıklığını gösterilmektedir. Her tür için geçerli durum veri anlık da en az her 24 saatte bir yenilenir:

| **Türü değiştir** | **Frequency** |
| --- | --- |
| Windows kayıt defteri | 50 dakika |
| Windows dosyası | 30 dakika |
| Linux dosyası | 15 dakika |
| Windows hizmetleri | 10 saniye ile 30 dakika arası</br> Varsayılan: 30 dakika |
| Linux daemons | 5 dakika |
| Windows yazılımı | 30 dakika |
| Linux yazılımı | 5 dakika |

Aşağıdaki tablo, Değişiklik İzleme için makine başına izlenen madde sınırlarını gösterir.

| **Kaynak** | **Sınırı**| **Notlar** |
|---|---|---|
|Dosya|500||
|Kayıt Defteri|250||
|Windows yazılımı|250|Yazılım düzeltmeleri içermez|
|Linux paketleri|1250||
|Hizmetler|250||
|Daemon|250||

Değişim İzleme ve Envanter kullanan bir makine için ortalama Log Analytics veri kullanımı ayda yaklaşık 40MB'dır. Bu değer yalnızca bir yaklaşımdır ve ortamınıza göre değişebilir. Sahip olduğunuz tam kullanımı görmek için ortamınızı izlemeniz önerilir.

### <a name="windows-service-tracking"></a>Windows hizmet izleme

Windows hizmetleri için varsayılan toplama sıklığı 30 dakikadır. Frekansı yapılandırmak için **İzlemeyi Değiştir'e**gidin. **Windows Hizmetleri** sekmesinde **Ayarları Edit** sekmesinde, Windows hizmetlerinin toplama sıklığını 10 saniyeden 30 dakikaya kadar değiştirmenize olanak tanıyan bir kaydırıcı vardır. Kaydırıcı çubuğunu istediğiniz frekansa taşıyın ve otomatik olarak kaydeder.

![Windows hizmetleri kaydırıcısı](./media/change-tracking/windowservices.png)

Aracı yalnızca değişiklikleri izler, bu aracının performansını optimize eder. Hizmet özgün durumuna geri döndürüldüyse, yüksek bir eşik ayarlaması değişiklikleri kaçırabilir. Sıklığı daha küçük bir değere ayarlamak, aksi takdirde kaçırılabilecek değişiklikleri yakalamanızı sağlar.

> [!NOTE]
> Aracı değişiklikleri 10 saniyelik bir arayla izleyebilse de, verilerin portalda görüntülenmesi birkaç dakika sürer. Portalda görüntülenme süresindeki değişiklikler hala izlenir ve günlüğe kaydedilir.

### <a name="registry-key-change-tracking"></a>Kayıt defteri anahtar değişikliği izleme

Kayıt defteri anahtarlarını izlemenin amacı, üçüncü taraf kodu ve kötü amaçlı yazılımların etkinleştirebileceği genişletilebilirlik noktalarını saptamaktır. Aşağıdaki liste, önceden yapılandırılmış kayıt defteri anahtarlarının listesini gösterir. Bu anahtarlar yapılandırıldı ancak etkinleştirildi. Bu kayıt defteri anahtarlarını izlemek için her birini etkinleştirmeniz gerekir.

> [!div class="mx-tdBreakAll"]
> |Kayıt Defteri Anahtarı | Amaç |
> |---------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> |`HKEY\LOCAL\MACHINE\Software\Classes\Directory\ShellEx\ContextMenuHandlers` | Doğrudan Windows Gezgini'ne bağlanan ve genellikle Explorer.exe ile işlem içinde çalışan ortak otomatik başlatma girişlerini izler.
> |`HKEY\LOCAL\MACHINE\Software\Microsoft\Windows\CurrentVersion\Group Policy\Scripts\Startup` | Başlangıçta çalışan komut dosyalarını izler.
> |`HKEY\LOCAL\MACHINE\Software\Microsoft\Windows\CurrentVersion\Group Policy\Scripts\Shutdown` | Kapatma sırasında çalışan komut dosyalarını izler.
> |`HKEY\LOCAL\MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Run` | Kullanıcı Windows hesabına girmeden önce yüklenen anahtarları izler. Anahtar, 64 bit bilgisayarlarda çalışan 32 bit programlar için kullanılır.
> |`HKEY\LOCAL\MACHINE\SOFTWARE\Microsoft\Active Setup\Installed Components` | Uygulama ayarlarındaki değişiklikleri izler.
> |`HKEY\LOCAL\MACHINE\Software\Classes\Directory\ShellEx\ContextMenuHandlers` | Doğrudan Windows Gezgini'ne bağlanan ve genellikle Explorer.exe ile işlem içinde çalışan ortak otomatik başlatma girişlerini izler.
> |`HKEY\LOCAL\MACHINE\Software\Classes\Directory\Shellex\CopyHookHandlers` | Doğrudan Windows Gezgini'ne bağlanan ve genellikle Explorer.exe ile işlem içinde çalışan ortak otomatik başlatma girişlerini izler.
> |`HKEY\LOCAL\MACHINE\Software\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers` | Simge yer kaplaması işleyicikaydı için monitörler.
> |`HKEY\LOCAL\MACHINE\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers` | 64 bit bilgisayarlarda çalışan 32 bit programlar için simge yer kaplaması işleyici kaydını izler.
> |`HKEY\LOCAL\MACHINE\Software\Microsoft\Windows\CurrentVersion\Explorer\Browser Helper Objects` | Internet Explorer için yeni tarayıcı yardımcı nesne eklentileri için monitörler. Geçerli sayfanın Belge Nesnesi Modeline (DOM) erişmek ve gezinmeyi denetlemek için kullanılır.
> |`HKEY\LOCAL\MACHINE\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Explorer\Browser Helper Objects` | Internet Explorer için yeni tarayıcı yardımcı nesne eklentileri için monitörler. Geçerli sayfanın Belge Nesnesi Modeline (DOM) erişmek ve 64 bit bilgisayarlarda çalışan 32 bit programların gezintisini denetlemek için kullanılır.
> |`HKEY\LOCAL\MACHINE\Software\Microsoft\Internet Explorer\Extensions` | Özel araç menüleri ve özel araç çubuğu düğmeleri gibi yeni Internet Explorer uzantıları için monitörler.
> |`HKEY\LOCAL\MACHINE\Software\Wow6432Node\Microsoft\Internet Explorer\Extensions` | 64 bit bilgisayarlarda çalışan 32 bit programlar için özel araç menüsü ve özel araç çubuğu düğmeleri gibi yeni Internet Explorer uzantıları için monitörler.
> |`HKEY\LOCAL\MACHINE\Software\Microsoft\Windows NT\CurrentVersion\Drivers32` | Wavemapper, wave1 ve wave2, msacm.imaadpcm, .msadpcm, .msgsm610 ve vidc ile ilişkili 32-bit sürücüleri izler. SİsteM'deki [sürücüler] bölümüne benzer. INI dosyası.
> |`HKEY\LOCAL\MACHINE\Software\Wow6432Node\Microsoft\Windows NT\CurrentVersion\Drivers32` | Wavemapper, wave1 ve wave2, msacm.imaadpcm, .msadpcm, .msgsm610 ve vidc ile ilişkili 32 bit sürücüleri 64 bit bilgisayarlarda çalışan 32 bit programlar için izler. SİsteM'deki [sürücüler] bölümüne benzer. INI dosyası.
> |`HKEY\LOCAL\MACHINE\System\CurrentControlSet\Control\Session Manager\KnownDlls` | Bilinen veya yaygın olarak kullanılan sistem DL'lerinin listesini izler; bu sistem, sistemin Truva atı sürümlerinde bırakarak insanların zayıf uygulama dizini izinlerinden yararlanmalarını önler.
> |`HKEY\LOCAL\MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon\Notify` | Windows işletim sistemi için etkileşimli oturum açma destek modeli Winlogon'dan etkinlik bildirimleri alabilen paketlerin listesini izler.

## <a name="network-requirements"></a>Ağ gereksinimleri

Aşağıdaki adresler özellikle Değişiklik İzleme için gereklidir. Bu adreslere iletişim 443 portu üzerinden yapılır.

|Azure Genel  |Azure Kamu  |
|---------|---------|
|*.ods.opinsights.azure.com     |*.ods.opinsights.azure.us         |
|*.oms.opinsights.azure.com     | *.oms.opinsights.azure.us        |
|*.blob.core.windows.net|*.blob.core.usgovcloudapi.net|
|*.azure-automation.net|*.azure-automation.us|

## <a name="use-change-tracking"></a>Değişiklik İzleme'yi Kullanma

Çözüm etkinleştirildikten sonra, Otomasyon hesabınızda **CONFIGURATION MANAGEMENT** altında **Değişiklik İzleme'yi** seçerek izlenen bilgisayarlarınızın değişikliklerinin özetini görüntüleyebilirsiniz.

Bilgisayarlarınızdaki değişiklikleri görüntüleyebilir ve ardından her olay için ayrıntıları inceleyebilirsiniz. Grafik ve değişiklik türü ve zaman aralıkları dayalı ayrıntılı bilgileri sınırlamak için grafiğin üst kısmında açılır düşüşler mevcuttur. Ayrıca, özel bir zaman aralığı seçmek için grafiğe tıklayıp sürükleyebilirsiniz. **Değişiklik Türü** aşağıdaki değerlerden biri olacak **Olaylar**, **Daemons**, **Dosyalar**, **Kayıt Defteri**, **Yazılım**, **Windows Hizmetleri**. Kategori değişikliğin türünü gösterir ve **eklenebilir**, **Değiştirilebilir**veya **Kaldırılabilir.**

![Değişiklik İzleme panosunun görüntüsü](./media/change-tracking/change-tracking-dash01.png)

Bir değişikliği veya olayı tıklattığınızda, bu değişiklikle ilgili ayrıntılı bilgiler ortaya çıkar. Örnekten de görebileceğiniz gibi, hizmetin başlangıç türü El Ile'den Otomatik'e değiştirildi.

![değişim izleme ayrıntıları görüntüsü](./media/change-tracking/change-tracking-details.png)

## <a name="search-logs"></a>Arama günlükleri

Portalda verilen ayrıntılara ek olarak, günlüklere karşı arama yapılabilir. İzleme yi **değiştir** sayfası açıkken, **Günlük Analizi'ni**tıklatın, bu **günlükler** sayfasını açar.

### <a name="sample-queries"></a>Örnek sorgular

Aşağıdaki tablo, bu çözüm tarafından toplanan değişiklik kayıtları için örnek günlük aramaları sağlar:

|Sorgu  |Açıklama  |
|---------|---------|
|YapılandırmaVerileri<br>&#124; nerede ConfigDataType == "WindowsServices" ve SvcStartupType == "Otomatik"<br>svcState == "Durduruldu" nerede &#124;<br>&#124; SoftwareName, Computer tarafından arg_max(TimeGenerated, *) özetlemek         | Otomatik olarak ayarlanmış ancak Durduruldu olarak bildirilen Windows Hizmetleri için en son stok kayıtlarını gösterir<br>Sonuçlar, softwarename ve bilgisayar için en son kayıt ile sınırlıdır      |
|Yapılandırma Değişikliği<br>&#124; nerede ConfigChangeType == "Yazılım" ve ChangeCategory == "Kaldırıldı"<br>TimeGenerated desc tarafından &#124; sipariş|Kaldırılan yazılımın değişiklik kayıtlarını gösterir|

## <a name="alert-on-changes"></a>Değişiklikler hakkında uyarı

Değişiklik İzleme ve Envanter'in önemli bir özelliği, yapılandırma durumu ve karma ortamınızın yapılandırma durumundaki değişiklikleri uyarabilme yeteneğidir.

Aşağıdaki örnekte, ekran görüntüsü dosyanın `C:\windows\system32\drivers\etc\hosts` bir makinede değiştirildiğini gösterir. Hosts dosyası, ana bilgisayar adlarını IP adreslerine çözmek için Windows tarafından kullanıldığından ve bağlantı sorunlarıyla veya trafiğin kötü amaçlı veya başka bir şekilde tehlikeli web sitelerine yönlendirilmesine neden olabilecek DNS'den bile önce geldiği için bu dosya önemlidir.

![Ana bilgisayar dosya değişikliğini gösteren bir grafik](./media/change-tracking/changes.png)

Bu değişikliği daha fazla analiz etmek **için, Log Analytics'i**tıklatarak Günlük aramasına gidin. Günlük aramasına giriş yaptıktan sonra, sorguyla `ConfigurationChange | where FieldsChanged contains "FileContentChecksum" and FileSystemPath contains "hosts"`birlikte Hosts dosyasında içerik değişikliklerini arayın. Bu sorgu, tam nitelikli yolu "ana bilgisayarlar" sözcüğü içeren dosyalar için dosya içeriği değişikliğini içeren değişiklikleri arar. Ayrıca, yol bölümünü tam nitelikli formuna (örneğin) `FileSystemPath == "c:\windows\system32\drivers\etc\hosts"`değiştirerek belirli bir dosya isteyebilirsiniz.

Sorgu istenen sonuçları döndürdükten sonra, uyarı oluşturma sayfasını açmak için Günlük arama deneyimindeki **Yeni uyarı kuralı** düğmesini tıklatın. Azure portalındaki **Azure Monitör'den** de bu deneyime göz atabilirsiniz. Uyarı oluşturma deneyiminde, sorgumuzu yeniden kontrol edin ve uyarı mantığını değiştirin. Bu durumda, çevredeki tüm makinelerde bir değişiklik bile algılanırsa uyarının tetiklenmesini istersiniz.

![Ana bilgisayar dosyasındaki değişiklikleri izlemek için değişiklik sorgusunu gösteren bir resim](./media/change-tracking/change-query.png)

Koşul mantığı ayarlandıktan sonra, tetiklenen uyarıya yanıt olarak eylemleri gerçekleştirmek için eylem grupları atayın. Bu durumda, gönderilecek e-postalar ve oluşturulacak bir ITSM bileti ayarladım.  Azure İşlevi, Otomasyon runbook, webhook veya Logic App'i tetikleme gibi diğer birçok yararlı eylem de alınabilir.

![Değişiklik konusunda uyarmak için bir eylem grubunu yapılandıran bir görüntü](./media/change-tracking/action-groups.png)

Tüm parametreler ve mantık ayarlandıktan sonra, uyarıyı ortama uygulayabiliriz.

### <a name="alert-suggestions"></a>Uyarı önerileri

Hosts dosyasında yapılan değişiklikler konusunda uyarı, Değişiklik İzleme veya Stok verileri için uyarıların iyi bir uygulaması olsa da, aşağıdaki bölümde örnek sorgularıyla birlikte tanımlanan durumlar da dahil olmak üzere, uyarı için daha birçok senaryo vardır.

|Sorgu  |Açıklama  |
|---------|---------|
|Yapılandırma Değişikliği <br>configChangeType == "Dosyalar" ve FileSystemPath "\\\\c:\\windows\\system32 drivers " içeren &#124;|Sistem kritik dosyalarındaki değişiklikleri izlemek için kullanışlıdır|
|Yapılandırma Değişikliği <br>FieldsChanged'in "FileContentChecksum" ve FileSystemPath ==\\\\"c:\\\\windows\\system32 drivers vb. ana bilgisayarları" içerdiği &#124;|Önemli yapılandırma dosyalarında yapılan değişiklikleri izlemek için kullanışlıdır|
|Yapılandırma Değişikliği <br>configChangeType == "WindowsServices" ve SvcName'nin "w3svc" ve SvcState == "Durduruldu" içerdiği &#124;|Sistem kritik hizmetlerinde yapılan değişiklikleri izlemek için kullanışlıdır|
|Yapılandırma Değişikliği <br>&#124; Nerede ConfigChangeType == "Daemons" ve SvcName "ssh" ve SvcState içerir != "Çalışan"|Sistem kritik hizmetlerinde yapılan değişiklikleri izlemek için kullanışlıdır|
|Yapılandırma Değişikliği <br>&#124; nerede ConfigChangeType == "Yazılım" ve ChangeCategory == "Eklendi"|Yazılım yapılandırmalarının kilitlenmigereken ortamlar için kullanışlıdır|
|YapılandırmaVerileri <br>SoftwareName'nin "İzleme Aracısı" ve CurrentVersion != "8.0.11081.0" içerdiği &#124;|Hangi makinelerin eski veya uyumlu olmayan bir yazılım sürümü yüklü olduğunu görmek için yararlıdır. Son bildirilen yapılandırma durumunu bildirir, değişiklikleri değil.|
|Yapılandırma Değişikliği <br>&#124; nerede RegistryKey ==\\\\@"HKEY_LOCAL_MACHINE YAZILIM Microsoft\\Windows\\CurrentVersion\\QualityCompat"| Önemli anti-virüs tuşları değişiklikleri izlemek için yararlıdır|
|Yapılandırma Değişikliği <br>registryKey @"HKEY_LOCAL_MACHINE\\\\System CurrentControlSet\\\\Services\\SharedAccess Parameters\\FirewallPolicy" içerdiği &#124;| Güvenlik duvarı ayarlarındaki değişiklikleri izlemek için kullanışlıdır|

## <a name="next-steps"></a>Sonraki adımlar

Çözümü kullanma hakkında daha fazla bilgi edinmek için İzlemeyi Değiştir'deki öğreticiyi ziyaret edin:

> [!div class="nextstepaction"]
> [Ortamınızdaki değişikliklerle ilgili sorunları giderme](automation-tutorial-troubleshoot-changes.md)

* Ayrıntılı değişiklik izleme verilerini görüntülemek için [Azure Monitor günlüklerinde Günlük aramalarını](../log-analytics/log-analytics-log-searches.md) kullanın.
