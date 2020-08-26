---
title: Sorun giderme Azure Cosmos DB istek hızı çok büyük özel durumlar
description: İstek hızının çok büyük özel durumlarını tanılamayı ve düzeltmesini öğrenin.
author: j82w
ms.service: cosmos-db
ms.date: 07/13/2020
ms.author: jawilley
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: f8b1129c94ecf80efb60a13a0b80b1cc1817ff3f
ms.sourcegitcommit: 927dd0e3d44d48b413b446384214f4661f33db04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88871097"
---
# <a name="diagnose-and-troubleshoot-azure-cosmos-db-request-rate-too-large-exceptions"></a>Tanılama ve sorun giderme Azure Cosmos DB istek hızı çok büyük özel durumlar
"Istek hızı çok büyük" iletisi veya 429 hata kodu, isteklerinizin kısıtlandığını gösterir.

## <a name="troubleshooting-steps"></a>Sorun giderme adımları
Aşağıdaki bölümde çok fazla istek için bilinen nedenler ve çözümler yer almaktadır.

### <a name="check-the-metrics"></a>Ölçümleri denetleyin
429 özel durum sayısını görmek için [Azure Cosmos DB izlemeyi](monitor-cosmos-db.md) denetleyin.

#### <a name="cause"></a>Neden:
Tüketilen üretilen iş (saniye başına Istek birimi), [sağlanan aktarım hızını](set-throughput.md)aşmış. SDK, belirtilen yeniden deneme ilkesine göre istekleri otomatik olarak yeniden dener. Bu hatayla sık sık karşılaşırsanız, koleksiyondaki aktarım hızını artırmayı düşünün. 429 hatası olup olmadığınızı görmek için portalın ölçümlerini denetleyin. [Depolama ve istek hacminin eşit](partition-data.md)bir şekilde dağıtımına neden olduğundan emin olmak için bölüm anahtarınızı gözden geçirin.

#### <a name="solution"></a>Çözüm:
1. Sağlanan aktarım hızını artırmak için [portalı veya SDK 'yı](set-throughput.md) kullanın.
1. Veritabanını veya kapsayıcıyı [Otomatik ölçeklendirme](provision-throughput-autoscale.md)ile değiştirin.

## <a name="next-steps"></a>Sonraki adımlar
* Azure Cosmos DB .NET SDK kullandığınızda sorunları [tanılayın ve sorun giderin](troubleshoot-dot-net-sdk.md) .
* [.Net v3](performance-tips-dotnet-sdk-v3-sql.md) ve [.net v2](performance-tips.md)için performans yönergeleri hakkında bilgi edinin.