---
title: Azure API Yönetimini Azure Uygulama Öngörüleriyle Tümleştir
titleSuffix: Azure API Management
description: Azure Uygulama Öngörüleri'nde Azure API Yönetimi'nden etkinlikleri nasıl günlüğe kaydedip görüntüleyebilirsiniz öğrenin.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: erikre
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 06/20/2018
ms.author: apimpm
ms.openlocfilehash: 48a83fad3395f6ecf06fb1f1ba95aa1b06a53431
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81259145"
---
# <a name="how-to-integrate-azure-api-management-with-azure-application-insights"></a>Azure API Management'ı Azure Application Insights ile Tümleştirme

Azure API Yönetimi, web geliştiricileri için genişletilebilir bir hizmet olan Azure Application Insights ile kolay tümleştirme olanağı sağlar. Bu kılavuz, bu tür tümleştirmenin her adımını gözden geçirir ve API Yönetimi hizmet örneğiniz üzerindeki performans etkisini azaltmaya yönelik stratejileri açıklar.

## <a name="prerequisites"></a>Ön koşullar

Bu kılavuzu izlemek için bir Azure API Yönetimi örneğine sahip olmanız gerekir. Eğer yoksa, önce [öğreticiyi](get-started-create-service-instance.md) tamamlayın.

## <a name="create-an-azure-application-insights-instance"></a>Azure Uygulama Öngörüleri örneği oluşturma

Azure Uygulama Öngörülerini kullanabilmeniz için önce hizmetin bir örneğini oluşturmanız gerekir.

1. Azure **portalını** açın ve **Uygulama Öngörüleri'ne**gidin.  
    ![App Insights oluşturmak](media/api-management-howto-app-insights/apim-app-insights-instance-1.png)  
2. **+ Ekle**'ye tıklayın.  
    ![App Insights oluşturmak](media/api-management-howto-app-insights/apim-app-insights-instance-2.png)  
3. Formu doldurun. **Uygulama Türü**olarak **Genel'i** seçin.
4. **Oluştur'u**tıklatın.

## <a name="create-a-connection-between-azure-application-insights-and-azure-api-management-service-instance"></a>Azure Uygulama Öngörüleri ve Azure API Yönetimi hizmeti örneği arasında bağlantı oluşturma

1. **Azure portalındaki** **Azure API Yönetimi hizmet örneğinize** gidin.
2. Soldaki menüden **Uygulama Öngörüleri'ni** seçin.
3. **+ Ekle**'ye tıklayın.  
    ![App Insights logger](media/api-management-howto-app-insights/apim-app-insights-logger-1.png)  
4. Daha önce oluşturulmuş **Uygulama Öngörüleri** örneğini seçin ve kısa bir açıklama sağlayın.
5. **Oluştur'u**tıklatın.
6. Enstrümantasyon anahtarına sahip bir Azure Application Insights kaydedicisi oluşturdunuz. Şimdi listede görünmelidir.  
    ![App Insights logger](media/api-management-howto-app-insights/apim-app-insights-logger-2.png)  

