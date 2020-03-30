---
title: Azure Monitör Kaynak Grubu Öngörüleri | Microsoft Dokümanlar
description: Azure Monitor ile Kaynak Grubu düzeyinde dağıtılmış uygulamalarınızın ve hizmetlerinizin sistem durumunu ve performansını anlama
ms.subservice: ''
ms.topic: conceptual
author: NumberByColors
ms.author: daviste
ms.date: 09/19/2018
ms.reviewer: mbullwin
ms.openlocfilehash: 6d97e40bf2bf2298fb53609621db8ff2c6f1038f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77663547"
---
# <a name="monitor-resource-groups-with-azure-monitor-preview"></a>Azure Monitor ile kaynak gruplarını izleme (önizleme)

Modern uygulamalar genellikle karmaşık tır ve bir hizmeti sunmak için birlikte çalışan birçok ayrı kısık parçayla son derece dağıtılır. Bu karmaşıklığı tanıyan Azure Monitor, kaynak grupları için izleme öngörüleri sağlar. Bu, kaynak grubunun&mdash;ve uygulamanızın&mdash;bir bütün olarak sağlık durumu ve performansı yla ilgili bağlam sunarken, bireysel kaynaklarınızın karşılaştığı sorunları değerlendirmeyi ve tanılamayı kolaylaştırır.

## <a name="access-insights-for-resource-groups"></a>Kaynak grupları için öngörülere erişin

1. Sol taraftaki gezinti çubuğundan **Kaynak grupları** seçin.
2. Keşfetmek istediğiniz kaynak gruplarından birini seçin. (Abonelişe göre filtreleme sağlayan çok sayıda kaynak grubunuz varsa bazen yararlı olabilir.)
3. Bir kaynak grubunun istatistiklerine erişmek için, herhangi bir kaynak grubunun sol tarafındaki menüde **Öngörüler'i** tıklatın.

![Kaynak grubu öngörülerine genel bakış sayfasının ekran görüntüsü](./media/resource-group-insights/0001-overview.png)

## <a name="resources-with-active-alerts-and-health-issues"></a>Etkin uyarılar ve sistem durumu sorunları olan kaynaklar

Genel bakış sayfası, her kaynağın geçerli Azure Kaynak Durumu ile birlikte kaç uyarının ateşlendiğini ve hala etkin olduğunu gösterir. Birlikte, bu bilgiler sorunlarla karşılaşan kaynakları hızla noktalamanıza yardımcı olabilir. Uyarılar, kodunuzdaki sorunları ve altyapınızı nasıl yapılandırdığınızı algılamanıza yardımcı olur. Azure Kaynak Durumu yüzeyleri, bireysel uygulamalarınıza özgü olmayan Azure platformunun kendisiyle ilgili bir sorundur.

![Azure Kaynak Durumu bölmesinin ekran görüntüsü](./media/resource-group-insights/0002-overview.png)

### <a name="azure-resource-health"></a>Azure Kaynak Durumu

Azure Kaynak Durumu'nu görüntülemek için tablonun üstündeki **Azure Kaynak Durumu Göster** kutusunu işaretleyin. Bu sütun, sayfanın hızlı bir şekilde yüklenmesine yardımcı olmak için varsayılan olarak gizlenir.

![Kaynak durumu grafiği eklendi](./media/resource-group-insights/0003-overview.png)

Varsayılan olarak, kaynaklar uygulama katmanına ve kaynak türüne göre gruplandırılır. **Uygulama katmanı,** kaynak türlerinin yalnızca kaynak grubu öngörüleri genel bakış sayfası bağlamında bulunan basit bir kategorilendirmesidir. Uygulama kodu, bilgi işlem altyapısı, ağ oluşturma, depolama + veritabanları ile ilgili kaynak türleri vardır. Yönetim araçları kendi uygulama katmanlarını alır ve diğer tüm kaynaklar **Diğer** uygulama katmanına ait olarak sınıflandırılır. Bu gruplandırma, uygulamanızın hangi alt sistemlerinin sağlıklı ve sağlıksız olduğunu bir bakışta görmenize yardımcı olabilir.

## <a name="diagnose-issues-in-your-resource-group"></a>Kaynak grubunuzdaki sorunları tanılama

