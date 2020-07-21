---
title: Key Vault için Azure Izleyici ile Key Vault izleme (Önizleme) | Microsoft Docs
description: Bu makalede, Anahtar kasaları için Azure Izleyicisi açıklanmaktadır.
services: azure-monitor
ms.topic: conceptual
author: mrbullwinkle
ms.author: mbullwin
ms.date: 04/13/2019
ms.openlocfilehash: 7b52a1ee67c22fb3bded49a80d35305bdf612f10
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86498972"
---
# <a name="monitoring-your-key-vault-service-with-azure-monitor-for-key-vault-preview"></a>Key Vault için Azure Izleyici ile Anahtar Kasası hizmetinizi izleme (Önizleme)
Key Vault için Azure Izleyici (Önizleme), Key Vault isteklerinizin, performanlarınızın, hatalarından ve gecikmelerinden oluşan Birleşik bir görünüm sunarak anahtar kasalarınızın kapsamlı bir şekilde izlenmesini sağlar.
Bu makale, Key Vault için Azure Izleyici deneyiminin nasıl ekleneceğini ve özelleştirildiğini anlamanıza yardımcı olur (Önizleme).

## <a name="introduction-to-azure-monitor-for-key-vault-preview"></a>Key Vault için Azure Izleyici 'ye giriş (Önizleme)

Deneyimle karşılaşmadan önce, bilgileri nasıl sunduklarını ve görselleştirir.
-    Gecikme, hataların dökümünü ve işlemlere ve gecikme süresine göre performansının anlık görüntü görünümünü gösteren **Ölçek perspektifinde** .
-   Ayrıntılı analizler gerçekleştirmek için belirli bir anahtar kasasının **detaya gitme analizini** yapın.
-    **Hangi ölçümleri** görmek istediğinizi değiştirmek, sınırlarınız ile hizalı eşikleri değiştirmek veya ayarlamak ve kendi çalışma kitabınızı kaydetmek için özelleştirebilirsiniz. Çalışma kitabındaki grafikler Azure panolarına sabitlenebilir.

Key Vault için Azure Izleyici, genel bir izleme çözümü sağlamak için hem günlükleri hem de ölçümleri birleştirir. Tüm kullanıcılar ölçüm tabanlı izleme verilerine erişebilir, ancak günlük tabanlı görselleştirmelerin eklenmesi kullanıcıların [Azure Key Vault günlüğünü etkinleştirmesini](../../key-vault/general/logging.md)gerektirebilir.

## <a name="configuring-your-key-vaults-for-monitoring"></a>Anahtar kasalarınızı izleme için yapılandırma

> [!NOTE]
> Günlüklerin etkinleştirilmesi, ek izleme olanakları sağlayan ücretli hizmettir.

1. Işlemler & gecikme süresi sekmesi kaç tane ve hangi anahtar kasalarının etkin olduğunu belirlemenize yardımcı olur. Toplamaya başlamak için Etkinleştir düğmesini seçin, bu, sizi tanılama günlüklerini etkinleştirmek için gereken anahtar kasalarını listeleyen ayrı bir çalışma kitabına getirecek şekilde **etkinleştirir** .

    ![Mavi etkinleştirme düğmesi görüntülenirken işlemler ve gecikme süresi sekmesinin ekran görüntüsü](./media/key-vaults-insights-overview/enable-logging.png)

2. Tanılama günlüklerini etkinleştirmek için, eylemler sütununun altındaki bağlantıyı **Etkinleştir** ' e tıklayın ve bir Log Analytics çalışma alanına Günlükler gönderen yeni bir tanılama ayarı oluşturun. Tüm günlüklerin aynı çalışma alanına gönderilmesi önerilir.

3. Tanılama ayarları kaydedildikten sonra, Key Vault öngörülerinin altındaki tüm günlük tabanlı grafikleri ve görselleştirmeleri görüntüleyebileceksiniz. Günlüklerin doldurulmasının başlaması için birkaç dakika sürebileceğini lütfen unutmayın.

4. Key Vault hizmetiniz için tanılama günlüklerini etkinleştirme hakkında daha fazla yardım için, [tam kılavuzu](../../key-vault/general/logging.md)okuyun.

## <a name="view-from-azure-monitor"></a>Azure Izleyici 'den görüntüle

