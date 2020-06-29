---
title: Teklif yayımlama-Azure Marketi
description: Belirtilen teklifi yayımlamak için API.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
ms.date: 04/08/2020
ms.author: dsindona
ms.openlocfilehash: ada8725a8963fc14fd326c244dcf3d5bb3dd372f
ms.sourcegitcommit: 374e47efb65f0ae510ad6c24a82e8abb5b57029e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2020
ms.locfileid: "85516313"
---
# <a name="publish-an-offer"></a>Teklif yayımlama

> [!NOTE]
> Bulut İş Ortağı Portalı API 'Leri iş ortağı merkezi ile tümleşiktir ve teklifleriniz iş ortağı merkezi 'ne geçirildikten sonra çalışmaya devam edecektir. Tümleştirme küçük değişiklikler sunar. İş Ortağı Merkezi 'ne geçişten sonra kodunuzun çalışmaya devam ettiğinden emin olmak için [bulut iş ortağı PORTALı API başvurusunda](./cloud-partner-portal-api-overview.md) listelenen değişiklikleri gözden geçirin.

Belirtilen teklif için yayımlama işlemini başlatır. Bu çağrı uzun süredir çalışan bir işlemdir.

  `POST  https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/publish?api-version=2017-10-31`

## <a name="uri-parameters"></a>URI parametreleri
--------------

|  **Adı**      |    **Açıklama**                               |  **Veri türü** |
|  ------------- |  ------------------------------------            |   -----------  |
|  PublisherId   | Yayımcı tanımlayıcı, örneğin`contoso`      |   Dize       |
|  OfferId       | Teklif tanımlayıcısı                                 |   Dize       |
|  api-sürümü   | API 'nin en son sürümü                        |   Tarih         |
|  |  |

## <a name="header"></a>Üst bilgi
------

|  **Adı**        |    **Değer**          |
|  --------        |    ---------          |
|  İçerik Türü    | `application/json`    |
|  Yetkilendirme   |  `Bearer YOUR_TOKEN`  |
|  |  |


## <a name="body-example"></a>Gövde örneği
------------

### <a name="request"></a>İstek

``` json
  { 
      'metadata': 
          { 
              'notification-emails': 'jdoe@contoso.com'
          } 
  }
```

### <a name="request-body-properties"></a>İstek gövdesi özellikleri

|  **Adı**               |   **Açıklama**                                                                                 |
|  ---------------------  | ------------------------------------------------------------------------------------------------- |
|  bildirim-e-postalar    | Yayımlama işleminin ilerleme durumunun bildirilmesi için, virgülle ayrılmış e-posta adresleri listesi. |
|  |  |


### <a name="response"></a>Yanıt

#### <a name="migrated-offers"></a>Geçirilmiş teklifler

`Location: /api/publishers/contoso/offers/contoso-offer/operations/56615b67-2185-49fe-80d2-c4ddf77bb2e8?api-version=2017-10-31`

#### <a name="non-migrated-offers"></a>Geçirilmeyen teklifler

`Location: /api/operations/contoso$contoso-offer$2$preview?api-version=2017-10-31`


### <a name="response-header"></a>Yanıt Üst Bilgisi

|  **Adı**             |    **Değer**                                                                 |
|  -------------------- | ---------------------------------------------------------------------------- |
| Konum    | Bu işlemin durumunu almak için göreli yol     |
|  |  |


### <a name="response-status-codes"></a>Yanıt durum kodları

| **Kod** |  **Açıklama**                                                                                                                           |
| ------   |  ----------------------------------------------------------------------------------------------------------------------------------------- |
| 202   | `Accepted`-İstek başarıyla kabul edildi. Yanıt, başlatılan işlemi izlemek için kullanılabilecek bir konum içerir. |
| 400   | `Bad/Malformed request`-Hata yanıtı gövdesi daha fazla bilgi sağlayabilir.                                                               |
| 422   | `Un-processable entity`-Yayımlanacak varlığın başarısız olduğunu belirtir.                                                        |
| 404   | `Not found`-İstemci tarafından belirtilen varlık yok.                                                                              |
|  |  |
