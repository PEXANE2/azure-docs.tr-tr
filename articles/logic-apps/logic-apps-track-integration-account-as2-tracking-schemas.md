---
title: B2B iletileri için AS2 izleme şemaları
description: Azure Logic Apps 'de AS2 iletilerini izlemek için izleme şemaları oluşturma
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 01/01/2020
ms.openlocfilehash: bccf69362279afd9e8148b20b61ff3ea9b472a03
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2020
ms.locfileid: "76906965"
---
# <a name="create-schemas-for-tracking-as2-messages-in-azure-logic-apps"></a>Azure Logic Apps 'de izleme AS2 iletileri için şemalar oluşturma

İşletmeden işletmeye (B2B) işlemleri için başarı, hata ve ileti özelliklerini izlemenize yardımcı olması için, tümleştirme hesabınızda bu AS2 izleme şemalarını kullanabilirsiniz:

* AS2 ileti izleme şeması
* AS2 Ileti değerlendirmesi bildirimi (MDN) izleme şeması

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

| Özellik | Gereklidir | Tür | Açıklama |
|----------|----------|------|-------------|
| senderPartnerName | Hayır | Dize | AS2 ileti göndericisinin iş ortağı adı |
| receiverPartnerName | Hayır | Dize | AS2 ileti alıcısının iş ortağı adı |
| as2To | Evet | Dize | AS2 iletisinin üst bilgilerinden AS2 ileti alıcısının adı |
| as2From | Evet | Dize | AS2 iletisinin üst bilgilerinden AS2 ileti gönderenin adı |
| agreementName | Hayır | Dize | İletilerin çözümlenme AS2 sözleşmesinin adı |
| yön | Evet | Dize | İleti akışının yönü, yani `receive` ya da `send` |
| Ileti | Hayır | Dize | AS2 iletisinin üst bilgilerinden AS2 ileti KIMLIĞI |
| dispositionType | Hayır | Dize | İleti değerlendirmesi bildirimi (MDN) değerlendirme türü değeri |
| fileName | Hayır | Dize | AS2 iletisinin üstbilgisindeki dosya adı |
| ımessagefailed | Evet | Boole | AS2 iletisinin başarısız olup olmadığı |
| ısmessagesigned | Evet | Boole | AS2 iletisinin imzalı olup olmadığı |
| ımessageşifrelendi | Evet | Boole | AS2 iletisinin şifreli olup olmadığı |
| ımessagecompressed | Evet | Boole | AS2 iletisinin sıkıştırılmış olup olmadığı |
| correlationMessageId | Hayır | Dize | AS2 ileti KIMLIĞI, iletileri MDNs ile ilişkilendirmek için |
| ıncomingheaders | Hayır | JToken sözlüğü | Gelen AS2 ileti üst bilgisi ayrıntıları |
| outgoingHeaders | Hayır | JToken sözlüğü | Giden AS2 ileti üst bilgisi ayrıntıları |
| isNrrEnabled | Evet | Boole | Değer bilinmiyorsa varsayılan değerin kullanılıp kullanılmayacağını belirtir |
| ımdnexted | Evet | Boole | Değer bilinmiyorsa varsayılan değerin kullanılıp kullanılmayacağını belirtir |
| mdnType | Evet | Sabit Listesi | İzin verilen değerler: `NotConfigured`, `Sync`ve `Async` |
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

| Özellik | Gereklidir | Tür | Açıklama |
|----------|----------|------|-------------|
| senderPartnerName | Hayır | Dize | AS2 ileti göndericisinin iş ortağı adı |
| receiverPartnerName | Hayır | Dize | AS2 ileti alıcısının iş ortağı adı |
| as2To | Evet | Dize | AS2 iletisini alan iş ortağı adı |
| as2From | Evet | Dize | AS2 iletisini gönderen iş ortağı adı |
| agreementName | Hayır | Dize | İletilerin çözümlenme AS2 sözleşmesinin adı |
| yön | Evet | Dize | İleti akışının yönü, yani `receive` ya da `send` |
| Ileti | Hayır | Dize | AS2 ileti KIMLIĞI |
| originalMessageId | Hayır | Dize | AS2 özgün ileti KIMLIĞI |
| dispositionType | Hayır | Dize | MDN değerlendirme türü değeri |
| ımessagefailed | Evet | Boole | AS2 iletisinin başarısız olup olmadığı |
| ısmessagesigned | Evet | Boole | AS2 iletisinin imzalı olup olmadığı |
| isNrrEnabled | Evet | Boole | Değer bilinmiyorsa varsayılan değerin kullanılıp kullanılmayacağını belirtir |
| Durum | Evet | Sabit Listesi | İzin verilen değerler: `Accepted`, `Rejected`ve `AcceptedWithErrors` |
| Micdoğrulamaları Icationstatus | Evet | Sabit Listesi | İzin verilen değerler:`NotApplicable`, `Succeeded`ve `Failed` |
| correlationMessageId | Hayır | Dize | MDN yapılandırılmış özgün iletinin KIMLIĞI olan bağıntı KIMLIĞI |
| ıncomingheaders | Hayır | JToken sözlüğü | Gelen ileti üst bilgisi ayrıntıları |
| outgoingHeaders | Hayır | JToken sözlüğü | Giden ileti üst bilgisi ayrıntıları |
|||||

## <a name="b2b-protocol-tracking-schemas"></a>B2B protokol izleme şemaları

B2B protokol izleme şemaları hakkında daha fazla bilgi için bkz.:

* [X12 izleme şemaları](logic-apps-track-integration-account-x12-tracking-schema.md)
* [B2B özel izleme şemaları](logic-apps-track-integration-account-custom-tracking-schema.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Izleyici günlükleri ile B2B iletilerini izleme](../logic-apps/monitor-b2b-messages-log-analytics.md)