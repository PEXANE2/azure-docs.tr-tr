---
title: B2B iletileri için X12 izleme şemaları
description: Azure Mantık Uygulamaları için X12 iletilerini izlemek için izleme şemaları oluşturun
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 01/01/2020
ms.openlocfilehash: 5b2df194761ebc167e67498a985960a4fce35f19
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76905294"
---
# <a name="create-schemas-for-tracking-x12-messages-in-azure-logic-apps"></a>Azure Mantık Uygulamalarında X12 iletilerini izlemek için şemalar oluşturun

İşletmeler arası (B2B) işlemlerin başarılarını, hatalarını ve ileti özelliklerini izlemenize yardımcı olmak için, entegrasyon hesabınızda şu X12 izleme şemalarını kullanabilirsiniz:

* X12 işlem seti izleme şeması
* X12 işlem seti bildirim izleme şeması
* X12 değişim izleme şeması
* X12 değişim bildirimi izleme şeması
* X12 fonksiyonel grup izleme şeması
* X12 fonksiyonel grup bildirim izleme şeması

## <a name="x12-transaction-set-tracking-schema"></a>X12 işlem seti izleme şeması

```json
{
   "agreementProperties": {
      "senderPartnerName": "",
      "receiverPartnerName": "",
      "senderQualifier": "",
      "senderIdentifier": "",
      "receiverQualifier": "",
      "receiverIdentifier": "",
      "agreementName": ""
   },
   "messageProperties": {
      "direction": "",
      "interchangeControlNumber": "",
      "functionalGroupControlNumber": "",
      "transactionSetControlNumber": "",
      "CorrelationMessageId": "",
      "messageType": "",
      "isMessageFailed": "",
      "isTechnicalAcknowledgmentExpected": "",
      "isFunctionalAcknowledgmentExpected": "",
      "needAk2LoopForValidMessages": "",
      "segmentsCount": ""
   }
}
```

| Özellik | Gerekli | Tür | Açıklama |
|----------|----------|------|-------------|
| gönderenPartnerName | Hayır | Dize | X12 ileti gönderenin iş ortağı adı |
| receiverPartnerName | Hayır | Dize | X12 ileti alıcının ortak adı |
| gönderenEleme | Evet | Dize | İş ortağı niteleyicisini gönder |
| gönderenIdentifier | Evet | Dize | İş ortağı tanımlayıcısı gönder |
| receiverEleme | Evet | Dize | İş ortağı niteleyicisi alma |
| alıcıIdentifier | Evet | Dize | İş ortağı tanımlayıcısı alın |
| anlaşmaAdı | Hayır | Dize | İletilerin çözüldüğü X12 anlaşmasının adı |
| yön | Evet | Sabit Listesi | İleti akışının yönü, ya `receive` da`send` |
| interchangeControlNumber | Hayır | Dize | Değişim kontrol numarası |
| fonksiyonelGroupControlNumber | Hayır | Dize | Fonksiyonel kontrol numarası |
| işlemSetControlNumber | Hayır | Dize | Hareket ayarlı kontrol numarası |
| KorelasyonMessageId | Hayır | Dize | {AgreementName}{*GroupControlNumber*}{TransactionSetControlNumber} kombinasyonu olan korelasyon ileti kimliği |
| Messagetype | Hayır | Dize | Hareket kümesi veya belge türü |
| isMessageFailed | Evet | Boole | X12 iletisinin başarısız olup olmadığı |
| isTechnicalAcknowledgmentBeklenen | Evet | Boole | Teknik bildirimin X12 anlaşmasında yapılandırılıp yapılandırılması |
| isFunctionalAcknowledgmentBeklenen | Evet | Boole | Fonksiyonel bildirimin X12 anlaşmasında yapılandırılıp yapılandırılması |
| needAk2LoopForValidMessages | Evet | Boole | AK2 döngüsünün geçerli bir ileti için gerekli olup olmadığı |
| segmentlerSay | Hayır | Tamsayı | X12 hareket kümesindeki segment sayısı |
|||||

## <a name="x12-transaction-set-acknowledgment-tracking-schema"></a>X12 işlem seti bildirim izleme şeması

