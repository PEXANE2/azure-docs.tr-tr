---
title: Teklif yayınlama | Azure Marketi
description: API belirtilen teklifi yayınlamak için.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
ms.date: 04/08/2020
ms.author: dsindona
ms.openlocfilehash: 960d5facb53f20719045c5fdbe2179f549aca3f2
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81255949"
---
# <a name="publish-an-offer"></a>Teklif yayımlama

> [!NOTE]
> Bulut İş Ortağı Portalı API'leri İş Ortağı Merkezi ile entegre edilmiştir ve teklifleriniz İş Ortağı Merkezi'ne geçtikten sonra çalışmaya devam edecektir. Entegrasyon küçük değişiklikler sunar. Kodunuzu İş Ortağı Merkezi'ne geçişten sonra çalışmaya devam etmesini sağlamak için [Bulut İş Ortağı Portalı API Başvurusu'nda](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-api-overview) listelenen değişiklikleri gözden geçirin.

Belirtilen teklif için yayımlama işlemini başlatır. Bu arama uzun süren bir işlemdir.

  `POST  https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/publish?api-version=2017-10-31`

## <a name="uri-parameters"></a>URI parametreleri
--------------

|  **Adı**      |    **Açıklama**                               |  **Veri türü** |
|  ------------- |  ------------------------------------            |   -----------  |
|  publisherId   | Yayımcı tanımlayıcısı, örneğin`contoso`      |   Dize       |
|  offerId       | Teklif tanımlayıcısı                                 |   Dize       |
|  api-sürümü   | API'nin en son sürümü                        |   Tarih         |
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

### <a name="request-body-properties"></a>Gövde özelliklerini isteme

|  **Adı**               |   **Açıklama**                                                                                 |
|  ---------------------  | ------------------------------------------------------------------------------------------------- |
|  bildirim-e-postalar    | Yayımlama işleminin ilerlemesi hakkında bilgilendirilecek e-posta adreslerinin virgülle ayrılmış listesi. |
|  |  |


### <a name="response"></a>Yanıt

#### <a name="migrated-offers"></a>Geçirilen teklifler

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
| 202   | `Accepted`- İstek başarıyla kabul edildi. Yanıt, başlatılan işlemi izlemek için kullanılabilecek bir konum içerir. |
| 400   | `Bad/Malformed request`- Hata yanıt gövdesi daha fazla bilgi sağlayabilir.                                                               |
| 422   | `Un-processable entity`- Yayımlanacak varlığın başarısız doğrulama olduğunu gösterir.                                                        |
| 404   | `Not found`- İstemci tarafından belirtilen varlık yok.                                                                              |
|  |  |
