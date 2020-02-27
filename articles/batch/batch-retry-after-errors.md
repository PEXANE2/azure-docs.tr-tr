---
title: Bir hatadan sonra görevleri yeniden deneme
description: Hataları denetleyip yeniden deneyin.
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
ms.openlocfilehash: eda567fda13d6caca679d0ce4947e042eca9530d
ms.sourcegitcommit: 96dc60c7eb4f210cacc78de88c9527f302f141a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2020
ms.locfileid: "77652015"
---
# <a name="detecting-and-handling-batch-service-errors"></a>Batch hizmeti hatalarını algılama ve işleme

REST hizmeti API 'SI ile çalışırken hataları denetlemeyi unutmamak önemlidir. Toplu işler çalıştırılırken hata oluşması yaygın değildir.

## <a name="common-errors"></a>Sık karşılaşılan hatalar 

- Ağ sorunları-toplu Işe erişilemeyen isteklerdir veya Batch yanıtı istemciye zamanında ulaşamamıştır.
- İç sunucu hataları-Bunlar standart 5xx durum kodu HTTP yanıtdır.
- Daraltma, yeniden deneme üst bilgisi ile 429 veya 503 durum kodu HTTP yanıtları gibi hatalara neden olabilir.
- 4xx hatası, AlreadyExists ve InvalidOperation gibi hataları içerir. Bu, kaynağın durum geçişi için doğru durumda olmadığı anlamına gelir.

## <a name="when-to-retry"></a>Ne zaman yeniden deneneceğini

Bir hata oluşursa Batch API 'Leri size bildirir. Hepsi yeniden denenebilirler ve hepsi bu amaçla küresel bir yeniden deneme işleyicisi içerir. Bu yerleşik mekanizmayı kullanmak en iyisidir.

Bir hatadan sonra yeniden denemeden önce bir bit (denemeler arasında birkaç saniye) beklemeniz gerekir. Çok sık veya çok hızlı yeniden deneme yaparsanız, yeniden deneme işleyicisi azalacaktır.


Her API ve varsayılan yeniden deneme ilkeleri hakkında ayrıntılı bilgi için [Batch durumu ve hata kodlarını](https://docs.microsoft.com/rest/api/batchservice/batch-status-and-error-codes)okuyun.
