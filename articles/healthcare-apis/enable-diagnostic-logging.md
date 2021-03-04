---
title: FHıR için Azure API 'de tanılama günlüğünü etkinleştirme
description: Bu makalede, FHıR için Azure API 'de tanılama günlüğü 'nün nasıl etkinleştirileceği açıklanmaktadır®
services: healthcare-apis
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: conceptual
ms.reviewer: dseven
ms.author: cavoeg
author: zxue
ms.date: 03/03/2021
ms.openlocfilehash: 8dde5c6206087c8bb1c1f456a0c858abbccf0cc4
ms.sourcegitcommit: 4b7a53cca4197db8166874831b9f93f716e38e30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102096318"
---
# <a name="enable-diagnostic-logging-in-azure-api-for-fhir"></a>FHıR için Azure API 'de tanılama günlüğünü etkinleştirme

Bu makalede, FHıR için Azure API 'de tanılama günlüğünü etkinleştirmeyi öğrenir ve bu günlüklere yönelik bazı örnek sorguları inceleyebilirsiniz. Tanılama günlüklerine erişim, mevzuat gereksinimleriyle (HIPAA gibi) uyumluluğun gerektiği her türlü sağlık hizmeti için önemlidir. FHıR için Azure API 'sindeki özelliği, tanılama günlüklerini sağlayan, Azure portal [**Tanılama ayarlarınızdaki**](../azure-monitor/essentials/diagnostic-settings.md) özelliktir. 

## <a name="view-and-download-fhir-metrics-data"></a>FHıR ölçüm verilerini görüntüleyin ve Indirin

Izleme kapsamında ölçümleri görüntüleyebilirsiniz | Portaldan ölçümler. Ölçümler, Istek sayısını, ortalama gecikme süresini, hata sayısını, veri boyutunu, ru kullanılan, kapasiteyi aşan istek sayısını ve kullanılabilirliği (%) içerir. Aşağıdaki ekran görüntüsünde, son 7 gün içinde çok az etkinlik içeren örnek bir ortam için kullanılan ru gösterilmektedir. Verileri JSON biçiminde indirebilirsiniz.

   :::image type="content" source="media/diagnostic-logging/fhir-metrics-rus-screen.png" alt-text="Portaldan FHıR ölçümleri için Azure API" lightbox="media/diagnostic-logging/fhir-metrics-rus-screen.png":::

## <a name="enable-audit-logs"></a>Denetim günlüklerini etkinleştir
1. FHıR için Azure API 'de tanılama günlüğünü etkinleştirmek üzere Azure portal için Azure API 'nizi seçin. 
2. **Tanılama ayarlarına** gidin 

   :::image type="content" source="media/diagnostic-logging/diagnostic-settings-screen.png" alt-text="Azure FHıR tanılama ayarlarını ekleyin." lightbox="media/diagnostic-logging/diagnostic-settings-screen.png":::

3. **+ Tanılama ayarı Ekle** ' yi seçin

4. Ayar için bir ad girin

5. Tanılama günlüklerinizi erişmek için kullanmak istediğiniz yöntemi seçin:

    1. Denetim veya el ile inceleme için **bir depolama hesabına arşivleme** . Kullanmak istediğiniz depolama hesabının zaten oluşturulmuş olması gerekir.
    2. Üçüncü taraf bir hizmet veya özel analitik çözüm tarafından giriş için **Olay Hub 'ına akış** . Bu adımı yapılandırmadan önce bir olay hub 'ı ad alanı ve Olay Hub 'ı ilkesi oluşturmanız gerekir.
    3. Azure Izleyici 'de Log Analytics çalışma alanına **akış** . Bu seçeneği seçmeden önce günlüklerinizi analiz çalışma alanınızı oluşturmanız gerekecektir.

6. **Auditlogs** ve/veya **allölçümler**' i seçin. Ölçümler hizmet adı, kullanılabilirlik, veri boyutu, toplam gecikme, toplam istek, toplam hata ve zaman damgası içerir. [Desteklenen ölçümler](https://docs.microsoft.com/azure/azure-monitor/essentials/metrics-supported#microsofthealthcareapisservices)hakkında daha fazla ayrıntı bulabilirsiniz. 

   :::image type="content" source="media/diagnostic-logging/fhir-diagnostic-setting.png" alt-text="Azure FHıR Tanılama ayarları. AuditLogs ve/veya Allölçümler ' i seçin." lightbox="media/diagnostic-logging/fhir-diagnostic-setting.png":::

