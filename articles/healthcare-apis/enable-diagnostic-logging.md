---
title: FHıR için Azure API 'de tanılama günlüğünü etkinleştirme
description: Bu makalede, FHıR için Azure API 'de tanılama günlüğü 'nün nasıl etkinleştirileceği açıklanmaktadır®
services: healthcare-apis
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: conceptual
ms.reviewer: dseven
ms.author: cavoeg
author: CaitlinV39
ms.date: 11/01/2019
ms.openlocfilehash: 54119585d4f1377b60b85fbad01fe90f097a304f
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "95905183"
---
# <a name="enable-diagnostic-logging-in-azure-api-for-fhir"></a>FHıR için Azure API 'de tanılama günlüğünü etkinleştirme

Bu makalede, FHıR için Azure API 'de tanılama günlüğünü etkinleştirmeyi öğrenir ve bu günlüklere yönelik bazı örnek sorguları inceleyebilirsiniz. Tanılama günlüklerine erişim, mevzuat gereksinimleriyle (HIPAA gibi) uyumluluğun gerektiği her türlü sağlık hizmeti için önemlidir. FHıR için Azure API 'sindeki özelliği, tanılama günlüklerini sağlayan, Azure portal [**Tanılama ayarlarınızdaki**](../azure-monitor/platform/diagnostic-settings.md) özelliktir. 

## <a name="enable-audit-logs"></a>Denetim günlüklerini etkinleştir
1. FHıR için Azure API 'de tanılama günlüğünü etkinleştirmek üzere Azure portal için Azure API 'nizi seçin. 
2. **Tanılama ayarları**  
 ![ Tanılama Ayarları ' na gidin](media/diagnostic-logging/diagnostic-settings-screen.png) 

3. **+ Tanılama ayarı Ekle** ' yi seçin

4. Ayar için bir ad girin

5. Tanılama günlüklerinizi erişmek için kullanmak istediğiniz yöntemi seçin:

    1. Denetim veya el ile inceleme için **bir depolama hesabına arşivleme** . Kullanmak istediğiniz depolama hesabının zaten oluşturulmuş olması gerekir.
    2. Üçüncü taraf bir hizmet veya özel analitik çözüm tarafından giriş için **Olay Hub 'ına akış** . Bu adımı yapılandırmadan önce bir olay hub 'ı ad alanı ve Olay Hub 'ı ilkesi oluşturmanız gerekir.
    3. Azure Izleyici 'de Log Analytics çalışma alanına **akış** . Bu seçeneği seçmeden önce günlüklerinizi analiz çalışma alanınızı oluşturmanız gerekecektir.

6. **Auditlogs** ve yakalamak istediğiniz ölçümleri seçin. FHıR için Azure IoT bağlayıcısını kullanıyorsanız, ölçümler için **hataları, trafiği ve gecikme süresini** seçtiğinizden emin olun. 

   :::image type="content" source="media/iot-metrics-export/diagnostic-setting-add.png" alt-text="IoT Connector2" lightbox="media/iot-metrics-export/diagnostic-setting-add.png":::

7. **Kaydet**’i seçin


> [!Note] 
> İlk günlüklerin Log Analytics göstermesi 15 dakika kadar sürebilir.  
 
Tanılama günlükleri ile çalışma hakkında daha fazla bilgi için lütfen [Azure kaynak günlüğü belgelerine](../azure-monitor/platform/platform-logs-overview.md) başvurun

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
