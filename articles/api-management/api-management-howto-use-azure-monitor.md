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
ms.openlocfilehash: bee93cf84f4beda0684127102942447630219881
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82128838"
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

## <a name="prerequisites"></a>Ön koşullar

+ [Azure API Management terminolojisini](api-management-terminology.md) öğrenin.
+ Şu hızlı başlangıcı tamamlayın: [Azure API Management örneği oluşturma](get-started-create-service-instance.md).
+ Ayrıca, şu öğreticiyi tamamlayın: [İlk API'nizi içeri aktarma ve yayımlama](import-and-publish.md).

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="view-metrics-of-your-apis"></a>API'lerinizin ölçümlerini görüntüleme

API Management, dakika başı yaydığı ölçümlerle API’lerinizin durumu hakkında neredeyse gerçek zamanlı görünürlük sağlar. Bazı kullanılabilir ölçümlerin özeti aşağıda verilmiştir:

* Kapasite: APıM hizmetlerinizi yükseltme/eski sürüme düşürme hakkında kararlar almanıza yardımcı olur. Ölçüm, dakika başına yayılır ve raporlama zamanındaki ağ geçidi kapasitesini yansıtır. Ölçüm, CPU ile bellek kullanımı gibi ağ geçidi kaynakları temel alınarak hesaplanan 0-100 aralığında değişir.
* Toplam Ağ Geçidi İsteği: dönem içindeki API isteklerinin sayısı. 
* Başarılı Ağ Geçidi İstekleri: 304, 307 ve 301’den küçük herhangi bir kod (örneğin, 200) dahil olmak üzere başarılı HTTP yanıt kodları almış API isteklerinin sayısı.
* Başarısız Ağ Geçidi İstekleri: 400 ve 500’den büyük herhangi bir kod dahil olmak üzere hatalı HTTP yanıt kodları almış API isteklerinin sayısı.
* Yetkisiz Ağ Geçidi İstekleri: 401, 403 ve 429 dahil olmak üzere HTTP yanıt kodları almış API isteklerinin sayısı.
* Diğer Ağ Geçidi İstekleri: Yukarıdaki kategorilerin hiçbirine ait olmayan HTTP yanıt kodları (örneğin, 418) almış API isteklerinin sayısı.

![ölçüm grafiği](./media/api-management-azure-monitor/apim-monitor-metrics.png)

Ölçümlere erişmek için:

1. Sayfanın alt kısmındaki menüden **Ölçümler**’i seçin.

    ![metrics](./media/api-management-azure-monitor/api-management-metrics-blade.png)

1. Açılan listeden ilgilendiğiniz ölçümleri seçin. Örneğin, **istekler**. 
1. Grafikte, API çağrılarının toplam sayısı gösterilmektedir.
1. Grafik, **istek** ölçümünün boyutları kullanılarak filtrelenebilir. Örneğin, **Filtre Ekle**' ye tıklayın, **arka uç yanıt kodu**' nu seçin, değer olarak 500 girin. Artık grafik, API arka ucunda başarısız olan isteklerin sayısını gösterir.   

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
| resourceId | string | /SUBSCRIPTIONS/\<aboneliğinin>/RESOURCEGROUPS/\<Resource-Group>/providers/microsoftAPI Management kaynağının kimliği. ICLAIMANA/HIZMET/\<ad> |
| properties | object | Geçerli isteğin özellikleri |
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
| LastError | object | Son istek işleme hatası | 
| elapsed | integer | Ağ geçidinin isteği aldığı ve hatanın gerçekleştiği sırada geçen milisaniye sayısı | 
| source | string | İlke veya işleme iç işleyicisinin adı hataya neden oldu | 
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