Azure Izleyici 'den, aboneliğinizdeki birden çok anahtar kasasından gelen istek, gecikme ve başarısızlık ayrıntılarını görüntüleyebilir ve performans sorunlarını ve kısıtlama senaryolarını belirlemenize yardımcı olabilirsiniz.

Tüm aboneliklerinizde depolama hesaplarınızın kullanımını ve işlemlerini görüntülemek için aşağıdaki adımları gerçekleştirin:

1. [Azure Portal](https://portal.azure.com/) oturum açın

2. Azure portal sol bölmeden **izleyici** ' yi seçin ve Içgörüler bölümünde **Anahtar Kasası (Önizleme)** öğesini seçin.

![Birden çok grafik içeren genel bakış deneyiminin ekran görüntüsü](./media/key-vaults-insights-overview/overview.png)

## <a name="overview-workbook"></a>Genel Bakış çalışma kitabı

Seçili abonelik için genel bakış çalışma kitabında, tablo, abonelik dahilinde gruplanmış Anahtar kasaları için etkileşimli Anahtar Kasası ölçümlerini görüntüler. Aşağıdaki açılan listelerden seçtiğiniz seçeneklere göre sonuçlara filtre uygulayabilirsiniz:

* Abonelikler – yalnızca anahtar kasaları içeren abonelikler listelenir.

* Anahtar kasaları: varsayılan olarak en fazla 5 Anahtar Kasası önceden seçilmiştir. Kapsam seçicide tüm veya birden çok anahtar kasalarını seçerseniz, en fazla 200 Anahtar Kasası döndürülür. Örneğin, seçtiğiniz üç abonelik arasında toplam 573 Anahtar Kasası varsa, yalnızca 200 kasa görüntülenecektir.

* Zaman aralığı: varsayılan olarak, yapılan ilgili seçimlere göre son 24 saati bilgileri görüntüler.

Sayaç kutucuğu, açılan liste altında, seçilen aboneliklerdeki anahtar kasalarının toplam sayısını kaydeder ve kaç tane seçili olduğunu yansıtır. Çalışma kitabının istek, başarısızlık ve gecikme süresi ölçümlerini rapor eden sütunları için koşullu renk kodlu ısı haritalarını vardır. En büyük renk en yüksek değere sahiptir ve en düşük değere göre daha açık bir renge sahiptir.

## <a name="failures-workbook"></a>Çalışma kitabı başarısız

Sayfanın üst kısmında bulunan **hataların** yanı sıra arızalar sekmesi açılır. API isabetlerinin, zaman içinde sıklık, belirli yanıt kodlarının miktarı ile birlikte gösterilir.

![Başarısızlık çalışma kitabının ekran görüntüsü](./media/key-vaults-insights-overview/failures.png)

Çalışma kitabındaki sütunlar için, API isabetlerinin bir mavi değerle rapor veren koşullu renk kodlaması veya ısı haritalarını vardır. En büyük renk en yüksek değere sahiptir ve en düşük değere göre daha açık bir renge sahiptir.

Çalışma kitabı başarıları (2xx durum kodları), kimlik doğrulama hatalarını (401/403 durum kodları), daraltma (429 durum kodları) ve diğer hataları (4xx durum kodları) görüntüler.

Durum kodlarının her birinin neyi temsil ettiğini daha iyi anlamak için, [Azure Key Vault durum ve yanıt kodlarıyla](../../key-vault/general/authentication-requests-and-responses.md)ilgili belgelerde okumanız önerilir.

## <a name="operations--latency-workbook"></a>İşlemler & gecikme çalışma kitabı

Sayfanın üst kısmında **işlemler & gecikme süresi** seçin ve **Işlemler & gecikme süresi** sekmesi açılır. Bu sekme, izleme için anahtar kasalarınızı eklemenizi sağlar. Daha ayrıntılı adımlar için, [izleme için anahtar kasalarınızı yapılandırma](#configuring-your-key-vaults-for-monitoring) bölümüne bakın.

Günlüğe kaydetme için anahtar kasalarınızın kaç tane etkin olduğunu görebilirsiniz. En az bir kasa doğru şekilde yapılandırıldıysa, anahtar kasalarınızın her biri için işlemleri ve durum kodlarını görüntüleyen tabloları görebileceksiniz. Tek işlemle ilgili ek bilgi almak için bir satırın Ayrıntılar bölümüne tıklayabilirsiniz.

![İşlem ve gecikme şemaları ekran görüntüsü](./media/key-vaults-insights-overview/logs.png)

Bu bölüm için herhangi bir veri görmüyorsanız, Azure Key Vault günlüklerin nasıl etkinleştirileceği hakkında üst bölüme başvurun veya aşağıdaki sorun giderme bölümüne bakın.

## <a name="view-from-a-key-vault-resource"></a>Key Vault kaynağından görüntüleme

Key Vault için Azure Izleyici 'ye doğrudan bir anahtar kasasından erişmek için:

1. Azure portal, Anahtar kasaları ' nı seçin.

2. Listeden bir Anahtar Kasası seçin. İzleme bölümünde Öngörüler ' i (Önizleme) seçin.

Bu görünümlere, Azure Izleyici düzeyi çalışma kitabından bir anahtar kasasının kaynak adı seçilerek de erişilebilir.

![Bir Anahtar Kasası kaynağından görünümün ekran görüntüsü](./media/key-vaults-insights-overview/key-vault-resource-view.png)

Anahtar Kasası için **genel bakış** çalışma kitabında, hızlı bir şekilde değerlendirmenize yardımcı olan çeşitli performans ölçümleri gösterilmektedir:

- Anahtar Kasası işlemleri, gecikme süresi ve kullanılabilirliğiyle ilgili en önemli ayrıntıları gösteren etkileşimli performans grafikleri.

- Ölçümler ve durum kutucukları, hizmet kullanılabilirliğini, Anahtar Kasası kaynağına toplam işlem sayısını ve genel gecikme süresini vurgular.

**Hatalara** veya **işlemlere** yönelik diğer sekmelerin birini seçmek ilgili çalışma kitaplarını açar.

![Başarısızlık görünümü ekran görüntüsü](./media/key-vaults-insights-overview/resource-failures.png)

Hatalar çalışma kitabı, seçili zaman çerçevesinde tüm Anahtar Kasası isteklerinin sonuçlarını ortadan kaldıracak ve başarı (2xx), kimlik doğrulama hataları (401/403), azaltma (429) ve diğer hatalar üzerinde kategori sağlar.

![İşlemler görünümünün ekran görüntüsü](./media/key-vaults-insights-overview/operations.png)

Işlemler çalışma kitabı, kullanıcıların, en üst düzey kutucuklar kullanılarak sonuç durumuna göre filtrelenebilir tüm işlemlerin tam ayrıntılarına derinlemesine bakış sağlamasına olanak tanır.

![İşlemler görünümünün ekran görüntüsü](./media/key-vaults-insights-overview/info.png)

Kullanıcılar, üst tablodaki belirli işlem türlerine göre görünümleri Ayrıca, kullanıcıların bir açılan bağlam bölmesinde tam işlem ayrıntılarını görüntüleyebileceği, daha düşük tabloyu dinamik olarak güncelleştiren şekilde de kapsamını belirleyebilir.

>[!NOTE]
> Kullanıcıların bu çalışma kitabını görüntülemek için tanılama ayarlarının etkin olması gerektiğini unutmayın. Tanılama ayarını etkinleştirme hakkında daha fazla bilgi edinmek için [Azure Key Vault günlüğe kaydetme](../../key-vault/general/logging.md)hakkında daha fazla bilgi edinin.

## <a name="pin-and-export"></a>PIN ve dışarı aktarma

Bölümün sağ üst köşesindeki raptiye simgesini seçerek ölçüm bölümlerinin herhangi birini bir Azure panosuna sabitleyebilirsiniz.

Çoklu abonelik ve Anahtar kasaları genel bakış veya başarısızlık çalışma kitapları, raptiye simgesinin solunda bulunan İndir simgesini seçerek Excel biçimindeki sonuçları dışarı aktarmayı destekler.

![PIN simgesinin seçili ekran görüntüsü](./media/key-vaults-insights-overview/pin.png)

## <a name="customize-azure-monitor-for-key-vault"></a>Key Vault için Azure Izleyicisini özelleştirme

Bu bölümde, veri analizi ihtiyaçlarınızı desteklemeye yönelik olarak özelleştirmek üzere çalışma kitabını düzenlemeyle ilgili yaygın senaryolar vurgulanmıştır:
*  Çalışma kitabını her zaman belirli bir abonelik veya Anahtar Kasası seçilecek şekilde kapsama
* Kılavuzdaki ölçümleri değiştirme
* İstek eşiğini değiştirme
* Renk işlemeyi değiştirme

En üstteki araç çubuğundan **Özelleştir** düğmesini seçerek düzenleme modunu etkinleştirerek özelleştirmeleri başlatabilirsiniz.

![Özelleştirme düğmesinin ekran görüntüsü](./media/key-vaults-insights-overview/customize.png)

Özelleştirmeler, yayımlanan çalışma kitabımızda varsayılan yapılandırmanın üzerine yazılmasını engellemek için özel bir çalışma kitabına kaydedilir. Çalışma kitapları, sizin için özel Raporlarım bölümünde veya kaynak grubuna erişimi olan herkesin erişebileceği paylaşılan Raporlar bölümünde bir kaynak grubuna kaydedilir. Özel çalışma kitabını kaydettikten sonra başlatmak için çalışma kitabı galerisine gitmeniz gerekir.

![Çalışma kitabı galerinin ekran görüntüsü](./media/key-vaults-insights-overview/gallery.png)

### <a name="specifying-a-subscription-or-key-vault"></a>Abonelik veya Anahtar Kasası belirtme

Aşağıdaki adımları gerçekleştirerek, çoklu abonelik ve Anahtar Kasası genel bakış ya da çalışma kitaplarını her çalıştırmada belirli bir aboneliğe veya anahtar kasalarına göre yapılandırabilirsiniz:

1. Portalda **izleyici** ' yi seçin ve ardından sol bölmedeki anahtar kasalarını **(Önizleme)** seçin.
2. **Genel bakış** çalışma kitabında, komut çubuğundan **Düzenle**' yi seçin.
3. **Abonelikler** açılan listesinden, varsayılan olarak yo kullanmak istediğiniz bir veya daha fazla aboneliği seçin. Çalışma kitabının toplam 10 aboneliği seçmeyi desteklediğini unutmayın.
4. **Anahtar kasaları** açılan listesinden, varsayılan olarak kullanmak istediğiniz bir veya daha fazla hesabı seçin. Çalışma kitabının toplam 200 depolama hesabı seçmeyi desteklediğini unutmayın.
5. Özelleştirmelerinizle çalışma kitabının bir kopyasını kaydetmek için komut çubuğundan **farklı kaydet** ' i seçin ve ardından okuma moduna dönmek Için **Düzenle bitti** ' ye tıklayın.

## <a name="troubleshooting"></a>Sorun giderme

Genel sorun giderme kılavuzu için, adanmış çalışma kitabı tabanlı Öngörüler [sorun giderme makalesine](troubleshoot-workbooks.md)bakın.

Bu bölüm, Key Vault için Azure Izleyicisi 'ni (Önizleme) kullanırken karşılaşabileceğiniz yaygın sorunlardan bazılarının tanılama ve sorunlarını gidermenize yardımcı olur. Belirli sorununuzla ilgili bilgileri bulmak için aşağıdaki listeyi kullanın.

### <a name="resolving-performance-issues-or-failures"></a>Performans sorunlarını veya başarısızlıklarını çözme

Key Vault (Önizleme) için Azure Izleyici ile belirttiğiniz anahtar kasasıyla ilgili sorunları gidermeye yardımcı olmak için [Azure Key Vault belgelerine](../../key-vault/index.yml)bakın.

### <a name="why-can-i-only-see-200-key-vaults"></a>Neden yalnızca 200 anahtar kasalarını görebilirim?

Seçilebileceğini ve görüntülenebilecek 200 anahtar kasalarının bir sınırı vardır. Seçili aboneliklerin sayısından bağımsız olarak, seçili anahtar kasalarının sayısında 200 limiti vardır.

### <a name="why-dont-i-see-all-my-subscriptions-in-the-subscription-picker"></a>Abonelik seçicide neden tüm aboneliklerimi görmüyorum?

Yalnızca Azure portal üstbilgisindeki "Dizin + abonelik" bölümünde seçilen abonelik filtresinden seçtiğiniz anahtar kasalarını içeren abonelikler gösteriliyor.

![Abonelik filtresi ekran görüntüsü](./media/key-vaults-insights-overview/Subscriptions.png)

### <a name="i-am-getting-an-error-message-that-the-query-exceeds-the-maximum-number-of-workspacesregions-allowed-what-to-do-now"></a>"Sorgunun izin verilen en fazla çalışma alanı sayısını/bölgeyi aştığını", şimdi ne yapacakdığına ilişkin bir hata mesajı alıyorum

Şu anda, 25 bölge ve 200 çalışma alanı için bir sınır vardır. verilerinizi görüntülemek için, aboneliklerin ve/veya kaynak gruplarının sayısını azaltmanız gerekir.

### <a name="i-want-to-make-changes-or-add-additional-visualizations-to-key-vault-insights-how-do-i-do-so"></a>Key Vault içgörüler için değişiklik yapmak veya ek görselleştirme eklemek istiyorum, bunu nasıl yapabilirim?

Değişiklik yapmak için, çalışma kitabını değiştirmek üzere "düzenleme modunu" seçin, sonra çalışmanızı belirlenen bir aboneliğe ve kaynak grubuna bağlı yeni bir çalışma kitabı olarak kaydedebilirsiniz.

### <a name="what-is-the-time-grain-once-we-pin-any-part-of-the-workbooks"></a>Çalışma kitaplarının herhangi bir bölümünü sabitledikten sonra zaman çizgisi nedir?

"Auto" zaman dilimi ' ni kullanıyoruz, bu nedenle hangi zaman aralığının seçili olduğuna bağlıdır.

### <a name="what-is-the-time-range-when-any-part-of-the-workbook-is-pinned"></a>Çalışma kitabının herhangi bir bölümünün sabitlendiği zaman aralığı nedir?

Zaman aralığı, pano ayarlarına bağlı olarak değişir.

### <a name="why-do-i-not-see-any-data-for-my-key-vault-under-the-operations--latency-sections"></a>Işlemler & gecikme bölümleri bölümünde Key Vault için hiçbir veri görmüyorum

Günlük tabanlı verilerinizi görüntülemek için, izlemek istediğiniz her Anahtar Kasası için günlükleri etkinleştirmeniz gerekir. Bu, her Anahtar Kasası için Tanılama ayarları altında yapılabilir. Verilerinizi belirlenen bir Log Analytics çalışma alanına göndermeniz gerekir.

### <a name="i-have-already-enabled-logs-for-my-key-vault-why-am-i-still-unable-to-see-my-data-under-operations--latency"></a>Key Vault için günlükleri zaten etkinleştirdim, neden hala verileri "gecikme süresi altında göremem gerekir &

Şu anda, tanılama günlükleri geriye dönük olarak çalışmaz, bu nedenle yalnızca anahtar kasalarınıza yapılan eylemler yapıldıktan sonra, veriler görünmeye başlar. Bu nedenle, anahtar kasanızın ne kadar etkin olduğuna bağlı olarak saatlerce bir güne kadar zaman alabilir.

Ayrıca, yüksek sayıda anahtar kasası ve abonelik seçtiyseniz, sorgu sınırlamaları nedeniyle verilerinizi görüntüleyemeyebilirsiniz. Verilerinizi görüntülemek için Seçili aboneliklerin veya anahtar kasalarının sayısını azaltmanız gerekebilir. 

### <a name="what-if-i-want-to-see-other-data-or-make-my-own-visualizations-how-can-i-make-changes-to-the-key-vault-insights"></a>Diğer verileri görmek veya kendi görselleştirmelerimi yapmak istersem ne yapmalıyım? Key Vault öngörülerini nasıl değiştirebilir?

Mevcut çalışma kitabını düzenleme modunun kullanımı ile düzenleyebilir ve sonra çalışmanızı tüm yeni değişiklerinizi alacak yeni bir çalışma kitabı olarak kaydedebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Çalışma kitaplarının desteklemek için tasarlandıkları senaryoları, mevcut raporların yeni nasıl yazılacağını ve özelleştirildiğini ve [Azure izleyici çalışma kitaplarını kullanarak etkileşimli raporlar oluşturma](../platform/workbooks-overview.md)konusunu gözden geçirin.