```json
{
   "agreementProperties": {
      "senderPartnerName": "",
      "receiverPartnerName": "",
      "senderQualifier": "",
      "senderIdentifier": "",
      "receiverQualifier": "",
      "receiverIdentifier": "",
      "agreementName": ""
   },
   "messageProperties": {
      "direction": "",
      "interchangeControlNumber": "",
      "functionalGroupControlNumber": "",
      "isaSegment": "",
      "gsSegment": "",
      "respondingfunctionalGroupControlNumber": "",
      "respondingFunctionalGroupId": "",
      "respondingtransactionSetControlNumber": "",
      "respondingTransactionSetId": "",
      "statusCode": "",
      "processingStatus": "",
      "CorrelationMessageId": "",
      "isMessageFailed": "",
      "ak2Segment": "",
      "ak3Segment": "",
      "ak5Segment": ""
   }
}
```

| Özellik | Gerekli | Tür | Açıklama |
|----------|----------|------|-------------|
| gönderenPartnerName | Hayır | Dize | X12 ileti gönderenin iş ortağı adı |
| receiverPartnerName | Hayır | Dize | X12 ileti alıcının ortak adı |
| gönderenEleme | Evet | Dize | İş ortağı niteleyicisini gönder |
| gönderenIdentifier | Evet | Dize | İş ortağı tanımlayıcısı gönder |
| receiverEleme | Evet | Dize | İş ortağı niteleyicisi alma |
| alıcıIdentifier | Evet | Dize | İş ortağı tanımlayıcısı alın |
| anlaşmaAdı | Hayır | Dize | İletilerin çözüldüğü X12 anlaşmasının adı |
| yön | Evet | Sabit Listesi | İleti akışının yönü, ya `receive` da`send` |
| interchangeControlNumber | Hayır | Dize | İşlevsel bildirimin değişim kontrol numarası. Değer yalnızca iş ortağına gönderilen iletiler için işlevsel bildirimin alındığı gönderme tarafı için doldurulur. |
| fonksiyonelGroupControlNumber | Hayır | Dize | Fonksiyonel bildirimin fonksiyonel grup kontrol sayısı. Değer, yalnızca iş ortağına gönderilen iletiler için işlevsel bildirimin alındığı gönderme tarafı için doldurulur |
| isaSegment | Hayır | Dize | İletinin ISA bölümü. Değer, yalnızca iş ortağına gönderilen iletiler için işlevsel bildirimin alındığı gönderme tarafı için doldurulur |
| gsSegment | Hayır | Dize | İletinin GS bölümü. Değer, yalnızca iş ortağına gönderilen iletiler için işlevsel bildirimin alındığı gönderme tarafı için doldurulur |
| respondingfunctionalGroupControlNumber | Hayır | Dize | Yanıt veren değişim kontrol numarası |
| yanıtFonksiyonelGroupId | Hayır | Dize | Bildirimde AK101 ile eşleyen yanıt veren işlevsel grup kimliği |
| respondingtransactionSetControlNumber | Hayır | Dize | Yanıt hareket kümesi kontrol numarası |
| yanıtİşlemSetiId | Hayır | Dize | Bildirimde AK201 ile eşleyen yanıt hareket kümesi kimliği |
| Statuscode | Evet | Boole | Hareket kümesi bildirim durum kodu |
| segmentlerSay | Evet | Sabit Listesi | İzin verilen bu değerlere sahip `Accepted` `Rejected`bildirim durum kodu: , ve`AcceptedWithErrors` |
| processingDurum | Evet | Sabit Listesi | Bu izin verilen değerlerle bildirimin `Received`işleme `Generated`durumu: , , ve`Sent` |
| KorelasyonMessageId | Hayır | Dize | {AgreementName}{*GroupControlNumber*}{TransactionSetControlNumber} kombinasyonu olan korelasyon ileti kimliği |
| isMessageFailed | Evet | Boole | X12 iletisinin başarısız olup olmadığı |
| ak2Segment | Hayır | Dize | Alınan işlevsel grup içinde bir hareket kümesi için bildirim |
| ak3Segment | Hayır | Dize | Veri segmentindeki hataları raporlar |
| ak5Segment | Hayır | Dize | AK2 segmentinde tanımlanan hareket kümesinin kabul edilip edilmemediğini ve neden reddedildiğini bildirir |
|||||

