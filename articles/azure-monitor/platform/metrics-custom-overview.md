---
title: Azure Monitör'de özel ölçümler
description: Azure Monitor'da özel ölçümler ve bunların nasıl modellendiği hakkında bilgi edinin.
author: ancav
services: azure-monitor
ms.topic: conceptual
ms.date: 09/09/2019
ms.author: ancav
ms.subservice: metrics
ms.openlocfilehash: e104877ef641a87eac4ba19bb3342c6e029bf80c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80294590"
---
# <a name="custom-metrics-in-azure-monitor"></a>Azure Monitör'de özel ölçümler

Azure'da kaynakları ve uygulamaları dağıttıkça, performansları ve sağlık durumları hakkında bilgi edinmek için telemetri toplamaya başlamak isteyebilirsiniz. Azure, bazı ölçümleri kutunun dışında kullanıma sunar. Bu ölçümlerstandart veya platform olarak adlandırılır. Ancak, doğaları sınırlı. Daha derin öngörüler sağlamak için bazı özel performans göstergeleri veya işletmeye özgü ölçümler toplamak isteyebilirsiniz.
Bu **özel** ölçümler, Azure kaynaklarınızla çalışan bir aracı veya hatta dışarıdan gelen bir izleme sistemi olan ve doğrudan Azure Monitor'a gönderilen uygulama telemetriniz aracılığıyla toplanabilir. Azure Monitor'da yayımlandıktan sonra, Azure kaynaklarınız ve uygulamalarınız için özel ölçümlere Azure tarafından yayılan standart ölçümlerle birlikte göz atabilir, sorgulayabilir ve uyarıda bulunabilirsiniz.

