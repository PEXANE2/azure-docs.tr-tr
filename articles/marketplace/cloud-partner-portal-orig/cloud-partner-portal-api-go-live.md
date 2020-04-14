---
title: Canlı Yayına Geç | Azure Marketi
description: Go Live API teklif canlı listeleme işlemini başlatır.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
ms.date: 04/08/2020
ms.author: dsindona
ms.openlocfilehash: ef22f7720a4af2239c55d1a01f9d3f11c878d66e
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81256323"
---
# <a name="go-live"></a>Canlı Yayına Git

> [!NOTE]
> Bulut İş Ortağı Portalı API'leri İş Ortağı Merkezi ile entegre edilmiştir ve teklifleriniz İş Ortağı Merkezi'ne geçtikten sonra çalışmaya devam edecektir. Entegrasyon küçük değişiklikler sunar. Kodunuzu İş Ortağı Merkezi'ne geçişten sonra çalışmaya devam etmesini sağlamak için [Bulut İş Ortağı Portalı API Başvurusu'nda](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-api-overview) listelenen değişiklikleri gözden geçirin.

Bu API, bir uygulamayı üretime itme işlemini başlatır. Bu işlem genellikle uzun sürelidir. Bu arama, [Yayımlama](./cloud-partner-portal-api-publish-offer.md) API işlemindeki bildirim e-posta listesini kullanır.

 `POST  https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/golive?api-version=2017-10-31` 

## <a name="uri-parameters"></a>URI parametreleri
--------------

|  **Adı**      |   **Açıklama**                                                           | **Veri türü** |
|  --------      |   ---------------                                                           | ------------- |
| publisherId    | Örneğin, teklifin alınması için yayımcı tanımlayıcısı`contoso`       |  Dize       |
| offerId        | Teklif tanımlayıcısı almak için teklif                                   |  Dize       |
| api-sürümü    | API'nin en son sürümü                                                   |  Tarih         |
|  |  |  |

## <a name="header"></a>Üst bilgi
------

|  **Adı**       |     **Değer**       |
|  ---------      |     ----------      |
| İçerik Türü    | `application/json`  |
| Yetkilendirme   | `Bearer YOUR_TOKEN` |
|  |  |

## <a name="body-example"></a>Gövde örneği

### <a name="response"></a>Yanıt

#### <a name="migrated-offers"></a>Geçirilen teklifler

`Location: /api/publishers/contoso/offers/contoso-offer/operations/56615b67-2185-49fe-80d2-c4ddf77bb2e8?api-version=2017-10-31`

#### <a name="non-migrated-offers"></a>Geçirilmeyen teklifler

`Location: /api/operations/contoso$contoso-offer$2$preview?api-version=2017-10-31`

### <a name="response-header"></a>Yanıt Üst Bilgisi

|  **Adı**             |      **Değer**                                                            |
|  --------             |      ----------                                                           |
| Konum    |  Bu işlemin durumunu almak için göreli yol            |
|  |  |

### <a name="response-status-codes"></a>Yanıt durum kodları

| **Kod** |  **Açıklama**                                                                        |
| -------- |  ----------------                                                                        |
|  202     | `Accepted`- İstek başarıyla kabul edildi. Yanıt, işlem durumunu izlemek için bir konum içerir. |
|  400     | `Bad/Malformed request`- Yanıt gövdesiiçinde ek hata bilgileri bulunur. |
|  404     |  `Not found`- Belirtilen varlık yok.                                       |
|  |  |
