---
title: Azure API Management’ta yayımlanmış API’leri izleme | Microsoft Docs
description: Azure API Management’ta API’nizi izleme hakkında bilgi edinmek için bu öğreticideki adımları izleyin.
services: api-management
author: vladvino
manager: cfowler
ms.service: api-management
ms.workload: mobile
ms.custom: mvc
ms.topic: tutorial
ms.date: 06/15/2018
ms.author: apimpm
ms.openlocfilehash: 7f6c7a651e133122dab86d6ed81572f239718b43
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2020
ms.locfileid: "86243248"
---
# <a name="monitor-published-apis"></a>Yayımlanan API’leri izleme

Azure İzleyici ile Azure kaynaklarından gelen ölçüm ve günlükleri görselleştirebilir, sorgulayabilir, yönlendirebilir, arşivleyebilir ve bunlar üzerinde işlem uygulayabilirsiniz.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Etkinlik günlüklerini görüntüleme
> * Kaynak günlüklerini görüntüle
> * API'nizin ölçümlerini görüntüleme 
> * API'niz yetkisiz çağrılar aldığında bir uyarı kuralı ayarlama

Aşağıdaki videoda, Azure İzleyici'yi kullanarak API Management’ı izleme işlemi gösterilmektedir. 

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Monitor-API-Management-with-Azure-Monitor/player]

## <a name="prerequisites"></a>Önkoşullar

