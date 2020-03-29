---
title: Render yöneticisi desteği - Azure Toplu İş
description: Azure Toplu Işoluşturma yöneticisi tümleştirmesi kullanma. Popüler render yöneticileri için yerleşik destek veya eklentiler hakkında bilgi edinin.
services: batch
ms.service: batch
author: mscurrell
ms.author: markscu
ms.date: 08/02/2018
ms.topic: conceptual
ms.openlocfilehash: 246907b16534d1a91833cab633a1973c97429f47
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75449693"
---
# <a name="using-azure-batch-with-render-farm-managers"></a>Render farm yöneticileriyle Azure Toplu İşlemini kullanma

Mevcut bir şirket içi render çiftliği kullanıyorsanız, render yöneticisinin işleme farm kapasitesini kontrol etme ve işleri oluşturma olasılığı yüksektir.

Azure, popüler render yöneticileri için yerleşik destek veya eklentisağlar. Daha sonra, kullanım için ödemeli uygulama lisansı ve düşük öncelikli VM'ler ile VM'ler de dahil olmak üzere Azure VM'leri ekleyebilir ve kaldırabilirsiniz.

Aşağıdaki render yöneticileri desteklenir:

* [PipelineFX Qube!](https://www.pipelinefx.com/)
* [Royal Render](https://www.royalrender.de/)
* [Thinkbox Son Tarih](https://deadline.thinkboxsoftware.com/)

## <a name="azure-render-hub"></a>Azure İşoluşturma Merkezi

Azure Render Hub, Azure render çiftliklerinin oluşturulmasını ve yönetimini kolaylaştırır.  Render Hub, PipelineFx Qube ve Deadline 10 için yerel desteğe sahiptir.  Daha fazla bilgi ve ayrıntılı talimatlar için [GitHub deposuna](https://github.com/Azure/azure-render-hub)bakın.

## <a name="using-azure-with-pipelinefx-qube"></a>PipelineFX Qube ile Azure'u kullanma

Azure Render Hub,Son Tarih de dahil olmak üzere popüler render yöneticilerini destekler.  Render Hub'ı dağıtma ve kullanma yla ilgili talimatlar için [GitHub deposuna](https://github.com/Azure/azure-render-hub)bakın.

Azure Toplu Toplu Havuz VM'lerinin Qube çalışanları olarak kullanılmasını sağlayacak komut dosyaları ve yönergeler [GitHub deposunda](https://github.com/Azure/azure-qube)da mevcuttur.

## <a name="using-azure-with-royal-render"></a>Azure'u Royal Render ile kullanma

Royal Render, Azure tabanlı VM'lerle bir render çiftliği genişletmenize olanak tanıyan Yerleşik Azure ve Azure Toplu İş tümleştirmesine sahiptir. Özet için [yardım dosyalarına](https://www.royalrender.de/help8/index.html?Cloudrendering.html)bakın.

Azure tümleştirmesini kullanan Bir Royal Render müşterisi örneği için [Denizanası Resimleri müşteri hikayesine](https://customers.microsoft.com/story/jellyfishpictures)bakın.

## <a name="using-azure-with-thinkbox-deadline"></a>Thinkbox Son Tarih ile Azure kullanma

Azure Render Hub,Son Tarih de dahil olmak üzere popüler render yöneticilerini destekler.  Render Hub'ı dağıtma ve kullanma yla ilgili talimatlar için [GitHub deposuna](https://github.com/Azure/azure-render-hub)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Uygun eklentive yönergeleri github'da kullanarak, uygun eklentiyi ve yönergeleri kullanarak render yöneticiniz için Azure Toplu Tümleştirme'yi deneyin.
