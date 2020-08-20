---
title: Azure Izleyici 'de özel ölçümler (Önizleme)
description: Azure Izleyici 'de özel ölçümler ve bunların nasıl modellendiği hakkında bilgi edinin.
author: ancav
ms.author: ancav
services: azure-monitor
ms.topic: conceptual
ms.date: 06/01/2020
ms.subservice: metrics
ms.openlocfilehash: 73c9b2bf8cf88ca5e8576c451c9d9ac5f0eae8a3
ms.sourcegitcommit: 628be49d29421a638c8a479452d78ba1c9f7c8e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88639911"
---
# <a name="custom-metrics-in-azure-monitor-preview"></a>Azure Izleyici 'de özel ölçümler (Önizleme)

Azure 'da kaynak ve uygulama dağıtırken, performans ve sistem durumu hakkında Öngörüler kazanmak için telemetri toplamaya başlamak isteyeceksiniz. Azure bazı ölçümleri sizin için kullanıma hazır hale getirir. Bu ölçümler [Standart veya platform](./metrics-supported.md)olarak adlandırılır. Ancak, bunlar doğası ile sınırlıdır. 

Daha derin Öngörüler sağlamak için bazı özel performans göstergeleri veya işletmeye özgü ölçümler toplamak isteyebilirsiniz. Bu **özel** ölçümler, uygulama Telemetriniz, Azure kaynaklarınızda çalışan bir aracı, hatta dışarıdan bir izleme sistemi ve doğrudan Azure izleyici 'ye gönderilebilir. Azure Izleyici 'de yayımlandıktan sonra, Azure tarafından oluşturulan standart ölçümleri kullanarak Azure kaynaklarınızın ve uygulamalarınızın özel ölçümlerine gözatabilir, bunları sorgulayabilir ve uyarı verebilirsiniz.

Azure Izleyici özel ölçümleri genel önizlemede geçerli. 

## <a name="methods-to-send-custom-metrics"></a>Özel ölçümleri gönderme yöntemleri

Özel ölçümler, çeşitli yöntemler aracılığıyla Azure Izleyici 'ye gönderilebilir:
- Azure Application Insights SDK 'sını kullanarak uygulamanızı işaretleyin ve Azure Izleyici 'ye özel telemetri gönderin. 
- Azure Izleyici aracısını (Önizleme) [Windows veya Linux Azure sanal](azure-monitor-agent-overview.md) makinesine yükleyip Azure izleyici ölçümlerine performans sayaçlarını göndermek için bir [veri toplama kuralı](data-collection-rule-azure-monitor-agent.md) kullanın.
- [Azure](collect-custom-metrics-guestos-resource-manager-vm.md)sanal makinenize, [sanal makine ölçek KÜMESINE](collect-custom-metrics-guestos-resource-manager-vmss.md), [klasik VM](collect-custom-metrics-guestos-vm-classic.md)'ye veya [Klasik Cloud Services](collect-custom-metrics-guestos-vm-cloud-service-classic.md) Windows Azure tanılama (wad) uzantısını yükleyip performans sayaçlarını Azure izleyici 'ye gönderin. 
- Azure Izleme çıkış eklentisini kullanarak Azure Linux sanal makinenize etkileyen bir Azure [Data telegraf Aracısı](collect-custom-metrics-linux-telegraf.md) 'nı yükleyip ölçümleri gönderin.
- Özel ölçümleri [doğrudan Azure izleyici REST API](./metrics-store-custom-rest-api.md)gönderin `https://<azureregion>.monitoring.azure.com/<AzureResourceID>/metrics` .

## <a name="pricing-model-and-retention"></a>Fiyatlandırma modeli ve bekletme

