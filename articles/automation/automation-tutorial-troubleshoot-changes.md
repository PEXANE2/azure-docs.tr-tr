---
title: Azure Otomasyonu 'nda Azure VM 'de yapılan değişiklikleri giderme | Microsoft Docs
description: Bu makalede, Azure VM 'de değişiklikleri nasıl giderebileceğiniz açıklanır.
services: automation
ms.subservice: change-inventory-management
keywords: değişiklik, izleme, değişiklik izleme, stok, Otomasyon
ms.date: 12/05/2018
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: 05b99c7f010ad307956b70890ee45f649111b373
ms.sourcegitcommit: 03662d76a816e98cfc85462cbe9705f6890ed638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90530548"
---
# <a name="troubleshoot-changes-on-an-azure-vm"></a>Azure VM 'deki değişikliklerle ilgili sorunları giderme

Bu öğreticide bir Azure sanal makinesi üzerindeki değişikliklerle ilgili sorunları gidermeyi öğreneceksiniz. Değişiklik İzleme ve envanteri etkinleştirerek, bilgisayarlarınızda yazılım, dosya, Linux Daemon 'ları, Windows Hizmetleri ve Windows kayıt defteri anahtarlarının değişikliklerini izleyebilirsiniz.
Bu yapılandırma değişikliklerinin tanımlanması, ortamınızdaki işletimsel sorunları belirlemenize yardımcı olabilir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * VM Değişiklik İzleme ve envanterini etkinleştirme
> * Durdurulan hizmetlerin değişiklik günlüklerinde arama yapma
> * Değişiklik izlemeyi yapılandırma
> * Etkinlik günlüğü bağlantısını etkinleştirme
> * Bir olay tetikleme
> * Değişiklikleri görüntüleme
> * Uyarı yapılandırma

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Azure aboneliği. Henüz bir hesabınız yoksa [MSDN abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) veya [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)için kaydolabilirsiniz.
* İzleyici ve eylem runbook 'larını ve Izleyici görevini tutacak bir [Otomasyon hesabı](./index.yml) .
* Özelliği için etkinleştirilecek bir [sanal makine](../virtual-machines/windows/quick-create-portal.md) .

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="enable-change-tracking-and-inventory"></a>Değişiklik İzlemeyi ve Sayımı Etkinleştirme

Bu öğretici için öncelikle Değişiklik İzleme ve stoku etkinleştirmeniz gerekir. Özelliği daha önce etkinleştirdiyseniz, bu adım gerekli değildir.

>[!NOTE]
>Alanlar gri ise, VM için başka bir Otomasyon özelliği etkinleştirilir ve aynı çalışma alanını ve otomasyon hesabını kullanmanız gerekir.

1. **Sanal makineler** ' i seçin ve LISTEDEN bir VM seçin.
2. Sol taraftaki menüde, **işlemler**altında **Stok** ' ı seçin. Envanter sayfası açılır.

    ![Değişikliği etkinleştir](./media/automation-tutorial-troubleshoot-changes/enableinventory.png)

