---
title: dosya dahil etme
description: dosya dahil etme
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 12/18/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 8c5c0c8f649e7cbab2c16688717de1aaabfb93c5
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "75477154"
---
Bu başvuru, Azure depolama için ölçeklenebilirlik ve performans hedeflerini ayrıntılardır. Burada listelenen ölçeklenebilirlik ve performans hedefleri, yüksek kaliteli hedeflerdir, ancak ulaşılabilir ' dir. Her durumda, depolama hesabınız tarafından elde edilen istek hızı ve bant genişliği depolanan nesnelerin boyutuna, kullanılan erişim desenlerine ve uygulamanızın gerçekleştirdiği iş yükünün türüne bağlıdır.

Performansının, gereksinimlerinizi karşılayıp karşılamadığını öğrenmek için hizmetinizi test ettiğinizden emin olun. Mümkünse, trafik hızında ani artışlar önleyin ve trafiğin bölümler arasında iyi bir şekilde dağıtıldığından emin olun.

Uygulamanız iş yükünüz için bir bölümün işleyebileceği sınıra ulaştığında, Azure Storage 503 (sunucu meşgul) hata kodunu veya hata kodu 500 (Işlem zaman aşımı) yanıtlarını döndürmeye başlar. 503 hatası oluşuyorsa, yeniden denemeler için uygulamanızı bir üstel geri alma ilkesi kullanacak şekilde değiştirmeyi göz önünde bulundurun. Üstel geri alma, bölümdeki yükün azaltılmasını ve söz konusu bölüme giden trafik artışlarını kolaylaştırır.
