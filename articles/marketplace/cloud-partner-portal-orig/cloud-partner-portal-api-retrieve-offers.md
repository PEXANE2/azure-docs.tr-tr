---
title: Teklifleri al API | Azure Marketi
description: API, yayımcı ad alanı altında tekliflerin özet listesini alır.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
ms.date: 04/08/2020
ms.author: dsindona
ms.openlocfilehash: 0413bc71c113bf1eef9f761dbeed4c0d0afe735c
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81255966"
---
<a name="retrieve-offers"></a>Teklifleri alma
===============

> [!NOTE]
> Bulut İş Ortağı Portalı API'leri İş Ortağı Merkezi ile entegre edilmiştir ve teklifleriniz İş Ortağı Merkezi'ne geçtikten sonra çalışmaya devam edecektir. Entegrasyon küçük değişiklikler sunar. Kodunuzu İş Ortağı Merkezi'ne geçişten sonra çalışmaya devam etmesini sağlamak için [Bulut İş Ortağı Portalı API Başvurusu'nda](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-api-overview) listelenen değişiklikleri gözden geçirin.

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