3. [Log Analytics](../azure-monitor/log-query/log-query-overview.md) çalışma alanını seçin. Bu çalışma alanı, Değişiklik İzleme ve envanter gibi özellikler tarafından oluşturulan verileri toplar. Çalışma alanı, birden fazla kaynaktan alınan verilerin incelenip analiz edilebileceği ortak bir konum sağlar.

    [!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

4. Kullanılacak Otomasyon hesabını seçin.

5. Dağıtımın konumunu yapılandırın.

5. VM 'niz için özelliği dağıtmak üzere **Etkinleştir** ' e tıklayın. 

Kurulum sırasında VM, Windows için Log Analytics Aracısı ve [karma Runbook Worker](automation-hybrid-runbook-worker.md)ile sağlanır. Değişiklik İzleme ve stokun etkinleştirilmesi 15 dakika kadar sürebilir. Bu süre boyunca tarayıcı penceresini kapatmamanız gerekir.

Özellik etkinleştirildikten sonra, sanal makinede yüklü yazılımlar ve değişiklikler hakkında bilgiler Azure Izleyici günlüklerine akar.
Verilerin çözümlemeye hazır hale gelmesi 30 dakika ile 6 saat arasında sürebilir.

## <a name="use-change-tracking-and-inventory-in-azure-monitor-logs"></a>Azure Izleyici günlüklerinde Değişiklik İzleme ve envanteri kullanma

Değişiklik İzleme ve envanter, Azure Izleyici günlüklerine gönderilen günlük verileri oluşturur. Sorguları çalıştırarak günlüklerde arama yapmak için değişiklik izleme sayfasının en üstünde **Log Analytics** ' yi seçin. Değişiklik izleme verileri, türü altında depolanır `ConfigurationChange` .

Aşağıdaki örnek Log Analytics sorgu, durdurulmuş olan tüm Windows hizmetlerini döndürür.

```loganalytics
ConfigurationChange
| where ConfigChangeType == "WindowsServices" and SvcState == "Stopped"
```

Azure Izleyici günlüklerinde günlük dosyalarını çalıştırma ve arama hakkında daha fazla bilgi edinmek için bkz. [Azure izleyici günlükleri](../azure-monitor/log-query/log-query-overview.md).

## <a name="configure-change-tracking"></a>Değişiklik izlemeyi yapılandırma

Değişiklik izleme ile VM 'nizin değişiklik izleme sayfasının en üstündeki **düzenleme ayarlarını** kullanarak toplanacak ve izlediğiniz dosyaları ve kayıt defteri anahtarlarını seçersiniz. Çalışma alanı yapılandırma sayfasında izlemek için Windows kayıt defteri anahtarlarını, Windows dosyalarını veya Linux dosyalarını ekleyebilirsiniz.

> [!NOTE]
> Değişiklik izleme ve stok aynı koleksiyon ayarlarını kullanır ve ayarlar bir çalışma alanı düzeyinde yapılandırılır.

### <a name="add-a-windows-registry-key"></a>Windows kayıt defteri anahtarı ekleme

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
   > **Links** özelliği için **Manage** değeri önerilmez. Dosya içeriğini alma desteklenmiyor.

## <a name="enable-activity-log-connection"></a>Etkinlik günlüğü bağlantısını etkinleştirme

1. VM'nizdeki Değişik izleme sayfasında **Etkinlik Günlüğü Bağlantısını Yönetme**'yi seçin. 

2. Azure etkinlik günlüğü sayfasında, sanal makinenizin Azure etkinlik günlüğüne Değişiklik İzleme ve envanterini bağlamak için **Bağlan** ' a tıklayın.

3. VM 'nizin genel bakış sayfasına gidin ve VM 'nizi durdurmak için **Durdur** ' u seçin. 

4. Sorulduğunda **Evet**'i seçerek VM'yi durdurun. 

5. VM serbest bırakıldığında, yeniden başlatmak için **Başlat** ' ı seçin. VM 'yi durdurma ve başlatma etkinlik günlüğünde bir olay günlüğe kaydeder. 

## <a name="view-changes"></a>Değişiklikleri görüntüleme

1. Değişiklik izleme sayfasına geri gidin ve sayfanın altındaki **Olaylar** sekmesini seçin. 

2. Bir süre sonra, değişiklik izleme olayları grafikte ve tabloda gösterilir. Grafik, zaman içinde gerçekleştirilen değişiklikleri gösterir. Üstteki çizgi grafik, Azure etkinlik günlüğü olaylarını görüntüler. Çubuk grafiklerin her bir satırı, farklı bir rafa yerleştirilebilir değişiklik türünü temsil eder. Bu türler Linux Daemon 'ları, dosyalar, Windows kayıt defteri anahtarları, yazılımlar ve Windows hizmetlerdir. Değişiklik sekmesi, görüntülenen değişiklikler için en son değişiklikten önce görüntülenen ayrıntıları gösterir.

    ![Portalda olayları görüntüleme](./media/automation-tutorial-troubleshoot-changes/viewevents.png)

3. Sistemde, hizmetler ve yazılımla ilgili değişiklikler dahil olmak üzere birden çok değişiklik olduğuna dikkat edin. Sayfanın en üstünde bulunan filtreleri kullanarak sonuçları **Değişiklik türü** veya zaman aralığına göre filtreleyebilirsiniz.

    ![VM 'deki değişikliklerin listesi](./media/automation-tutorial-troubleshoot-changes/change-tracking-list.png)

4. Bir **Windowsservices** değişikliği seçin. Bu seçim, değişiklik ayrıntılarını ve değişiklikten önceki ve sonraki değerleriyle ilgili ayrıntıları gösteren değişiklik ayrıntıları sayfasını açar. Bu örnekte Yazılım Koruması hizmeti durdurulmuştur.

    ![Değişiklik ayrıntılarını portalda görüntüleme](./media/automation-tutorial-troubleshoot-changes/change-details.png)

## <a name="configure-alerts"></a>Uyarı yapılandırma

Azure portalda değişiklikleri görüntülemek faydalı olabilir ancak durdurulan bir hizmet gibi bir değişiklik olduğunda uyarılmak daha faydalıdır. Durdurulmuş bir hizmet için uyarı ekleyelim. 

1. Azure portal **izleyici**' ye gidin. 

2. **Paylaşılan hizmetler**altında **Uyarılar** ' ı seçin ve **+ Yeni uyarı kuralı**' na tıklayın.

3. Kaynak seçmek için **Seç** ' e tıklayın. 

4. Kaynak seçin sayfasında, **kaynak türüne göre filtrele** açılan menüsünden **Log Analytics** ' yi seçin. 

5. Log Analytics çalışma alanınızı seçip **bitti**' ye tıklayın.

    ![Bir kaynak seçin](./media/automation-tutorial-troubleshoot-changes/select-a-resource.png)

6. **Koşul Ekle**' ye tıklayın.

7. Sinyal mantığını yapılandır sayfasındaki tabloda **özel günlük araması**' nı seçin. 

8. Arama sorgusu metin kutusuna aşağıdaki sorguyu girin:

    ```loganalytics
    ConfigurationChange | where ConfigChangeType == "WindowsServices" and SvcName == "W3SVC" and SvcState == "Stopped" | summarize by Computer
    ```

    Bu sorgu, belirtilen zaman çerçevesinde W3SVC hizmeti durdurulan bilgisayarları döndürür.

9. **Uyarı mantığı**altındaki **eşik** için **0**girin. İşiniz bittiğinde **bitti**' ye tıklayın.

    ![Sinyal mantığını yapılandırma](./media/automation-tutorial-troubleshoot-changes/configure-signal-logic.png)

10. **Eylem grupları**' nın altında **Yeni oluştur** ' u seçin. Eylem grubu, birden çok uyarıda kullanabileceğiniz eylemlerden oluşan bir gruptur. Eylemlere e-posta bildirimleri, runbook'lar, web kancaları ve diğer birçok şey dahildir. Eylem grupları hakkında daha fazla bilgi edinmek için bkz. [Eylem grupları oluşturma ve yönetme](../azure-monitor/platform/action-groups.md).

11. **Uyarı ayrıntıları**' nın altında, uyarı için bir ad ve açıklama girin. 

12. **Önem derecesi** değerini **Bilgilendirici (önem derecesi 2)**, **Uyarı (önem derecesi 1)**, veya **Kritik (önem derecesi 0)** olarak ayarlayın.

13. **Eylem grubu adı** kutusuna uyarı için ad ve kısa ad. Bu eylem grubu kullanılarak bildirim gönderildiğinde tam grup adı yerine kısa ad kullanılır.

14. **Eylemler**için, eylem Için **e-posta yöneticileri**gibi bir ad girin. 

15. **Eylem türü**Için **e-posta/SMS/Push/Voice**' ı seçin. 

16. **Ayrıntılar**Için **Ayrıntıları Düzenle**' yi seçin.

    ![Eylem grubu ekle](./media/automation-tutorial-troubleshoot-changes/add-action-group.png)

17. E-posta/SMS/gönderim/ses bölmesine bir ad girin, **e-posta** kutusunu seçin ve ardından geçerli bir e-posta adresi girin. İşiniz bittiğinde, bölmedeki **Tamam** ' a ve ardından eylem grubu Ekle sayfasında **Tamam** ' a tıklayın.

18. Uyarı e-postası konusunu özelleştirmek için, **eylemleri Özelleştir**' i seçin. 

19. **Oluşturma kuralı**Için **e-posta konusu**' ı seçin ve **Uyarı kuralı oluştur**' u seçin. Bu uyarı, bir güncelleştirme dağıtımı başarılı olduğunda sizi uyarır ve güncelleştirme dağıtımı çalıştırmasının hangi makineleri kapsadığını bildirir. Aşağıdaki görüntü, W3SVC hizmeti durdurulduğunda alınan örnek bir e-postadır.

    ![Ekran yakalama, W 3 S V C Hizmetleri durdurulduğunda alınan bir e-posta bildirimi gösterir.](./media/automation-tutorial-troubleshoot-changes/email.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * VM Değişiklik İzleme ve envanterini etkinleştirme
> * Durdurulan hizmetlerin değişiklik günlüklerinde arama yapma
> * Değişiklik izlemeyi yapılandırma
> * Etkinlik günlüğü bağlantısını etkinleştir
> * Bir olay tetikleme
> * Değişiklikleri görüntüleme
> * Uyarı yapılandırma

Daha fazla bilgi edinmek için Değişiklik İzleme ve envanter özelliğine genel bakış ' a geçin.

> [!div class="nextstepaction"]
> [Değişiklik İzleme ve envantere genel bakış](./change-tracking.md)
