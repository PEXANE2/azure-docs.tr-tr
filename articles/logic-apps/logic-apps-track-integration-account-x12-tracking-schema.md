---
title: B2B iletileri için x12 izleme şemaları
description: Azure Logic Apps için x12 iletilerini izlemek üzere izleme şemaları oluşturma
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 01/01/2020
ms.openlocfilehash: 5b2df194761ebc167e67498a985960a4fce35f19
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "76905294"
---
# <a name="create-schemas-for-tracking-x12-messages-in-azure-logic-apps"></a>Azure Logic Apps 'de izleme x12 iletileri için şemalar oluşturma

İşletmeden işletmeye (B2B) işlemleri için başarı, hata ve ileti özelliklerini izlemenize yardımcı olması için, tümleştirme hesabınızda bu x12 izleme şemalarını kullanabilirsiniz:

* X12 işlem kümesi izleme şeması
* X12 işlem kümesi bildirim izleme şeması
* X12 değişim izleme şeması
* X12 değişim bildirimi izleme şeması
* X12 işlevsel grup izleme şeması
* X12 işlevsel Grup onay izleme şeması

## <a name="x12-transaction-set-tracking-schema"></a>X12 işlem kümesi izleme şeması

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
| senderPartnerName | Hayır | Dize | X12 ileti göndericisinin iş ortağı adı |
| receiverPartnerName | Hayır | Dize | X12 ileti alıcısının iş ortağı adı |
| senderQualifier | Evet | Dize | İş ortağı niteleyicisi gönder |
| Senderıdentifier | Evet | Dize | İş ortağı tanımlayıcısı gönder |
| receiverQualifier | Evet | Dize | Alma ortağı niteleyicisi |
| Receiverıdentifier | Evet | Dize | İş ortağı tanımlayıcısını al |
| agreementName | Hayır | Dize | İletilerin çözümlenme x12 sözleşmesinin adı |
| yön | Evet | Sabit listesi | İleti akışının yönü, `receive` ya da`send` |
| interchangeControlNumber | Hayır | Dize | Değişim denetim numarası |
| functionalGroupControlNumber | Hayır | Dize | İşlevsel denetim numarası |
| transactionSetControlNumber | Hayır | Dize | İşlem kümesi denetim numarası |
| Correlationmessageıd | Hayır | Dize | {AgreementName} {*Groupcontrolnumber*} {Transactionsetcontrolnumber} birleşimi olan BAĞıNTı ileti kimliği |
| messageType | Hayır | Dize | İşlem kümesi veya belge türü |
| ımessagefailed | Evet | Boole | X12 iletisinin başarısız olup olmadığı |
| isTechnicalAcknowledgmentExpected | Evet | Boole | Teknik bildirim x12 anlaşmasında yapılandırılıp yapılandırılmadığını belirtir |
| isFunctionalAcknowledgmentExpected | Evet | Boole | X12 anlaşmasında işlevsel onay yapılandırılıp yapılandırılmadığını belirtir |
| needAk2LoopForValidMessages | Evet | Boole | AK2 döngüsünün geçerli bir ileti için gerekli olup olmadığı |
| segmentsCount | Hayır | Tamsayı | X12 işlem kümesindeki parçaların sayısı |
|||||

