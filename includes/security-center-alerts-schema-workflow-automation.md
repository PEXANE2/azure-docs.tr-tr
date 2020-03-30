---
title: include dosyası
description: include dosyası
services: data-factory
author: memildin
ms.service: data-factory
ms.topic: include
ms.date: 03/10/2020
ms.author: memildin
ms.custom: include file
ms.openlocfilehash: 0b6864c3304b86e80549297fc073a2e387000d64
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80272642"
---
```json
{
  "VendorName": "Microsoft",
  "AlertType": "SUSPECT_SVCHOST",
  "StartTimeUtc": "2016-12-20T13:38:00.000Z",
  "EndTimeUtc": "2019-12-20T13:40:01.733Z",
  "ProcessingEndTime": "2019-09-16T12:10:19.5673533Z",
  "TimeGenerated": "2016-12-20T13:38:03.000Z",
  "IsIncident": false,
  "Severity": "High",
  "Status": "New",
  "ProductName": "Azure Security Center",
  "SystemAlertId": "2342409243234234_F2BFED55-5997-4FEA-95BD-BB7C6DDCD061",
  "CompromisedEntity": "WebSrv1",
  "Intent": "Execution",
  "AlertDisplayName": "Suspicious process detected",
  "Description": "Suspicious process named 'SVCHOST.EXE' was running from path: %{Process Path}",
  "RemediationSteps": ["contact your security information team"],
  "ExtendedProperties": {
    "Process Path": "c:\\temp\\svchost.exe",
    "Account": "Contoso\\administrator",
    "PID": 944,
    "ActionTaken": "Detected"
  },
  "Entities": [],
  "ResourceIdentifiers": [
        {
            Type: "AzureResource",
            AzureResourceId: "/subscriptions/86057C9F-3CDD-484E-83B1-7BF1C17A9FF8/resourceGroups/backend-srv/providers/Microsoft.Compute/WebSrv1"
        },
        {
            Type: "LogAnalytics",
            WorkspaceId: "077BA6B7-8759-4F41-9F97-017EB7D3E0A8",
            WorkspaceSubscriptionId: "86057C9F-3CDD-484E-83B1-7BF1C17A9FF8",
            WorkspaceResourceGroup: "omsrg",
            AgentId: "5A651129-98E6-4E6C-B2CE-AB89BD815616",
        }
  ]
}
```

### <a name="the-data-model-of-the-schema"></a>Şema veri modeli

|Alan|Veri türü|Açıklama|
|----|----|----|
|**AlertDisplayName**|Dize|Uyarının görüntü adı.|
|**Alerttype**|Dize|Uyarı türü. Aynı türdeki uyarılar aynı değere sahip olmalıdır. Bu alan, bir uyarı örneğinin değil, uyarı türünü temsil eden anahtarlı bir dizedir. Aynı algılama mantığı/analitik tüm uyarı örnekleri uyarı türü için aynı değere sahip olmalıdır.|
|**Tehlikeye Varlık**|Dize|Bu uyarıyla en çok ilişkili kaynağın görüntü adı.|
|**Açıklama**|Dize|Uyarının açıklaması.|
|**Son ZamanUtc**|DateTime|Uyarıya dahil edilen son olayın veya etkinliğin zamanı.  Alan, UTC saat dilimi bilgileri de dahil olmak üzere ISO8601 biçimine uygun bir dize olmalıdır.|
|**Varlıklar**|Ayrılmaz (IEntity)|Uyarıyla ilgili varlıkların listesi. Bu liste, farklı türde varlıkların bir karışımını tutabilir. Varlıklar türü, Varlıklar bölümünde tanımlanan türlerden herhangi biri olabilir. Aşağıdaki listede olmayan varlıklar da gönderilebilir, ancak işlenmeleri garanti edilmez (uyarı yeni varlık türleri ile doğrulamada başarısız olmaz).|
|**Genişletilmiş Özellikler**|Sözlük (String, String)|Sağlayıcılar (isteğe bağlı olarak) buraya özel alanlar içerebilir.|
|**Niyet**|Sabit Listesi|Uyarının arkasındaki öldürme zinciriyle ilgili niyet. Desteklenen değerler listesi ve Azure Güvenlik Merkezi'nin desteklenen öldürme zinciri amaçlarının açıklamaları için [Bkz.](../articles/security-center/alerts-reference.md#intentions)<br/>Bu alanın birden çok değeri olabilir (virgülle ayrılmış).|
|**IsIncident**|Bool|Bu alan, uyarının bir olay (birkaç uyarının bileşik gruplandırması) veya tek bir uyarı olup olmadığını belirler. Alan için varsayılan değer 'false' (tek bir uyarı anlamına gelir).|
|**İşlemeSon Zaman**|DateTime|Uyarıyı tutan özgün üründeki son kullanıcı ya da uyarıyı elinde bulunduran son kullanıcıya erişilebildiği saat.|
|**ProductName**|Dize|Bu uyarıyı yayınlayan ürünün adı (Azure Güvenlik Merkezi, Azure ATP, Microsoft Defender ATP, O365 ATP, MCAS vb.).|
|**Düzeltme Adımları**|Liste<String>|Uyarıyı düzeltmek için el ile eylem öğeleri.|
|**ResourceIdentifiers**|Liste (Kaynak Tanımlayıcıları)|Uyarıyı doğru ürün pozlama grubuna (kiracı, çalışma alanı, abonelik vb.) yönlendirmek için kullanılabilecek bu uyarının kaynak tanımlayıcıları. Uyarı başına farklı türde birden fazla tanımlayıcı olabilir.|
|**Önem Derecesi**|Sabit Listesi|Sağlayıcı tarafından bildirilen uyarının önem derecesi. Olası Değerler: Bilgi, Düşük, Orta ve Yüksek.|
|**Başlangıç ZamanıUtc**|DateTime|Uyarıya dahil edilen ilk olayın veya etkinliğin zamanı. Alan, UTC saat dilimi bilgileri de dahil olmak üzere ISO8601 biçimine uygun bir dize olmalıdır.|
|**Durum**|Sabit Listesi|Uyarının yaşam döngüsü durumu.<br/>Desteklenen durumlar şunlardır: Yeni, Çözümlenmiş, Görevden Alınan, Bilinmeyen.<br/>Desteklenen seçenekler dışında bir değer belirten bir uyarı 'Bilinmiyor' durumu atanır.<br/>Değer belirtmeyen bir uyarıya 'Yeni' durumu atanır.|
|**SystemAlertId**|Dize|Uyarı tanımlayıcısı.|
|**TimeGenerated**|DateTime|Uyarının uyarı sağlayıcısı tarafından oluşturulduğu saat. Dahili uyarı sağlayıcıları tarafından bildirilmemişse, bir ürün ürün tarafından işlenmek üzere alınan zamanı atamayı seçebilir.  Alan, UTC saat dilimi bilgileri de dahil olmak üzere ISO8601 biçimine uygun bir dize olmalıdır.|
|**Satıcıadı**|Dize|Uyarıyı yükselten satıcının adı.|
|||
