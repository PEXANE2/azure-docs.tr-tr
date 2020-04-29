---
title: Teklif yayımlama | Azure Marketi
description: Belirtilen teklifi yayımlamak için API.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
ms.date: 04/08/2020
ms.author: dsindona
ms.openlocfilehash: 960d5facb53f20719045c5fdbe2179f549aca3f2
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81255949"
---
# <a name="publish-an-offer"></a>Teklif yayımlama

> [!NOTE]
> Bulut İş Ortağı Portalı API 'Leri iş ortağı merkezi ile tümleşiktir ve teklifleriniz iş ortağı merkezi 'ne geçirildikten sonra çalışmaya devam edecektir. Tümleştirme küçük değişiklikler sunar. İş Ortağı Merkezi 'ne geçişten sonra kodunuzun çalışmaya devam ettiğinden emin olmak için [bulut iş ortağı PORTALı API başvurusunda](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-api-overview) listelenen değişiklikleri gözden geçirin.

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

|  **Adı**        |    **Deeri**          |
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

|  **Adı**             |    **Deeri**                                                                 |
|  -------------------- | ---------------------------------------------------------------------------- |
| Konum    | Bu işlemin durumunu almak için göreli yol     |
|  |  |


### <a name="response-status-codes"></a>Yanıt durum kodları

| **Kodudur** |  **Açıklama**                                                                                                                           |
| ------   |  ----------------------------------------------------------------------------------------------------------------------------------------- |
| 202   | `Accepted`-İstek başarıyla kabul edildi. Yanıt, başlatılan işlemi izlemek için kullanılabilecek bir konum içerir. |
| 400   | `Bad/Malformed request`-Hata yanıtı gövdesi daha fazla bilgi sağlayabilir.                                                               |
| 422   | `Un-processable entity`-Yayımlanacak varlığın başarısız olduğunu belirtir.                                                        |
| 404   | `Not found`-İstemci tarafından belirtilen varlık yok.                                                                              |
|  |  |
