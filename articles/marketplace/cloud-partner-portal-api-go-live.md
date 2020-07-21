---
title: Canlı API-Azure Marketi 'ne git
description: Go Live API 'SI teklifi canlı Listeleme işlemini başlatır.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
author: dsindona
ms.author: dsindona
ms.date: 07/14/2020
ms.openlocfilehash: 5c5448a853447a0eacc8d974a5b00d5d1e8f4be2
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86535952"
---
# <a name="go-live"></a>Canlı git

> [!NOTE]
> Bulut İş Ortağı Portalı API 'Leri ile tümleşiktir ve Iş Ortağı Merkezi 'nde çalışmaya devam edecektir. Geçiş küçük değişiklikler sunar. Iş Ortağı Merkezi 'ne geçtikten sonra kodunuzun çalışmaya devam ettiğinden emin olmak için [bulut iş ortağı PORTALı API başvurusunda](./cloud-partner-portal-api-overview.md) listelenen değişiklikleri gözden geçirin. CPP API 'Leri yalnızca Iş Ortağı Merkezi 'ne geçişten önce tümleştirilmiş mevcut ürünler için kullanılmalıdır; Yeni ürünlerin Iş Ortağı Merkezi gönderme API 'Leri kullanması gerekir.

Bu API, bir uygulamayı üretime iletme işlemini başlatır. Bu işlem genellikle uzun süredir çalışır. Bu çağrı, API 'yi [Yayımla](./cloud-partner-portal-api-publish-offer.md) işlemindeki bildirim e-posta listesini kullanır.

 `POST  https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/golive?api-version=2017-10-31` 

## <a name="uri-parameters"></a>URI parametreleri
--------------

|  **Ad**      |   **Açıklama**                                                           | **Veri türü** |
|  --------      |   ---------------                                                           | ------------- |
| PublisherId    | Örneğin alma teklifi için yayımcı tanımlayıcısı`contoso`       |  Dize       |
| OfferId        | Alma teklifinin teklif tanımlayıcısı                                   |  Dize       |
| api-sürümü    | API 'nin en son sürümü                                                   |  Tarih         |
|  |  |  |

## <a name="header"></a>Üst bilgi
------

|  **Ad**       |     **Değer**       |
|  ---------      |     ----------      |
| İçerik Türü    | `application/json`  |
| Yetkilendirme   | `Bearer YOUR_TOKEN` |
|  |  |

## <a name="body-example"></a>Gövde örneği

### <a name="response"></a>Yanıt

#### <a name="migrated-offers"></a>Geçirilmiş teklifler

`Location: /api/publishers/contoso/offers/contoso-offer/operations/56615b67-2185-49fe-80d2-c4ddf77bb2e8?api-version=2017-10-31`

#### <a name="non-migrated-offers"></a>Geçirilmeyen teklifler

`Location: /api/operations/contoso$contoso-offer$2$preview?api-version=2017-10-31`

### <a name="response-header"></a>Yanıt Üst Bilgisi

|  **Ad**             |      **Değer**                                                            |
|  --------             |      ----------                                                           |
| Konum    |  Bu işlemin durumunu almak için göreli yol            |
|  |  |

### <a name="response-status-codes"></a>Yanıt durum kodları

| **Kod** |  **Açıklama**                                                                        |
| -------- |  ----------------                                                                        |
|  202     | `Accepted`-İstek başarıyla kabul edildi. Yanıt, işlem durumunu izlemek için bir konum içerir. |
|  400     | `Bad/Malformed request`-Yanıt gövdesi içinde ek hata bilgileri bulunur. |
|  404     |  `Not found`-Belirtilen varlık yok.                                       |
|  |  |
