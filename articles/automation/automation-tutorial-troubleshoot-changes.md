---
title: Bir Azure sanal makinesindeki değişikliklerle ilgili sorunları giderme | Microsoft Docs
description: Bir Azure sanal makinesi üzerindeki değişikliklerle ilgili sorunları gidermek için Değişiklik İzleme özelliğini kullanabilirsiniz.
services: automation
ms.subservice: change-inventory-management
keywords: değişiklik, izleme, otomasyon
ms.date: 12/05/2018
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: 89f5e00c75b6b85c9a14de02504136907cde62b5
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81604690"
---
# <a name="troubleshoot-changes-in-your-environment"></a>Ortamınızdaki değişikliklerle ilgili sorunları giderme

Bu öğreticide bir Azure sanal makinesi üzerindeki değişikliklerle ilgili sorunları gidermeyi öğreneceksiniz. Değişiklik İzleme etkinleştirerek, bilgisayarlarınızda yazılım, dosya, Linux Daemon 'ları, Windows Hizmetleri ve Windows kayıt defteri anahtarlarında yapılan değişiklikleri izleyebilirsiniz.
Bu yapılandırma değişikliklerinin tanımlanması, ortamınızdaki işletimsel sorunları belirlemenize yardımcı olabilir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * VM'de Değişiklik İzleme ve Stok özelliklerini etkinleştirme
> * Durdurulan hizmetlerin değişiklik günlüklerinde arama yapma
> * Değişiklik izlemeyi yapılandırma
> * Etkinlik günlüğü bağlantısını etkinleştirme
> * Bir olay tetikleme
> * Değişiklikleri görüntüleme
> * Uyarı yapılandırma

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Azure aboneliği. Henüz bir hesabınız yoksa [MSDN abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) veya [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)için kaydolabilirsiniz.
* İzleyici ve eylem runbook 'larını ve Izleyici görevini tutacak bir [Otomasyon hesabı](automation-offering-get-started.md) .
* Sisteme eklenecek bir [sanal makine](../virtual-machines/windows/quick-create-portal.md).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="enable-change-tracking-and-inventory"></a>Değişiklik İzleme ve Stok özelliklerini etkinleştirme

Bu öğretici için öncelikle sanal makinenizin Değişiklik İzleme ve envanterini etkinleştirmeniz gerekir. Daha önce bu VM için başka bir otomasyon çözümünü etkinleştirdiyseniz bu adımı atlayabilirsiniz.

1. Sol taraftaki menüden **sanal makineler** ' i seçin ve LISTEDEN bir VM seçin.
1. Sol taraftaki menüde, **işlemler**altında **Stok** ' ı seçin. Envanter sayfası açılır.

![Değişikliği etkinleştir](./media/automation-tutorial-troubleshoot-changes/enableinventory.png)

Kullanılacak konumu, Log Analytics çalışma alanını ve otomasyon hesabını yapılandırın ve **Etkinleştir**' e tıklayın. Bu alanların gri renkte olması VM için etkinleştirilmiş başka bir otomasyon çözümü olduğunu gösterir ve bu durumda aynı çalışma alanı ile Otomasyon hesabının kullanılması gerekir.

[Log Analytics](../log-analytics/log-analytics-overview.md?toc=%2fazure%2fautomation%2ftoc.json) çalışma alanı, Stok gibi özellikler ve hizmetler tarafından oluşturulan verileri toplamak için kullanılır.
Çalışma alanı, birden fazla kaynaktan alınan verilerin incelenip analiz edilebileceği ortak bir konum sağlar.

Ekleme sırasında, sanal makine, Windows için Log Analytics Aracısı ve karma Runbook Worker ile sağlanır.
Aracı, VM ile iletişim kurmak ve yüklü yazılımlar hakkında bilgi almak için kullanılır.

Çözümün etkinleştirilmesi 15 dakika sürebilir. Bu süre boyunca tarayıcı penceresini kapatmamanız gerekir.
Çözüm etkinleştirildikten sonra VM 'deki yüklü yazılım ve değişiklikler hakkında bilgiler Azure Izleyici günlüklerine akar.
Verilerin çözümlemeye hazır hale gelmesi 30 dakika ile 6 saat arasında sürebilir.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="using-change-tracking-in-azure-monitor-logs"></a>Azure Izleyici günlüklerinde Değişiklik İzleme kullanma

Değişiklik izleme, Azure Izleyici günlüklerine gönderilen günlük verilerini oluşturur.
Sorguları çalıştırarak günlüklerde arama yapmak için değişiklik izleme sayfasının en üstünde **Log Analytics** ' yi seçin.
Değişiklik izleme verileri, türü `ConfigurationChange`altında depolanır.
Aşağıdaki örnek Log Analytics sorgusu, durdurulmuş olan tüm Windows Hizmetleri'ni döndürür.

