---
title: Teklifleri alma API 'SI-Azure Marketi
description: Yayımcı ad alanı altındaki tekliflerin özetlenen listesini almak için API.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
ms.date: 04/08/2020
ms.author: dsindona
ms.openlocfilehash: ee8d0c773a5ec4d362eae66e289838b9646247e6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85516307"
---
<a name="retrieve-offers"></a>Teklifleri alma
===============

> [!NOTE]
> Bulut İş Ortağı Portalı API 'Leri iş ortağı merkezi ile tümleşiktir ve teklifleriniz iş ortağı merkezi 'ne geçirildikten sonra çalışmaya devam edecektir. Tümleştirme küçük değişiklikler sunar. İş Ortağı Merkezi 'ne geçişten sonra kodunuzun çalışmaya devam ettiğinden emin olmak için [bulut iş ortağı PORTALı API başvurusunda](./cloud-partner-portal-api-overview.md) listelenen değişiklikleri gözden geçirin.

Bir yayımcı ad alanı altındaki tekliflerin özetlenen listesini alır.

 `GET https://cloudpartner.azure.com/api/publishers/<publisherId>/offers?api-version=2017-10-31`

<a name="uri-parameters"></a>URI parametreleri
--------------

| **Adı**         |  **Açıklama**                         |  **Veri türü** |
| -------------    |  ------------------------------------    |  -----------   |
|  PublisherId     | Yayımcı tanımlayıcı, örneğin`contoso` |   Dize    |
|  api-sürümü     | En son API sürümü                    |    Tarih        |
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

### <a name="response-body-properties"></a>Yanıt gövdesi özellikleri

|  **Adı**       |       **Açıklama**                                                                                                  |
|  -------------  |      --------------------------------------------------------------------------------------------------------------    |
|  Offertypeıd    | Teklifin türünü tanımlar                                                                                           |
|  PublisherId    | Yayımcıyı benzersiz bir şekilde tanımlayan tanımlayıcı                                                                      |
|  durum         | Teklifin durumu. Olası değerler listesi için bkz. [teklif durumu](#offer-status) aşağıdaki.                         |
|  kimlik             | Yayımcıyı yayımcı ad alanında benzersiz şekilde tanımlayan GUID.                                                    |
|  sürüm        | Teklifin geçerli sürümü. Sürüm özelliği istemci tarafından değiştirilemiyor. Her yayımladıktan sonra artırılır. |
|  tanım     | İş yükünün gerçek tanımının özetlenen görünümünü içerir. Ayrıntılı bir tanım almak için, belirli bir [teklifi al](./cloud-partner-portal-api-retrieve-specific-offer.md) API 'sini kullanın. |
|  changedTime    | Teklifin en son değiştirildiği UTC saati                                                                              |
|  |  |


### <a name="response-status-codes"></a>Yanıt durum kodları

| **Kod**  |  **Açıklama**                                                                                                   |
| -------   |  ----------------------------------------------------------------------------------------------------------------- |
|  200      | `OK`-İstek başarıyla işlendi ve yayımcının altındaki tüm teklifler istemciye geri döndü.  |
|  400      | `Bad/Malformed request`-Hata yanıtı gövdesinde daha fazla bilgi bulunabilir.                                    |
|  403      | `Forbidden`-İstemcinin belirtilen ad alanına erişimi yok.                                          |
|  404      | `Not found`-Belirtilen varlık yok.                                                                 |
|  |  |


### <a name="offer-status"></a>Teklif durumu

|  **Adı**                    | **Açıklama**                                  |
|  ------------------------    | -----------------------------------------------  |
|  Neveryayınlandı              | Teklif hiç yayımlanmadı.                  |
|  NotStarted                  | Teklif yenidir ancak başlatılmamış.                 |
|  WaitingForPublisherReview   | Teklif, yayımcı onayını bekliyor.         |
|  Çalışıyor                     | Teklif gönderimi işleniyor.             |
|  Başarılı oldu                   | Teklif gönderimi işlemeyi tamamladı.       |
|  İptal edildi                    | Teklif Gönderimi iptal edildi.                   |
|  Başarısız                      | Teklif gönderimi başarısız oldu.                         |
|  |  |
