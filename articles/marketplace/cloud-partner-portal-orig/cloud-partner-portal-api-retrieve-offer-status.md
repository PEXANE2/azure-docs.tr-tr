---
title: Teklif durumunu alın | Azure Marketi
description: API teklifin geçerli durumunu alır.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
ms.date: 09/13/2018
ms.author: dsindona
ms.openlocfilehash: 2f5211716145d6c05bbfb0132c4a6ba2f9cceabe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80280516"
---
<a name="retrieve-offer-status"></a>Teklif durumunu alma 
=====================

Teklifin geçerli durumunu alır.

  `GET  https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/status?api-version=2017-10-31`

<a name="uri-parameters"></a>URI parametreleri
--------------

|  **Adı**       |   **Açıklama**                            |  **Veri türü** |
|  -------------  |  ------------------------------------------  |  ------------  |
|  publisherId    | Yayımcı tanımlayıcısı, örneğin`Contoso`  |     Dize     |
|  offerId        | Teklifi benzersiz bir şekilde tanımlayan GUID      |     Dize     |
|  api-sürümü    | API'nin en son sürümü                        |     Tarih       |
|  |  |


<a name="header"></a>Üst bilgi
------

|  Adı           |  Değer               |
|  -------------  | -------------------  |
|  İçerik Türü   |  `application/json`  |
|  Yetkilendirme  | `Bearer YOUR_TOKEN`  |
|  |  |


<a name="body-example"></a>Gövde örneği
------------

### <a name="response"></a>Yanıt

``` json
  {
      "status": "succeeded",
      "messages": [],
      "steps": [
      {
          "estimatedTimeFrame": "< 15 min",
          "id": "displaydummycertify",
          "stepName": "Validate Pre-Requisites",
          "description": "Offer settings provided are validated.",
          "status": "complete",
          "messages": [
              {
                  "messageHtml": "Step completed.",
                  "level": "information",
                  "timestamp": "2018-03-16T17:50:45.7215661Z"
              }
          ],       
          "progressPercentage": 100
      },
      {
          "estimatedTimeFrame": "~2-3 days",
          "id": "displaycertify",
          "stepName": "Certification",
          "description": "Your offer is analyzed by our certification systems for issues.",
          "status": "notStarted",
          "messages": [],
          "progressPercentage": 0
      },
      {
          "estimatedTimeFrame": "< 1 day",
          "id": "displayprovision",
          "stepName": "Provisioning",
          "description": "Your virtual machine is being replicated in our production systems.",
          "status": "notStarted",
          "messages": [],
          "progressPercentage": 0
      },
      {
          "estimatedTimeFrame": "< 1 hour",
          "id": "displaypackage",
          "stepName": "Packaging and Lead Generation Registration",
          "description": "Your virtual machine is being packaged for customers. Additionally, lead systems are being configured and set up.",
          "status": "notStarted",
          "messages": [],
          "progressPercentage": 0
      },
      {
          "estimatedTimeFrame": "< 1 hour",
          "id": "publisher-signoff",
          "stepName": "Publisher signoff",
          "description": "Offer is available to preview. Ensure that everything looks good before making your offer live.",
          "status": "complete",
          "messages": [],
          "progressPercentage": 0
      },
      {
          "estimatedTimeFrame": "~2-5 days",
          "id": "live",
          "stepName": "Live",
          "description": "Offer is publicly visible and is available for purchase.",
          "status": "complete",
          "messages": [],
          "progressPercentage": 0
      }
      ],
      "previewLinks": [],
      liveLinks": [],
      "notificationEmails": "jdoe@contoso.com"
  } 
```


### <a name="response-body-properties"></a>Yanıt gövde özellikleri

|  **Adı**             |    **Açıklama**                                                                             |
| --------------------  |   -------------------------------------------------------------------------------------------- |
|  durum               | Teklifin durumu. Olası değerler listesi için aşağıdaki [Teklif durumuna](#offer-status) bakın. |
|  sayısı             | Teklifle ilişkili ileti dizisi                                                    |
|  adımlar                | Teklifin yayımlama sırasında sunduğu adımların dizilimi                      |
|  tahminiTimeFrame   | Bu adımı tamamlamak için gereken sürenin tahmini, dostça biçimde                       |
|  id                   | Adımın tanımlayıcısı                                                                         |
|  stepName             | Adımın adı                                                                               |
|  açıklama          | Adımın açıklaması                                                                        |
|  durum               | Adımın durumu. Olası değerler listesi için aşağıdaki [Adım durumuna](#step-status) bakın.    |
|  sayısı             | Adımla ilgili ileti dizisi                                                          |
|  işlemYüzdesi    | Adımın yüzde tamamlanması                                                              |
|  önizlemeLinkler         | *Şu anda uygulanmıyor*                                                                    |
|  liveLinks            | *Şu anda uygulanmıyor*                                                                    |
|  bildirimE-postalar   | İşlemin ilerlemesi hakkında bilgilendirilecek e-posta adreslerinin virgülle ayrılmış listesi        |
|  |  |


### <a name="response-status-codes"></a>Yanıt durum kodları

| **Kod** |   **Açıklama**                                                                                 |
| -------  |   ----------------------------------------------------------------------------------------------- |
|  200     |  `OK`- İstek başarıyla işlendi ve teklifin geçerli durumu iade edildi. |
|  400     | `Bad/Malformed request`- Hata yanıt gövdesi daha fazla bilgi içerebilir.                 |
|  404     | `Not found`- Belirtilen varlık yok.                                                |
|  |  |


### <a name="offer-status"></a>Teklif durumu

|  **Adı**                    |    **Açıklama**                                       |
|  --------------------------  |  ------------------------------------------------------  |
|  Hiç Yayınlanmadı              | Teklif hiç yayınlanmadı.                          |
|  Başlatılmama                  | Teklif yeni ve başlamadı.                            |
|  WaitingForPublisherReview   | Teklif yayıncının onayını bekliyor.                 |
|  Çalışıyor                     | Teklif gönderimi işleniyor.                     |
|  Başarılı oldu                   | Teklif gönderimi işlemi tamamlandı.               |
|  İptal edildi                    | Teklif sunumu iptal edildi.                           |
|  Başarısız                      | Teklif gönderme başarısız oldu.                                 |
|  |  |


### <a name="step-status"></a>Adım Durumu

|  **Adı**                    |    **Açıklama**                           |
|  -------------------------   |  ------------------------------------------  |
|  Başlatılmama                  | Adım başlamadı.                        |
|  Devam Ediyor                  | Adım çalışıyor.                             |
|  WaitingForPublisherReview   | Adım yayımcı onayı bekliyor.      |
|  BeklemeOnayı          | Adım işlem onayı bekliyor.        |
|  Engellendi                     | Adım engellendi.                             |
|  Reddedilen                    | Adım reddedildi.                            |
|  Complete                    | Adım tamamlandı.                            |
|  İptal edildi                    | Step iptal edildi.                           |
|  |  |
