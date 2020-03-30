---
title: Azure monitör uyarıları için ortak uyarı şeması
description: Ortak uyarı şemasını anlamak, neden kullanmanız gerektiği ve nasıl etkinleştirdiğiniz
ms.topic: conceptual
ms.subservice: alerts
ms.date: 03/14/2019
ms.openlocfilehash: 1445e8cf38b2694146fc8749ba5e77f2297de969
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79249054"
---
# <a name="common-alert-schema"></a>Ortak uyarı şeması

Bu makalede, ortak uyarı şemasının ne olduğu, kullanmanın yararları ve nasıl etkinleştirilen açıklanmaktadır.

## <a name="what-is-the-common-alert-schema"></a>Ortak uyarı şeması nedir?

Ortak uyarı şeması, bugün Azure'da uyarı bildirimleri için tüketim deneyimini standartlaştırır. Tarihsel olarak, bugün Azure'daki üç uyarı türü (metrik, günlük ve etkinlik günlüğü) kendi e-posta şablonları, webhook şemaları vb. vardır. Ortak uyarı şeması yla artık tutarlı bir şemaya sahip uyarı bildirimleri alabilirsiniz.

Herhangi bir uyarı örneği **etkilenen kaynağı** ve **uyarının nedenini**açıklar ve bu örnekler aşağıdaki bölümlerde ortak şemada açıklanır:
* **Temel Bilgiler**: Uyarının **hangi kaynakta** olduğunu açıklayan tüm uyarı türlerinde yaygın olan **standartlaştırılmış alanlar**kümesi (örneğin, önem derecesi veya açıklama). 
* **Uyarı Bağlamı**: **Uyarı türüne göre**değişen alanlar ile **uyarının nedenini**açıklayan bir alan kümesi. Örneğin, bir metrik uyarı, uyarı bağlamında metrik ad ve metrik değer gibi alanlara sahip olurken, etkinlik günlüğü uyarısı uyarıyı oluşturan olay hakkında bilgi sahibi olur. 

Müşterilerden duyduğumuz tipik tümleştirme senaryoları, sorumlu ekibin üzerinde çalışmaya başladığı bazı pivotlara (örneğin, kaynak grubu) dayalı olarak uyarı örneğinin ilgili ekibe yönlendirmesini içerir. Ortak uyarı şemasıyla, temel alanları yararlanarak, ilgili ekiplerin daha fazla araştırma yapmak üzere olduğu gibi bağlam alanlarını bırakarak uyarı türleri arasında standart yönlendirme mantığına sahip olabilirsiniz.

Bu, potansiyel olarak daha az tümleştirmeye sahip olabileceğiniz anlamına gelir ve bu da bunları yönetme ve koruma işlemini _çok daha_ basit bir görev haline getirir. Ayrıca, gelecekteki uyarı yükü zenginleştirmeleri (örneğin, özelleştirme, tanısal zenginleştirme, vb.) yalnızca ortak şemada ortaya çıkar.

## <a name="what-enhancements-does-the-common-alert-schema-bring"></a>Ortak uyarı şeması ne gibi geliştirmeler getiriyor?

Ortak uyarı şeması öncelikle uyarı bildirimlerinizde kendini gösterir. Göreceğiniz geliştirmeler aşağıda listelenmiştir:

| Eylem | sekmesinden |
|:---|:---|
| SMS | Tüm uyarı türleri için tutarlı bir SMS şablonu. |
| Email | Sorunları bir bakışta kolayca tanılamanızı sağlayan tutarlı ve ayrıntılı bir e-posta şablonu. Portaldaki uyarı örneğine ve etkilenen kaynağa gömülü derin bağlantılar, düzeltme işlemine hızla atlayabildiğinizden emin olun. |
| Webhook/Logic App/Azure İşlevi/Otomasyon Runbook | Tüm uyarı türleri için tutarlı bir JSON yapısı, farklı uyarı türleri arasında kolayca tümleştirmeler oluşturmanıza olanak tanır. |

Yeni şema, yakın gelecekte hem Azure portalı hem de Azure mobil uygulamasında daha zengin bir uyarı tüketimi deneyimi ne olanak sağlayacak. 

