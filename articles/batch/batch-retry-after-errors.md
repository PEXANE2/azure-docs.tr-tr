---
title: Hatadan sonra görevleri yeniden deneme
description: Hataları denetleyin ve yeniden deneyin.
services: batch
documentationcenter: .net
author: LauraBrenner
manager: evansma
editor: ''
tags: azure-batch
ms.assetid: 16279b23-60ff-4b16-b308-5de000e4c028
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 02/15/2020
ms.author: labrenne
ms.custom: seodec18
ms.openlocfilehash: 94ed936e619461a2dbf7ec837c2d80e21c01c88e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77920000"
---
# <a name="detecting-and-handling-batch-service-errors"></a>Toplu servis hatalarını algılama ve işleme

Bir REST hizmeti API ile çalışırken hataları kontrol etmeyi unutmayın önemlidir. Toplu iş çalışırken hataların oluşması sık rastlanan bir durum değildir.

## <a name="common-errors"></a>Sık karşılaşılan hatalar 

- Ağ hataları - bunlar Toplu İşleme'ye hiç ulaşmamış veya Toplu İşlem yanıtı istemciye zamanında ulaşmamış isteklerdir.
- Dahili sunucu hataları - bu standart 5xx durum kodu HTTP yanıt vardır.
- Azaltma, Yeniden Deneme üstbilgisiyle 429 veya 503 durum kodu HTTP yanıtları gibi hatalara neden olabilir.
- AlreadyExists ve Geçersiz Çalışma gibi hataları içeren 4xx hataları. Bu, kaynağın durum geçişi için doğru durumda olmadığı anlamına gelir.

Çeşitli hata kodları ve belirli hata kodları hakkında ayrıntılı bilgi için [Toplu İşlem Durumu ve Hata Kodları'na](https://docs.microsoft.com/rest/api/batchservice/batch-status-and-error-codes)bakın.

## <a name="when-to-retry"></a>Ne zaman yeniden denemelisin

Toplu İşlem API'ler bir hata olduğunda sizi bilgilendirecektir. Hepsi yeniden denenebilir ve hepsi bu amaç için küresel bir yeniden deneme işleyicisi içerir. Bu yerleşik mekanizmayı kullanmak en iyisidir.

Bir hatadan sonra, yeniden denemeden önce biraz (yeniden denemeler arasında birkaç saniye) beklemeniz gerekir. Çok sık veya çok hızlı bir şekilde yeniden denerseniz, yeniden deneme işleyicisi gazla çalışır.

### <a name="for-more-information"></a>Daha fazla bilgi edinmek için  

[Toplu API'ler ve araçlar](batch-apis-tools.md) API başvuru bilgilerine bağlantılar. Örneğin .NET API'de, gerekli yeniden deneme ilkesinin belirtilmesi gereken bir [RetryPolicyProvider sınıfı]( https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.retrypolicyprovider?view=azure-dotnet) vardır. 

Her API ve varsayılan yeniden deneme ilkeleri hakkında ayrıntılı bilgi için [Toplu İşlem Durumu ve Hata Kodları'nı](https://docs.microsoft.com/rest/api/batchservice/batch-status-and-error-codes)okuyun.
