---
title: İşlem API 'SI iptali-Microsoft ticari Market
description: Teklif üzerinde sürmekte olan bir işlemi iptal etmek için API
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
author: emuench
ms.author: mingshen
ms.date: 07/14/2020
ms.openlocfilehash: 462ca525be9cf46c87acdf4025223a98afaf8e3b
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86520383"
---
# <a name="cancel-operation"></a>İşlemi iptal et

> [!NOTE]
> Bulut İş Ortağı Portalı API 'Leri ile tümleşiktir ve Iş Ortağı Merkezi 'nde çalışmaya devam edecektir. Geçiş küçük değişiklikler sunar. Iş Ortağı Merkezi 'ne geçtikten sonra kodunuzun çalışmaya devam ettiğinden emin olmak için [bulut iş ortağı PORTALı API başvurusunda](./cloud-partner-portal-api-overview.md) listelenen değişiklikleri gözden geçirin. CPP API 'Leri yalnızca Iş Ortağı Merkezi 'ne geçişten önce tümleştirilmiş mevcut ürünler için kullanılmalıdır; Yeni ürünlerin Iş Ortağı Merkezi gönderme API 'Leri kullanması gerekir.

Bu API, teklifte sürmekte olan bir işlemi iptal eder. Bu API 'ye geçiş yapmak için [OPERATIONS API 'Sini alma işlemini](./cloud-partner-portal-api-retrieve-operations.md) kullanın `operationId` . İptali genellikle zaman uyumlu bir işlemdir, ancak bazı karmaşık senaryolarda var olan bir işlemin iptal edilmesi için yeni bir işlem gerekebilir. Bu durumda, HTTP yanıt gövdesi, durumu sorgulamak için kullanılması gereken işlemin konumunu içerir.

  `POST https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/cancel?api-version=2017-10-31`

## <a name="uri-parameters"></a>URI parametreleri

--------------

|  **Ad**    |      **Açıklama**                                  |    **Veri türü**  |
| ------------ |     ----------------                                  |     -----------   |
| PublisherId  |  Yayımcı tanımlayıcısı, örneğin,`contoso`         |   Dize          |
| OfferId      |  Teklif tanımlayıcısı                                     |   Dize          |
| api-sürümü  |  Geçerli API sürümü                               |    Tarih           |
|  |  |  |

## <a name="header"></a>Üst bilgi
------

|  **Ad**              |  **Değer**         |
|  ---------             |  ----------        |
|  İçerik Türü          |  uygulama/json  |
|  Yetkilendirme         |  BELIRTECINIZI taşıyıcı |
|  |  |

## <a name="body-example"></a>Gövde örneği
------------

### <a name="request"></a>İstek

``` json
{
   "metadata": {
     "notification-emails": "jondoe@contoso.com"
    }
}     
```

### <a name="request-body-properties"></a>İstek gövdesi özellikleri

|  **Ad**                |  **Açıklama**                                               |
|  --------                |  ---------------                                               |
|  bildirim-e-postalar     | Yayımlama işleminin ilerleme durumunun bildirilmesi için e-posta kimliklerinin virgülle ayrılmış listesi. |
|  |  |

### <a name="response"></a>Yanıt

#### <a name="migrated-offers"></a>Geçirilmiş teklifler

`Location: /api/publishers/contoso/offers/contoso-offer/operations/56615b67-2185-49fe-80d2-c4ddf77bb2e8?api-version=2017-10-31`

#### <a name="non-migrated-offers"></a>Geçirilmeyen teklifler

`Location: /api/operations/contoso$contoso-offer$2$preview?api-version=2017-10-31`

### <a name="response-header"></a>Yanıt Üst Bilgisi

|  **Ad**             |    **Değer**                       |
|  ---------            |    ----------                      |
| Konum    | Bu işlemin durumunu almak için göreli yol. |
|  |  |

### <a name="response-status-codes"></a>Yanıt durum kodları

| **Kod**  |  **Açıklama**                                                                       |
|  ------   |  ------------------------------------------------------------------------               |
|  200      | Tamam. İstek başarıyla işlendi ve işlem zaman uyumlu olarak iptal edildi. |
|  202      | Eden. İstek başarıyla işlendi ve işlem iptal ediliyor sürecinde. İptal işleminin konumu yanıt üst bilgisinde döndürülür. |
|  400      | Hatalı/hatalı biçimlendirilmiş istek. Hata yanıtı gövdesi daha fazla bilgi sağlayabilir.  |
|  403      | Erişim yasak. İstemcinin istekte belirtilen ad alanına erişimi yok. |
|  404      | Bulunamadı. Belirtilen varlık yok. |
|  |  |
