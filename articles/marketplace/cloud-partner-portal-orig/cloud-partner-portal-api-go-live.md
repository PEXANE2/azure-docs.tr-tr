---
title: Canlı Yayına Geç | Azure Marketi
description: Go Live API teklif canlı listeleme işlemini başlatır.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
ms.date: 09/13/2018
ms.author: dsindona
ms.openlocfilehash: bf7bebf6e72e373811879a311d70255c29988ed6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80288589"
---
<a name="go-live"></a>Canlı Yayına Git
=======

Bu API, bir uygulamayı üretime itme işlemini başlatır. Bu işlem genellikle uzun sürelidir. Bu arama, [Yayımlama](./cloud-partner-portal-api-publish-offer.md) API işlemindeki bildirim e-posta listesini kullanır.

 `POST  https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/golive?api-version=2017-10-31` 

<a name="uri-parameters"></a>URI parametreleri
--------------

|  **Adı**      |   **Açıklama**                                                           | **Veri türü** |
|  --------      |   ---------------                                                           | ------------- |
| publisherId    | Örneğin, teklifin alınması için yayımcı tanımlayıcısı`contoso`       |  Dize       |
| offerId        | Teklif tanımlayıcısı almak için teklif                                   |  Dize       |
| api-sürümü    | API'nin en son sürümü                                                   |  Tarih         |
|  |  |  |


<a name="header"></a>Üst bilgi
------

|  **Adı**       |     **Değer**       |
|  ---------      |     ----------      |
| İçerik Türü    | `application/json`  |
| Yetkilendirme   | `Bearer YOUR_TOKEN` |
|  |  |


<a name="body-example"></a>Gövde örneği
------------

### <a name="response"></a>Yanıt

`Operation-Location: https://cloudpartner.azure.com/api/publishers/contoso/offers/contoso-virtualmachineoffer/operations/56615b67-2185-49fe-80d2-c4ddf77bb2e8`


### <a name="response-header"></a>Yanıt Üst Bilgisi

|  **Adı**             |      **Değer**                                                            |
|  --------             |      ----------                                                           |
| Operasyon-Konum    |  İşlemin geçerli durumunu belirlemek için sorgulanacak URL            |
|  |  |


### <a name="response-status-codes"></a>Yanıt durum kodları

| **Kod** |  **Açıklama**                                                                        |
| -------- |  ----------------                                                                        |
|  202     | `Accepted`- İstek başarıyla kabul edildi. Yanıt, işlem durumunu izlemek için bir konum içerir. |
|  400     | `Bad/Malformed request`- Yanıt gövdesiiçinde ek hata bilgileri bulunur. |
|  404     |  `Not found`- Belirtilen varlık yok.                                       |
|  |  |
