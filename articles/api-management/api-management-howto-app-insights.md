---
title: Azure API Management Azure Application Insights tümleştirme
titleSuffix: Azure API Management
description: Azure Application Insights 'de Azure API Management olaylarını günlüğe kaydetme ve görüntüleme hakkında bilgi edinin.
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
ms.openlocfilehash: 12aeea8393a00d7d2662c826f847265bdbdc0119
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75442722"
---
# <a name="how-to-integrate-azure-api-management-with-azure-application-insights"></a>Azure API Management Azure Application Insights tümleştirme

Azure API Management, birden çok platformda uygulama oluşturup yöneten web geliştiricileri için genişletilebilir bir hizmet olan Azure Application Insights ile kolay tümleştirme sağlar. Bu kılavuzda, bu tür tümleştirmenin her adımında izlenecek yol gösterilmektedir ve API Management hizmet örneğiniz üzerindeki performans etkisini azaltmaya yönelik stratejiler açıklanmaktadır.

## <a name="prerequisites"></a>Ön koşullar

Bu kılavuzu izlemek için bir Azure API Management örneğine sahip olmanız gerekir. Henüz bir hesabınız yoksa, önce [öğreticiyi](get-started-create-service-instance.md) doldurun.

## <a name="create-an-azure-application-insights-instance"></a>Azure Application Insights örneği oluşturma

Azure Application Insights kullanabilmeniz için önce hizmetin bir örneğini oluşturmanız gerekir.

1. **Azure Portal** açın ve **Application Insights**gidin.  
    ![App Insights oluşturma](media/api-management-howto-app-insights/apim-app-insights-instance-1.png)  
2. **+ Ekle**'ye tıklayın.  
    ![App Insights oluşturma](media/api-management-howto-app-insights/apim-app-insights-instance-2.png)  
3. Formu doldur. **Uygulama türü**olarak **genel** ' i seçin.
4. **Oluştur**'a tıklayın.

## <a name="create-a-connection-between-azure-application-insights-and-azure-api-management-service-instance"></a>Azure Application Insights ile Azure API Management hizmet örneği arasında bağlantı oluşturma

1. **Azure Portal** **Azure API Management hizmet örneğinize** gidin.
2. Soldaki menüden **Application Insights** seçin.
3. **+ Ekle**'ye tıklayın.  
    ![App Insights günlükçüsü](media/api-management-howto-app-insights/apim-app-insights-logger-1.png)  
4. Daha önce oluşturulan **Application Insights** örneğini seçin ve kısa bir açıklama sağlayın.
5. **Oluştur**'a tıklayın.
6. Bir izleme anahtarı ile Azure Application Insights günlükçüsü oluşturdunuz. Artık listede görünmelidir.  
    ![App Insights günlükçüsü](media/api-management-howto-app-insights/apim-app-insights-logger-2.png)  