Özel ölçümler ve ölçüm sorguları için faturalandırma özelliğinin ne zaman etkinleştirileceğini hakkında ayrıntılı bilgi edinmek için [Azure izleyici fiyatlandırma sayfasını](https://azure.microsoft.com/pricing/details/monitor/) kontrol edin. Özel ölçümler ve ölçüm sorguları dahil olmak üzere tüm ölçümler için belirli fiyat ayrıntıları bu sayfada bulunabilir. Özet olarak, Azure Izleyici ölçüm deposuna standart ölçümleri (Platform ölçümleri) alma maliyeti yoktur, ancak özel ölçümler genel kullanılabilirliği girerken maliyette yer alacak. Ölçüm API sorguları, ücretlendirilir.

Özel ölçümler, [Platform ölçümleriyle aynı süre](data-platform-metrics.md#retention-of-metrics)boyunca tutulur. 

> [!NOTE]  
> Azure Izleyici 'ye Application Insights SDK aracılığıyla gönderilen ölçümler, alınan günlük verileri olarak faturalandırılır. Bunlar yalnızca [özel ölçüm boyutlarında Application Insights özelliği etkinleştir ' in](../app/pre-aggregated-metrics-log-metrics.md#custom-metrics-dimensions-and-pre-aggregation) seçili olması halinde ek ölçüm ücretlerine uygulanır. Bu onay kutusu, daha karmaşık bir uyarı sağlamak için özel ölçüm API 'sini kullanarak Azure Izleyici ölçümleri veritabanına veri gönderir.  Bölgenizdeki [Application Insights fiyatlandırma modeli](../app/pricing.md#pricing-model) ve [fiyatları](https://azure.microsoft.com/pricing/details/monitor/)hakkında daha fazla bilgi edinin.


## <a name="how-to-send-custom-metrics"></a>Özel ölçümleri gönderme

Azure Izleyici 'ye özel ölçümler gönderdiğinizde, bildirilen her bir veri noktası veya değer aşağıdaki bilgileri içermelidir.

### <a name="authentication"></a>Kimlik Doğrulaması
Azure Izleyici 'ye özel ölçümler göndermek için, ölçümü gönderen varlığın isteğin **taşıyıcı** üst bilgisinde geçerli bir Azure Active Directory (Azure AD) belirteci olması gerekir. Geçerli bir taşıyıcı belirteç edinmenin birkaç desteklenen yolu vardır:
1. [Azure kaynakları Için Yönetilen kimlikler](../../active-directory/managed-identities-azure-resources/overview.md). Azure kaynağına bir kimlik verir (örneğin, VM). Yönetilen Hizmet Kimliği (MSI), belirli işlemleri gerçekleştirmek için kaynak izinleri vermek üzere tasarlanmıştır. Bir örnek, bir kaynağın kendisiyle ilgili ölçümleri yaymalarına izin verir. Bir kaynağa veya MSI öğesine başka bir kaynakta **Izleme ölçümleri yayımcı** izinleri verilebilir. Bu izinle, MSI diğer kaynaklar için ölçümleri de yayabilir.
2. [Azure AD hizmet sorumlusu](../../active-directory/develop/app-objects-and-service-principals.md). Bu senaryoda, bir Azure AD uygulamasına veya hizmetine bir Azure kaynağı hakkında ölçümleri yayma izinleri atanabilir.
Azure Izleyici, isteğin kimliğini doğrulamak için Azure AD ortak anahtarlarını kullanarak uygulama belirtecini doğrular. Mevcut **Izleme ölçümleri yayımcı** rolü zaten bu izne sahip. Azure portal kullanılabilir. Hizmet sorumlusu, için özel ölçümleri yaydığı kaynaklara bağlı olarak, gereken kapsamda **Izleme ölçümleri yayımcı** rolü verilebilir. Örnek olarak bir abonelik, kaynak grubu veya belirli bir kaynaktır.

> [!TIP]  
> Özel ölçümleri yayan bir Azure AD belirteci istediğinizde, belirtecin istendiği hedef kitle veya kaynak için olduğundan emin olun `https://monitoring.azure.com/` . Sondaki '/' öğesini eklediğinizden emin olun.

### <a name="subject"></a>Konu
Bu özellik, için özel ölçümün hangi Azure Kaynak KIMLIĞIYLE bildirileceğini yakalar. Bu bilgiler, yapılmakta olan API çağrısının URL 'SI ile kodlanacak. Her API yalnızca tek bir Azure kaynağı için ölçüm değerleri gönderebilir.

> [!NOTE]  
> Bir kaynak grubunun veya aboneliğin kaynak KIMLIĞI için özel ölçümleri yayalamazsınız.


### <a name="region"></a>Region
Bu özellik, için ölçüm yaymakta olduğunuz kaynağın hangi Azure bölgesine dağıtıldığını yakalar. Ölçümler, kaynağın dağıtıldığı bölge ile aynı Azure Izleyici bölgesel uç noktasına verilmelidir. Örneğin, Batı ABD dağıtılan bir sanal makinenin özel ölçümleri WestUS bölgesel Azure Izleyici uç noktasına gönderilmelidir. Bölge bilgileri de API çağrısının URL 'SI olarak kodlanır.

> [!NOTE]  
> Genel Önizleme sırasında, özel ölçümler yalnızca Azure bölgelerinin bir alt kümesinde kullanılabilir. Desteklenen bölgelerin bir listesi, bu makalenin sonraki bölümlerinde belgelenmiştir.
>
>

### <a name="timestamp"></a>Zaman damgası
Azure Izleyici 'ye gönderilen her veri noktasının bir zaman damgasıyla işaretlenmesi gerekir. Bu zaman damgası, ölçüm değerinin ölçüldüğü veya toplandığı tarih/saati yakalar. Azure Izleyici, zaman damgalarına sahip ölçüm verilerini geçmişte ve 5 dakika içinde en fazla 20 dakika olacak şekilde kabul eder. Zaman damgası ISO 8601 biçiminde olmalıdır.

### <a name="namespace"></a>Ad Alanı
Ad alanları, benzer ölçümleri birlikte sınıflandırmanız veya gruplandırmanız için bir yoldur. Ad alanlarını kullanarak, farklı Öngörüler veya performans göstergeleri toplayabilecek ölçüm grupları arasında yalıtım elde edebilirsiniz. Örneğin, uygulamanızı profilini oluşturan bellek kullanım ölçümlerini izleyen **contosomemoryölçümler** adlı bir ad alanı olabilir. **Contosoapptransaction** adlı başka bir ad alanı, uygulamanızdaki Kullanıcı işlemleri hakkında tüm ölçümleri izleyebilir.

### <a name="name"></a>Ad
**Ad** , bildirilen ölçümün adıdır. Genellikle, ne ölçülerin tanımlanmasına yardımcı olmak için ad açıklayıcı bir yoldur. Bir örnek, belirli bir VM 'de kullanılan bellek baytlarının sayısını ölçen bir ölçümdür. **Kullanılmakta olan bellek baytları**gibi bir ölçüm adı olabilir.

### <a name="dimension-keys"></a>Boyut anahtarları
Boyut, toplanmakta olan ölçüm hakkında ek özellikler tanımlamaya yardımcı olan bir anahtar veya değer çiftidir. Ek özellikleri kullanarak, daha derin Öngörüler sağlayan ölçüm hakkında daha fazla bilgi toplayabilirsiniz. Örneğin, kullanılmakta olan **bellek baytları** , bir VM 'deki her işlemin kaç baytlık bellek kullandığını yakalayan **işlem** adlı bir boyut anahtarına sahip olabilir. Bu anahtarı kullanarak, belleğe özgü işlemlerin ne kadar iyi kullanıldığını görmek veya bellek kullanımına göre ilk beş işlemi tanımlamak için ölçüyü filtreleyebilirsiniz.
Boyutlar isteğe bağlıdır, tüm ölçümler boyutlara sahip olamaz. Özel bir ölçüm en fazla 10 boyut içerebilir.

### <a name="dimension-values"></a>Boyut değerleri
Ölçüm veri noktasını bildirirken, raporlanan ölçümdeki her boyut anahtarı için karşılık gelen bir boyut değeri vardır. Örneğin, sanal makinenizde ContosoApp tarafından kullanılan belleği raporlamak isteyebilirsiniz:

* Ölçüm adı, **kullanılmakta olan bellek baytı**olacaktır.
* Boyut anahtarı **işlem**olurdu.
* Boyut değeri **ContosoApp.exe**.

Ölçüm değeri yayımlarken, her boyut anahtarı için yalnızca tek bir boyut değeri belirtebilirsiniz. VM 'de birden çok işlem için aynı bellek kullanımını topluyorsanız, bu zaman damgası için birden çok ölçüm değeri rapor edebilirsiniz. Her ölçüm değeri, **işlem** boyutu anahtarı için farklı bir boyut değeri belirler.
Boyutlar isteğe bağlıdır, tüm ölçümler boyutlara sahip olamaz. Ölçüm gönderisi boyut anahtarlarını tanımlıyorsa, karşılık gelen boyut değerleri zorunludur.

### <a name="metric-values"></a>Ölçüm değerleri
Azure Izleyici, tüm ölçümleri tek dakikalık ayrıntı düzeyi aralıklarında depolar. Belirli bir dakika boyunca bir metriğin birkaç kez örnekleme yapması gerekebileceğini anladık. Bu ölçümlere örnek olarak CPU kullanımı verilebilir. Ya da birçok ayrı olay için ölçülmesi gerekebilir. Bir örnek, oturum açma işlemi gecikmelerinin bir örneğidir. Azure Izleyici 'de için yaymanız ve ödediğiniz ham değer sayısını sınırlandırmak için, değerleri yerel olarak ön toplayabilir ve aktarabilirsiniz:

* **Min**: dakika boyunca tüm örnek ve ölçülerden gözlenen en düşük değer.
* **Max**: dakika boyunca tüm örnek ve ölçülerden gözlenen maksimum değer.
* **Sum**: dakika boyunca tüm örnek ve ölçülerden gözlenen tüm değerlerin toplamı.
* **Sayı**: dakika boyunca gerçekleştirilen örnek sayısı ve ölçümler.

Örneğin, bir dakika boyunca uygulamanıza verilen dört oturum açma işlemi varsa, her biri için sonuç ölçülen gecikme süresi şu şekilde olabilir:

|İşlem 1|İşlem 2|İşlem 3|İşlem 4|
|---|---|---|---|
|7 MS|4 MS|13 MS|16 MS|
|

Daha sonra, Azure Izleyici 'de ortaya çıkan ölçüm yayını aşağıdaki gibi olacaktır:
* En az: 4
* En fazla: 16
* Toplam: 40
* Sayı: 4

Uygulamanız yerel olarak önceden toplanırsa ve her bir ayrık örneği veya olayı koleksiyon üzerine hemen yaymaya ihtiyaç duyuyorsa, ham ölçü değerlerini yayabilirsiniz. Örneğin, uygulamanızda her oturum açma işlemi gerçekleştiğinde, Azure Izleyici 'de yalnızca tek bir ölçümle bir ölçüm yayımlarsınız. Bu nedenle, 12 MS geçen bir oturum açma işlemi için ölçüm yayını aşağıdaki gibi olacaktır:
* Min: 12
* En fazla: 12
* Toplam: 12
* Sayı: 1

Bu işlemle, belirli bir dakika boyunca aynı ölçüm Plus boyut birleşimine ait birden çok değer yayabilirsiniz. Daha sonra Azure Izleyici, belirli bir dakika boyunca sunulan tüm ham değerleri alır ve bunları birlikte toplar.

### <a name="sample-custom-metric-publication"></a>Örnek özel ölçüm yayını
Aşağıdaki örnekte, bir sanal makine için ölçüm ad alanı **bellek profili** altında **kullanımda olan bellek baytları** adlı bir özel ölçüm oluşturursunuz. Ölçümün **işlem**adlı tek bir boyutu vardır. Verilen zaman damgası için, ölçüm değerlerini iki farklı işlem için yayıyoruz:

```json
{
    "time": "2018-08-20T11:25:20-7:00",
    "data": {

      "baseData": {

        "metric": "Memory Bytes in Use",
        "namespace": "Memory Profile",
        "dimNames": [
          "Process"        ],
        "series": [
          {
            "dimValues": [
              "ContosoApp.exe"
            ],
            "min": 10,
            "max": 89,
            "sum": 190,
            "count": 4
          },
          {
            "dimValues": [
              "SalesApp.exe"
            ],
            "min": 10,
            "max": 23,
            "sum": 86,
            "count": 4
          }
        ]
      }
    }
  }
```
> [!NOTE]  
> Application Insights, tanılama uzantısı ve etkileyen XData telegraf Aracısı zaten doğru bölgesel uç noktaya karşı ölçüm değerlerini göstermek üzere yapılandırılmıştır ve her bir egörev için önceki tüm özellikleri taşır.
>
>

## <a name="custom-metric-definitions"></a>Özel Ölçüm tanımları
Azure Izleyici 'nin yayınlanmadan önce özel bir ölçümü önceden tanımlamanız gerekmez. Yayımlanan her ölçüm veri noktası ad alanı, ad ve boyut bilgilerini içerir. Bu nedenle, bir özel ölçüm Azure Izleyici 'ye ilk kez yayıldığından, ölçüm tanımı otomatik olarak oluşturulur. Bu ölçüm tanımı, ölçümün ölçüm tanımlarına göre yayıldığını tüm kaynaklar üzerinde bulunabilir.

> [!NOTE]  
> Azure Izleyici, özel bir ölçüm için **birimleri** tanımlamayı henüz desteklememektedir.

## <a name="using-custom-metrics"></a>Özel ölçümleri kullanma
Özel ölçümler Azure Izleyici 'ye gönderildikten sonra, Azure portal aracılığıyla bunlara gözatabilir ve bunları Azure Izleyici REST API 'Leri aracılığıyla sorgulayabilirsiniz. Ayrıca, belirli koşullar karşılandığında size bildirimde bulunan uyarılar oluşturabilirsiniz.

> [!NOTE]
> Özel ölçümleri görüntülemek için bir okuyucu veya katkıda bulunan rolü olmanız gerekir.

### <a name="browse-your-custom-metrics-via-the-azure-portal"></a>Özel ölçümlerinize Azure portal aracılığıyla gözatamazsınız
1.    [Azure portalına](https://portal.azure.com) gidin.
2.    **İzleyici** bölmesini seçin.
3.    **Ölçümler**’i seçin.
4.    Özel ölçümler oluşturmuş olduğunuz bir kaynağı seçin.
5.    Özel ölçümünüzün ölçüm ad alanını seçin.
6.    Özel ölçümü seçin.

## <a name="supported-regions"></a>Desteklenen bölgeler
Genel Önizleme sırasında, özel ölçümleri yayımlama özelliği yalnızca Azure bölgelerinin bir alt kümesinde bulunabilir. Bu kısıtlama, ölçümlerin yalnızca desteklenen bölgelerden birindeki kaynaklar için yayımlanalabileceği anlamına gelir. Aşağıdaki tabloda, özel ölçümler için desteklenen Azure bölgelerinin kümesi listelenmektedir. Ayrıca, bu bölgelerdeki kaynaklara yönelik ölçümlerin yayımlanacağı ilgili uç noktaları listeler:

|Azure bölgesi |Bölgesel uç nokta ön eki|
|---|---|
| **ABD ve Kanada** | |
|Orta Batı ABD | https: \/ /westcentralus.Monitoring.Azure.com |
|Batı ABD 2       | https: \/ /westus2.Monitoring.Azure.com |
|Orta Kuzey ABD | https: \/ /northcentralus.Monitoring.Azure.com
|Orta Güney ABD| https: \/ /southcentralus.Monitoring.Azure.com |
|Central US      | https: \/ /centralus.Monitoring.Azure.com |
|Orta Kanada | https: \/ /canadacentral.Monitoring.Azure.com |
|Doğu ABD| https: \/ /eastus.Monitoring.Azure.com |
|Doğu ABD 2 | https: \/ /eastus2.Monitoring.Azure.com |
| **Avrupa** | |
|Kuzey Avrupa    | https: \/ /northeurope.Monitoring.Azure.com |
|West Europe     | https: \/ /westeurope.Monitoring.Azure.com |
|Güney Birleşik Krallık | https: \/ /uksouth.Monitoring.Azure.com
|Orta Fransa | https: \/ /francecentral.Monitoring.Azure.com |
| **Afrika** | |
|Güney Afrika Kuzey | https: \/ /southafricanorth.Monitoring.Azure.com |
| **Asya** | |
|Orta Hindistan | https: \/ /centralindia.Monitoring.Azure.com |
|Doğu Avustralya | https: \/ /australiaeast.Monitoring.Azure.com |
|Doğu Japonya | https: \/ /japaneast.Monitoring.Azure.com |
|Güneydoğu Asya  | https: \/ /SoutheastAsia.Monitoring.Azure.com |
|Doğu Asya | https: \/ /eastasia.Monitoring.Azure.com |
|Güney Kore - Orta   | https: \/ /koreacentral.Monitoring.Azure.com |

## <a name="latency-and-storage-retention"></a>Gecikme süresi ve depolama saklama

Yeni bir ölçüm veya bir ölçüme eklenen yeni bir boyutun eklenmesi, en fazla 2 ila 3 dakika sürer. Sistemde bir kez, verilerin 30 saniyeden az %99 ' de bir süre içinde görüntülenmesi gerekir. 

Bir ölçümü siler veya bir boyut kaldırırsanız, değişiklik sistemden silinecek bir aya kadar sürebilir.

## <a name="quotas-and-limits"></a>Kotalar ve sınırlar
Azure Izleyici, özel ölçümler üzerinde aşağıdaki kullanım sınırlarını uygular:

|Kategori|Sınır|
|---|---|
|Etkin zaman serisi/abonelikler/bölge|50,000|
|Ölçüm başına Boyut anahtarları|10|
|Ölçüm ad alanları, ölçüm adları, Boyut anahtarları ve boyut değerleri için dize uzunluğu|256 karakter|

Etkin bir zaman serisi, son 12 saat içinde yayımlanmış ölçüm değerleri olan ölçüm, boyut anahtarı veya boyut değerinin herhangi bir benzersiz birleşimi olarak tanımlanır.

## <a name="next-steps"></a>Sonraki adımlar
Farklı hizmetlerden özel ölçümler kullanın: 
 - [Sanal makineler](collect-custom-metrics-guestos-resource-manager-vm.md)
 - [Sanal makine ölçek kümesi](collect-custom-metrics-guestos-resource-manager-vmss.md)
 - [Azure sanal makineleri (klasik)](collect-custom-metrics-guestos-vm-classic.md)
 - [Telegraf aracısını kullanan Linux sanal makinesi](collect-custom-metrics-linux-telegraf.md)
 - [REST API](./metrics-store-custom-rest-api.md)
 - [Klasik Cloud Services](collect-custom-metrics-guestos-vm-cloud-service-classic.md)
 

