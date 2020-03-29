---
title: B2B iletileri için AS2 izleme şemaları
description: Azure Logic Apps'ta AS2 iletilerini izlemek için izleme şemaları oluşturun
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 01/01/2020
ms.openlocfilehash: bccf69362279afd9e8148b20b61ff3ea9b472a03
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76906965"
---
# <a name="create-schemas-for-tracking-as2-messages-in-azure-logic-apps"></a>Azure Mantık Uygulamalarında AS2 iletilerini izlemek için şemalar oluşturun

İşletmeler arası (B2B) işlemlerin başarılarını, hatalarını ve ileti özelliklerini izlemenize yardımcı olmak için, entegrasyon hesabınızda şu AS2 izleme şemalarını kullanabilirsiniz:

* AS2 ileti izleme şeması
* AS2 İleti Düzenleme Bildirimi (MDN) izleme şeması

## <a name="as2-message-tracking-schema"></a>AS2 ileti izleme şeması

```json
{
   "agreementProperties": {
      "senderPartnerName": "",
      "receiverPartnerName": "",
      "as2To": "",
      "as2From": "",
      "agreementName": ""
   },
   "messageProperties": {
      "direction": "",
      "messageId": "",
      "dispositionType": "",
      "fileName": "",
      "isMessageFailed": "",
      "isMessageSigned": "",
      "isMessageEncrypted": "",
      "isMessageCompressed": "",
      "correlationMessageId": "",
      "incomingHeaders": {},
      "outgoingHeaders": {},
      "isNrrEnabled": "",
      "isMdnExpected": "",
      "mdnType": ""
    }
}
```

| Özellik | Gerekli | Tür | Açıklama |
|----------|----------|------|-------------|
| gönderenPartnerName | Hayır | Dize | AS2 ileti gönderenin iş ortağı adı |
| receiverPartnerName | Hayır | Dize | AS2 ileti alıcının ortak adı |
| as2To | Evet | Dize | AS2 iletisinin üsterlerinden AS2 ileti alıcısının adı |
| as2Kaynak | Evet | Dize | AS2 iletisinin üsterlerinden GÖNDERENin adı |
| anlaşmaAdı | Hayır | Dize | İletilerin çözüldüğü AS2 anlaşmasının adı |
| yön | Evet | Dize | İleti akışının yönü, ya `receive` da`send` |
| Messageıd | Hayır | Dize | AS2 iletisinin üsterlerinden AS2 ileti kimliği |
| dispositionType | Hayır | Dize | İleti Disposition Bildirimi (MDN) disposition türü değeri |
| fileName | Hayır | Dize | AS2 iletisinin üstbilgisinden dosya adı |
| isMessageFailed | Evet | Boole | AS2 iletisinin başarısız olup olmadığı |
| isMessageSigned | Evet | Boole | AS2 iletisinin imzalanıp imzalanmadığını |
| isMessageEncrypted | Evet | Boole | AS2 iletisinin şifrelenip şifrelenmediğini |
| isMessageCompressed | Evet | Boole | AS2 iletisinin sıkıştırılıp sıkıştırılmadığı |
| correlationMessageId | Hayır | Dize | İletileri MDN'lerle ilişkilendirmek için AS2 ileti kimliği |
| gelen Headers | Hayır | JToken Sözlüğü | Gelen AS2 ileti üstbilgisi ayrıntıları |
| giden Headers | Hayır | JToken Sözlüğü | Giden AS2 ileti üstbilgi ayrıntıları |
| isNrrEnabled | Evet | Boole | Değer bilinmiyorsa varsayılan değerin kullanılıp kullanılmayacağı |
| isMdnBeklenen | Evet | Boole | Değer bilinmiyorsa varsayılan değerin kullanılıp kullanılmayacağı |
| mdnTip | Evet | Sabit Listesi | İzin verilen `NotConfigured` `Sync`değerler: , , ve`Async` |
|||||

## <a name="as2-mdn-tracking-schema"></a>AS2 MDN izleme şeması

```json
{
   "agreementProperties": {
      "senderPartnerName": "",
      "receiverPartnerName": "",
      "as2To": "",
      "as2From": "",
      "agreementName": ""
   },
   "messageProperties": {
      "direction": "",
      "messageId": "",
      "originalMessageId": "",
      "dispositionType": "",
      "isMessageFailed": "",
      "isMessageSigned": "",
      "isNrrEnabled": "",
      "statusCode": "",
      "micVerificationStatus": "",
      "correlationMessageId": "",
      "incomingHeaders": {
      },
      "outgoingHeaders": {
      }
   }
}
```

| Özellik | Gerekli | Tür | Açıklama |
|----------|----------|------|-------------|
| gönderenPartnerName | Hayır | Dize | AS2 ileti gönderenin iş ortağı adı |
| receiverPartnerName | Hayır | Dize | AS2 ileti alıcının ortak adı |
| as2To | Evet | Dize | AS2 iletisini alan iş ortağı adı |
| as2Kaynak | Evet | Dize | AS2 iletisini gönderen iş ortağı adı |
| anlaşmaAdı | Hayır | Dize | İletilerin çözüldüğü AS2 anlaşmasının adı |
| yön | Evet | Dize | İleti akışının yönü, ya `receive` da`send` |
| Messageıd | Hayır | Dize | AS2 ileti kimliği |
| originalMessageId | Hayır | Dize | AS2 orijinal ileti kimliği |
| dispositionType | Hayır | Dize | MDN disposition türü değeri |
| isMessageFailed | Evet | Boole | AS2 iletisinin başarısız olup olmadığı |
| isMessageSigned | Evet | Boole | AS2 iletisinin imzalanıp imzalanmadığını |
| isNrrEnabled | Evet | Boole | Değer bilinmiyorsa varsayılan değerin kullanılıp kullanılmayacağı |
| Statuscode | Evet | Sabit Listesi | İzin verilen `Accepted` `Rejected`değerler: , , ve`AcceptedWithErrors` |
| micVerificationStatus | Evet | Sabit Listesi | İzin verilen`NotApplicable` `Succeeded`değerler: , , ve`Failed` |
| correlationMessageId | Hayır | Dize | MDN yapılandırılmış orijinal iletinin kimliği olan Korelasyon Kimliği |
| gelen Headers | Hayır | JToken Sözlüğü | Gelen ileti üstbilgisi ayrıntıları |
| giden Headers | Hayır | JToken Sözlüğü | Giden ileti üstbilgi ayrıntıları |
|||||

## <a name="b2b-protocol-tracking-schemas"></a>B2B protokol izleme şemaları

B2B protokol izleme şemaları hakkında bilgi için bkz:

* [X12 izleme şemaları](logic-apps-track-integration-account-x12-tracking-schema.md)
* [B2B özel izleme şemaları](logic-apps-track-integration-account-custom-tracking-schema.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure İzleyici günlükleri ile B2B iletilerini izleme](../logic-apps/monitor-b2b-messages-log-analytics.md)