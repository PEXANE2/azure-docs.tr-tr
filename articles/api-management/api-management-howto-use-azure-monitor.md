---
title: Azure API Management’ta yayımlanmış API’leri izleme | Microsoft Docs
description: Azure API Management’ta API’nizi izleme hakkında bilgi edinmek için bu öğreticideki adımları izleyin.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 06/15/2018
ms.author: apimpm
ms.openlocfilehash: b06301ab424a29d8f0e31e8f4dee26265327896b
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79238373"
---
# <a name="monitor-published-apis"></a>Yayımlanan API’leri izleme

Azure İzleyici ile Azure kaynaklarından gelen ölçüm ve günlükleri görselleştirebilir, sorgulayabilir, yönlendirebilir, arşivleyebilir ve bunlar üzerinde işlem uygulayabilirsiniz.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Etkinlik günlüklerini görüntüleme
> * Tanılama günlüklerini görüntüleme
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

* Kapasite: APIM hizmetlerinizi yükseltme/düşürme konusunda karar vermenize yardımcı olur. Ölçüm, dakika başına yayılır ve raporlama zamanındaki ağ geçidi kapasitesini yansıtır. Ölçüm, CPU ile bellek kullanımı gibi ağ geçidi kaynakları temel alınarak hesaplanan 0-100 aralığında değişir.
* Toplam Ağ Geçidi İsteği: dönem içindeki API isteklerinin sayısı. 
* Başarılı Ağ Geçidi İstekleri: 304, 307 ve 301’den küçük herhangi bir kod (örneğin, 200) dahil olmak üzere başarılı HTTP yanıt kodları almış API isteklerinin sayısı.
* Başarısız Ağ Geçidi İstekleri: 400 ve 500’den büyük herhangi bir kod dahil olmak üzere hatalı HTTP yanıt kodları almış API isteklerinin sayısı.
* Yetkisiz Ağ Geçidi İstekleri: 401, 403 ve 429 dahil olmak üzere HTTP yanıt kodları almış API isteklerinin sayısı.
* Diğer Ağ Geçidi İstekleri: Yukarıdaki kategorilerin hiçbirine ait olmayan HTTP yanıt kodları (örneğin, 418) almış API isteklerinin sayısı.

![ölçüm grafiği](./media/api-management-azure-monitor/apim-monitor-metrics.png)

Ölçümlere erişmek için:

1. Sayfanın alt kısmındaki menüden **Ölçümler**’i seçin.

    ![metrics](./media/api-management-azure-monitor/api-management-metrics-blade.png)

1. Açılan listeden ilgilendiğiniz ölçümleri seçin. Örneğin, **İstekler**. 
1. Grafikte, API çağrılarının toplam sayısı gösterilmektedir.
1. Grafik, **İstekler** ölçümünün boyutları kullanılarak filtrelenebilir. Örneğin, Ekle **filtresine**tıklayın, **Arka Uç Yanıt Kodu'nu**seçin, değer olarak 500 girin. Grafik, API arka ucunda başarısız olan isteklerin sayısını gösterir.   

## <a name="set-up-an-alert-rule-for-unauthorized-request"></a>Yetkisiz istekler için uyarı kuralı ayarlama

Ölçümlere ve etkinlik günlüklerine göre uyarı alacak şekilde yapılandırmanızı ayarlayabilirsiniz. Azure İzleyici, tetiklendiğinde aşağıdaki işlemleri yapmak üzere bir uyarı yapılandırmanıza olanak tanır:

* E-posta bildirimi gönderme
* Web kancası çağırma
* Bir Azure Mantıksal Uygulamasını çağırma

Uyarıları yapılandırmak için:

1. Sayfanın altındaki menü çubuğundan **Uyarılar'ı** seçin.

    ![alerts](./media/api-management-azure-monitor/alert-menu-item.png)

2. Bu uyarı için **Yeni uyarı kuralını** tıklatın.
3. Koşul **Ekle'ye**tıklayın.
4. Sinyal türüaçılırken **Ölçümler'i** seçin.
5. İzlenecek sinyal olarak **Yetkisiz Ağ Geçidi İsteği'ni** seçin.

    ![alerts](./media/api-management-azure-monitor/signal-type.png)

