---
title: İşlem API 'sini iptal et | Azure Marketi
description: İşlemleri iptal et.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
ms.date: 04/08/2020
ms.author: dsindona
ms.openlocfilehash: f9e55ff2c581f9392a125f6dc3ec8d903e9876a4
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81256442"
---
# <a name="cancel-operation"></a>İşlemi iptal et

> [!NOTE]
> Bulut İş Ortağı Portalı API 'Leri iş ortağı merkezi ile tümleşiktir ve teklifleriniz iş ortağı merkezi 'ne geçirildikten sonra çalışmaya devam edecektir. Tümleştirme küçük değişiklikler sunar. İş Ortağı Merkezi 'ne geçişten sonra kodunuzun çalışmaya devam ettiğinden emin olmak için [bulut iş ortağı PORTALı API başvurusunda](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-api-overview) listelenen değişiklikleri gözden geçirin.

Bu API, teklifte sürmekte olan bir işlemi iptal eder. Bu API 'ye geçiş `operationId` yapmak IÇIN [Operations API 'sini alma işlemini](./cloud-partner-portal-api-retrieve-operations.md) kullanın. İptali genellikle zaman uyumlu bir işlemdir, ancak bazı karmaşık senaryolarda var olan bir işlemin iptal edilmesi için yeni bir işlem gerekebilir. Bu durumda, HTTP yanıt gövdesi, durumu sorgulamak için kullanılması gereken işlemin konumunu içerir.

  `POST https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/cancel?api-version=2017-10-31`

## <a name="uri-parameters"></a>URI parametreleri

--------------

|  **Adı**    |      **Açıklama**                                  |    **Veri türü**  |
| ------------ |     ----------------                                  |     -----------   |
| PublisherId  |  Yayımcı tanımlayıcısı, örneğin,`contoso`         |   Dize          |
| OfferId      |  Teklif tanımlayıcısı                                     |   Dize          |
| api-sürümü  |  Geçerli API sürümü                               |    Tarih           |
|  |  |  |

## <a name="header"></a>Üst bilgi
------

|  **Adı**              |  **Deeri**         |
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

|  **Adı**                |  **Açıklama**                                               |
|  --------                |  ---------------                                               |
|  bildirim-e-postalar     | Yayımlama işleminin ilerleme durumunun bildirilmesi için e-posta kimliklerinin virgülle ayrılmış listesi. |
|  |  |

### <a name="response"></a>Yanıt

#### <a name="migrated-offers"></a>Geçirilmiş teklifler

`Location: /api/publishers/contoso/offers/contoso-offer/operations/56615b67-2185-49fe-80d2-c4ddf77bb2e8?api-version=2017-10-31`

#### <a name="non-migrated-offers"></a>Geçirilmeyen teklifler

`Location: /api/operations/contoso$contoso-offer$2$preview?api-version=2017-10-31`

### <a name="response-header"></a>Yanıt Üst Bilgisi

|  **Adı**             |    **Deeri**                       |
|  ---------            |    ----------                      |
| Konum    | Bu işlemin durumunu almak için göreli yol. |
|  |  |

### <a name="response-status-codes"></a>Yanıt durum kodları

| **Kodudur**  |  **Açıklama**                                                                       |
|  ------   |  ------------------------------------------------------------------------               |
|  200      | Tamam. İstek başarıyla işlendi ve işlem zaman uyumlu olarak iptal edildi. |
|  202      | Eden. İstek başarıyla işlendi ve işlem iptal ediliyor sürecinde. İptal işleminin konumu yanıt üst bilgisinde döndürülür. |
|  400      | Hatalı/hatalı biçimlendirilmiş istek. Hata yanıtı gövdesi daha fazla bilgi sağlayabilir.  |
|  403      | Erişim yasak. İstemcinin istekte belirtilen ad alanına erişimi yok. |
|  404      | Bulunamadı. Belirtilen varlık yok. |
|  |  |