## <a name="x12-transaction-set-acknowledgment-tracking-schema"></a>X12 işlem kümesi bildirim izleme şeması

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
| senderPartnerName | Hayır | Dize | X12 ileti göndericisinin iş ortağı adı |
| receiverPartnerName | Hayır | Dize | X12 ileti alıcısının iş ortağı adı |
| senderQualifier | Evet | Dize | İş ortağı niteleyicisi gönder |
| Senderıdentifier | Evet | Dize | İş ortağı tanımlayıcısı gönder |
| receiverQualifier | Evet | Dize | Alma ortağı niteleyicisi |
| Receiverıdentifier | Evet | Dize | İş ortağı tanımlayıcısını al |
| agreementName | Hayır | Dize | İletilerin çözümlenme x12 sözleşmesinin adı |
| yön | Evet | Sabit listesi | İleti akışının yönü, `receive` ya da`send` |
| interchangeControlNumber | Hayır | Dize | İşlevsel bildirim değişim denetimi numarası. Değer yalnızca, iş ortağına gönderilen iletiler için işlevsel bildirim alınan gönderme tarafında doldurulur. |
| functionalGroupControlNumber | Hayır | Dize | İşlevsel onay için işlevsel Grup denetim numarası. Değer yalnızca, iş ortağına gönderilen iletiler için işlevsel onay değerinin alındığı gönderme tarafı için doldurulur |
| ısasegment | Hayır | Dize | İletinin ISA kesimi. Değer yalnızca, iş ortağına gönderilen iletiler için işlevsel onay değerinin alındığı gönderme tarafı için doldurulur |
| gsSegment | Hayır | Dize | İleti kesimini ekler. Değer yalnızca, iş ortağına gönderilen iletiler için işlevsel onay değerinin alındığı gönderme tarafı için doldurulur |
| respondingfunctionalGroupControlNumber | Hayır | Dize | Yanıt veren değişim denetim numarası |
| respondingFunctionalGroupId | Hayır | Dize | Bildirim içinde AK101 ile eşlenen, yanıt veren işlevsel Grup KIMLIĞI |
| respondingtransactionSetControlNumber | Hayır | Dize | Yanıt veren işlem kümesi denetim numarası |
| respondingTransactionSetId | Hayır | Dize | Bildirim içinde AK201 ile eşlenen yanıt veren işlem kümesi KIMLIĞI |
| Durum | Evet | Boole | İşlem kümesi bildirimi durum kodu |
| segmentsCount | Evet | Sabit listesi | İzin verilen bu değerlere sahip bilgilendirme durum kodu: `Accepted` , `Rejected` , ve`AcceptedWithErrors` |
| processingStatus | Evet | Sabit listesi | Bu izin verilen değerlerle bildirim işleme durumu: `Received` , `Generated` ve`Sent` |
| Correlationmessageıd | Hayır | Dize | {AgreementName} {*Groupcontrolnumber*} {Transactionsetcontrolnumber} birleşimi olan BAĞıNTı ileti kimliği |
| ımessagefailed | Evet | Boole | X12 iletisinin başarısız olup olmadığı |
| ak2Segment | Hayır | Dize | Alınan işlevsel grup içindeki bir işlem kümesi için bildirim |
| ak3Segment | Hayır | Dize | Bir veri kesimindeki hataları raporlar |
| ak5Segment | Hayır | Dize | AK2 segmentinde tanımlanan işlem kümesinin kabul edildiğini veya reddedildiğini ve neden olduğunu bildirir |
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
| senderPartnerName | Hayır | Dize | X12 ileti göndericisinin iş ortağı adı |
| receiverPartnerName | Hayır | Dize | X12 ileti alıcısının iş ortağı adı |
| senderQualifier | Evet | Dize | İş ortağı niteleyicisi gönder |
| Senderıdentifier | Evet | Dize | İş ortağı tanımlayıcısı gönder |
| receiverQualifier | Evet | Dize | Alma ortağı niteleyicisi |
| Receiverıdentifier | Evet | Dize | İş ortağı tanımlayıcısını al |
| agreementName | Hayır | Dize | İletilerin çözümlenme x12 sözleşmesinin adı |
| yön | Evet | Sabit listesi | İleti akışının yönü, `receive` ya da`send` |
| interchangeControlNumber | Hayır | Dize | Değişim denetim numarası |
| ısasegment | Hayır | Dize | İleti ISA segmenti |
| isTechnicalAcknowledgmentExpected | Boole | Teknik bildirim x12 anlaşmasında yapılandırılıp yapılandırılmadığını belirtir  |
| ımessagefailed | Evet | Boole | X12 iletisinin başarısız olup olmadığı |
| isa09 | Hayır | Dize | X12 belge değişim tarihi |
| isa10 | Hayır | Dize | X12 belge değişim saati |
| ısa11 | Hayır | Dize | X12 Interchange denetim standartları tanımlayıcısı |
| ISA12 | Hayır | Dize | X12 Interchange Control sürüm numarası |
| isa14 | Hayır | Dize | X12 onay istendi |
| isa15 | Hayır | Dize | Test veya üretim için gösterge |
| isa16 | Hayır | Dize | Öğe ayırıcısı |
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
| senderPartnerName | Hayır | Dize | X12 ileti göndericisinin iş ortağı adı |
| receiverPartnerName | Hayır | Dize | X12 ileti alıcısının iş ortağı adı |
| senderQualifier | Evet | Dize | İş ortağı niteleyicisi gönder |
| Senderıdentifier | Evet | Dize | İş ortağı tanımlayıcısı gönder |
| receiverQualifier | Evet | Dize | Alma ortağı niteleyicisi |
| Receiverıdentifier | Evet | Dize | İş ortağı tanımlayıcısını al |
| agreementName | Hayır | Dize | İletilerin çözümlenme x12 sözleşmesinin adı |
| yön | Evet | Sabit listesi | İleti akışının yönü, `receive` ya da`send` |
| interchangeControlNumber | Hayır | Dize | İş ortaklarından alınan teknik bildirim değişim denetimi numarası |
| ısasegment | Hayır | Dize | İş ortaklarından alınan teknik bildirim için ISA segmenti |
| respondingInterchangeControlNumber | Hayır | Dize | İş ortaklarından alınan teknik bildirim için değişim denetim numarası |
| ımessagefailed | Evet | Boole | X12 iletisinin başarısız olup olmadığı |
| Durum | Evet | Sabit listesi | Bu izin verilen değerlere sahip değişim bildirimi durum kodu: `Accepted` , `Rejected` , ve`AcceptedWithErrors` |
| processingStatus | Evet | Sabit listesi | İzin verilen bu değerlere sahip bildirim durumu: `Received` , `Generated` , ve`Sent` |
| ta102 | Hayır | Dize | Değişim tarihi |
| ta103 | Hayır | Dize | Değişim saati |
| ta105 | Hayır | Dize | Değişim notunun kodu |
|||||