## <a name="x12-interchange-tracking-schema"></a>X12 değişim izleme şeması

```json
{
   "agreementProperties": {
      "senderPartnerName": "",
      "receiverPartnerName": "",
      "senderQualifier": "",
      "senderIdentifier": "",
      "receiverQualifier": "",
      "receiverIdentifier": "",
      "agreementName": ""
   },
   "messageProperties": {
      "direction": "",
      "interchangeControlNumber": "",
      "isaSegment": "",
      "isTechnicalAcknowledgmentExpected": "",
      "isMessageFailed": "",
      "isa09": "",
      "isa10": "",
      "isa11": "",
      "isa12": "",
      "isa14": "",
      "isa15": "",
      "isa16": ""
   }
}
```

| Özellik | Gerekli | Tür | Açıklama |
|----------|----------|------|-------------|
| gönderenPartnerName | Hayır | Dize | X12 ileti gönderenin iş ortağı adı |
| receiverPartnerName | Hayır | Dize | X12 ileti alıcının ortak adı |
| gönderenEleme | Evet | Dize | İş ortağı niteleyicisini gönder |
| gönderenIdentifier | Evet | Dize | İş ortağı tanımlayıcısı gönder |
| receiverEleme | Evet | Dize | İş ortağı niteleyicisi alma |
| alıcıIdentifier | Evet | Dize | İş ortağı tanımlayıcısı alın |
| anlaşmaAdı | Hayır | Dize | İletilerin çözüldüğü X12 anlaşmasının adı |
| yön | Evet | Sabit Listesi | İleti akışının yönü, ya `receive` da`send` |
| interchangeControlNumber | Hayır | Dize | Değişim kontrol numarası |
| isaSegment | Hayır | Dize | İleti ISA kesimi |
| isTechnicalAcknowledgmentBeklenen | Boole | Teknik bildirimin X12 anlaşmasında yapılandırılıp yapılandırılması  |
| isMessageFailed | Evet | Boole | X12 iletisinin başarısız olup olmadığı |
| isa09 | Hayır | Dize | X12 belge değişim tarihi |
| isa10 | Hayır | Dize | X12 belge değişim süresi |
| isa11 | Hayır | Dize | X12 değişim kontrol standartları tanımlayıcısı |
| isa12 | Hayır | Dize | X12 değişim kontrol sürüm numarası |
| isa14 | Hayır | Dize | X12 bildirimi istenir |
| isa15 | Hayır | Dize | Test veya üretim için gösterge |
| isa16 | Hayır | Dize | Eleman ayırıcısı |
|||||

## <a name="x12-interchange-acknowledgment-tracking-schema"></a>X12 değişim bildirimi izleme şeması

```json
{
   "agreementProperties": {
      "senderPartnerName": "",
      "receiverPartnerName": "",
      "senderQualifier": "",
      "senderIdentifier": "",
      "receiverQualifier": "",
      "receiverIdentifier": "",
      "agreementName": ""
   },
   "messageProperties": {
      "direction": "",
      "interchangeControlNumber": "",
      "isaSegment": "",
      "respondingInterchangeControlNumber": "",
      "isMessageFailed": "",
      "statusCode": "",
      "processingStatus": "",
      "ta102": "",
      "ta103": "",
      "ta105": ""
   }
}
```