[Webhooks/Logic Apps/Azure İşlevler/Otomasyon Runbook'ları için şema tanımları hakkında daha fazla bilgi edinin.](https://aka.ms/commonAlertSchemaDefinitions)

> [!NOTE]
> Aşağıdaki eylemler ortak uyarı şema desteklemez: ITSM Bağlayıcısı.

## <a name="how-do-i-enable-the-common-alert-schema"></a>Ortak uyarı şemasını nasıl etkinleştirebilirim?

Eylem Grupları, hem portal hem de REST API aracılığıyla ortak uyarı şemasını seçebilir veya devre dışı kullanabilirsiniz. Yeni şemaya geçmek için geçiş bir eylem düzeyinde vardır. Örneğin, bir e-posta eylemi ve webhook eylemi için ayrı ayrı kabul etmeniz gerekir.

> [!NOTE]
> 1. Aşağıdaki uyarı türleri varsayılan olarak ortak şema desteği (gerekli optin):
>     * Akıllı algılama uyarıları
> 1. Aşağıdaki uyarı türleri şu anda ortak şemayı desteklemez:
>     * Azure Monitor tarafından [VM'ler için](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-overview) oluşturulan uyarılar
>     * Azure Maliyet [Yönetimi](https://docs.microsoft.com/azure/billing/billing-cost-management-budget-scenario) tarafından oluşturulan uyarılar

### <a name="through-the-azure-portal"></a>Azure portalı aracılığıyla

![Ortak uyarı şema opt in](media/alerts-common-schema/portal-opt-in.png)

1. Bir eylem grubunda varolan veya yeni bir eylemi açın. 
1. Gösterildiği gibi ortak uyarı şemasını etkinleştirmek için geçiş için 'Evet' seçeneğini belirleyin.

### <a name="through-the-action-groups-rest-api"></a>Eylem Grupları ARACıLıĞıYLA REST API

Ortak uyarı şemasına geçmek için [Eylem Grupları API'sini](https://docs.microsoft.com/rest/api/monitor/actiongroups) de kullanabilirsiniz. REST API [çağrısını oluşturur ken veya güncellerken,](https://docs.microsoft.com/rest/api/monitor/actiongroups/createorupdate) aşağıdaki eylemlerden herhangi biri için "useCommonAlertSchema" (tercih etmek için) veya 'false' (devre dışı bırakmak için) bayrağını ayarlayabilirsiniz - e-posta/webhook/logic app/Azure İşlevi/otomasyon runbook.

Örneğin, [REST API'yi oluşturmak veya güncelleştirmek](https://docs.microsoft.com/rest/api/monitor/actiongroups/createorupdate) için yapılan aşağıdaki istek gövdesi aşağıdakileri yapar:

* E-posta eylem "John Doe e-posta" için ortak uyarı şeması etkinleştirin
* E-posta eylem "Jane Smith'in e-posta" için ortak uyarı şema devre dışı
* Webhook eylem "Örnek webhook" için ortak uyarı şemasını etkinleştirin

```json
{
  "properties": {
    "groupShortName": "sample",
    "enabled": true,
    "emailReceivers": [
      {
        "name": "John Doe's email",
        "emailAddress": "johndoe@email.com",
        "useCommonAlertSchema": true
      },
      {
        "name": "Jane Smith's email",
        "emailAddress": "janesmith@email.com",
        "useCommonAlertSchema": false
      }
    ],
    "smsReceivers": [
      {
        "name": "John Doe's mobile",
        "countryCode": "1",
        "phoneNumber": "1234567890"
      },
      {
        "name": "Jane Smith's mobile",
        "countryCode": "1",
        "phoneNumber": "0987654321"
      }
    ],
    "webhookReceivers": [
      {
        "name": "Sample webhook",
        "serviceUri": "http://www.example.com/webhook",
        "useCommonAlertSchema": true
      }
    ]
  },
  "location": "Global",
  "tags": {}
}
```





## <a name="next-steps"></a>Sonraki adımlar

- [Webhooks/Logic Apps/Azure İşlevler/Otomasyon Runbook'ları için ortak uyarı şeması tanımları.](https://aka.ms/commonAlertSchemaDefinitions)
- [Tüm uyarılarınızı işlemek için ortak uyarı şemasından yararlanan bir mantık uygulaması oluşturmayı öğrenin.](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-common-schema-integrations) 