+ [Azure API Management terminolojisini](api-management-terminology.md) öğrenin.
+ Şu hızlı başlangıcı tamamlayın: [Azure API Management örneği oluşturma](get-started-create-service-instance.md).
+ Ayrıca, şu öğreticiyi tamamlayın: [İlk API'nizi içeri aktarma ve yayımlama](import-and-publish.md).

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="view-metrics-of-your-apis"></a>API'lerinizin ölçümlerini görüntüleme

API Management, dakika başı yaydığı ölçümlerle API’lerinizin durumu hakkında neredeyse gerçek zamanlı görünürlük sağlar. En sık kullanılan iki ölçüm aşağıda verilmiştir. Tüm kullanılabilir ölçümlerin bir listesi için bkz. [desteklenen ölçümler](../azure-monitor/platform/metrics-supported.md#microsoftapimanagementservice).

* Kapasite: APıM hizmetlerinizi yükseltme/eski sürüme düşürme hakkında kararlar almanıza yardımcı olur. Ölçüm, dakika başına yayılır ve raporlama zamanındaki ağ geçidi kapasitesini yansıtır. Ölçüm, CPU ile bellek kullanımı gibi ağ geçidi kaynakları temel alınarak hesaplanan 0-100 aralığında değişir.
* İstekler: APıM hizmetleriniz aracılığıyla API trafiğini çözümlemenize yardımcı olur. Ölçüm, dakika başına yayılır ve yanıt kodları, konum, ana bilgisayar adı ve hatalar dahil olmak üzere boyutlara sahip ağ geçidi isteklerinin sayısını raporlar. 

> [!IMPORTANT]
> Aşağıdaki ölçümler 2019 Mayıs 'Tan itibaren kullanımdan kaldırılmıştır ve 2023 Ağustos 'ta kullanımdan kaldırılacak: toplam ağ geçidi Isteği, başarılı ağ geçidi Istekleri, yetkisiz ağ geçidi Istekleri, başarısız ağ geçidi istekleri, diğer ağ geçidi Istekleri. Lütfen eşdeğer işlevselliği sağlayan Istek ölçüsüne geçiş yapın.

![ölçüm grafiği](./media/api-management-azure-monitor/apim-monitor-metrics.png)

Ölçümlere erişmek için:

1. Sayfanın alt kısmındaki menüden **Ölçümler**’i seçin.

    ![metrics](./media/api-management-azure-monitor/api-management-metrics-blade.png)

2. Açılan listeden ilgilendiğiniz ölçümleri seçin. Örneğin, **istekler**. 
3. Grafikte, API çağrılarının toplam sayısı gösterilmektedir.
4. Grafik, **istek** ölçümünün boyutları kullanılarak filtrelenebilir. Örneğin, **Filtre Ekle**' ye tıklayın, **arka uç yanıt kodu**' nu seçin, değer olarak 500 girin. Artık grafik, API arka ucunda başarısız olan isteklerin sayısını gösterir.   

## <a name="set-up-an-alert-rule-for-unauthorized-request"></a>Yetkisiz istekler için uyarı kuralı ayarlama

Ölçümlere ve etkinlik günlüklerine göre uyarı alacak şekilde yapılandırmanızı ayarlayabilirsiniz. Azure İzleyici, tetiklendiğinde aşağıdaki işlemleri yapmak üzere bir uyarı yapılandırmanıza olanak tanır:

* E-posta bildirimi gönderme
* Web kancası çağırma
* Bir Azure Mantıksal Uygulamasını çağırma

Uyarıları yapılandırmak için:

1. Sayfanın alt kısmındaki menü çubuğundan **Uyarılar** ' ı seçin.

    ![alerts](./media/api-management-azure-monitor/alert-menu-item.png)

2. Bu uyarı için **Yeni bir uyarı kuralına** tıklayın.
3. **Koşul Ekle**' ye tıklayın.
4. Sinyal türü açılan kutusunda **ölçümler** ' i seçin.
5. İzlenecek sinyal olarak **yetkisiz ağ geçidi isteği** ' ni seçin.

    ![alerts](./media/api-management-azure-monitor/signal-type.png)

6. **Sinyal mantığını Yapılandır** görünümünde, uyarının tetiklenme sonrasında bir eşik belirtin ve **bitti**' ye tıklayın.

    ![alerts](./media/api-management-azure-monitor/threshold.png)

7. Mevcut bir eylem grubu seçin veya yeni bir tane oluşturun. Aşağıdaki örnekte, yöneticilere bir e-posta gönderilir. 

    ![alerts](./media/api-management-azure-monitor/action-details.png)

8. Uyarı kuralının adını, açıklamasını girin ve önem derecesini seçin. 
9. **Uyarı kuralı oluştur**' a basın.
10. Şimdi, bir API anahtarı olmadan konferans API 'sini çağırmayı deneyin. Uyarı tetiklenir ve e-posta yöneticilere gönderilir. 

## <a name="activity-logs"></a>Etkinlik Günlükleri

Etkinlik günlükleri, API Management hizmetleriniz üzerinde gerçekleştirilen işlemler hakkında bilgi sağlar. Etkinlik günlüklerini kullanarak, API Management hizmetleriniz üzerinde gerçekleştirilen herhangi bir yazma işlemi (PUT, POST, DELETE) için "ne, kim ve ne zaman" sorularını yanıtlayabilirsiniz.

> [!NOTE]
> Etkinlik günlükleri, okuma (GET) işlemlerini ya da Azure portalında gerçekleştirilen veya özgün Yönetim API’leri kullanan işlemleri içermez.

API Management hizmetinizdeki etkinlik günlüklerine veya Azure İzleyici’deki tüm Azure kaynaklarınızın günlüklerine erişebilirsiniz. 

![etkinlik günlükleri](./media/api-management-azure-monitor/apim-monitor-activity-logs.png)

Etkinlik günlüklerini görüntülemek için:

1. APIM hizmet örneğinizi seçin.
2. **Etkinlik günlüğü**’ne tıklayın.

    ![etkinlik günlüğü](./media/api-management-azure-monitor/api-management-activity-logs-blade.png)

3. İstenen filtre kapsamını seçin ve **Uygula**’ya tıklayın.

## <a name="resource-logs"></a>Kaynak günlükleri

Kaynak günlükleri, denetim ve sorun giderme amacıyla önemli işlemler ve hatalar hakkında zengin bilgiler sağlar. Kaynak günlükleri etkinlik günlüklerinden farklıdır. Etkinlik günlükleri, Azure kaynaklarınız üzerinde gerçekleştirilen işlemlere yönelik öngörüler sağlar. Kaynak günlükleri, kaynağınızın gerçekleştirdiği işlemlere ilişkin öngörüler sağlar.

Kaynak günlüklerini yapılandırmak için:

1. APIM hizmet örneğinizi seçin.
2. **Tanılama ayarları**'na tıklayın.

    ![Kaynak günlükleri](./media/api-management-azure-monitor/api-management-diagnostic-logs-blade.png)

3. **Tanılamayı aç**’a tıklayın. Kaynak günlüklerini, ölçümlerle birlikte bir depolama hesabına arşivleyebilir, bunları bir olay hub 'ına bağlayabilir veya Azure Izleyici günlüklerine gönderebilirsiniz. 

API Management Şu anda her bir girdiyle aşağıdaki şemaya sahip her bir API isteği hakkında kaynak günlükleri (toplu olarak saatlik) sağlar:

```json
{  
    "isRequestSuccess" : "",
    "time": "",
    "operationName": "",
    "category": "",
    "durationMs": ,
    "callerIpAddress": "",
    "correlationId": "",
    "location": "",
    "httpStatusCodeCategory": "",
    "resourceId": "",
    "properties": {   
        "method": "", 
        "url": "", 
        "clientProtocol": "", 
        "responseCode": , 
        "backendMethod": "", 
        "backendUrl": "", 
        "backendResponseCode": ,
        "backendProtocol": "",  
        "requestSize": , 
        "responseSize": , 
        "cache": "", 
        "cacheTime": "", 
        "backendTime": , 
        "clientTime": , 
        "apiId": "",
        "operationId": "", 
        "productId": "", 
        "userId": "", 
        "apimSubscriptionId": "", 
        "backendId": "",
        "lastError": { 
            "elapsed" : "", 
            "source" : "", 
            "scope" : "", 
            "section" : "" ,
            "reason" : "", 
            "message" : ""
        } 
    }      
}  
```

| Özellik  | Tür | Açıklama |
| ------------- | ------------- | ------------- |
| isRequestSuccess | boole | HTTP isteği, 2xx veya 3xx aralığı içinde yanıt durum koduyla tamamlandıysa true olur |
| time | date-time | Ağ geçidinin başladığı zaman zaman damgası isteği işleme |
| operationName | string | 'Microsoft.ApiManagement/GatewayLogs' sabit değeri |
| category | string | 'GatewayLogs' sabit değeri |
| durationMs | integer | Kısa süre sonra gönderilen istek sayısı dolduğunda, ağ geçidi tarafından alınan istek süresi (milisaniye). ClienTime, cacheTime ve backendTime bilgilerini içerir. |
| callerIpAddress | string | İlk Ağ Geçidi çağıranın (bir aracı olabilir) IP adresi |
| correlationId | string | API Management tarafından atanmış benzersiz http isteği tanımlayıcısı |
| location | string | İsteği işleyen Ağ Geçidinin bulunduğu Azure bölgesinin adı |
| httpStatusCodeCategory | string | Http yanıtı durum kodunun kategorisi: Başarılı (301 veya daha küçük ya da 304 ya da 307), Yetkisiz (401, 403, 429), Hatalı (400, 500 ve 600 arası), Diğer |
| resourceId | string | /SUBSCRIPTIONS/ \<subscription> /ResourceGroups/ \<resource-group> /providers/microsoftAPI Management kaynağının kimliği. ıMANAGEMENTPACK/HIZMET/\<name> |
| properties | nesne | Geçerli isteğin özellikleri |
| method | string | Gelen isteğin HTTP yöntemi |
| url | string | Gelen isteğin URL’si |
| clientProtocol | string | Gelen isteğin HTTP protokolü sürümü |
| responseCode | integer | Bir istemciye gönderilen HTTP yanıtının durum kodu |
| backendMethod | string | Arka uca gönderilen isteğin HTTP yöntemi |
| backendUrl | string | Arka uca gönderilen isteğin URL’si |
| backendResponseCode | integer | Arka uçtan alınan HTTP yanıtının kodu |
| backendProtocol | string | Arka uca gönderilen isteğin HTTP protokolü sürümü | 
| requestSize | integer | İstek işlenirken bir istemciden alınan bayt sayısı | 
| responseSize | integer | İstek işlenirken bir istemciye gönderilen bayt sayısı | 
| cache | string | API Management önbelleğinin istek işlemeye katılım durumu (örn. hit, miss, none) | 
| cacheTime | integer | Genel API Management önbelleği GÇ (bağlanma, gönderme ve alma bayt’ları) için harcanan milisaniye sayısı | 
| backendTime | integer | Genel arka uç GÇ (bağlanma, gönderme ve alma bayt’ları) için harcanan milisaniye sayısı | 
| clientTime | integer | Genel istemci G/Ç (bağlanma, gönderme ve alma bayt’ları) için harcanan milisaniye sayısı | 
| apiId | string | Geçerli istek için API varlığı tanımlayıcısı | 
| operationId | string | Geçerli istek için işlem varlığı tanımlayıcısı | 
| productId | string | Geçerli istek için ürün varlığı tanımlayıcısı | 
| userId | string | Geçerli istek için kullanıcı varlığı tanımlayıcısı | 
| apimSubscriptionId | string | Geçerli istek için abonelik varlığı tanımlayıcısı | 
| backendId | string | Geçerli istek için arka uç varlığı tanımlayıcısı | 
| LastError | nesne | Son istek işleme hatası | 
| elapsed | integer | Ağ geçidinin isteği aldığı ve hatanın gerçekleştiği sırada geçen milisaniye sayısı | 
| kaynak | dize | İlke veya işleme iç işleyicisinin adı hataya neden oldu | 
| scope | string | Hataya neden olan ilkeyi içeren ilke belgesinin kapsamı | 
| section | string | Hataya neden olan ilkeyi içeren ilke belgesinin bölümü | 
| reason | string | Hata nedeni | 
| message | string | Hata iletisi | 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Etkinlik günlüklerini görüntüleme
> * Kaynak günlüklerini görüntüle
> * API'nizin ölçümlerini görüntüleme
> * API'niz yetkisiz çağrılar aldığında bir uyarı kuralı ayarlama

Sonraki öğreticiye ilerleyin:

> [!div class="nextstepaction"]
> [İzleme çağrıları](api-management-howto-api-inspector.md)