6. **Yapılsinyal mantığı** görünümünde, uyarının tetiklendiği bir eşik belirtin ve **Bitti'yi**tıklatın.

    ![alerts](./media/api-management-azure-monitor/threshold.png)

7. Varolan bir Eylem Grubu seçin veya yeni bir grup oluşturun. Aşağıdaki örnekte, yöneticilere bir e-posta gönderilecektir. 

    ![alerts](./media/api-management-azure-monitor/action-details.png)

8. Bir ad, uyarı kuralının açıklaması sağlayın ve önem düzeyini seçin. 
9. **Uyarı kuralını oluştur'a**basın.
10. Şimdi, API anahtarı olmadan Konferans API'sini aramayı deneyin. Uyarı tetiklenecek ve yöneticilere e-posta gönderilecektir. 

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

## <a name="diagnostic-logs"></a>Tanılama Günlükleri

Tanılama günlükleri, denetim ve sorun giderme konularında önemli işlem ve hatalar hakkında zengin bilgiler sağlar. Tanılama günlükleri, etkinlik günlüklerinden farklıdır. Etkinlik günlükleri, Azure kaynaklarınız üzerinde gerçekleştirilen işlemler hakkında bilgi sağlar. Tanılama günlükleri, kaynağınızın kendisi tarafından gerçekleştirilen işlemler hakkında bilgi sağlar.

Tanılama günlüklerini yapılandırmak için:

1. APIM hizmet örneğinizi seçin.
2. **Tanılama ayarları**'na tıklayın.

    ![tanılama günlükleri](./media/api-management-azure-monitor/api-management-diagnostic-logs-blade.png)

3. **Tanılamayı aç**’a tıklayın. Tanılama günlüklerini ölçümlerle birlikte bir depolama hesabına arşivleyebilir, bir Etkinlik Hub'ına aktarabilir veya Azure Monitor günlüklerine gönderebilirsiniz. 

API Management şu anda her bir girdi aşağıdaki şemayı içerecek şekilde tek tek API isteğiyle ilgili tanılama günlükleri (saatlik olarak toplanır) sağlar:

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
| time | date-time | Ağ geçidi nin isteği işlemeye başladığı zaman damgası |
| operationName | string | 'Microsoft.ApiManagement/GatewayLogs' sabit değeri |
| category | string | 'GatewayLogs' sabit değeri |
| durationMs | integer | Tam olarak gönderilen yanıta kadar istek alındığı andan milisaniye sayısı. ClienTime, cacheTime ve backendTime içerir. |
| callerIpAddress | string | İlk Ağ Geçidi çağıranın (bir aracı olabilir) IP adresi |
| correlationId | string | API Management tarafından atanmış benzersiz http isteği tanımlayıcısı |
| location | string | İsteği işleyen Ağ Geçidinin bulunduğu Azure bölgesinin adı |
| httpStatusCodeCategory | string | Http yanıtı durum kodunun kategorisi: Başarılı (301 veya daha küçük ya da 304 ya da 307), Yetkisiz (401, 403, 429), Hatalı (400, 500 ve 600 arası), Diğer |
| resourceId | string | API Yönetimi kaynağının kimliği /ABONELIKLER/\<abonelik\<>/KAYNAK GRUPLARI/ kaynak grubu>/Providers/MICROSOFT. APIMANAGEMENT/SERVICE/\<isim> |
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
| elapsed | integer | Ağ geçidinin isteği aldığı gün ile hatanın oluştuğu an arasında geçen milisaniye sayısı | 
| source | string | İlke veya işleme iç işleyicisinin adı hataya neden oldu | 
| scope | string | Hataya neden olan ilkeyi içeren ilke belgesinin kapsamı | 
| section | string | Hataya neden olan ilkeyi içeren ilke belgesinin bölümü | 
| reason | string | Hata nedeni | 
| message | string | Hata iletisi | 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Etkinlik günlüklerini görüntüleme
> * Tanılama günlüklerini görüntüleme
> * API'nizin ölçümlerini görüntüleme
> * API'niz yetkisiz çağrılar aldığında bir uyarı kuralı ayarlama

Sonraki öğreticiye ilerleyin:

> [!div class="nextstepaction"]
> [İzleme çağrıları](api-management-howto-api-inspector.md)
