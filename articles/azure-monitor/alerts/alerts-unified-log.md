---
title: Azure Izleyici 'de günlük uyarıları
description: Belirttiğiniz günlük sorgusu koşulu karşılandığında e-postaları, bildirimleri, çağrı Web siteleri URL 'Lerini (Web kancaları) veya Otomasyonu tetikleyin
author: yanivlavi
ms.author: yalavi
ms.topic: conceptual
ms.date: 09/22/2020
ms.openlocfilehash: 786e9b472d1f900e94e5d0cfa6a00e0f85547704
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102037702"
---
# <a name="log-alerts-in-azure-monitor"></a>Azure Izleyici 'de günlük uyarıları

## <a name="overview"></a>Genel Bakış

Günlük uyarıları, [Azure uyarıları](./alerts-overview.md)'nda desteklenen uyarı türlerinden biridir. Günlük uyarıları, kullanıcıların kaynak günlüklerini her ayarlama sıklığını değerlendirmek ve sonuçlara göre bir uyarı tetiklemesi için [Log Analytics](../logs/log-analytics-tutorial.md) bir sorgu kullanmasına izin verir. Kurallar, [eylem gruplarını](./action-groups.md)kullanarak bir veya daha fazla eylemi tetikleyebilir.

> [!NOTE]
> [Log Analytics çalışma alanındaki](../logs/log-analytics-tutorial.md) günlük verileri Azure izleyici ölçüm deposuna gönderilebilir. Ölçüm uyarıları [farklı davranışa](alerts-metric-overview.md)sahiptir ve bu işlem, çalıştığınız verilere bağlı olarak daha çok tercih edilebilir. Günlükleri ölçümlere yönlendirme hakkında bilgi için bkz. [Günlükler Için ölçüm uyarısı](alerts-metric-logs.md).

> [!NOTE]
> Şu anda API sürümü `2020-05-01-preview` ve kaynak merkezli günlük uyarıları için ek ücret alınmaz.  Önizlemede olan özellikler için fiyatlandırma, gelecekte duyurulacak ve faturalandırma başlamadan önce bir bildirim sunulacaktır. Bildirim döneminden sonra yeni API sürümü ve kaynak merkezli günlük uyarılarını kullanmaya devam etmeyi tercih etmeniz gerekir, ilgili ücret üzerinden faturalandırılırsınız.

## <a name="prerequisites"></a>Önkoşullar

Günlük uyarıları Log Analytics verilerde sorgu çalıştırır. İlk olarak, [günlük verilerini toplamaya](../essentials/resource-logs.md) başlamanız ve günlük verilerini sorun için sorgulamanızı gerekir. [Kendi sorgunuzu yazmaya](../logs/log-analytics-tutorial.md)ne bulabileceğinizi veya başladığınızı anlamak için Log Analytics [Uyarı sorgu örnekleri konusunu](../logs/example-queries.md) kullanabilirsiniz.

