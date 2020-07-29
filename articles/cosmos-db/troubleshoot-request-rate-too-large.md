---
title: Azure Cosmos DB istek hızını büyük özel duruma göre giderin
description: İstek hızını tanılama ve büyük özel durum çözme
author: j82w
ms.service: cosmos-db
ms.date: 07/13/2020
ms.author: jawilley
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: b20ee2e10648d2d8eb248fc3f98c4fb33805f914
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87294526"
---
# <a name="diagnose-and-troubleshoot-cosmos-db-too-many-requests"></a>Çok fazla istek Cosmos DB tanılama ve sorun giderme
' İstek hızı çok büyük ' veya 429 hata kodu, isteklerinizin kısıtlandığını gösterir.

## <a name="troubleshooting-steps"></a>Sorun giderme adımları
Aşağıdaki listede çok fazla istek için bilinen nedenler ve çözümler yer almaktadır.

### <a name="1-check-the-metrics"></a>1. ölçümleri denetleyin
Müşteri, 429 özel durum sayısını kontrol etmek için [Azure Cosmos DB izlemeyi](monitor-cosmos-db.md) denetlemelidir.

## <a name="cause"></a>Neden:
Tüketilen üretilen iş (RU/s), [sağlanan aktarım hızını](set-throughput.md)aşmış. SDK, belirtilen yeniden deneme ilkesine göre istekleri otomatik olarak yeniden dener. Bu hatayla sık sık karşılaşırsanız, koleksiyondaki aktarım hızını artırmayı düşünün. 429 hatası olup olmadığınızı görmek için portalın ölçümlerini denetleyin. [Depolama ve istek hacminin eşit](partition-data.md)bir şekilde dağıtımına neden olduğundan emin olmak için bölüm anahtarınızı gözden geçirin.

## <a name="solution"></a>Çözüm:
1. Sağlanan aktarım hızını artırmak için [portalı veya SDK 'yı](set-throughput.md) kullanın.
2. Veritabanını veya kapsayıcıyı [Otomatik ölçeklendirme](provision-throughput-autoscale.md)ile değiştirin.

## <a name="next-steps"></a>Sonraki adımlar
* Azure Cosmos DB .NET SDK kullanırken sorunları [Tanılama ve giderme](troubleshoot-dot-net-sdk.md)
* [.Net v3](performance-tips-dotnet-sdk-v3-sql.md) ve [.net v2](performance-tips.md) için performans yönergeleri hakkında bilgi edinin