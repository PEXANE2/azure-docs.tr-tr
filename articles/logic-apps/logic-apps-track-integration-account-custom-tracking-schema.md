---
title: B2B iletileri için özel izleme şemaları
description: Azure Mantık Uygulamalarında B2B mesajlarını izlemek için özel izleme şemaları oluşturun
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 01/01/2020
ms.openlocfilehash: c82f9cbfaf2e23ddaa5e4b05f4aac4795d3e16a9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76903059"
---
# <a name="create-custom-tracking-schemas-that-monitor-end-to-end-workflows-in-azure-logic-a"></a>Azure Mantık A'da uçlardan uca iş akışlarını izleyen özel izleme şemaları oluşturun

Azure Logic Apps, iş akışınızın bazı bölümleri için etkinleştirebileceğiniz yerleşik izleme ye sahiptir. Ancak, bir mantık uygulaması, BizTalk Server, SQL Server veya başka bir katmanı içeren iş akışları gibi, iş akışlarının başından sonuna kadar olayları günlüğe kaydeden özel izleme ayarlayabilirsiniz. Bu makalede, mantık uygulamanızın dışındaki katmanlarda kullanabileceğiniz özel kodlar sağlanıyor.

## <a name="custom-tracking-schema"></a>Özel izleme şeması

```json
{
   "sourceType": "",
   "source": {
      "workflow": {
         "systemId": ""
      },
      "runInstance": {
         "runId": ""
      },
      "operation": {
         "operationName": "",
         "repeatItemScopeName": "",
         "repeatItemIndex": ,
         "trackingId": "",
         "correlationId": "",
         "clientRequestId": ""
      }
   },
   "events": [
      {
         "eventLevel": "",
         "eventTime": "",
         "recordType": "",
         "record": {}
      }
   ]
}
```

| Özellik | Gerekli | Tür | Açıklama |
|----------|----------|------|-------------|
| Sourcetype | Evet | Dize | Bu izin verilen değerlere sahip `Microsoft.Logic/workflows`çalıştırılan kaynağın türü: ,`custom` |
| source | Evet | String veya JToken | Kaynak türü ise, `Microsoft.Logic/workflows`kaynak bilgilerin bu şemayı izlemesi gerekir. Kaynak türü ise, `custom`şema bir JToken olduğunu. |
| Systemıd | Evet | Dize | Mantık uygulama sistemi kimliği |
| runId | Evet | Dize | Mantık uygulaması çalıştır kimliği |
| operationName | Evet | Dize | Operasyonun adı, örneğin, eylem veya tetikleyici |
| repeatItemScopeName | Evet | Dize | Eylem bir `foreach`veya `until` döngü içindeyse madde adını yineleme |
| repeatItemIndex | Evet | Tamsayı | Eylemin bir `foreach` veya `until` döngü içinde olduğunu ve yinelenen madde dizin numarası olduğunu gösterir. |
| trackingId | Hayır | Dize | İletileri ilişkilendirmek için kimliği izleme |
| correlationId | Hayır | Dize | İletileri ilişkilendirmek için korelasyon kimliği |
| clientRequestId | Hayır | Dize | İstemci iletileri ilişkilendirmek için bu özelliği doldurabilir |
| Eventlevel | Evet | Dize | Olayın düzeyi |
| eventTime | Evet | DateTime | UTC formatında olayın saati: *YYYY-MM-DDTHH:MM:SS.00000Z* |
| recordType | Evet | Dize | Yalnızca bu izin verilen değere sahip izleme kaydının türü:`custom` |
| kaydet | Evet | JToken | Yalnızca JToken biçimine sahip özel kayıt türü |
|||||

## <a name="b2b-protocol-tracking-schemas"></a>B2B protokol izleme şemaları

B2B protokol izleme şemaları hakkında bilgi için bkz:

* [AS2 izleme şemaları](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [X12 izleme şemaları](logic-apps-track-integration-account-x12-tracking-schema.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Monitor günlükleriyle B2B mesajlarını izleme](../logic-apps/monitor-b2b-messages-log-analytics.md) hakkında daha fazla bilgi edinin