[Azure Izleme katılımcısı](../roles-permissions-security.md) , günlük uyarılarını oluşturmak, değiştirmek ve güncelleştirmek için gereken ortak bir roldür. Kaynak günlükleri için erişim & sorgu yürütme hakları da gereklidir. Kaynak günlüklerine kısmi erişim sorguları başarısız olabilir veya kısmi sonuçlar döndürebilir. [Azure 'da günlük uyarılarını yapılandırma hakkında daha fazla bilgi edinin](./alerts-log.md).

> [!NOTE]
> Eski [Log Analytics uyarı API 'si](./api-alerts.md)kullanılarak yönetilmek üzere kullanılan Log Analytics için günlük uyarıları. [Geçerli ScheduledQueryRules API 'sine geçiş hakkında daha fazla bilgi edinin](../alerts/alerts-log-api-switch.md).

## <a name="query-evaluation-definition"></a>Sorgu değerlendirmesi tanımı

Günlük arama kuralları koşul tanımının başladığı yer:

- Hangi sorgu çalıştırılacak?
- Sonuçlar nasıl kullanılır?

Aşağıdaki bölümlerde, Yukarıdaki mantığı ayarlamak için kullanabileceğiniz farklı parametreler açıklanır.

### <a name="log-query"></a>Günlük sorgusu
Kuralı değerlendirmek için kullanılan [Log Analytics](../logs/log-analytics-tutorial.md) sorgusu. Bu sorgu tarafından döndürülen sonuçlar, bir uyarının tetiklenip tetiklenmeyeceğini tespit etmek için kullanılır. Sorgunun kapsamı şunları içerebilir:

- Sanal makine gibi belirli bir kaynak.
- Abonelik veya kaynak grubu gibi bir at ölçeği kaynağı.
- [Kaynak arası sorgu](../logs/cross-workspace-query.md#querying-across-log-analytics-workspaces-and-from-application-insights)kullanan birden çok kaynak. 
 
> [!IMPORTANT]
> Uyarı sorgularında en iyi performansı ve sonuçların uygunluğunu sağlamak için kısıtlamalar vardır. [Burada daha fazla bilgi edinebilirsiniz](./alerts-log-query.md).

> [!IMPORTANT]
> Kaynak merkezli ve [çapraz kaynak sorgusu](../logs/cross-workspace-query.md#querying-across-log-analytics-workspaces-and-from-application-insights) yalnızca geçerli scheduledQueryRules API 'si kullanılarak desteklenir. Eski [Log Analytics uyarı API](./api-alerts.md)'sini kullanırsanız, anahtarını değiştirmeniz gerekir. [Geçiş hakkında daha fazla bilgi edinin](./alerts-log-api-switch.md)

#### <a name="query-time-range"></a>Sorgu zaman aralığı

Zaman aralığı, kural koşulu tanımında ayarlanır. Çalışma alanlarında ve Application Insights, **Dönem** olarak adlandırılır. Diğer tüm kaynak türlerinde, **geçersiz kılma sorgu zaman aralığı** olarak adlandırılır.

Log Analytics 'te olduğu gibi, zaman aralığı sorgu verilerini belirtilen aralığa göre sınırlandırır. Sorguda kullanım **dışı komut kullanılsa** bile, zaman aralığı uygulanır.

Örneğin, bir sorgu, zaman aralığı 60 dakika olduğunda ve metin **önce (1D)** olsa bile, bir sorgu 60 dakika tarar. Zaman aralığı ve sorgu zaman filtrelemenin eşleşmesi gerekir. Örnek örnekte, **Dönem**  /  **geçersiz kılma sorgu zaman aralığını** bir güne değiştirmek beklenen şekilde çalışır.

### <a name="measure"></a>Measure

Günlük uyarıları, değerlendirilen sayısal değerlere oturum açma günlüğü kaydeder. İki farklı şeyi ölçebilirsiniz:

#### <a name="count-of-the-results-table-rows"></a>Sonuç tablosu satırlarının sayısı

Sonuç sayısı varsayılan ölçüdür. Windows olay günlükleri, syslog, uygulama özel durumları gibi olaylarla çalışmak için idealdir. Değerlendirilen zaman penceresinde günlük kayıtları gerçekleştiğinde veya gerçekleşmezse tetikler.

Günlük uyarıları, günlükteki verileri algılamaya çalıştığınızda en iyi şekilde çalışır. Günlüklerde veri eksikliklerini algılamaya çalıştığınızda daha az iyi çalışır. Örneğin, sanal makine sinyalde uyarma.

Çalışma alanları ve Application Insights için, seçim **sonuç sayısıyla** **temel alınarak** çağırılır. Diğer tüm kaynak türlerinde, seçim **tablosu satırlarıyla** **Ölçü** denir.

> [!NOTE]
> Günlükler yarı yapılandırılmış veriler olduğundan, bunlar doğal olarak ölçüden daha fazla sallanmış olduğundan, günlüklerde veri eksikliklerini algılamaya çalışırken hatalı ateşlenir ve [ölçüm uyarılarını](alerts-metric-overview.md)kullanmayı göz önünde bulundurmanız gerekir. [Günlükler için ölçüm uyarılarını](alerts-metric-logs.md)kullanarak günlüklerden ölçüm deposuna veri gönderebilirsiniz.

##### <a name="example-of-results-table-rows-count-use-case"></a>Sonuç tablosu satır sayısı kullanım durumu örneği

Uygulamanızın 500 hata koduyla (Iç sunucu hatası) yanıt verdiğini bilmeniz gerekir. Aşağıdaki ayrıntılarla bir uyarı kuralı oluşturursunuz:

- **Sorgulayamadı** 

```Kusto
requests
| where resultCode == "500"
```

- **Zaman aralığı/toplama ayrıntı düzeyi:** 15 dakika
- **Uyarı sıklığı:** 15 dakika
- **Eşik değeri:** 0 ' dan büyük

Ardından, 500 hata kodu ile biten istekler için uyarı kuralları izleyicileri. Sorgu, son 15 dakika içinde 15 dakikada bir çalışır. Bir kayıt bulunursa, uyarıyı tetikler ve yapılandırılan eylemleri tetikler.

#### <a name="calculation-of-measure-based-on-a-numeric-column-such-as-cpu-counter-value"></a>Sayısal bir sütuna (CPU sayaç değeri gibi) göre ölçü hesaplama

Çalışma alanları ve Application Insights için, seçim **ölçüm ölçümüyle** **temel alınarak** çağırılır. Diğer tüm kaynak türlerinde, herhangi bir sayı sütun adı seçimi ile **Ölçü** denir.

### <a name="aggregation-type"></a>Toplama türü

Birden çok kayıt üzerinde gerçekleştirilen ve bunları tek bir sayısal değere toplayan hesaplama. Örnek:
- **Sayı** sorgudaki kayıt sayısını döndürür
- **Average** , tanımlanan ölçü sütun [**toplama ayrıntı**](#aggregation-granularity) düzeyinin ortalamasını döndürür.

Çalışma alanlarında ve Application Insights, yalnızca **ölçüm ölçümü** ölçü türünde desteklenir. Sorgu sonucu, Kullanıcı tanımlı bir toplandıktan sonra sayısal bir değer sağlayan Aggregbir değer adlı bir sütun içermelidir. Diğer tüm kaynak türlerinde, bu ad alanından **toplama türü** seçilidir.

### <a name="aggregation-granularity"></a>Toplama ayrıntı düzeyi

Birden çok kaydı tek bir sayısal değere toplamak için kullanılan aralığı belirler. Örneğin, **5 dakika** belirlediyseniz, kayıtlar belirtilen **toplama türü** kullanılarak 5 dakikalık aralıklarla gruplandırılır.

Çalışma alanlarında ve Application Insights, yalnızca **ölçüm ölçümü** ölçü türünde desteklenir. Sorgu sonucu, sorgu sonuçlarında aralığı ayarlayan [bin ()](/azure/kusto/query/binfunction) içermelidir. Diğer tüm kaynak türlerinde, bu ayarı denetleyen alana **toplama ayrıntı düzeyi** denir.

> [!NOTE]
> As [bin ()](/azure/kusto/query/binfunction) , düzensiz zaman aralıklarına yol açabilir. Uyarı hizmeti, [bin ()](/azure/kusto/query/binfunction) işlevini otomatik olarak [bin_at ()](/azure/kusto/query/binatfunction) işlevine, çalışma zamanında, sabit bir nokta ile sonuçları sağlamak üzere dönüştürür.

### <a name="split-by-alert-dimensions"></a>Uyarı boyutlarına göre Böl

Uyarıları sayı veya dize sütunlarına göre, benzersiz kombinasyonlara gruplandırarak ayrı uyarılarda ayırın. Ölçekte (abonelik veya kaynak grubu kapsamı) kaynak merkezli uyarılar oluştururken Azure Kaynak KIMLIĞI sütununa göre ayırabilirsiniz. Azure Kaynak KIMLIĞI sütununu bölmek, uyarının hedefini belirtilen kaynakla değiştirecek.

Azure Kaynak KIMLIĞI sütununa göre bölme işlemi, birden fazla Azure kaynağında aynı koşulu izlemek istediğinizde önerilir. Örneğin, %80 üzerinde CPU kullanımı için tüm sanal makineleri izleme. Ayrıca, kapsamdaki birden fazla kaynak üzerinde bir koşul istediğinizde, kaynak grubu kapsamındaki en az beş makinenin %80 ' unun üzerinde CPU kullanımına sahip olduğunu izleme gibi bir koşulu istediğiniz zaman bölmemeye karar verebilirsiniz.

Çalışma alanlarında ve Application Insights, yalnızca **ölçüm ölçümü** ölçü türünde desteklenir. Alanı **toplama** olarak adlandırılır. Üç sütun ile sınırlıdır. Sorgudaki sütunlara göre üçten fazla grup bulunması beklenmeyen sonuçlara yol açabilir. Diğer tüm kaynak türlerinde, koşulun **boyutlara göre Böl** bölümüne (altı bölme ile sınırlıdır) göre yapılandırılmıştır.

#### <a name="example-of-splitting-by-alert-dimensions"></a>Uyarı boyutlarına göre bölme örneği

Örneğin, belirli bir kaynak grubunda Web sitenizi/uygulamanızı çalıştıran birden çok sanal makinenin hatalarını izlemek istiyorsunuz. Bunu şu şekilde bir günlük uyarı kuralı kullanarak yapabilirsiniz:

- **Sorgulayamadı** 

    ```Kusto
    // Reported errors
    union Event, Syslog // Event table stores Windows event records, Syslog stores Linux records
    | where EventLevelName == "Error" // EventLevelName is used in the Event (Windows) records
    or SeverityLevel== "err" // SeverityLevel is used in Syslog (Linux) records
    ```

    Çalışma alanları ve **ölçüm ölçümü** uyarı mantığı ile Application Insights kullanılırken, bu satırın sorgu metnine eklenmesi gerekir:

    ```Kusto
    | summarize AggregatedValue = count() by Computer, bin(TimeGenerated, 15m)
    ```

- **Kaynak kimliği sütunu:** _ResourceId (uyarı KURALLARıNDA kaynak kimliği sütununa göre bölme yalnızca abonelikler ve kaynak grupları için kullanılabilir)
- **Boyutlar/toplanan:**
  - Computer = VM1, VM2 (uyarı kuralları tanımındaki filtreleme değerleri şu anda çalışma alanları ve Application Insights için kullanılamaz. Sorgu metninde filtre uygulayın.)
- **Zaman aralığı/toplama ayrıntı düzeyi:** 15 dakika
- **Uyarı sıklığı:** 15 dakika
- **Eşik değeri:** 0 ' dan büyük

Bu kural, bir sanal makinede son 15 dakika içinde hata olayları varsa izler. Her sanal makine ayrı olarak izlenir ve eylemleri ayrı ayrı tetikler.

> [!NOTE]
> Uyarı boyutlarına göre bölme yalnızca geçerli scheduledQueryRules API 'SI için kullanılabilir. Eski [Log Analytics uyarı API](./api-alerts.md)'sini kullanırsanız, anahtarını değiştirmeniz gerekir. [Geçiş hakkında daha fazla bilgi edinin](./alerts-log-api-switch.md). Ölçekteki kaynak merkezli uyarı yalnızca API sürümünde `2020-05-01-preview` ve üzerinde desteklenir.

## <a name="alert-logic-definition"></a>Uyarı mantıksal tanımı

Sonuçları çalıştırmak ve yürütmek üzere sorgu tanımladıktan sonra, uyarı mantığını ve ne zaman tetiklenmesi gerektiğini tanımlamanız gerekir. Aşağıdaki bölümlerde kullanabileceğiniz farklı parametreler açıklanır:

### <a name="threshold-and-operator"></a>Threshold ve işleç

Sorgu sonuçları, eşik ve işleçle karşılaştırılan bir sayıya dönüştürülür.

### <a name="frequency"></a>Sıklık

Sorgunun çalıştırıldığı Aralık. 5 dakikadan bir güne ayarlanabilir. Günlük kayıtlarını kaçırmamak için [sorgu zaman aralığından](#query-time-range) daha küçük veya bu değere eşit olmalıdır.

Örneğin, zaman aralığını 30 dakika, sıklık değerini 1 saat olarak ayarlarsanız.  Sorgu 00:00 adresinde çalışıyorsa, 23:30 ve 00:00 arasındaki kayıtları döndürür. Sorgunun bir sonraki çalışması, 00:30 ve 01:00 arasında kayıt döndüren 01:00 ' dir. 00:00 ile 00:30 arasında oluşturulan tüm kayıtlar hiçbir şekilde değerlendirilmeyecektir.

### <a name="number-of-violations-to-trigger-alert"></a>Uyarı tetiklenecek ihlal sayısı

Uyarı değerlendirme süresini ve bir uyarının tetiklenmesi için gereken başarısızlık sayısını belirtebilirsiniz. Bir uyarının tetiklenmesi için daha iyi bir etki süresi tanımlamanızı sağlar. 

Örneğin, kural [**toplama ayrıntı düzeyi**](#aggregation-granularity) ' 5 dakika ' olarak tanımlanmışsa, son saatin üç hatası (15 dakika) oluştuysa bir uyarı tetikleyebilirsiniz. Bu ayar, uygulama iş ilkeniz tarafından tanımlanır.

## <a name="state-and-resolving-alerts"></a>Uyarıları durum ve çözme

Günlük uyarıları durum bilgisiz. Daha önce tetiklense bile, koşulun her karşılanışında, uyarılar tetiklenir. Tetiklenen uyarılar çözümlenmiyor. [Uyarıyı kapalı olarak işaretleyebilirsiniz](../alerts/alerts-managing-alert-states.md). Ayrıca, bir uyarı kuralı tetiklendikten sonra bir süre tetiklemesini engellemek için eylemleri kapatabilirsiniz.

Çalışma alanlarında ve Application Insights, **uyarıları bastır** olarak adlandırılır. Diğer tüm kaynak türlerinde, bu, **sessiz eylemler** olarak adlandırılır. 

Bu uyarı değerlendirmesi örneğine bakın:

| Saat    | Günlük koşulu değerlendirmesi | Sonuç 
| ------- | ----------| ----------| ------- 
| 00:05 | FALSE | Uyarı tetikleyemedi. Hiçbir eylem çağrılmadı.
| 00:10 | TRUE  | Uyarı ateşlenir ve eylem grupları çağırılır. Yeni uyarı durumu ETKIN.
| 00:15 | TRUE  | Uyarı ateşlenir ve eylem grupları çağırılır. Yeni uyarı durumu ETKIN.
| 00:20 | FALSE | Uyarı tetikleyemedi. Hiçbir eylem çağrılmadı. Uyumsuz Uyarılar durumu ETKIN kalır.

## <a name="pricing-and-billing-of-log-alerts"></a>Günlük uyarılarını fiyatlandırma ve faturalama

Fiyatlandırma bilgileri, [Azure izleyici fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/monitor/)bulunur. Günlük uyarıları kaynak sağlayıcısı altında şu `microsoft.insights/scheduledqueryrules` şekilde listelenir:

- Kaynak grubu ve uyarı özellikleriyle birlikte tam kaynak adıyla gösterilen Application Insights uyarıları günlüğe kaydedin.
- Kaynak grubu ve uyarı özellikleriyle birlikte tam kaynak adıyla gösterilen Log Analytics Uyarıları günlüğe kaydedin; [Scheduledqueryrules API](/rest/api/monitor/scheduledqueryrules)kullanılarak oluşturulduğunda.
- [Eski log ANALYTICS API](./api-alerts.md) 'den oluşturulan günlük uyarıları, [Azure kaynaklarını](../../azure-resource-manager/management/overview.md) izlenemez ve benzersiz kaynak adları zorlanmaz. Bu uyarılar hala `microsoft.insights/scheduledqueryrules` Bu kaynak adlandırma yapısına sahip gizli kaynaklar olarak oluşturulur `<WorkspaceName>|<savedSearchId>|<scheduleId>|<ActionId>` . Eski API 'deki günlük uyarıları, kaynak grubu ve uyarı özellikleriyle birlikte gizli kaynak adıyla birlikte gösterilir.

> [!NOTE]
> Gibi desteklenmeyen kaynak karakterleri `<, >, %, &, \, ?, /` `_` , gizli kaynak adlarında ile değiştirilir ve bu, fatura bilgilerinde de yansıtılacaktır.

> [!NOTE]
> Eski [Log Analytics uyarı API 'si](./api-alerts.md) ve [Log Analytics kayıtlı aramaların ve uyarıların](../insights/solutions.md)eski şablonları kullanılarak yönetilmek üzere kullanılan Log Analytics için günlük uyarıları. [Geçerli ScheduledQueryRules API 'sine geçiş hakkında daha fazla bilgi edinin](../alerts/alerts-log-api-switch.md). Herhangi bir uyarı kuralı yönetiminin, geçiş kararı verinceye ve gizli kaynakları kullanmayabilmeniz için [eski log ANALYTICS API](./api-alerts.md) kullanılarak yapılması gerekir.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure 'da günlük uyarılarında oluşturma](./alerts-log.md)hakkında bilgi edinin.
* [Azure 'da günlük uyarılarında Web kancalarını](../alerts/alerts-log-webhook.md)anlayın.
* [Azure uyarıları](./alerts-overview.md)hakkında bilgi edinin.
* [Log Analytics](../logs/log-query-overview.md)hakkında daha fazla bilgi edinin.