| Özellik | Gerekli | Tür | Açıklama |
|----------|----------|------|-------------|
| gönderenPartnerName | Hayır | Dize | X12 ileti gönderenin iş ortağı adı |
| receiverPartnerName | Hayır | Dize | X12 ileti alıcının ortak adı |
| gönderenEleme | Evet | Dize | İş ortağı niteleyicisini gönder |
| gönderenIdentifier | Evet | Dize | İş ortağı tanımlayıcısı gönder |
| receiverEleme | Evet | Dize | İş ortağı niteleyicisi alma |
| alıcıIdentifier | Evet | Dize | İş ortağı tanımlayıcısı alın |
| anlaşmaAdı | Hayır | Dize | İletilerin çözüldüğü X12 anlaşmasının adı |
| yön | Evet | Sabit Listesi | İleti akışının yönü, ya `receive` da`send` |
| interchangeControlNumber | Hayır | Dize | Ortaklardan alınan teknik bildirimin değişim kontrol numarası |
| isaSegment | Hayır | Dize | İş ortaklarından alınan teknik bildirim için ISA segmenti |
| yanıtInterchangeControlNumber | Hayır | Dize | Ortaklardan alınan teknik bildirim için değişim kontrol numarası |
| isMessageFailed | Evet | Boole | X12 iletisinin başarısız olup olmadığı |
| Statuscode | Evet | Sabit Listesi | Bu izin verilen değerlerle değişim bildirim `Accepted` `Rejected`durum kodu: , , ve`AcceptedWithErrors` |
| processingDurum | Evet | Sabit Listesi | İzin verilen bu değerlerle `Received`bildirim `Generated`durumu: , , ve`Sent` |
| ta102 | Hayır | Dize | Değişim tarihi |
| ta103 | Hayır | Dize | Değişim süresi |
| ta105 | Hayır | Dize | Değişim notu kodu |
|||||

## <a name="x12-functional-group-tracking-schema"></a>X12 fonksiyonel grup izleme şeması

```json
{
   "agreementProperties": {
      "senderPartnerName": "",
      "receiverPartnerName": "",
      "senderQualifier": "",
      "senderIdentifier": "",
      "receiverQualifier": "",
      "receiverIdentifier": "",
      "agreementName": ""
   },
   "messageProperties": {
      "direction": "",
      "interchangeControlNumber": "",
      "functionalGroupControlNumber": "",
      "gsSegment": "",
      "isTechnicalAcknowledgmentExpected": "",
      "isFunctionalAcknowledgmentExpected": "",
      "isMessageFailed": "",
      "gs01": "",
      "gs02": "",
      "gs03": "",
      "gs04": "",
      "gs05": "",
      "gs07": "",
      "gs08": ""
   }
}
```

| Özellik | Gerekli | Tür | Açıklama |
|----------|----------|------|-------------|
| gönderenPartnerName | Hayır | Dize | X12 ileti gönderenin iş ortağı adı |
| receiverPartnerName | Hayır | Dize | X12 ileti alıcının ortak adı |
| gönderenEleme | Evet | Dize | İş ortağı niteleyicisini gönder |
| gönderenIdentifier | Evet | Dize | İş ortağı tanımlayıcısı gönder |
| receiverEleme | Evet | Dize | İş ortağı niteleyicisi alma |
| alıcıIdentifier | Evet | Dize | İş ortağı tanımlayıcısı alın |
| anlaşmaAdı | Hayır | Dize | İletilerin çözüldüğü X12 anlaşmasının adı |
| yön | Evet | Sabit Listesi | İleti akışının yönü, alma veya gönderme |
| interchangeControlNumber | Hayır | Dize | Değişim kontrol numarası |
| fonksiyonelGroupControlNumber | Hayır | Dize | Fonksiyonel kontrol numarası |
| gsSegment | Hayır | Dize | İleti GS kesimi |
| isTechnicalAcknowledgmentBeklenen | Evet | Boole | Teknik bildirimin X12 anlaşmasında yapılandırılıp yapılandırılması |
| isFunctionalAcknowledgmentBeklenen | Evet | Boole | Fonksiyonel bildirimin X12 anlaşmasında yapılandırılıp yapılandırılması |
| isMessageFailed | Evet | Boole | X12 iletisinin başarısız olup olmadığı |
| gs01 | Hayır | Dize | İşlevsel tanımlayıcı kodu |
| gs02 | Hayır | Dize | Uygulama gönderenin kodu |
| gs03 | Hayır | Dize | Uygulama alıcıkodu |
| gs04 | Hayır | Dize | Fonksiyonel grup tarihi |
| gs05 | Hayır | Dize | Fonksiyonel grup süresi |
| gs07 | Hayır | Dize | Sorumlu ajans kodu |
| gs08 | Hayır | Dize | Sürüm, sürüm veya endüstri için tanımlayıcı kodu |
|||||

