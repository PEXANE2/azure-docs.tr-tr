---
title: B2B iletileri için özel izleme şemaları
description: Azure Logic Apps B2B iletilerini izlemek için özel izleme şemaları oluşturma
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 01/01/2020
ms.openlocfilehash: c82f9cbfaf2e23ddaa5e4b05f4aac4795d3e16a9
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76903059"
---
# <a name="create-custom-tracking-schemas-that-monitor-end-to-end-workflows-in-azure-logic-a"></a>Azure Logic A 'da uçtan uca iş akışlarını izleyen özel izleme şemaları oluşturma

Azure Logic Apps, iş akışınızın parçaları için etkinleştirebilmeniz için yerleşik izlemeye sahiptir. Ancak, bir mantıksal uygulama, BizTalk Server, SQL Server ya da başka bir katman içeren iş akışları gibi, olayları başlangıçtan başlayarak iş akışlarının sonuna kadar günlüğe kaydeden özel izleme ayarlayabilirsiniz. Bu makale, mantıksal uygulamanızın dışındaki katmanlarda kullanabileceğiniz özel kod sağlar.

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
| sourceType | Yes | Dize | İzin verilen bu değerlere sahip çalıştırma kaynağı türü: `Microsoft.Logic/workflows`,`custom` |
| source | Yes | String veya JToken | Kaynak türü ise `Microsoft.Logic/workflows`, kaynak bilgilerinin bu şemayı izlemesi gerekir. Kaynak türü ise `custom`, şema bir jtoken olur. |
| SystemId | Yes | Dize | Mantıksal uygulama sistem KIMLIĞI |
| RunId | Yes | Dize | Mantıksal uygulama çalıştırma KIMLIĞI |
| operationName | Yes | Dize | İşlemin adı, örneğin eylem veya tetikleyici |
| repeatItemScopeName | Yes | Dize | Eylem bir `foreach`veya `until` döngü içindeyse öğe adını Yinele |
| repeatItemIndex | Yes | Tamsayı | Eylemin bir `foreach` veya `until` döngüsünün içinde olduğunu ve yinelenen öğe dizin numarası olduğunu gösterir. |
| Trackingıd | Hayır | Dize | İletileri ilişkilendirmek için izleme KIMLIĞI |
| correlationId | Hayır | Dize | İletilerle bağıntılı bağıntı KIMLIĞI |
| Clientrequestıd 'ye sahip | Hayır | Dize | İstemci bu özelliği bağıntılı iletilerle doldurabilir |
| eventLevel | Yes | Dize | Etkinliğin düzeyi |
| eventTime | Yes | DateTime | Olayın UTC biçiminde saati: *yyyy-MM-DDTHH: mm: ss. 00000Z* |
| recordType | Yes | Dize | Yalnızca bu izin verilen değere sahip parça kaydının türü:`custom` |
| kaydet | Yes | JToken | Yalnızca JToken biçimindeki özel kayıt türü |
|||||

## <a name="b2b-protocol-tracking-schemas"></a>B2B protokol izleme şemaları

B2B protokol izleme şemaları hakkında daha fazla bilgi için bkz.:

* [AS2 izleme şemaları](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [X12 izleme şemaları](logic-apps-track-integration-account-x12-tracking-schema.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure izleyici günlükleri Ile B2B iletilerini izleme](../logic-apps/monitor-b2b-messages-log-analytics.md) hakkında daha fazla bilgi edinin