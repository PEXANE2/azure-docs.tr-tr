---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 12/18/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 8c5c0c8f649e7cbab2c16688717de1aaabfb93c5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "75477154"
---
Bu başvuru, Azure Depolama için ölçeklenebilirlik ve performans hedeflerini ayrıntılarıyla anlatır. Burada listelenen ölçeklenebilirlik ve performans hedefleri üst düzey hedeflerdir, ancak ulaşılabilir. Her durumda, depolama hesabınız tarafından elde edilen istek oranı ve bant genişliği depolanan nesnelerin boyutuna, kullanılan erişim desenlerine ve uygulamanızın gerçekleştirdiği iş yükünün türüne bağlıdır.

Performansının gereksinimlerinizi karşılayıp karşılamadığını belirlemek için hizmetinizi test edin. Mümkünse, trafik hızında ani ani artışlardan kaçının ve trafiğin bölümler arasında iyi dağıtıldığından emin olun.

Uygulamanız iş yükünüz için bir bölümün işleyebileceği sınırına ulaştığında, Azure Depolama hata kodu 503 (Sunucu Meşgul) veya hata kodu 500 (Operasyon Zaman) yanıtları döndürmeye başlar. 503 hata oluşuyorsa, yeniden denemeler için üstel bir geri çekilme ilkesi kullanmak üzere uygulamanızı değiştirmeyi düşünün. Üstel geri dönüş, bölüm üzerindeki yükün azalmasını ve bu bölüme giden trafikteki ani artışları azaltmasını sağlar.