## <a name="x12-functional-group-acknowledgment-tracking-schema"></a>X12 fonksiyonel grup bildirim izleme şeması

```json
{
   "agreementProperties": {
      "senderPartnerName": "",
      "receiverPartnerName": "",
      "senderQualifier": "",
      "senderIdentifier": "",
      "receiverQualifier": "",
      "receiverIdentifier": "",
      "agreementName": ""
   },
   "messageProperties": {
      "direction": "",
      "interchangeControlNumber": "",
      "functionalGroupControlNumber": "",
      "isaSegment": "",
      "gsSegment": "",
      "respondingfunctionalGroupControlNumber": "",
      "respondingFunctionalGroupId": "",
      "isMessageFailed": "",
      "statusCode": "",
      "processingStatus": "",
      "ak903": "",
      "ak904": "",
      "ak9Segment": ""
   }
}
```

| Özellik | Gerekli | Tür | Açıklama |
|----------|----------|------|-------------|
| gönderenPartnerName | Hayır | Dize | X12 ileti gönderenin iş ortağı adı |
| receiverPartnerName | Hayır | Dize | X12 ileti alıcının ortak adı |
| gönderenEleme | Evet | Dize | İş ortağı niteleyicisini gönder |
| gönderenIdentifier | Evet | Dize | İş ortağı tanımlayıcısı gönder |
| receiverEleme | Evet | Dize | İş ortağı niteleyicisi alma |
| alıcıIdentifier | Evet | Dize | İş ortağı tanımlayıcısı alın |
| anlaşmaAdı | Hayır | Dize | İletilerin çözüldüğü X12 anlaşmasının adı |
| yön | Evet | Sabit Listesi | İleti akışının yönü, ya `receive` da`send` |
| interchangeControlNumber | Hayır | Dize | Ortaklardan teknik bir bildirim alındığı zaman gönderen taraf için doldurulan değişim kontrol numarası |
| fonksiyonelGroupControlNumber | Hayır | Dize | İşortaklarından teknik bir bildirim alındığı zaman gönderme tarafı için doldurulan teknik bildirimin fonksiyonel grup kontrol numarası |
| isaSegment | Hayır | Dize | Değişim kontrol numarasıyla aynıdır, ancak yalnızca belirli durumlarda doldurulur |
| gsSegment | Hayır | Dize | İşlevsel grup kontrol numarasıyla aynıdır, ancak yalnızca belirli durumlarda doldurulur |
| respondingfunctionalGroupControlNumber | Hayır | Dize | Orijinal fonksiyonel grubun kontrol numarası |
| yanıtFonksiyonelGroupId | Hayır | Dize | Bildirim fonksiyonel grup kimliğinde AK101'e haritalar |
| isMessageFailed | Boole | X12 iletisinin başarısız olup olmadığı |
| Statuscode | Evet | Sabit Listesi | İzin verilen bu değerlere sahip `Accepted` `Rejected`bildirim durum kodu: , ve`AcceptedWithErrors` |
| processingDurum | Evet | Sabit Listesi | Bu izin verilen değerlerle bildirimin `Received`işleme `Generated`durumu: , , ve`Sent` |
| ak903 | Hayır | Dize | Alınan hareket kümelerinin sayısı |
| ak904 | Hayır | Dize | Tanımlanan işlevsel grupta kabul edilen hareket kümesi sayısı |
| ak9Segment | Hayır | Dize | AK1 segmentinde tanımlanan fonksiyonel grubun kabul edilip edilmediği veya reddedilip reddedilmediği ve neden |
|||||

## <a name="b2b-protocol-tracking-schemas"></a>B2B protokol izleme şemaları

B2B protokol izleme şemaları hakkında bilgi için bkz:

* [AS2 izleme şemaları](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [B2B özel izleme şemaları](logic-apps-track-integration-account-custom-tracking-schema.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure İzleyici günlükleri ile B2B iletilerini izleme](../logic-apps/monitor-b2b-messages-log-analytics.md)