## <a name="x12-functional-group-tracking-schema"></a>X12 işlevsel grup izleme şeması

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
| senderPartnerName | Hayır | Dize | X12 ileti göndericisinin iş ortağı adı |
| receiverPartnerName | Hayır | Dize | X12 ileti alıcısının iş ortağı adı |
| senderQualifier | Evet | Dize | İş ortağı niteleyicisi gönder |
| Senderıdentifier | Evet | Dize | İş ortağı tanımlayıcısı gönder |
| receiverQualifier | Evet | Dize | Alma ortağı niteleyicisi |
| Receiverıdentifier | Evet | Dize | İş ortağı tanımlayıcısını al |
| agreementName | Hayır | Dize | İletilerin çözümlenme x12 sözleşmesinin adı |
| yön | Evet | Sabit listesi | İleti akışının yönü, al veya Gönder |
| interchangeControlNumber | Hayır | Dize | Değişim denetim numarası |
| functionalGroupControlNumber | Hayır | Dize | İşlevsel denetim numarası |
| gsSegment | Hayır | Dize | İleti GS segmenti |
| isTechnicalAcknowledgmentExpected | Evet | Boole | Teknik bildirim x12 anlaşmasında yapılandırılıp yapılandırılmadığını belirtir |
| isFunctionalAcknowledgmentExpected | Evet | Boole | X12 anlaşmasında işlevsel onay yapılandırılıp yapılandırılmadığını belirtir |
| ımessagefailed | Evet | Boole | X12 iletisinin başarısız olup olmadığı |
| gs01 | Hayır | Dize | İşlevsel tanımlayıcı kodu |
| gs02 | Hayır | Dize | Uygulama göndericisinin kodu |
| gs03 | Hayır | Dize | Uygulama alıcısının kodu |
| gs04 | Hayır | Dize | İşlevsel grup tarihi |
| gs05 | Hayır | Dize | İşlevsel grup süresi |
| gs07 | Hayır | Dize | Sorumlu Kurumu kodu |
| gs08 | Hayır | Dize | Sürüm, sürüm veya sektör için tanımlayıcı kodu |
|||||

## <a name="x12-functional-group-acknowledgment-tracking-schema"></a>X12 işlevsel Grup onay izleme şeması

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
| senderPartnerName | Hayır | Dize | X12 ileti göndericisinin iş ortağı adı |
| receiverPartnerName | Hayır | Dize | X12 ileti alıcısının iş ortağı adı |
| senderQualifier | Evet | Dize | İş ortağı niteleyicisi gönder |
| Senderıdentifier | Evet | Dize | İş ortağı tanımlayıcısı gönder |
| receiverQualifier | Evet | Dize | Alma ortağı niteleyicisi |
| Receiverıdentifier | Evet | Dize | İş ortağı tanımlayıcısını al |
| agreementName | Hayır | Dize | İletilerin çözümlenme x12 sözleşmesinin adı |
| yön | Evet | Sabit listesi | İleti akışının yönü, `receive` ya da`send` |
| interchangeControlNumber | Hayır | Dize | İş ortaklarından teknik bir bildirim alındığında gönderme tarafı için de olan değişim denetim numarası |
| functionalGroupControlNumber | Hayır | Dize | İş ortaklarından teknik bir bildirim alındığında gönderme tarafına yönelik olan teknik bildirim 'nin işlevsel Grup denetim numarası |
| ısasegment | Hayır | Dize | Interchange Control numarasıyla aynıdır, ancak yalnızca belirli durumlarda doldurulur |
| gsSegment | Hayır | Dize | İşlevsel Grup denetim numarasıyla aynı ancak yalnızca belirli durumlarda doldurulmuş |
| respondingfunctionalGroupControlNumber | Hayır | Dize | Özgün işlevsel grubun denetim numarası |
| respondingFunctionalGroupId | Hayır | Dize | Bildirim işlev grubu KIMLIĞINDE AK101 eşlenir |
| ımessagefailed | Boole | X12 iletisinin başarısız olup olmadığı |
| Durum | Evet | Sabit listesi | İzin verilen bu değerlere sahip bilgilendirme durum kodu: `Accepted` , `Rejected` , ve`AcceptedWithErrors` |
| processingStatus | Evet | Sabit listesi | Bu izin verilen değerlerle bildirim işleme durumu: `Received` , `Generated` ve`Sent` |
| ak903 | Hayır | Dize | Alınan işlem kümesi sayısı |
| ak904 | Hayır | Dize | Tanımlanan işlevsel grupta kabul edilen işlem kümesi sayısı |
| ak9Segment | Hayır | Dize | AK1 segmentinde tanımlanan işlevsel grubun kabul edilip edilmediğini ve neden |
|||||

## <a name="b2b-protocol-tracking-schemas"></a>B2B protokol izleme şemaları

B2B protokol izleme şemaları hakkında daha fazla bilgi için bkz.:

* [AS2 izleme şemaları](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [B2B özel izleme şemaları](logic-apps-track-integration-account-custom-tracking-schema.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure İzleyici günlükleri ile B2B iletilerini izleme](../logic-apps/monitor-b2b-messages-log-analytics.md)