---
title: B2B iletileri için AS2 izleme şemaları
description: Enterprise Integration Pack Azure Logic Apps için tümleştirme hesaplarında B2B iletilerini izleyen AS2 izleme şemaları oluşturma
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 01/27/2017
ms.openlocfilehash: 515d7cfc985ee9929f70de2c862170ff79ae4d60
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74792815"
---
# <a name="create-schemas-for-tracking-as2-messages-and-mdns-in-integration-accounts-for-azure-logic-apps"></a>Azure Logic Apps tümleştirme hesaplarında AS2 iletileri ve MDNs 'leri izlemek için şemalar oluşturun

İşletmeden işletmeye (B2B) işlemleri için başarı, hata ve ileti özelliklerini izlemenize yardımcı olması için, tümleştirme hesabınızda bu AS2 izleme şemalarını kullanabilirsiniz:

* AS2 ileti izleme şeması
* AS2 MDN izleme şeması

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
      "incomingHeaders": {
       },
      "outgoingHeaders": {
       },
      "isNrrEnabled": "",
      "isMdnExpected": "",
      "mdnType": ""
    }
}
```

| Özellik | Tür | Açıklama |
| --- | --- | --- |
| senderPartnerName | Dize | AS2 ileti göndericisinin iş ortağı adı. Seçim |
| receiverPartnerName | Dize | AS2 ileti alıcısının iş ortağı adı. Seçim |
| as2To | Dize | AS2 iletisinin üst bilgilerinden AS2 ileti alıcısının adı. Girilmesi |
| as2From | Dize | AS2 iletisinin üst bilgilerinden AS2 ileti gönderici adı. Girilmesi |
| agreementName | Dize | İletilerin çözümlenme AS2 sözleşmesinin adı. Seçim |
| yön | Dize | İleti akışının yönü, alma veya gönderme. Girilmesi |
| Ileti | Dize | AS2 iletisinin üst bilgilerinden AS2 ileti KIMLIĞI (Isteğe bağlı) |
| dispositionType |Dize | İleti değerlendirmesi bildirimi (MDN) değerlendirme türü değeri. Seçim |
| fileName | Dize | Dosya adı, AS2 iletisinin üst bilgisinden. Seçim |
| ımessagefailed |Boole | AS2 iletisinin başarısız olup olmadığı. Girilmesi |
| ısmessagesigned | Boole | AS2 iletisinin imzalı olup olmadığı. Girilmesi |
| ımessageşifrelendi | Boole | AS2 iletisinin şifrelenip şifrelenmediği. Girilmesi |
| ımessagecompressed |Boole | AS2 iletisinin sıkıştırılıp sıkıştırılmayacağı. Girilmesi |
| Correlationmessageıd | Dize | AS2 ileti KIMLIĞI, iletileri MDNs ile ilişkilendirmek için. Seçim |
| ıncomingheaders |JToken sözlüğü | Gelen AS2 ileti üst bilgisi ayrıntıları. Seçim |
| outgoingHeaders |JToken sözlüğü | Giden AS2 ileti üst bilgisi ayrıntıları. Seçim |
| isNrrEnabled | Boole | Değer bilinmiyorsa varsayılan değeri kullanın. Girilmesi |
| ımdnexted | Boole | Değer bilinmiyorsa varsayılan değeri kullanın. Girilmesi |
| mdnType | Sabit Listesi | İzin verilen değerler **notconfigured**, **Sync**ve **Async**. Girilmesi |
||||

## <a name="as2-mdn-tracking-schema"></a>AS2 MDN izleme şeması

```json
{
   "agreementProperties": {
      "senderPartnerName": "",
      "receiverPartnerName": "",
      "as2To": "",
      "as2From": "",
      "agreementName": "g"
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

| Özellik | Tür | Açıklama |
| --- | --- | --- |
| senderPartnerName | Dize | AS2 ileti göndericisinin iş ortağı adı. Seçim |
| receiverPartnerName | Dize | AS2 ileti alıcısının iş ortağı adı. Seçim |
| as2To | Dize | AS2 iletisini alan iş ortağı adı. Girilmesi |
| as2From | Dize | AS2 iletisini gönderen iş ortağı adı. Girilmesi |
| agreementName | Dize | İletilerin çözümlenme AS2 sözleşmesinin adı. Seçim |
| yön |Dize | İleti akışının yönü, alma veya gönderme. Girilmesi |
| Ileti | Dize | AS2 ileti KIMLIĞI. Seçim |
| Originalmessageıd |Dize | AS2 özgün ileti KIMLIĞI. Seçim |
| dispositionType | Dize | MDN değerlendirme türü değeri. Seçim |
| ımessagefailed |Boole | AS2 iletisinin başarısız olup olmadığı. Girilmesi |
| ısmessagesigned |Boole | AS2 iletisinin imzalı olup olmadığı. Girilmesi |
| isNrrEnabled | Boole | Değer bilinmiyorsa varsayılan değeri kullanın. Girilmesi |
| Durum | Sabit Listesi | İzin verilen değerler **kabul**edilir, **reddedilir**ve **acceptedwitherrors**. Girilmesi |
| Micdoğrulamaları Icationstatus | Sabit Listesi | İzin verilen değerler **notapplıcable**, **başarılı**ve **başarısız**. Girilmesi |
| Correlationmessageıd | Dize | Bağıntı KIMLIĞI. Özgün ileti oluşturma KIMLIĞI (MDN 'nin yapılandırıldığı iletinin ileti KIMLIĞI). Seçim |
| ıncomingheaders | JToken sözlüğü | Gelen ileti üst bilgisi ayrıntılarını gösterir. Seçim |
| outgoingHeaders |JToken sözlüğü | Giden ileti üst bilgisi ayrıntılarını gösterir. Seçim |
||||

## <a name="b2b-protocol-tracking-schemas"></a>B2B protokol izleme şemaları

B2B protokol izleme şemaları hakkında daha fazla bilgi için bkz.:

* [X12 izleme şemaları](logic-apps-track-integration-account-x12-tracking-schema.md)
* [B2B özel izleme şemaları](logic-apps-track-integration-account-custom-tracking-schema.md)

## <a name="next-steps"></a>Sonraki adımlar

* [B2B iletilerini izleme](logic-apps-monitor-b2b-message.md) hakkında bilgi edinin
* [Azure izleyici GÜNLÜKLERINDE B2B iletilerini izleme](../logic-apps/logic-apps-track-b2b-messages-omsportal.md) hakkında bilgi edinin