> [!NOTE]
> Sahnenin arkasında, Application Insights örneğinin Izleme anahtarını içeren API Management örneğiniz için bir [günlükçü](https://docs.microsoft.com/rest/api/apimanagement/2019-01-01/logger/createorupdate) varlığı oluşturulur.

## <a name="enable-application-insights-logging-for-your-api"></a>API 'niz için Application Insights günlüğe kaydetmeyi etkinleştirme

1. **Azure Portal** **Azure API Management hizmet örneğinize** gidin.
2. Soldaki menüden **API'ler** seçeneğini belirleyin.
3. Bu örnekte, API 'nize tıklayarak bu örnek **Tanıtım Konferansı API 'si**.
4. Üstteki çubukta **Ayarlar** sekmesine gidin.
5. **Tanılama günlükleri** bölümüne gidin.  
    ![App Insights günlükçüsü](media/api-management-howto-app-insights/apim-app-insights-api-1.png)  
6. **Etkinleştir** kutusunu işaretleyin.
7. **Hedef** açılan menüsünde, eklenen günlükçüyü seçin.
8. **Örnekleme (%)** olarak **100** girin ve **her zaman günlük hatalarını** işaretle onay kutusunu işaretleyin.
9. **Save (Kaydet)** düğmesine tıklayın.

> [!WARNING]
> **Gövde alanının ilk baytlarında** **0** varsayılan değerini geçersiz kılmak, API 'nizin performansını önemli ölçüde azaltabilir.

> [!NOTE]
> Sahnenin arkasında, API düzeyinde ' ApplicationInsights ' adlı bir [Tanılama](https://docs.microsoft.com/rest/api/apimanagement/2019-01-01/diagnostic/createorupdate) varlığı oluşturulur.

| Ayar adı                        | Değer türü                        | Açıklama                                                                                                                                                                                                                                                                                                                                      |
|-------------------------------------|-----------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Etkinleştirme                              | boole                           | Bu API 'nin günlüğe kaydetmenin etkinleştirilip etkinleştirilmeyeceğini belirtir.                                                                                                                                                                                                                                                                                                |
| Hedef                         | Azure Application Insights günlükçüsü | Kullanılacak Azure Application Insights günlükçüsü belirtir                                                                                                                                                                                                                                                                                           |
| Örnekleme (%)                        | decimal                           | 0 ile 100 (yüzde) arasında değerler. <br/> Azure Application Insights hangi istek yüzdesinin günlüğe kaydedileceğini belirtir. %0 örnekleme, %100 örnekleme, günlüğe kaydedilen tüm isteklerin olduğu anlamına gelen sıfır istek günlüğe kaydedilir. <br/> Bu ayar, Azure Application Insights isteklerin günlüğe kaydedilmesini azaltmaya yönelik performans etkilerini azaltmak için kullanılır (aşağıdaki bölümüne bakın). |
| Her zaman günlüğe kaydetme hataları                   | boole                           | Bu ayar seçilirse, **örnekleme** ayarından bağımsız olarak tüm başarısızlıklar Azure Application Insights kaydedilir.                                                                                                                                                                                                                  |
| Temel seçenekler: üst bilgiler              | list                              | İstekler ve yanıtlar için Azure Application Insights günlüğe kaydedilecek üst bilgileri belirtir.  Varsayılan: günlüğe hiçbir üst bilgi kaydedilmez.                                                                                                                                                                                                             |
| Temel seçenekler: Ilk gövde baytları  | integer                           | Azure Application Insights istek ve yanıt için gövdenin kaç tane ilk baytından kaydedildiğini belirtir.  Varsayılan: gövde günlüğe kaydedilmez.                                                                                                                                                                                                    |
| Gelişmiş Seçenekler: ayrıntı düzeyi         |                                   | Ayrıntı düzeyini belirtir. Yalnızca daha yüksek önem düzeyine sahip özel izlemeler günlüğe kaydedilir. Varsayılan: bilgi.                                                                                                                                                                                                                               |
| Gelişmiş Seçenekler: ön uç Isteği  |                                   | *Ön uç Isteklerinin* Azure Application Insights günlüğe kaydedilip edilmeyeceğini belirtir. *Ön uç isteği* , Azure API Management hizmetine gelen bir isteğidir.                                                                                                                                                                        |
| Gelişmiş Seçenekler: ön uç yanıtı |                                   | *Ön uç yanıtlarının* Azure Application Insights günlüğe kaydedilip edilmeyeceğini belirtir. *Ön uç yanıtı* , Azure API Management hizmetinden giden bir yanıttır.                                                                                                                                                                   |
| Gelişmiş Seçenekler: arka uç Isteği   |                                   | *Arka uç Isteklerinin* Azure Application Insights günlüğe kaydedilip edilmeyeceğini belirtir. *Arka uç isteği* , Azure API Management hizmetinden giden bir istek.                                                                                                                                                                        |
| Gelişmiş Seçenekler: arka uç yanıtı  |                                   | *Arka uç yanıtlarının* Azure Application Insights günlüğe kaydedilip edilmeyeceğini belirtir. *Arka uç yanıtı* , Azure API Management hizmetine gelen bir yanıttır.                                                                                                                                                                       |

> [!NOTE]
> Günlükçüleri farklı düzeylerde, tek API günlükçüsü veya tüm API 'Ler için bir günlükçü belirtebilirsiniz.
>  
> Her ikisini de belirtirseniz:
> + Bunlar farklı Günlükçüler ise, her ikisi de kullanılır (çoğullama günlükleri),
> + Bunlar aynı Günlükçüler, ancak farklı ayarlara sahip ise, tek bir API (daha ayrıntılı düzey) için bir tane, tüm API 'Leri için geçersiz kılınır.

## <a name="what-data-is-added-to-azure-application-insights"></a>Azure Application Insights hangi verileri eklendi

Azure Application Insights şunları alır:

+ Her gelen istek (*ön uç isteği*, *ön uç yanıtı*) için telemetri öğesi *iste* ,
+ Bir arka uç hizmetine iletilen her istek için *bağımlılık* telemetri öğesi (*arka uç isteği*, *arka uç yanıtı*),
+ Her başarısız istek için *özel durum* telemetrisi öğesi.

Başarısız istek bir istek ve şu şekilde:

+ kapalı bir istemci bağlantısı nedeniyle başarısız oldu veya
+ API ilkelerinin *hata durumunda* tetiklenmesi veya
+ , 4xx veya 5xx ile eşleşen bir yanıt HTTP durum koduna sahiptir.

## <a name="performance-implications-and-log-sampling"></a>Performans etkileri ve günlük örnekleme

> [!WARNING]
> Tüm olayların günlüğe kaydedilmesi, gelen isteklerin hızına bağlı olarak ciddi performans etkilerine sahip olabilir.

İç yük testlerine bağlı olarak, bu özelliğin etkinleştirilmesi, istek hızı saniye başına 1.000 isteği aştığında üretilen iş için %40 %50 oranında azalmaya neden oldu. Azure Application Insights, uygulama performanslarını değerlendirmek için istatistiksel analizler kullanmak üzere tasarlanmıştır. Bir denetim sistemi olması amaçlanmamıştır ve yüksek hacimli API 'Ler için her bireysel isteği günlüğe kaydetmeye uygun değildir.

**Örnekleme** ayarını ayarlayarak günlüğe kaydedilen isteklerin sayısını değiştirebilirsiniz (yukarıdaki adımlara bakın). %100 değeri tüm isteklerin günlüğe kaydedildiği, %0 oranında hiçbir günlük kaydı olmadığı anlamına gelir. **Örnekleme** , önemli performans düşüşüne karşı etkili bir şekilde karşı çok sayıda telemetri hacmi azaltmaya yardımcı olur.

Üst bilgilerin ve istek gövdesinin ve yanıtların, hafifletmesini performans sorunları üzerinde olumlu bir etkisi olacaktır.

## <a name="video"></a>Video

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2pkXv]
>
>

## <a name="next-steps"></a>Sonraki adımlar

+ [Azure Application Insights](https://docs.microsoft.com/azure/application-insights/)hakkında daha fazla bilgi edinin.
+ [Azure Event Hubs oturum açmayı](api-management-howto-log-event-hubs.md)göz önünde bulundurun.
