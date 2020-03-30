---
title: Teklifleri al API | Azure Marketi
description: API, yayımcı ad alanı altında tekliflerin özet listesini alır.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
ms.date: 09/13/2018
ms.author: dsindona
ms.openlocfilehash: a1f15e269481b9706f81fd02f19effc9ad37df32
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80280499"
---
<a name="retrieve-offers"></a>Teklifleri alma
===============

Yayımcı ad alanı altında tekliflerin özet listesini alır.

 `GET https://cloudpartner.azure.com/api/publishers/<publisherId>/offers?api-version=2017-10-31`


<a name="uri-parameters"></a>URI parametreleri
--------------

| **Adı**         |  **Açıklama**                         |  **Veri türü** |
| -------------    |  ------------------------------------    |  -----------   |
|  publisherId     | Yayımcı tanımlayıcısı, örneğin`contoso` |   Dize    |
|  api-sürümü     | API'nin en son sürümü                    |    Tarih        |
|  |  |


<a name="header"></a>Üst bilgi
------

|  **Adı**        |         **Değer**       |
|  --------------- |       ----------------  |
|  İçerik Türü    | `application/json`      |
|  Yetkilendirme   | `Bearer YOUR_TOKEN`     |
|  |  |


<a name="body-example"></a>Gövde örneği
------------

### <a name="response"></a>Yanıt

``` json
  200 OK 
  [ 
      {  
          "offerTypeId": "microsoft-azure-virtualmachines",
          "publisherId": "contoso",
          "status": "published",
          "id": "059afc24-07de-4126-b004-4e42a51816fe",
          "version": 1,
          "definition": {
              "displayText": "Contoso Virtual Machine"
          },
          "changedTime":"2017-05-23T23:33:47.8802283Z"
      }
  ]
```

### <a name="response-body-properties"></a>Yanıt gövde özellikleri

|  **Adı**       |       **Açıklama**                                                                                                  |
|  -------------  |      --------------------------------------------------------------------------------------------------------------    |
|  teklifTypeId    | Teklif türünü tanımlar                                                                                           |
|  publisherId    | Yayımcıyı benzersiz olarak tanımlayan tanımlayıcı                                                                      |
|  durum         | Teklifin durumu. Olası değerler listesi için aşağıdaki [Teklif durumuna](#offer-status) bakın.                         |
|  id             | Teklifi yayımcı ad alanında benzersiz olarak tanımlayan GUID.                                                    |
|  version        | Teklifin geçerli sürümü. Sürüm özelliği istemci tarafından değiştirilemez. Her yayından sonra artımlı. |
|  tanım     | İş yükünün gerçek tanımının özet bir görünümünü içerir. Ayrıntılı bir tanım elde etmek [için, Özel teklif](./cloud-partner-portal-api-retrieve-specific-offer.md) API'sini Al'ı kullanın. |
|  değiştirilenZaman    | Teklifin en son değiştirildiğinde UTC zamanı                                                                              |
|  |  |


### <a name="response-status-codes"></a>Yanıt durum kodları

| **Kod**  |  **Açıklama**                                                                                                   |
| -------   |  ----------------------------------------------------------------------------------------------------------------- |
|  200      | `OK`- İstek başarıyla işlendi ve yayımcı kapsamındaki tüm teklifler istemciye iade edildi.  |
|  400      | `Bad/Malformed request`- Hata yanıt gövdesi daha fazla bilgi içerebilir.                                    |
|  403      | `Forbidden`- İstemcinin belirtilen ad alanına erişimi yoktur.                                          |
|  404      | `Not found`- Belirtilen varlık yok.                                                                 |
|  |  |


### <a name="offer-status"></a>Teklif Durumu

|  **Adı**                    | **Açıklama**                                  |
|  ------------------------    | -----------------------------------------------  |
|  Hiç Yayınlanmadı              | Teklif hiç yayınlanmadı.                  |
|  Başlatılmama                  | Teklif yeni dir, ancak başlanmaz.                 |
|  WaitingForPublisherReview   | Teklif yayıncının onayını bekliyor.         |
|  Çalışıyor                     | Teklif gönderimi işleniyor.             |
|  Başarılı oldu                   | Teklif gönderimi işlemi tamamlandı.       |
|  İptal edildi                    | Teklif sunumu iptal edildi.                   |
|  Başarısız                      | Teklif gönderme başarısız oldu.                         |
|  |  |
