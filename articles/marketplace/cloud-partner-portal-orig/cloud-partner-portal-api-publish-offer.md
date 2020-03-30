---
title: Teklif yayınlama | Azure Marketi
description: API belirtilen teklifi yayınlamak için.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
ms.date: 09/13/2018
ms.author: dsindona
ms.openlocfilehash: 4163bf5727c327d559b81db42f99684aa0cc8d5b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80280533"
---
<a name="publish-an-offer"></a>Teklif yayımlama
================

Belirtilen teklif için yayımlama işlemini başlatır. Bu arama uzun süren bir işlemdir.

  `POST  https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/publish?api-version=2017-10-31`

<a name="uri-parameters"></a>URI parametreleri
--------------

|  **Adı**      |    **Açıklama**                               |  **Veri türü** |
|  ------------- |  ------------------------------------            |   -----------  |
|  publisherId   | Yayımcı tanımlayıcısı, örneğin`contoso`      |   Dize       |
|  offerId       | Teklif tanımlayıcısı                                 |   Dize       |
|  api-sürümü   | API'nin en son sürümü                        |   Tarih         |
|  |  |


<a name="header"></a>Üst bilgi
------

|  **Adı**        |    **Değer**          |
|  --------        |    ---------          |
|  İçerik Türü    | `application/json`    |
|  Yetkilendirme   |  `Bearer YOUR_TOKEN`  |
|  |  |


<a name="body-example"></a>Gövde örneği
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

   `Operation-Location: /api/operations/contoso$56615b67-2185-49fe-80d2-c4ddf77bb2e8$2$preview?api-version=2017-10-31`


### <a name="response-header"></a>Yanıt Üst Bilgisi

|  **Adı**             |    **Değer**                                                                 |
|  -------------------- | ---------------------------------------------------------------------------- |
| Operasyon-Konum    | İşlemin geçerli durumunu belirlemek için sorgulanabilen URL.    |
|  |  |


### <a name="response-status-codes"></a>Yanıt durum kodları

| **Kod** |  **Açıklama**                                                                                                                           |
| ------   |  ----------------------------------------------------------------------------------------------------------------------------------------- |
| 202   | `Accepted`- İstek başarıyla kabul edildi. Yanıt, başlatılan işlemi izlemek için kullanılabilecek bir konum içerir. |
| 400   | `Bad/Malformed request`- Hata yanıt gövdesi daha fazla bilgi sağlayabilir.                                                               |
| 422   | `Un-processable entity`- Yayımlanacak varlığın başarısız doğrulama olduğunu gösterir.                                                        |
| 404   | `Not found`- İstemci tarafından belirtilen varlık yok.                                                                              |
|  |  |