7. **Kaydet**’i seçin


> [!Note] 
> İlk günlüklerin Log Analytics göstermesi 15 dakika kadar sürebilir. Ayrıca, FHıR için Azure API 'SI bir kaynak grubundan veya abonelikten diğerine taşınırsa, taşıma tamamlandıktan sonra ayarı güncelleştirin. 
 
Tanılama günlükleri ile çalışma hakkında daha fazla bilgi için lütfen [Azure kaynak günlüğü belgelerine](../azure-monitor/essentials/platform-logs-overview.md) başvurun

## <a name="audit-log-details"></a>Denetim günlüğü ayrıntıları
Şu anda, FHıR hizmeti için Azure API 'SI, denetim günlüğünde aşağıdaki alanları döndürür: 

|Alan Adı  |Tür  |Notlar  |
|---------|---------|---------|
|Callerıdentity|Dinamik|Kimlik bilgilerini içeren bir genel özellik paketi
|Callerıdentityıssuer|Dize|Veren 
|Callerıdentityobjectıd|Dize|Object_Id 
|Callerıpaddress|Dize|Arayanın IP adresi 
|CorrelationId|Dize| Bağıntı Kimliği
|FhirResourceType|Dize|İşlemin yürütüldüğü kaynak türü
|Günlüğe kaydetme kategorisi|Dize|Günlük kategorisi (Şu anda ' AuditLogs ' günlük kategorisini döndürüyor)
|Konum|Dize|İsteği işleyen sunucunun konumu (örn., Orta Güney ABD)
|OperationDuration|int|Bu isteğin tamamlanması için geçen süre (saniye)
|OperationName|Dize| İşlem türünü açıklar (ör. güncelleştirme, arama türü)
|RequestUri|Dize|İstek URI 'SI 
|ResultType|Dize|Kullanılabilir değerler şu anda **başlatılmış**, **başarılı** veya **başarısız**
|Durum|int|HTTP durum kodu. (örn., 200) 
|TimeGenerated|DateTime|Etkinliğin tarih ve saati|
|Özellikler|Dize| FhirResourceType 'ın özelliklerini açıklar
|SourceSystem|Dize| Kaynak sistem (Bu durumda her zaman Azure)
|TenantId|Dize|Kiracı Kimliği
|Tür|Dize|Günlük türü (Bu durumda her zaman Microsofthealthilererceisauditlog)
|_ResourceId|Dize|Kaynakla ilgili ayrıntılar

## <a name="sample-queries"></a>Örnek sorgular

Günlük verilerinizi araştırmak için kullanabileceğiniz birkaç temel Application Insights sorgusu aşağıda verilmiştir.

**En son 100** günlüğü görmek için bu sorguyu çalıştırın:

```Application Insights
MicrosoftHealthcareApisAuditLogs
| limit 100
```

İşlemleri **Fhır kaynak türüne** göre gruplandırmak için bu sorguyu çalıştırın:

```Application Insights
MicrosoftHealthcareApisAuditLogs 
| summarize count() by FhirResourceType
```

Tüm **başarısız sonuçları** almak için bu sorguyu Çalıştır

```Application Insights
MicrosoftHealthcareApisAuditLogs 
| where ResultType == "Failed" 
```

## <a name="conclusion"></a>Sonuç 
Tanılama günlüklerine erişimi olması, bir hizmeti izlemek ve uyumluluk raporları sağlamak için gereklidir. FHıR için Azure API, bu eylemleri tanılama günlükleri aracılığıyla yapmanıza olanak sağlar. 
 
FHIR, HL7’nin kayıtlı ticari markasıdır ve HL7’nin izniyle kullanılır.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, FHıR için Azure API 'SI için Denetim günlüklerini etkinleştirmeyi öğrendiniz. Daha sonra, FHıR için Azure API 'sinde yapılandırabileceğiniz diğer ek ayarlar hakkında bilgi edinin
 
>[!div class="nextstepaction"]
>[Ek ayarlar](azure-api-for-fhir-additional-settings.md)
