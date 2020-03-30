---
title: İşlem API'yi iptal etme | Azure Marketi
description: İşlemleri iptal et.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
ms.date: 09/13/2018
ms.author: dsindona
ms.openlocfilehash: 6d4c1f52f0f3b1e05ec06f5a66a36323f346d4eb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80280550"
---
# <a name="cancel-operation"></a>İşlemi iptal etme 

Bu API, teklif üzerinde şu anda devam etmekte olan bir işlemi iptal eder. Bu [API'ye](./cloud-partner-portal-api-retrieve-operations.md) geçmek `operationId` için Operasyon API'sini kullanın. İptal genellikle eşzamanlı bir işlemdir, ancak bazı karmaşık senaryolarda varolan bir işlemi iptal etmek için yeni bir işlem gerekebilir. Bu durumda, HTTP yanıt gövdesi, durum sorgusu için kullanılması gereken işlemin konumunu içerir.

İstekle birlikte virgülle ayrılmış bir e-posta adresi listesi sağlayabilirsiniz ve API bu adresleri işlemin ilerlemesi hakkında bilgilendirecektir.

  `POST https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/cancel?api-version=2017-10-31`

<a name="uri-parameters"></a>URI parametreleri
--------------

|  **Adı**    |      **Açıklama**                                  |    **Veri türü**  |
| ------------ |     ----------------                                  |     -----------   |
| publisherId  |  Yayımcı tanımlayıcısı, örneğin,`contoso`         |   Dize          |
| offerId      |  Teklif tanımlayıcısı                                     |   Dize          |
| api-sürümü  |  API'nin geçerli sürümü                               |    Tarih           |
|  |  |  |


<a name="header"></a>Üst bilgi
------

|  **Adı**              |  **Değer**         |
|  ---------             |  ----------        |
|  İçerik Türü          |  uygulama/json  |
|  Yetkilendirme         |  Taşıyıcı SINER TOKEN |
|  |  |


<a name="body-example"></a>Gövde örneği
------------

### <a name="request"></a>İstek

``` json
{
   "metadata": {
     "notification-emails": "jondoe@contoso.com"
    }
}     
```

### <a name="request-body-properties"></a>Gövde özelliklerini isteme

|  **Adı**                |  **Açıklama**                                               |
|  --------                |  ---------------                                               |
|  bildirim-e-postalar     | Virgül, yayımlama işleminin ilerlemesinden haberdar edilecek e-posta kimliklerinin listesini ayırdı. |
|  |  |


### <a name="response"></a>Yanıt

  `Operation-Location: https://cloudpartner.azure.com/api/publishers/contoso/offers/contoso-virtualmachineoffer/operations/56615b67-2185-49fe-80d2-c4ddf77bb2e8`


### <a name="response-header"></a>Yanıt Üst Bilgisi

|  **Adı**             |    **Değer**                       |
|  ---------            |    ----------                      |
| Operasyon-Konum    | İşlemin geçerli durumunu belirlemek için sorgulanabilen URL. |
|  |  |


### <a name="response-status-codes"></a>Yanıt durum kodları

| **Kod**  |  **Açıklama**                                                                       |
|  ------   |  ------------------------------------------------------------------------               |
|  200      | Tamam. İstek başarıyla işlendi ve işlem eşzamanlı olarak iptal edildi. |
|  202      | Kabul. İstek başarıyla işlendi ve işlem iptal ediliyor. İptal işleminin konumu yanıt üstbilgisinde döndürülür. |
|  400      | Kötü/Malformasyonlu istek. Hata yanıt gövdesi daha fazla bilgi sağlayabilir.  |
|  403      | Erişim yasak. İstemcinin istekte belirtilen ad alanına erişimi yoktur. |
|  404      | Bulunamadı. Belirtilen varlık yok. |
|  |  |