```loganalytics
ConfigurationChange
| where ConfigChangeType == "WindowsServices" and SvcState == "Stopped"
```

Azure Izleyici günlüklerinde günlük dosyalarını çalıştırma ve arama hakkında daha fazla bilgi edinmek için bkz. [Azure izleyici günlükleri](../azure-monitor/log-query/log-query-overview.md).

## <a name="configure-change-tracking"></a>Değişiklik izlemeyi yapılandırma

Değişiklik izleme, VM üzerindeki yapılandırma değişikliklerini izlemenizi sağlar. Aşağıdaki adımlar kayıt defteri anahtarlarının ve dosyaların izlenmesini yapılandırma konusunda yol göstermektedir.

Toplanıp izlenecek dosyaları ve Kayıt defteri anahtarlarını belirlemek için Değişiklik izleme sayfasının üst kısmından **Ayarları düzenle**'yi seçin.

> [!NOTE]
> Stok ve Değişiklik izleme özellikleri aynı koleksiyon ayarlarını kullanır ve bu ayarlar çalışma alanı düzeyinde yapılandırılır.

Çalışma alanı yapılandırması sayfasında, izlenecek Windows kayıt defteri anahtarlarını, Windows dosyalarını veya Linux dosyalarını, sonraki üç bölümde gösterildiği gibi ekleyin.

### <a name="add-a-windows-registry-key"></a>Windows Kayıt Defteri anahtarı ekleme

1. **Windows Kayıt Defteri** sekmesinde **Ekle**'yi seçin. 

1. Değişiklik İzleme için Windows kayıt defteri Ekle sayfasında, izlenecek anahtarın bilgilerini girin ve **Kaydet** ' e tıklayın.

|Özellik  |Açıklama  |
|---------|---------|
|Etkin     | Ayarın uygulanmış olup olmadığını belirler        |
|Öğe Adı     | İzlenecek dosyanın kolay adı        |
|Grup     | Dosyaları mantıksal bir biçimde gruplandırmaya yönelik grup adı        |
|Windows Kayıt Defteri Anahtarı   | Dosyanın denetleneceği yol. Örneğin: "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\User Shell Folders\Common Startup"      |

### <a name="add-a-windows-file"></a>Windows dosyası ekleme

1. **Windows Dosyaları** sekmesinde **Ekle**'yi seçin. 

1. Değişiklik İzleme için Windows dosyası Ekle sayfasında, izlenecek dosya veya dizin bilgilerini girin ve **Kaydet** ' e tıklayın.

|Özellik  |Açıklama  |
|---------|---------|
|Etkin     | Ayarın uygulanmış olup olmadığını belirler        |
|Öğe Adı     | İzlenecek dosyanın kolay adı        |
|Grup     | Dosyaları mantıksal bir biçimde gruplandırmaya yönelik grup adı        |
|Yolu girin     | Dosyayı denetlemek için kullanılacak yol (örneğin, "c:\temp\\\*.txt")<br>"%winDir%\System32\\\*.*" gibi ortam değişkenleri de kullanabilirsiniz         |
|Özyineleme     | İzlenecek öğe aranırken özyinelemenin kullanılıp kullanılmadığını belirler.        |
|Dosya içeriğini tüm ayarlar için karşıya yükleme| İzlenen değişikliklerin dosya içeriği karşıya yükleme işlemini açar veya kapatır. Kullanılabilir seçenekler: **True** veya **False**.|

### <a name="add-a-linux-file"></a>Linux dosyası ekleme

1. **Linux Dosyaları** sekmesinde **Ekle**'yi seçin. 

1. Değişiklik İzleme için Linux dosyası Ekle sayfasında, izlenecek dosya veya dizin bilgilerini girin ve **Kaydet**' e tıklayın.

