---
title: B2B iletileri için özel izleme şemaları
description: Enterprise Integration Pack Azure Logic Apps için tümleştirme hesaplarında B2B iletilerini izleyen özel izleme şemaları oluşturun
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 01/27/2017
ms.openlocfilehash: 7d7c5ef9e9a86c8b061a56fe41c0c8bbfc5ddbb3
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74792786"
---
# <a name="create-custom-tracking-schemas-that-monitor-end-to-end-workflows-in-azure-logic-apps"></a>Azure Logic Apps içinde uçtan uca iş akışlarını izleyen özel izleme şemaları oluşturma

AS2 veya x12 iletilerini izleme gibi işletmeden işletmeye iş akışınızın farklı bölümlerinde etkinleştirebileceğiniz yerleşik izleme vardır. Bir mantıksal uygulama, BizTalk Server, SQL Server veya başka bir katman içeren iş akışları oluşturduğunuzda, olayları iş akışınızın başından sonuna kadar günlüğe kaydeden özel izlemeyi etkinleştirebilirsiniz. 

Bu makale, mantıksal uygulamanızın dışındaki katmanlarda kullanabileceğiniz özel kod sağlar. 

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
         "repeatItemIndex": "",
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
         "record": {                
         }
      }
   ]
}
```

| Özellik | Gereklidir | Tür | Açıklama |
| --- | --- | --- | --- |
| sourceType | Yes |   | Çalışma kaynağının türü. İzin verilen değerler **Microsoft. Logic/iş akışları** ve **özel**. |
| source | Yes |   | Kaynak türü **Microsoft. Logic/iş akışlarıyla**, kaynak bilgilerinin bu şemayı izlemesi gerekir. Kaynak türü **özel**ise, şema bir jtoken olur. |
| SystemId | Yes | Dize | Mantıksal uygulama sistemi KIMLIĞI. |
| RunId | Yes | Dize | Mantıksal uygulama çalıştırma KIMLIĞI. |
| operationName | Yes | Dize | İşlemin adı (örneğin, eylem veya tetikleyici). |
| repeatItemScopeName | Yes | Dize | Eylem `foreach`/`until` döngüsünün içindeyse öğe adını tekrarlayın. |
| repeatItemIndex | Yes | Tamsayı | Eylemin `foreach`/`until` döngüsünün içinde olup olmadığı. Yinelenen öğe dizinini gösterir. |
| Trackingıd | Hayır | Dize | İletileri ilişkilendirmek için izleme KIMLIĞI. |
| correlationId | Hayır | Dize | İlişki KIMLIĞI, iletilerin ilişkilendirilmesi. |
| Clientrequestıd 'ye sahip | Hayır | Dize | İstemci, iletileri ilişkilendirmek için onu doldurabilir. |
| eventLevel | Yes |   | Etkinliğin düzeyi. |
| eventTime | Yes |   | Etkinliğin saati, UTC biçiminde YYYY-MM-DDTHH: MM: SS. 00000Z. |
| RecordType | Yes |   | İzleme kaydının türü. İzin verilen değer **özel**. |
| kayıtlar | Yes |   | Özel kayıt türü. İzin verilen biçim JToken. |
||||

## <a name="b2b-protocol-tracking-schemas"></a>B2B protokol izleme şemaları

B2B protokol izleme şemaları hakkında daha fazla bilgi için bkz.:

* [AS2 izleme şemaları](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)   
* [X12 izleme şemaları](logic-apps-track-integration-account-x12-tracking-schema.md)

## <a name="next-steps"></a>Sonraki adımlar

* [B2B iletilerini izleme](logic-apps-monitor-b2b-message.md) hakkında daha fazla bilgi edinin
* [Azure izleyici GÜNLÜKLERINDE B2B iletilerini izleme](../logic-apps/logic-apps-track-b2b-messages-omsportal.md) hakkında bilgi edinin