Kaynak grubu öngörüleri sayfası, sorunları tanılamanıza yardımcı olacak kapsama birkaç başka araç sağlar

   |         |          |
   | ---------------- |:-----|
   | [**Uyarılar**](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-unified-alerts)      |  Uyarılarınızı görüntüleyin, oluşturun ve yönetin. |
   | [**Ölçümler**](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics) | Metrik tabanlı verilerinizi görselleştirin ve keşfedin.    |
   | [**Etkinlik günlükleri**](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) | Azure'da gerçekleşen abonelik düzeyi olayları.  |
   | [**Uygulama haritası**](https://docs.microsoft.com/azure/application-insights/app-insights-app-map) | Performans darboğazlarını veya başarısız etkin noktalarını belirlemek için dağıtılmış uygulamanızın topolojisine gidin. |

## <a name="failures-and-performance"></a>Hatalar ve performans

Uygulamanızın yavaş çalıştığını veya kullanıcıların hata bildirdiğini fark ettiyseniz ne olur? Sorunları yalıtmak için tüm kaynaklarınızı aramak zaman alır.

**Performans** ve **Hatalar** sekmeleri, birçok yaygın kaynak türü için performans ve hata tanılama görünümlerini bir araya getirerek bu işlemi basitleştirir.

Çoğu kaynak türü, Azure Monitörçalışma Kitabı şablonlarından oluşan bir galeri açar. Oluşturduğunuz her çalışma kitabı özelleştirilebilir, kaydedilebilir, ekibinizle paylaşılabilir ve gelecekte benzer sorunları tanılamak için yeniden kullanılabilir.

### <a name="investigate-failures"></a>Hataları araştırma

Hatalar sekmesini test etmek için sol menüde **Araştır** altında **Hatalar'ı** seçin.

Seçiminiz yapıldıktan sonra sol taraftaki menü çubuğu değişir ve size yeni seçenekler sunar.

![Hata genel bakış bölmesi ekran görüntüsü](./media/resource-group-insights/00004-failures.png)

Uygulama Hizmeti seçildiğinde, azure monitör çalışma kitabı şablonlarından oluşan bir galeri sunulur.

![Uygulama çalışma kitabı galerisinin ekran görüntüsü](./media/resource-group-insights/0005-failure-insights-workbook.png)

Failure Insights şablonu seçimi çalışma kitabını açar.

![Hata raporunun ekran görüntüsü](./media/resource-group-insights/0006-failure-visual.png)

Satırlardan herhangi birini seçebilirsiniz. Seçim daha sonra grafik ayrıntıları görünümünde görüntülenir.

![Hata ayrıntılarının ekran görüntüsü](./media/resource-group-insights/0007-failure-details.png)

Çalışma kitapları, özel raporlar ve görselleştirmeler oluşturmanın zor çalışmalarını kolayca tüketilebilir bir biçimde özetler. Bazı kullanıcılar yalnızca önceden oluşturulmuş parametreleri ayarlamak istese de, çalışma kitapları tamamen özelleştirilebilir.

Bu çalışma kitabının dahili olarak nasıl çalıştığını anlamak için üst çubuktaki **Edit'i** seçin.

![Ek edit seçeneğinin ekran görüntüsü](./media/resource-group-insights/0008-failure-edit.png)

Çalışma kitabının çeşitli öğelerinin yakınında bir dizi **Edit** kutusu görünür. İşlemler tablosunun altındaki **Edit** kutusunu seçin.

![Kutularını edit ekran görüntüsü](./media/resource-group-insights/0009-failure-edit-graph.png)

Bu, tablo görselleştirme sürüş altında yatan günlük sorgusu ortaya çıkarır.

 ![Günlük sorgu penceresinin ekran görüntüsü](./media/resource-group-insights/0010-failure-edit-query.png)

Sorguyu doğrudan değiştirebilirsiniz. Ya da bir referans olarak kullanabilirsiniz ve kendi özel parametreli çalışma kitabı tasarlarken ondan ödünç.

### <a name="investigate-performance"></a>Performansı araştırma

Performans, kendi çalışma kitapları galerisini sunar. Uygulama Hizmeti için önceden oluşturulmuş Uygulama Performansı çalışma kitabı aşağıdaki görünümü sunar:

 ![Performans görünümünün ekran görüntüsü](./media/resource-group-insights/0011-performance.png)

Bu durumda, düzenlemeyi seçerseniz, bu görselleştirme kümesinin Azure Monitör Ölçümleri tarafından desteklendiğini görürsünüz.

 ![Azure Ölçümleri ile performans görünümünün ekran görüntüsü](./media/resource-group-insights/0012-performance-metrics.png)

## <a name="troubleshooting"></a>Sorun giderme

### <a name="enabling-access-to-alerts"></a>Uyarılara erişimi etkinleştirme

Kaynak Grupları için Azure Monitor'daki uyarıları görmek için, bu abonelik için Sahip veya Katılımcı rolüne sahip bir kişinin abonelikteki herhangi bir kaynak grubu için Kaynak Grupları için Azure Monitörü açması gerekir. Bu, okuma erişimi olan herkesin abonelikteki tüm kaynak grupları için Kaynak Grupları için Azure Monitor'da uyarıları görmesini sağlar. Sahibi veya Katılımcı rolünüz varsa, bu sayfayı birkaç dakika içinde yenileyin.

Kaynak Grupları için Azure Monitörü, uyarı durumunu almak için Azure Monitor Uyarıları Yönetimi sistemine güvenir. Uyarılar Yönetimi varsayılan olarak her kaynak grubu ve abonelik için yapılandırılmamıştır ve yalnızca Sahibi veya Katılımcı rolü olan biri tarafından etkinleştirilebilir. Aşağıdakiler tarafından etkinleştirilebilir:
* Abonelikteki herhangi bir kaynak grubu için Kaynak Grupları için Azure Monitörü açma.
* Veya aboneye giderek, **Kaynak Sağlayıcıları**tıklayarak, ardından **Alerts.Management için Kaydol'u**tıklatın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Monitör Çalışma Kitapları](https://docs.microsoft.com/azure/application-insights/app-insights-usage-workbooks)
- [Azure Kaynak Durumu](https://docs.microsoft.com/azure/service-health/resource-health-overview)
- [Azure Monitör Uyarıları](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-unified-alerts)
