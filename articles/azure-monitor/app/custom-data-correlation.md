---
title: Azure Uygulama Öngörüleri | Microsoft Dokümanlar
description: Uygulama Öngörüleri'nden gelen verileri veri zenginleştirme veya arama tabloları, Uygulama İçen Bilgiler dışındaki veri kaynakları ve özel veriler gibi diğer veri kümelerine ilişkilendirin.
ms.topic: conceptual
author: eternovsky
ms.author: evternov
ms.date: 08/08/2018
ms.reviewer: mbullwin
ms.openlocfilehash: 121e4699bd6a72f6865d3a6ffdef58c1b3806047
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79082770"
---
# <a name="correlating-application-insights-data-with-custom-data-sources"></a>Uygulama Öngörüleri verilerini özel veri kaynaklarıyla ilişkilendirme

Uygulama Öngörüleri birkaç farklı veri türü toplar: özel durumlar, izlemeler, sayfa görünümleri ve diğerleri. Bu genellikle uygulamanızın performansını, güvenilirliğini ve kullanımını araştırmak için yeterli olsa da, Uygulama Öngörüleri'nde depolanan verileri diğer tamamen özel veri kümeleri ile ilişkilendirmek için yararlı olduğu durumlar vardır.

Özel veri isteyebileceğiniz bazı durumlar şunlardır:

- Veri zenginleştirme veya arama tabloları: örneğin, sunucu sahibi ve bulunabileceği laboratuvar konumu ile bir sunucu adı tamamlayın 
- Uygulama Dışı Öngörüler veri kaynaklarıyla korelasyon: örneğin, gönderi mama zaman tahminlerinizin ne kadar doğru olduğunu belirlemek için bir web mağazasındaki satın alma yla ilgili verileri satın alma-gerçekleştirme hizmetinizden gelen bilgilerle ilişkilendirin 
- Tamamen özel veriler: Müşterilerimizin çoğu, Uygulama Öngörülerini destekleyen Azure Monitor günlük platformunun sorgu dilini ve performansını sever ve uygulama istatistikleriyle hiç ilgili olmayan verileri sorgulamak için kullanmak ister. Örneğin, [burada](https://www.catapultsystems.com/blogs/using-log-analytics-and-a-special-guest-to-forecast-electricity-generation/)belirtildiği gibi akıllı bir ev yükleme parçası olarak güneş paneli performansını izlemek için.

## <a name="how-to-correlate-custom-data-with-application-insights-data"></a>Özel verileri Application Insights verileriyle ilişkilendirme 

Application Insights güçlü Azure Monitor günlük platformu tarafından desteklenen bu yana, verileri yutmak için Azure Monitor'un tüm gücünü kullanabiliriz. Daha sonra, bu özel verileri Azure Monitor günlüklerinde kullanabileceğimiz verilerle ilişkilendirecek "birleştirme" işleci kullanarak sorgular yazacağız. 

## <a name="ingesting-data"></a>Verileri alma

Bu bölümde, verilerinizi Azure Monitor günlüklerine nasıl alacağınızı gözden geçireceğiz.

Zaten bir tane yoksa, [bu yönergeleri](../learn/quick-collect-azurevm.md) izleyerek ve "çalışma alanı oluşturma" adımını da ekleyerek yeni bir Log Analytics çalışma alanı sağlar.

Azure Monitor'a günlük verileri göndermeye başlamak için. Çeşitli seçenekler vardır:

- Eşzamanlı bir mekanizma için, doğrudan veri [toplayıcı API'sini](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-collector-api) arayabilir veya Logic App bağlayıcımızı kullanabilirsiniz – sadece "Azure Log Analytics"i arayabilir ve "Veri Gönder" seçeneğini seçebilirsiniz:

  ![Ekran görüntüsü seçin ve eylemi](./media/custom-data-correlation/01-logic-app-connector.png)  

- Eşzamanlı bir seçenek için, bir işleme ardışık hattı oluşturmak için Veri Toplayıcı API'sını kullanın. Ayrıntılar için [bu makaleye](https://docs.microsoft.com/azure/log-analytics/log-analytics-create-pipeline-datacollector-api) bakın.

## <a name="correlating-data"></a>Verileri ilişkilendirme

Uygulama Öngörüleri, Azure Monitor günlük platformuna dayanır. Bu nedenle, Azure Monitor'a aldığımız verileri Uygulama Öngörüleri verilerimizle ilişkilendirmek için [çapraz kaynak birleştirmelerini](https://docs.microsoft.com/azure/log-analytics/log-analytics-cross-workspace-search) kullanabiliriz.

Örneğin, "myLA" adlı log Analytics çalışma alanında laboratuvar envanterimizi ve konumlarımızı "LabLocations_CL" adlı bir tabloya sindirebiliriz. Daha sonra "myAI" adı verilen Application Insights uygulamasında izlenen isteklerimizi gözden geçirmek ve isteklere hizmet eden makine adlarını daha önce belirtilen özel tabloda depolanan bu makinelerin konumlarıyla ilişkilendirmek istersek, aşağıdaki sorguyu Uygulama Öngörüleri veya Azure Monitör bağlamı:

```
app('myAI').requests
| join kind= leftouter (
    workspace('myLA').LabLocations_CL
    | project Computer_S, Owner_S, Lab_S
) on $left.cloud_RoleInstance == $right.Computer
```

## <a name="next-steps"></a>Sonraki Adımlar

- [Veri Toplayıcı API](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-collector-api) başvurusuna göz atın.
- Çapraz kaynak [birleştirmeleri](https://docs.microsoft.com/azure/log-analytics/log-analytics-cross-workspace-search)hakkında daha fazla bilgi için.
