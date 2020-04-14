---
title: İşlem API'yi iptal etme | Azure Marketi
description: İşlemleri iptal et.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
ms.date: 04/08/2020
ms.author: dsindona
ms.openlocfilehash: f9e55ff2c581f9392a125f6dc3ec8d903e9876a4
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81256442"
---
# <a name="cancel-operation"></a>İşlemi iptal etme

> [!NOTE]
> Bulut İş Ortağı Portalı API'leri İş Ortağı Merkezi ile entegre edilmiştir ve teklifleriniz İş Ortağı Merkezi'ne geçtikten sonra çalışmaya devam edecektir. Entegrasyon küçük değişiklikler sunar. Kodunuzu İş Ortağı Merkezi'ne geçişten sonra çalışmaya devam etmesini sağlamak için [Bulut İş Ortağı Portalı API Başvurusu'nda](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-api-overview) listelenen değişiklikleri gözden geçirin.

Bu API, teklif üzerinde şu anda devam etmekte olan bir işlemi iptal eder. Bu [API'ye](./cloud-partner-portal-api-retrieve-operations.md) geçmek `operationId` için Operasyon API'sini kullanın. İptal genellikle eşzamanlı bir işlemdir, ancak bazı karmaşık senaryolarda varolan bir işlemi iptal etmek için yeni bir işlem gerekebilir. Bu durumda, HTTP yanıt gövdesi, durum sorgusu için kullanılması gereken işlemin konumunu içerir.

  `POST https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/cancel?api-version=2017-10-31`

## <a name="uri-parameters"></a>URI parametreleri

--------------

|  **Adı**    |      **Açıklama**                                  |    **Veri türü**  |
| ------------ |     ----------------                                  |     -----------   |
| publisherId  |  Yayımcı tanımlayıcısı, örneğin,`contoso`         |   Dize          |
| offerId      |  Teklif tanımlayıcısı                                     |   Dize          |
| api-sürümü  |  API'nin geçerli sürümü                               |    Tarih           |
|  |  |  |

## <a name="header"></a>Üst bilgi
------

|  **Adı**              |  **Değer**         |
|  ---------             |  ----------        |
|  İçerik Türü          |  uygulama/json  |
|  Yetkilendirme         |  Taşıyıcı SINER TOKEN |
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

### <a name="request-body-properties"></a>Gövde özelliklerini isteme

|  **Adı**                |  **Açıklama**                                               |
|  --------                |  ---------------                                               |
|  bildirim-e-postalar     | Virgül, yayımlama işleminin ilerlemesinden haberdar edilecek e-posta kimliklerinin listesini ayırdı. |
|  |  |

### <a name="response"></a>Yanıt

#### <a name="migrated-offers"></a>Geçirilen teklifler

`Location: /api/publishers/contoso/offers/contoso-offer/operations/56615b67-2185-49fe-80d2-c4ddf77bb2e8?api-version=2017-10-31`

#### <a name="non-migrated-offers"></a>Geçirilmeyen teklifler

`Location: /api/operations/contoso$contoso-offer$2$preview?api-version=2017-10-31`

### <a name="response-header"></a>Yanıt Üst Bilgisi

|  **Adı**             |    **Değer**                       |
|  ---------            |    ----------                      |
| Konum    | Bu işlemin durumunu almak için göreli yol. |
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