## <a name="send-custom-metrics"></a>Özel ölçümler gönderme
Özel ölçümler Azure Monitor'a çeşitli yöntemlerle gönderilebilir:
- Azure Application Insights SDK'yı kullanarak uygulamanızı enstrüman edin ve Azure Monitor'a özel telemetri gönderin. 
- [Azure VM'](collect-custom-metrics-guestos-resource-manager-vm.md)nizde Windows Azure Diagnostics (WAD) uzantısını, [sanal makine ölçek setine,](collect-custom-metrics-guestos-resource-manager-vmss.md) [klasik VM'](collect-custom-metrics-guestos-vm-classic.md)ye veya [klasik Bulut Hizmetlerine](collect-custom-metrics-guestos-vm-cloud-service-classic.md) yükleyin ve Azure Monitor' a performans sayaçları gönderin. 
- Azure Linux VM'nize [InfluxData Telegraf aracısını](collect-custom-metrics-linux-telegraf.md) yükleyin ve Azure Monitor çıktı eklentisini kullanarak ölçümler gönderin.
- Özel ölçümleri doğrudan Azure Monitor REST `https://<azureregion>.monitoring.azure.com/<AzureResourceID>/metrics` [API'sine](../../azure-monitor/platform/metrics-store-custom-rest-api.md)gönderin.

Azure Monitor'a özel ölçümler gönderdiğinde, bildirilen her veri noktası veya değeri aşağıdaki bilgileri içermelidir.

### <a name="authentication"></a>Kimlik doğrulaması
Azure Monitor'a özel ölçümler göndermek için, metriyi gönderen **varlığın,** talebin Taşıyıcı üstbilgisinde geçerli bir Azure Etkin Dizin (Azure AD) belirteci gerekir. Geçerli bir taşıyıcı belirteci edinmenin desteklenen birkaç yolu vardır:
1. [Azure kaynakları için yönetilen kimlikler.](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) VM gibi bir Azure kaynağına kimlik verir. Yönetilen Hizmet Kimliği (MSI), kaynaklara belirli işlemleri yürütmek için izin vermek üzere tasarlanmıştır. Bir örnek, kaynağın kendisi hakkında ölçümler yontmasını sağlamaktır. Bir kaynak veya MSI' ye başka bir kaynakta **İzleme Ölçümleri Yayımcısı** izinleri verilebilir. Bu izinle, MSI diğer kaynaklar için de ölçümler yatabilir.
2. [Azure AD Hizmet Sorumlusu.](https://docs.microsoft.com/azure/active-directory/develop/app-objects-and-service-principals) Bu senaryoda, bir Azure AD uygulaması veya hizmeti, bir Azure kaynağı hakkında ölçümler oluşturmak için izinler atanabilir.
İsteğin kimliğini doğrulamak için Azure Monitor, Azure AD ortak anahtarlarını kullanarak uygulama belirteci doğrular. Varolan **İzleme Ölçümleri Yayımcısı** rolü zaten bu izne sahiptir. Azure portalında kullanılabilir. Hizmet ilkesi, hangi kaynaklar için özel ölçümler yayan kaynaklara bağlı olarak, gerekli kapsamda **İzleme Ölçümleri Yayımcısı** rolü verilebilir. Örnekler bir abonelik, kaynak grubu veya belirli bir kaynaktır.

> [!NOTE]  
> Özel ölçümler yatsın diye bir Azure AD belirteci istediğinizde, belirteç için `https://monitoring.azure.com/`istenen hedef kitlenin veya kaynağın . '/''

### <a name="subject"></a>Özne
Bu özellik, özel ölçümiçin bildirilen Hangi Azure kaynak kimliğini yakalar. Bu bilgiler, yapılan API çağrısının URL'sinde kodlanır. Her API yalnızca tek bir Azure kaynağı için metrik değerler gönderebilir.

> [!NOTE]  
> Bir kaynak grubunun veya aboneliğin kaynak kimliğine karşı özel ölçümler yarayamayapamazsınız.
>
>

### <a name="region"></a>Bölge
Bu özellik, metrik leri yaydığınız kaynağın hangi Azure bölgesinde dağıtılmış olduğunu yakalar. Ölçümler, kaynağın dağıtıldığı bölgeyle aynı Azure Monitor bölgesel bitiş noktasına yayılmalıdır. Örneğin, Batı ABD'de dağıtılan bir VM için özel ölçümler WestUS bölgesel Azure Monitor bitiş noktasına gönderilmelidir. Bölge bilgileri, API çağrısının URL'sinde de kodlanır.

> [!NOTE]  
> Genel önizleme sırasında, özel ölçümler yalnızca Azure bölgelerinin bir alt kümesinde kullanılabilir. Desteklenen bölgelerin listesi bu makalenin sonraki bir bölümünde belgelenmiştir.
>
>

### <a name="timestamp"></a>Zaman damgası
Azure Monitor'a gönderilen her veri noktası bir zaman damgası ile işaretlenmelidir. Bu zaman damgası, metrik değerin ölçüldüğü veya toplandığı DateTime'ı yakalar. Azure Monitor, geçmişte 20 dakikaya, gelecekte 5 dakikaya kadar zaman damgalı metrik verileri kabul eder. Zaman damgası ISO 8601 formatında olmalıdır.

### <a name="namespace"></a>Ad Alanı
Ad alanları, benzer ölçümleri kategorilere ayırmanın veya gruplandırmanın bir yoludur. Ad alanlarını kullanarak, farklı öngörüler veya performans göstergeleri toplayabilen ölçüm grupları arasında yalıtım elde edebilirsiniz. Örneğin, uygulamanızın profilini çıkaran bellek kullanım ölçümlerini izleyen **contosomemorymetrics** adlı bir ad alanınız olabilir. **Contosoapptransaction** adı verilen başka bir ad alanı, uygulamanızdaki kullanıcı hareketleri ile ilgili tüm ölçümleri izleyebilir.

### <a name="name"></a>Adı
**Ad,** bildirilen metrin adıdır. Genellikle, ad ölçülenleri belirlemeye yardımcı olacak kadar açıklayıcıdır. Belirli bir VM'de kullanılan bellek baytlarının sayısını ölçen bir ölçüm e-örnektir. **Kullanılan Memory Bytes**gibi bir metrik adı olabilir.

### <a name="dimension-keys"></a>Boyut tuşları
Boyut, toplanan metrikle ilgili ek özellikleri açıklamaya yardımcı olan bir anahtar veya değer çiftidir. Ek özellikleri kullanarak, daha derin öngörüler sağlayan metrik hakkında daha fazla bilgi toplayabilirsiniz. Örneğin, **Kullanımdaki Bellek Baytları** ölçümü, bir VM'deki her işlemde kaç bayt bellek tüketir yakalar **İşlem** adlı bir boyut anahtarına sahip olabilir. Bu anahtarı kullanarak, belleğe özgü işlemlerin ne kadar kullandığını görmek veya bellek kullanımına göre ilk beş işlemi tanımlamak için ölçümü filtreleyebilirsiniz.
Boyutlar isteğe bağlıdır, tüm ölçümlerin boyutları olmayabilir. Özel bir metrik en fazla 10 boyuta sahip olabilir.

### <a name="dimension-values"></a>Boyut değerleri
Bir metrik veri noktası raporederken, raporlanan metrikteki her boyut anahtarı için karşılık gelen bir boyut değeri vardır. Örneğin, ContosoApp tarafından kullanılan belleği VM'nizde bildirmek isteyebilirsiniz:

* Metrik adı **Kullanımda Memory Bytes**olacaktır.
* Boyut anahtarı **Süreç**olacaktır.
* Boyut değeri **ContosoApp.exe**olacaktır.

Bir metrik değer yayımlarken, boyut anahtarı başına yalnızca tek bir boyut değeri belirtebilirsiniz. VM'deki birden çok işlem için aynı bellek kullanımını toplarsanız, bu zaman damgası için birden çok metrik değer bildirebilirsiniz. Her metrik **değer, İşlem** boyutu anahtarı için farklı bir boyut değeri belirtir.
Boyutlar isteğe bağlıdır, tüm ölçümlerin boyutları olmayabilir. Bir metrik gönderi boyut anahtarlarını tanımlıyorsa, karşılık gelen boyut değerleri zorunludur.

### <a name="metric-values"></a>Ölçüm değerleri
Azure Monitor tüm ölçümleri bir dakikalık parçalı aralıklarla depolar. Belirli bir dakika içinde bir ölçümün birkaç kez örnekalınması gerekebileceğini anlıyoruz. Bir örnek CPU kullanımıdır. Ya da birçok ayrık olaylar için ölçülmesi gerekebilir. Oturum açma işlemi gecikmeleri buna bir örnektir. Azure Monitor'da yatıp ödemeniz gereken ham değer sayısını sınırlamak için, değerleri yerel olarak önceden toplayabilir ve yayabilirsiniz:

* **Min**: Dakika boyunca tüm numune ve ölçümlerden en az gözlenen değerdir.
* **Max**: Dakika boyunca tüm numune ve ölçümlerden en fazla görülen değerdir.
* **Sum**: Dakika boyunca tüm numune ve ölçümlerden gözlenen tüm değerlerin toplamı.
* **Sayım**: Dakika içinde alınan numune ve ölçüm sayısı.

Örneğin, belirli bir dakika içinde uygulamanızda 4 oturum açma işlemi varsa, her biri için ortaya çıkan ölçülen gecikmeler aşağıdaki gibi olabilir:

|İşlem 1|İşlem 2|İşlem 3|İşlem 4|
|---|---|---|---|
|7 ms|4 ms|13 ms|16 ms|
|

Ardından Azure Monitor'a gelen metrik yayın aşağıdaki gibi olacaktır:
* Dk: 4
* Maksimum: 16
* Toplam: 40
* Sayı: 4

Uygulamanız yerel olarak önceden toplayamıyorsa ve her ayrık örneği veya olayı toplama dan hemen sonra yakılması gerekiyorsa, ham ölçü değerlerini yatabilirsiniz. Örneğin, uygulamanızda her oturum açma işlemi gerçekleştiğinde, yalnızca tek bir ölçümle Azure Monitor'a bir metrik yayımlarsınız. Yani 12 ms alan bir oturum açma işlemi için metrik yayın aşağıdaki gibi olacaktır:
* Dk: 12
* Maksimum: 12
* Toplam: 12
* Sayı: 1

Bu işlemle, belirli bir dakika içinde aynı metrik artı boyut kombinasyonu için birden çok değer yarayabilirsiniz. Azure Monitor daha sonra belirli bir dakika boyunca yayılan tüm ham değerleri alır ve bunları bir araya toplar.

### <a name="sample-custom-metric-publication"></a>Örnek özel metrik yayın
Aşağıdaki örnekte, sanal bir makine için metrik ad alanı Bellek **Profili** altında **Kullanımda Bellek Baytları** adlı özel bir metrik oluşturursunuz. Metrin **Süreç**adı verilen tek bir boyutu vardır. Verilen zaman damgası için, iki farklı işlem için metrik değerler yontmaktadır:

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
> Uygulama Öngörüleri, tanılama uzantısı ve InfluxData Telegraf aracısı, metrik değerleri doğru bölgesel uç noktaya göre yakılacak ve her emisyonda önceki tüm özellikleri taşıyacak şekilde yapılandırılmıştır.
>
>

## <a name="custom-metric-definitions"></a>Özel metrik tanımlar
Azure Monitor'da özel bir metnin yayımlanmasından önce önceden tanımlamaya gerek yoktur. Yayınlanan her metrik veri noktası ad alanı, ad ve boyut bilgilerini içerir. Böylece, Azure Monitor'a ilk kez özel bir metrik yayımlanır, metrik tanım otomatik olarak oluşturulur. Bu metrik tanım, metrik tanımları aracılığıyla karşısına çıkarılan herhangi bir kaynakta keşfedilebilir.

> [!NOTE]  
> Azure Monitör, özel bir metrik için **Birimleri** tanımlamayı henüz desteklemez.

## <a name="using-custom-metrics"></a>Özel ölçümleri kullanma
Azure Monitor'a özel ölçümler gönderildikten sonra Azure portalı üzerinden bunlara göz atabilir ve Azure Monitor REST API'leri aracılığıyla sorgulayabilirsiniz. Ayrıca, belirli koşullar yerine getirildiğinde sizi bilgilendirmek için bu uyarılar da oluşturabilirsiniz.

> [!NOTE]
> Özel ölçümleri görüntülemek için okuyucu veya katkıda bulunan rol olmanız gerekir.

### <a name="browse-your-custom-metrics-via-the-azure-portal"></a>Azure portalı üzerinden özel ölçümlerinize göz atın
1.    [Azure portalına](https://portal.azure.com)gidin.
2.    **Monitör** bölmesini seçin.
3.    **Ölçümler**’i seçin.
4.    Karşı özel ölçümler yaydığınız bir kaynak seçin.
5.    Özel ölçümüz için ölçümler ad alanını seçin.
6.    Özel metrik seçin.

## <a name="supported-regions"></a>Desteklenen bölgeler
Genel önizleme sırasında, özel ölçümleryayımlabilme özelliği yalnızca Azure bölgelerinin bir alt kümesinde kullanılabilir. Bu kısıtlama, ölçümlerin yalnızca desteklenen bölgelerden birinde kaynaklar için yayımlanabileceği anlamına gelir. Aşağıdaki tabloda, özel ölçümler için desteklenen Azure bölgeleri kümesi listelenir. Ayrıca, bu bölgelerdeki kaynaklar için ölçümlerin yayımlanması gereken ilgili uç noktaları da listeler:

|Azure bölgesi |Bölgesel uç nokta öneki|
|---|---|
| **ABD ve Kanada** | |
|Orta Batı ABD | https:\//westcentralus.monitoring.azure.com/ |
|Batı ABD 2       | https:\//westus2.monitoring.azure.com/ |
|Orta Kuzey ABD | https:\//northcentralus.monitoring.azure.com
|Orta Güney ABD| https:\//southcentralus.monitoring.azure.com/ |
|Orta ABD      | https:\//centralus.monitoring.azure.com |
|Orta Kanada | https:\//canadacentral.monitoring.azure.comc
|Doğu ABD| https:\//eastus.monitoring.azure.com/ |
| **Avrupa** | |
|Kuzey Avrupa    | https:\//northeurope.monitoring.azure.com/ |
|Batı Avrupa     | https:\//westeurope.monitoring.azure.com/ |
|Güney Birleşik Krallık | https:\//uksouth.monitoring.azure.com
|Orta Fransa | https:\//francecentral.monitoring.azure.com |
| **Afrika** | |
|Güney Afrika Kuzey | https:\//southafricanorth.monitoring.azure.com
| **Asya** | |
|Orta Hindistan | https:\//centralindia.monitoring.azure.com
|Doğu Avustralya | https:\//australiaeast.monitoring.azure.com
|Doğu Japonya | https:\//japaneast.monitoring.azure.com
|Güneydoğu Asya  | https:\//southeastasia.monitoring.azure.com |
|Doğu Asya | https:\//eastasia.monitoring.azure.com
|Güney Kore - Orta   | https:\//koreacentral.monitoring.azure.com


## <a name="quotas-and-limits"></a>Kotalar ve sınırlar
Azure Monitor, özel ölçümlere aşağıdaki kullanım limitlerini uygular:

|Kategori|Sınır|
|---|---|
|Etkin zaman serileri/abonelikleri/bölgesi|50.000|
|Metrik başına boyut anahtarları|10|
|Metrik ad alanları, metrik adlar, boyut anahtarları ve boyut değerleri için dize uzunluğu|256 karakter|

Etkin zaman serisi, son 12 saat içinde yayınlanmış metrik değerlere sahip metrik, boyut anahtarı veya boyut değerinin benzersiz bir birleşimi olarak tanımlanır.

## <a name="next-steps"></a>Sonraki adımlar
Farklı hizmetlerden özel ölçümler kullanın: 
 - [Virtual Machines](collect-custom-metrics-guestos-resource-manager-vm.md)
 - [Sanal makine ölçek kümesi](collect-custom-metrics-guestos-resource-manager-vmss.md)
 - [Azure Sanal Makineler (klasik)](collect-custom-metrics-guestos-vm-classic.md)
 - [Telegraf aracısını kullanan Linux Sanal Makine](collect-custom-metrics-linux-telegraf.md)
 - [REST API](../../azure-monitor/platform/metrics-store-custom-rest-api.md)
 - [Klasik Bulut Hizmetleri](collect-custom-metrics-guestos-vm-cloud-service-classic.md)
 