|Özellik  |Açıklama  |
|---------|---------|
|Etkin     | Ayarın uygulanmış olup olmadığını belirler        |
|Öğe Adı     | İzlenecek dosyanın kolay adı        |
|Grup     | Dosyaları mantıksal bir biçimde gruplandırmaya yönelik grup adı        |
|Yolu girin     | Dosyanın denetleneceği yol. Örneğin: “/etc/*.conf”       |
|Yol Türü     | İzlenecek öğenin türü için olası değerler: Dosya ve Dizin        |
|Özyineleme     | İzlenecek öğe aranırken özyinelemenin kullanılıp kullanılmadığını belirler.        |
|Sudo Kullan     | Bu ayar, öğe denetlenirken sudonun kullanılıp kullanılmadığını belirler.         |
|Bağlantılar     | Bu ayar, dizinleri dolaşırken sembolik bağlantıların nasıl ele alındığını belirler.<br> **Yoksay** - Sembolik bağlantıları yoksayar ve başvurulan dosyaları veya dizinleri içermez<br>**İzle** - Özyineleme sırasında sembolik bağlantıları izler ve başvurulan dosyaları veya dizinleri de içerir<br>**Yönet** - Sembolik bağlantıları izler ve döndürülen içeriğin işlenmesinde değişiklik yapılmasına olanak sağlar      |
|Dosya içeriğini tüm ayarlar için karşıya yükleme| İzlenen değişikliklerin dosya içeriği karşıya yükleme işlemini açar veya kapatır. Kullanılabilir seçenekler: True veya False.|

   > [!NOTE]
   > **Bağlantıları Yönet** seçeneği önerilmez. Dosya içeriğini alma desteklenmiyor.

## <a name="enable-activity-log-connection"></a>Etkinlik günlüğü bağlantısını etkinleştirme

VM'nizdeki Değişik izleme sayfasında **Etkinlik Günlüğü Bağlantısını Yönetme**'yi seçin. Bu görev Azure Etkinlik günlüğü sayfasını açar. SANAL makinenizin Azure etkinlik günlüğüne Değişiklik İzleme bağlanmak için **Bağlan** ' a tıklayın.

Bu ayar etkin durumdayken VM'nizin Özet sayfasına gidip **Durdur**'u seçerek VM'nizi durdurun. Sorulduğunda **Evet**'i seçerek VM'yi durdurun. Serbest bırakıldıktan sonra **Başlat**'ı seçerek VM'nizi yeniden başlatın.

VM'yi durdurup başlattığınızda etkinlik günlüğüne bir olay kaydedilir. Değişiklik izleme sayfasına dönün. Sayfanın en altındaki **Olaylar** sekmesini seçin. Bir süre sonra olaylar grafik ve tablo şeklinde gösterilir. Önceki adımda olduğu gibi her bir olayı seçerek ayrıntılı bilgileri görüntüleyebilirsiniz.

![Değişiklik ayrıntılarını portalda görüntüleme](./media/automation-tutorial-troubleshoot-changes/viewevents.png)

## <a name="view-changes"></a>Değişiklikleri görüntüleme

Değişiklik izleme ve Stok çözümünü etkinleştirdikten sonra sonuçları Değişiklik izleme sayfasında görüntüleyebilirsiniz.

VM'nizin içinde **İŞLEMLER** bölümünden **Değişiklik izleme**'yi seçin.

![Sanal makinede yapılan değişikliklerin listesini gösteren ekran görüntüsü](./media/automation-tutorial-troubleshoot-changes/change-tracking-list.png)

Grafik, zaman içinde gerçekleştirilen değişiklikleri gösterir.
Etkinlik Günlüğü bağlantısı eklediğinizde en üstteki çizgi grafik Azure Etkinlik Günlüğü olaylarını görüntüler.
Çubuk grafiklerin her satırı farklı bir izlenebilir Değişiklik türünü temsil eder.
Bu türler Linux daemon'ları, dosyalar, Windows Kayıt Defteri anahtarları, yazılımlar ve Windows hizmetleridir.
Değişiklik sekmesinde görselleştirmede gösterilen değişikliklerle ilgili ayrıntılar, değişikliğin oluşma zamanına göre azalan düzende (en son gerçekleşen en başta) gösterilir.
**Olaylar** sekmesindeki tabloda bağlı Etkinlik Günlüğü olayları ile ayrıntıları en son gerçekleşen en başta olacak şekilde gösterilir.

Sonuçlara bakarak hizmet ve yazılım değişiklikleri dahil olmak üzere sistemde birçok değişiklik yapıldığını görebilirsiniz. Sayfanın en üstünde bulunan filtreleri kullanarak sonuçları **Değişiklik türü** veya zaman aralığına göre filtreleyebilirsiniz.

Bir **Windowsservices** değişikliği seçin. Bu seçim, değişiklik ayrıntılarını ve değişiklikten önceki ve sonraki değerleriyle ilgili ayrıntıları gösteren değişiklik ayrıntıları sayfasını açar. Bu örnekte Yazılım Koruması hizmeti durdurulmuştur.

![Değişiklik ayrıntılarını portalda görüntüleme](./media/automation-tutorial-troubleshoot-changes/change-details.png)

## <a name="configure-alerts"></a>Uyarı yapılandırma

Azure portalda değişiklikleri görüntülemek faydalı olabilir ancak durdurulan bir hizmet gibi bir değişiklik olduğunda uyarılmak daha faydalıdır.

Azure portalda durdurulmuş hizmetler için bir uyarı eklemek istiyorsanız **İzleme**’ye gidin. Ardından **Paylaşılan Hizmetler** altında **Uyarılar**’ı seçin ve **+ Yeni uyarı kuralı**’na tıklayın

Kaynak seçmek için **Seç** ' e tıklayın. Kaynak seçin sayfasında, **kaynak türüne göre filtrele** açılan menüsünden **Log Analytics** ' yi seçin. Log Analytics çalışma alanınızı ve ardından **Bitti**'yi seçin.

![Bir kaynak seçin](./media/automation-tutorial-troubleshoot-changes/select-a-resource.png)

**Koşul Ekle**' ye tıklayın, tablodaki sinyal mantığını Yapılandır sayfasında, **özel günlük araması**' nı seçin. Arama sorgusu metin kutusuna aşağıdaki sorguyu girin:

```loganalytics
ConfigurationChange | where ConfigChangeType == "WindowsServices" and SvcName == "W3SVC" and SvcState == "Stopped" | summarize by Computer
```

Bu sorgu, belirtilen zaman çerçevesinde W3SVC hizmeti durdurulan bilgisayarları döndürür.

**Uyarı mantığı** bölümünde **Eşik** alanına **0** değerini girin. İşiniz bittiğinde **Bitti**'yi seçin.

![Sinyal mantığını yapılandırma](./media/automation-tutorial-troubleshoot-changes/configure-signal-logic.png)

**Eylem grupları**' nın altında **Yeni oluştur**' u seçin. Eylem grubu, birden çok uyarıda kullanabileceğiniz eylemlerden oluşan bir gruptur. Eylemlere e-posta bildirimleri, runbook'lar, web kancaları ve diğer birçok şey dahildir. Eylem grupları hakkında daha fazla bilgi edinmek için bkz. [Eylem grupları oluşturma ve yönetme](../azure-monitor/platform/action-groups.md).

**Uyarı ayrıntıları**' nın altında, uyarı için bir ad ve açıklama girin. **Önem derecesi** değerini **Bilgilendirici (önem derecesi 2)**, **Uyarı (önem derecesi 1)**, veya **Kritik (önem derecesi 0)** olarak ayarlayın.

**Eylem grubu adı** kutusuna uyarı için ad ve kısa ad. Bu eylem grubu kullanılarak bildirim gönderildiğinde tam grup adı yerine kısa ad kullanılır.

**Eylemler** bölümünde eyleme için **E-posta Yöneticileri** gibi bir ad girin. **EYLEM TÜRÜ** bölümünde **E-posta/SMS/Anında İletme/Ses**'i seçin. **AYRINTILAR** bölümünden **Ayrıntıları düzenle**’yi seçin.

![Eylem grubu ekle](./media/automation-tutorial-troubleshoot-changes/add-action-group.png)

E-posta/SMS/Anında İletme/Ses bölmesine bir ad girin. **E-posta** onay kutusunu seçip geçerli bir e-posta adresi girin. Bölmedeki **Tamam** ' a ve ardından eylem grubu Ekle sayfasında **Tamam** ' a tıklayın.

Uyarı e-postasının konusunu özelleştirmek için **Eylemleri Özelleştirin** bölümündeki **Kural oluştur**, kısmında **E-posta konusu**'nu seçin. İşleminiz bittiğinde **Uyarı kuralı oluştur**'u seçin. Bu uyarı, bir güncelleştirme dağıtımı başarılı olduğunda sizi uyarır ve güncelleştirme dağıtımı çalıştırmasının hangi makineleri kapsadığını bildirir.

Aşağıdaki görüntüde W3SVC hizmeti durduğunda alınan örnek bir e-posta vardır.

![e-posta](./media/automation-tutorial-troubleshoot-changes/email.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * VM'de Değişiklik İzleme ve Stok özelliklerini etkinleştirme
> * Durdurulan hizmetlerin değişiklik günlüklerinde arama yapma
> * Değişiklik izlemeyi yapılandırma
> * Etkinlik günlüğü bağlantısını etkinleştirme
> * Bir olay tetikleme
> * Değişiklikleri görüntüleme
> * Uyarı yapılandırma

Daha fazla bilgi edinmek için Değişiklik izleme ve Stok çözümü özetine göz atın.

> [!div class="nextstepaction"]
> [Değişiklik yönetimi ve Stok çözümü](automation-change-tracking.md)

