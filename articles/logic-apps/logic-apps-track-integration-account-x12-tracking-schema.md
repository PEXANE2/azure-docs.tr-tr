---
title: B2B iletileri için x12 izleme şemaları
description: Enterprise Integration Pack Azure Logic Apps için tümleştirme hesaplarında B2B iletilerini izleyen x12 izleme şemaları oluşturma
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 01/27/2017
ms.openlocfilehash: 6f2356600f5b6a637da731c650b26d968092e2f6
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74791718"
---
# <a name="create-schemas-for-tracking-x12-messages-in-integration-accounts-for-azure-logic-apps"></a>Azure Logic Apps tümleştirme hesaplarında x12 iletilerini izlemek için şemalar oluşturun

İşletmeden işletmeye (B2B) işlemleri için başarı, hata ve ileti özelliklerini izlemenize yardımcı olması için, tümleştirme hesabınızda bu x12 izleme şemalarını kullanabilirsiniz:

* X12 işlem kümesi izleme şeması
* X12 işlem kümesi onay izleme şeması
* X12 değişim izleme şeması
* X12 değişim onayı izleme şeması
* X12 işlevsel grup izleme şeması
* X12 işlevsel grup onayı izleme şeması

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
      "needAk2LoopForValidMessages":  "",
      "segmentsCount": ""
   }
}
```

| Özellik | Tür | Açıklama |
| --- | --- | --- |
| senderPartnerName | Dize | X12 ileti göndericisinin iş ortağı adı. Seçim |
| receiverPartnerName | Dize | X12 ileti alıcısının iş ortağı adı. Seçim |
| senderQualifier | Dize | İş ortağı niteleyicisi gönderin. Girilmesi |
| Senderıdentifier | Dize | İş ortağı tanımlayıcısı gönderin. Girilmesi |
| receiverQualifier | Dize | İş ortağı niteleyicisi alma. Girilmesi |
| Receiverıdentifier | Dize | İş ortağı tanımlayıcısını al. Girilmesi |
| agreementName | Dize | İletilerin çözümlenme x12 sözleşmesinin adı. Seçim |
| yön | Sabit Listesi | İleti akışının yönü, alma veya gönderme. Girilmesi |
| interchangeControlNumber | Dize | Değişim denetim numarası. Seçim |
| functionalGroupControlNumber | Dize | İşlevsel denetim numarası. Seçim |
| transactionSetControlNumber | Dize | İşlem kümesi denetim numarası. Seçim |
| Correlationmessageıd | Dize | Bağıntı ileti KIMLIĞI. {AgreementName} {*Groupcontrolnumber*} {Transactionsetcontrolnumber} birleşimi. Seçim |
| messageType | Dize | İşlem kümesi veya belge türü. Seçim |
| ımessagefailed | Boole | X12 iletisinin başarısız olup olmadığı. Girilmesi |
| isTechnicalAcknowledgmentExpected | Boole | Teknik bildirimle x12 anlaşmasında yapılandırılıp yapılandırılmadığını belirtir. Girilmesi |
| isFunctionalAcknowledgmentExpected | Boole | X12 anlaşmasında işlevsel onay yapılandırılıp yapılandırılmadığını belirtir. Girilmesi |
| needAk2LoopForValidMessages | Boole | Geçerli bir ileti için AK2 döngüsünün gerekli olup olmadığı. Girilmesi |
| segmentsCount | Tamsayı | X12 işlem kümesindeki parçaların sayısı. Seçim |
||||

## <a name="x12-transaction-set-acknowledgement-tracking-schema"></a>X12 işlem kümesi onay izleme şeması

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

| Özellik | Tür | Açıklama |
| --- | --- | --- |
| senderPartnerName | Dize | X12 ileti göndericisinin iş ortağı adı. Seçim |
| receiverPartnerName | Dize | X12 ileti alıcısının iş ortağı adı. Seçim |
| senderQualifier | Dize | İş ortağı niteleyicisi gönderin. Girilmesi |
| Senderıdentifier | Dize | İş ortağı tanımlayıcısı gönderin. Girilmesi |
| receiverQualifier | Dize | İş ortağı niteleyicisi alma. Girilmesi |
| Receiverıdentifier | Dize | İş ortağı tanımlayıcısını al. Girilmesi |
| agreementName | Dize | İletilerin çözümlenme x12 sözleşmesinin adı. Seçim |
| yön | Sabit Listesi | İleti akışının yönü, alma veya gönderme. Girilmesi |
| interchangeControlNumber | Dize | İşlevsel bildirimle değişim denetimi numarası. Değer yalnızca, iş ortağına gönderilen iletiler için işlevsel onay alındığı gönderme tarafı için doldurulur. Seçim |
| functionalGroupControlNumber | Dize | İşlevsel onay için işlevsel Grup denetim numarası. Değer yalnızca, iş ortağına gönderilen iletiler için işlevsel onay alındığı gönderme tarafı için doldurulur. Seçim |
| ısasegment | Dize | İletinin ISA kesimi. Değer yalnızca, iş ortağına gönderilen iletiler için işlevsel onay alındığı gönderme tarafı için doldurulur. Seçim |
| gsSegment | Dize | İleti kesimini ekler. Değer yalnızca, iş ortağına gönderilen iletiler için işlevsel onay alındığı gönderme tarafı için doldurulur. Seçim |
| respondingfunctionalGroupControlNumber | Dize | Değişim denetim numarası yanıt veriyor. Seçim |
| respondingFunctionalGroupId | Dize | Onay içinde AK101 ile eşlenen işlevsel Grup KIMLIĞI yanıt veriyor. Seçim |
| respondingtransactionSetControlNumber | Dize | İşlem kümesi denetim numarası yanıt veriyor. Seçim |
| respondingTransactionSetId | Dize | Onay içinde AK201 ile eşlenen işlem kümesi KIMLIĞI yanıt veriyor. Seçim |
| Durum | Boole | İşlem kümesi onayı durum kodu. Girilmesi |
| segmentsCount | Sabit Listesi | Onay durum kodu. İzin verilen değerler **kabul**edilir, **reddedilir**ve **acceptedwitherrors**. Girilmesi |
| processingStatus | Sabit Listesi | Bildirim işleme durumu. İzin verilen değerler **alınır**, **oluşturulur**ve **gönderilir**. Girilmesi |
| Correlationmessageıd | Dize | Bağıntı ileti KIMLIĞI. {AgreementName} {*Groupcontrolnumber*} {Transactionsetcontrolnumber} birleşimi. Seçim |
| ımessagefailed | Boole | X12 iletisinin başarısız olup olmadığı. Girilmesi |
| ak2Segment | Dize | Alınan işlev grubu içindeki bir işlem kümesi için onay alındı. Seçim |
| ak3Segment | Dize | Bir veri kesimindeki hataları bildirir. Seçim |
| ak5Segment | Dize | AK2 segmentinde tanımlanan işlem kümesinin kabul edildiğini veya reddedildiğini ve neden olduğunu bildirir. Seçim |
||||

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

| Özellik | Tür | Açıklama |
| --- | --- | --- |
| senderPartnerName | Dize | X12 ileti göndericisinin iş ortağı adı. Seçim |
| receiverPartnerName | Dize | X12 ileti alıcısının iş ortağı adı. Seçim |
| senderQualifier | Dize | İş ortağı niteleyicisi gönderin. Girilmesi |
| Senderıdentifier | Dize | İş ortağı tanımlayıcısı gönderin. Girilmesi |
| receiverQualifier | Dize | İş ortağı niteleyicisi alma. Girilmesi |
| Receiverıdentifier | Dize | İş ortağı tanımlayıcısını al. Girilmesi |
| agreementName | Dize | İletilerin çözümlenme x12 sözleşmesinin adı. Seçim |
| yön | Sabit Listesi | İleti akışının yönü, alma veya gönderme. Girilmesi |
| interchangeControlNumber | Dize | Değişim denetim numarası. Seçim |
| ısasegment | Dize | İleti ISA segmenti. Seçim |
| isTechnicalAcknowledgmentExpected | Boole | Teknik bildirimle x12 anlaşmasında yapılandırılıp yapılandırılmadığını belirtir. Girilmesi |
| ımessagefailed | Boole | X12 iletisinin başarısız olup olmadığı. Girilmesi |
| isa09 | Dize | X12 belge değişim tarihi. Seçim |
| isa10 | Dize | X12 belge değişim süresi. Seçim |
| ısa11 | Dize | X12 Interchange denetim standartları tanımlayıcısı. Seçim |
| ISA12 | Dize | X12 Interchange Control sürüm numarası. Seçim |
| isa14 | Dize | X12 onayı istendi. Seçim |
| isa15 | Dize | Test veya üretim için gösterge. Seçim |
| isa16 | Dize | Öğe ayırıcısı. Seçim |
||||

## <a name="x12-interchange-acknowledgement-tracking-schema"></a>X12 değişim onayı izleme şeması

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

| Özellik | Tür | Açıklama |
| --- | --- | --- |
| senderPartnerName | Dize | X12 ileti göndericisinin iş ortağı adı. Seçim |
| receiverPartnerName | Dize | X12 ileti alıcısının iş ortağı adı. Seçim |
| senderQualifier | Dize | İş ortağı niteleyicisi gönderin. Girilmesi |
| Senderıdentifier | Dize | İş ortağı tanımlayıcısı gönderin. Girilmesi |
| receiverQualifier | Dize | İş ortağı niteleyicisi alma. Girilmesi |
| Receiverıdentifier | Dize | İş ortağı tanımlayıcısını al. Girilmesi |
| agreementName | Dize | İletilerin çözümlenme x12 sözleşmesinin adı. Seçim |
| yön | Sabit Listesi | İleti akışının yönü, alma veya gönderme. Girilmesi |
| interchangeControlNumber | Dize | İş ortaklarından alınan teknik bildirimle ilgili değişim denetimi numarası. Seçim |
| ısasegment | Dize | İş ortaklarından alınan teknik onay için ISA segmenti. Seçim |
| respondingInterchangeControlNumber |Dize | İş ortaklarından alınan teknik bildirimle ilgili değişim denetim numarası. Seçim |
| ımessagefailed | Boole | X12 iletisinin başarısız olup olmadığı. Girilmesi |
| Durum | Sabit Listesi | Değişim onayı durum kodu. İzin verilen değerler **kabul**edilir, **reddedilir**ve **acceptedwitherrors**. Girilmesi |
| processingStatus | Sabit Listesi | Onay durumu. İzin verilen değerler **alınır**, **oluşturulur**ve **gönderilir**. Girilmesi |
| ta102 | Dize | Değişim tarihi. Seçim |
| ta103 | Dize | Değiş tokuş süresi. Seçim |
| ta105 | Dize | Değişim notunun kodu. Seçim |
||||

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

| Özellik | Tür | Açıklama |
| --- | --- | --- |
| senderPartnerName | Dize | X12 ileti göndericisinin iş ortağı adı. Seçim |
| receiverPartnerName | Dize | X12 ileti alıcısının iş ortağı adı. Seçim |
| senderQualifier | Dize | İş ortağı niteleyicisi gönderin. Girilmesi |
| Senderıdentifier | Dize | İş ortağı tanımlayıcısı gönderin. Girilmesi |
| receiverQualifier | Dize | İş ortağı niteleyicisi alma. Girilmesi |
| Receiverıdentifier | Dize | İş ortağı tanımlayıcısını al. Girilmesi |
| agreementName | Dize | İletilerin çözümlenme x12 sözleşmesinin adı. Seçim |
| yön | Sabit Listesi | İleti akışının yönü, alma veya gönderme. Girilmesi |
| interchangeControlNumber | Dize | Değişim denetim numarası. Seçim |
| functionalGroupControlNumber | Dize | İşlevsel denetim numarası. Seçim |
| gsSegment | Dize | İleti GS segmenti. Seçim |
| isTechnicalAcknowledgmentExpected | Boole | Teknik bildirimle x12 anlaşmasında yapılandırılıp yapılandırılmadığını belirtir. Girilmesi |
| isFunctionalAcknowledgmentExpected | Boole | X12 anlaşmasında işlevsel onay yapılandırılıp yapılandırılmadığını belirtir. Girilmesi |
| ımessagefailed | Boole | X12 iletisinin başarısız olup olmadığı. Girilmesi|
| gs01 | Dize | İşlevsel tanımlayıcı kodu. Seçim |
| gs02 | Dize | Uygulama göndericisinin kodu. Seçim |
| gs03 | Dize | Uygulama alıcısının kodu. Seçim |
| gs04 | Dize | İşlevsel grup tarihi. Seçim |
| gs05 | Dize | İşlevsel grup zamanı. Seçim |
| gs07 | Dize | Sorumlu acentekodu. Seçim |
| gs08 | Dize | Sürüm/sürüm/sektör tanımlayıcı kodu. Seçim |
||||

## <a name="x12-functional-group-acknowledgement-tracking-schema"></a>X12 işlevsel grup onayı izleme şeması

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

| Özellik | Tür | Açıklama |
| --- | --- | --- |
| senderPartnerName | Dize | X12 ileti göndericisinin iş ortağı adı. Seçim |
| receiverPartnerName | Dize | X12 ileti alıcısının iş ortağı adı. Seçim |
| senderQualifier | Dize | İş ortağı niteleyicisi gönderin. Girilmesi |
| Senderıdentifier | Dize | İş ortağı tanımlayıcısı gönderin. Girilmesi |
| receiverQualifier | Dize | İş ortağı niteleyicisi alma. Girilmesi |
| Receiverıdentifier | Dize | İş ortağı tanımlayıcısını al. Girilmesi |
| agreementName | Dize | İletilerin çözümlenme x12 sözleşmesinin adı. Seçim |
| yön | Sabit Listesi | İleti akışının yönü, alma veya gönderme. Girilmesi |
| interchangeControlNumber | Dize | İş ortaklarından teknik bir onay alındığında gönderme tarafına yönelik olan değişim denetim numarası. Seçim |
| functionalGroupControlNumber | Dize | İş ortaklarından teknik onay alındığında gönderme tarafına yönelik olan teknik bildirimle ilgili işlevsel Grup denetimi numarası. Seçim |
| ısasegment | Dize | Interchange Control numarasıyla aynıdır, ancak yalnızca belirli durumlarda doldurulur. Seçim |
| gsSegment | Dize | İşlevsel Grup denetim numarasıyla aynıdır, ancak yalnızca belirli durumlarda doldurulur. Seçim |
| respondingfunctionalGroupControlNumber | Dize | Özgün işlevsel grubun denetim numarası. Seçim |
| respondingFunctionalGroupId | Dize | Onay işlev grubu KIMLIĞINDEKI AK101 ile eşlenir. Seçim |
| ımessagefailed | Boole | X12 iletisinin başarısız olup olmadığı. Girilmesi |
| Durum | Sabit Listesi | Onay durum kodu. İzin verilen değerler **kabul**edilir, **reddedilir**ve **acceptedwitherrors**. Girilmesi |
| processingStatus | Sabit Listesi | Bildirim işleme durumu. İzin verilen değerler **alınır**, **oluşturulur**ve **gönderilir**. Girilmesi |
| ak903 | Dize | Alınan işlem kümesi sayısı. Seçim |
| ak904 | Dize | Tanımlanan işlevsel grupta kabul edilen işlem kümesi sayısı. Seçim |
| ak9Segment | Dize | AK1 segmentinde tanımlanan işlevsel grubun kabul edilip edilmediğini ve neden olduğunu. Seçim |
|||| 

## <a name="b2b-protocol-tracking-schemas"></a>B2B protokol izleme şemaları

B2B protokol izleme şemaları hakkında daha fazla bilgi için bkz.:

* [AS2 izleme şemaları](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)   
* [B2B özel izleme şemaları](logic-apps-track-integration-account-custom-tracking-schema.md)

## <a name="next-steps"></a>Sonraki adımlar

* [B2B iletilerini izleme](logic-apps-monitor-b2b-message.md)hakkında daha fazla bilgi edinin.
* [Azure izleyici GÜNLÜKLERINDE B2B iletilerini izleme](../logic-apps/logic-apps-track-b2b-messages-omsportal.md)hakkında bilgi edinin.