> [!NOTE]
> Sahnenin arkasında, API Yönetimi örneğinde Uygulama İstatistikleri örneğinin Enstrümantasyon Anahtarını içeren bir [Logger](https://docs.microsoft.com/rest/api/apimanagement/2019-12-01/logger/createorupdate) varlığı oluşturulur.

## <a name="enable-application-insights-logging-for-your-api"></a>API'niz için Uygulama Öngörüleri günlüğe kaydetmeyi etkinleştirin

1. **Azure portalındaki** **Azure API Yönetimi hizmet örneğinize** gidin.
2. Soldaki menüden **API'ler** seçeneğini belirleyin.
3. Bu durumda **Demo Konferansı API'niz,** API'nizi tıklatın.
4. Üst çubuktan **Ayarlar** sekmesine gidin.
5. **Tanılama Günlükleri** bölümüne gidin.  
    ![App Insights logger](media/api-management-howto-app-insights/apim-app-insights-api-1.png)  
6. **Etkinleştir** kutusunu işaretleyin.
7. **Hedef** açılır düşüşünde ekli kaydedicinizi seçin.
8. Örnekleme olarak **100** **(%)** olarak giriş yapın ve **Her Zaman günlük hataları** onay kutusunu işaretleyin.
9. **Kaydet**'e tıklayın.

> [!WARNING]
> Gövde alanının **Ilk baytlarında** varsayılan değer **0'ın** geçersiz kılınması API'lerinizin performansını önemli ölçüde azaltabilir.

> [!NOTE]
> Sahnenin arkasında, API düzeyinde 'applicationinsights' adlı bir [Tanılama](https://docs.microsoft.com/rest/api/apimanagement/2019-12-01/diagnostic/createorupdate) varlığı oluşturulur.

| Ayar adı                        | Değer türü                        | Açıklama                                                                                                                                                                                                                                                                                                                                      |
|-------------------------------------|-----------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Etkinleştirme                              | boole                           | Bu API'nin günlüğe kaydetmesinin etkin olup olmadığını belirtir.                                                                                                                                                                                                                                                                                                |
| Hedef                         | Azure Uygulama Öngörüleri logger | Kullanılacak Azure Application Insights logger'ı belirtir                                                                                                                                                                                                                                                                                           |
| Örnekleme (%)                        | decimal                           | 0 ile 100 (yüzde) arasında değerler. <br/> Taleplerin yüzde kaçının Azure Application Insights'a günlüğe kaydolacağını belirtir. %0 örnekleme sıfır istek günlüğe kaydedilmiş anlamına gelirken, %100 örnekleme tüm istekler günlüğe kaydedilmiş anlamına gelir. <br/> Bu ayar, günlüğe kaydetme isteklerinin Azure Uygulama Öngörüleri'ne ilişkin performans etkilerini azaltmak için kullanılır (aşağıdaki bölüme bakın). |
| Her zaman günlük hataları                   | boole                           | Bu ayar seçilirse, **Örnekleme** ayarından bağımsız olarak tüm hatalar Azure Uygulama Öngörüleri'ne günlüğe kaydedilir.                                                                                                                                                                                                                  |
| Temel Seçenekler: Üstbilgiler              | list                              | İstek ler ve yanıtlar için Azure Application Insights'a günlüğe kaydedilecek üstbilgileri belirtir.  Varsayılan: üstbilgi günlüğe kaydedilmez.                                                                                                                                                                                                             |
| Temel Seçenekler: Vücudun ilk bayt  | integer                           | İstek ve yanıtlar için gövdenin ilk baytlarının Azure Application Insights'a kaç kez günlüğe kaydedildiğini belirtir.  Varsayılan: gövde günlüğe kaydedilmez.                                                                                                                                                                                                    |
| Gelişmiş Seçenekler: Verbosity         |                                   | Ayrıntılı lık düzeyini belirtir. Yalnızca daha yüksek önem düzeyine sahip özel izlemeler günlüğe kaydedilir. Varsayılan: Bilgi.                                                                                                                                                                                                                               |
| Gelişmiş Seçenekler: Frontend İsteği  |                                   | *Ön uç isteklerinin* Azure Application Insights'a kaydedilip günlüğe kaydedilmeyeceğini belirtir. *Frontend isteği,* Azure API Yönetimi hizmetine gelen bir istektir.                                                                                                                                                                        |
| Gelişmiş Seçenekler: Frontend Yanıt |                                   | *Ön uç yanıtlarının* Azure Application Insights'a kaydedilip günlüğe kaydedilmeyeceğini belirtir. *Frontend yanıtı,* Azure API Yönetimi hizmetinden giden bir yanıttır.                                                                                                                                                                   |
| Gelişmiş Seçenekler: Arka Uç İsteği   |                                   | *Arka uç isteklerinin* Azure Application Insights'a kaydedilip günlüğe kaydedilmeyeceğini belirtir. *Arka uç isteği,* Azure API Yönetimi hizmetinden giden bir istektir.                                                                                                                                                                        |
| Gelişmiş Seçenekler: Backend Yanıt  |                                   | *Arka uç yanıtlarının* Azure Application Insights'a kaydedilip günlüğe kaydedilmeyeceğini belirtir. *Arka uç yanıtı,* Azure API Yönetimi hizmetine gelen bir yanıttır.                                                                                                                                                                       |

> [!NOTE]
> Tüm API'ler için tek API kaydedici veya logger olmak gibi farklı düzeylerde kaydediciler belirtebilirsiniz.
>  
> Her ikisini de belirtirseniz:
> + farklı loggers ise, o zaman her ikisi de kullanılacak (çoklama günlükleri),
> + onlar aynı loggers ancak farklı ayarları varsa, o zaman tek API (daha ayrıntılı düzey) için bir tüm API'ler için geçersiz kılar.

## <a name="what-data-is-added-to-azure-application-insights"></a>Azure Uygulama Öngörüleri'ne hangi veriler eklenir?

Azure Uygulama Öngörüleri alır:

+ *Gelen* her istek için telemetri öğesi isteği *(ön uç isteği*, *önuç yanıtı),*
+ *Bağımlılık* telemetri öğesi, bir arka uç hizmetine iletilen her istek için *(arka uç isteği*, *arka uç yanıtı),*
+ *Her* başarısız istek için özel durum telemetri öğesi.

Başarısız bir istek, aşağıdakileri içeren bir istektir:

+ kapalı bir istemci bağlantısı nedeniyle başarısız veya
+ API ilkelerinin bir *hata* bölümünü tetikledi veya
+ 4xx veya 5xx eşleşen bir yanıt HTTP durum kodu vardır.

## <a name="performance-implications-and-log-sampling"></a>Performans etkileri ve günlük örneklemesi

> [!WARNING]
> Tüm olayları günlüğe kaydetme, gelen istek oranına bağlı olarak ciddi performans etkileri ne olabilir.

Dahili yük testlerine dayanarak, bu özelliği etkinleştirmek, istek oranı saniyede 1.000 isteği aştığında, iş çıkarmada %40-50'lik bir azalmaya neden oldu. Azure Application Insights, uygulama performanslarını değerlendirmek için istatistiksel analiz kullanmak üzere tasarlanmıştır. Bir denetim sistemi olması amaçlanmamıştır ve yüksek hacimli API'ler için her bir isteği günlüğe kaydetmeye uygun değildir.

**Örnekleme** ayarını ayarlayarak günlüğe kaydedilen istek sayısını işleyebilirsiniz (yukarıdaki adımlara bakın). %100 değeri tüm isteklerin günlüğe kaydedildiği anlamına gelirken, %0 hiçbir günlüğe kaydetmeyi yansıtmaz. **Örnekleme,** telemetri hacmini azaltmaya yardımcı olur ve önemli performans bozulmasını önlerken, günlüğe kaydetmenin avantajlarını da taşır.

Üstbilgi ve istek ve yanıtların gövdesinin günlüğe kaydedilmesini atlamak, performans sorunlarını hafifletme üzerinde de olumlu bir etki yaratacaktır.

## <a name="video"></a>Video

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2pkXv]
>
>

## <a name="next-steps"></a>Sonraki adımlar

+ [Azure Uygulama Öngörüleri](https://docs.microsoft.com/azure/application-insights/)hakkında daha fazla bilgi edinin.
+ [Azure Etkinlik Hub'ları ile günlüğe kaydetmeyi](api-management-howto-log-event-hubs.md)düşünün.
