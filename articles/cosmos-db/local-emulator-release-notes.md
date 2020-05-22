---
title: Azure Cosmos öykünücüsü indirme ve sürüm notları
description: Farklı sürümler ve indirme bilgileri için Azure Cosmos öykünücü sürüm notlarını alın.
ms.service: cosmos-db
ms.topic: tutorial
author: milismsft
ms.author: adrianmi
ms.date: 06/20/2019
ms.openlocfilehash: 6dad625bbbcb8279e83ac42e1492c251d5b0299c
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83746998"
---
# <a name="azure-cosmos-emulator---release-notes-and-download-information"></a>Azure Cosmos öykünücüsü-sürüm notları ve indirme bilgileri

Bu makalede, Azure Cosmos öykünücü sürüm notları her sürümde yapılan Özellik güncelleştirmelerinin bir listesi ile gösterilir. Ayrıca, indirme ve kullanma öykünücünün en son sürümünü de listeler.

## <a name="download"></a>İndir

| | |
|---------|---------|
|**MSI indirme**|[Microsoft İndirme Merkezi](https://aka.ms/cosmosdb-emulator)|
|**Kullanmaya başlayın**|[Azure Cosmos öykünücüsü ile yerel olarak geliştirme](local-emulator.md)|

## <a name="release-notes"></a>Sürüm notları

### <a name="2110"></a>2.11.0

- Bu sürüm, otomatik ölçeklendirme sağlanan aktarım hızı için destek sunar. Bu yeni özellikler, istek birimleri (RU/s) içinde özel bir en yüksek sağlanan aktarım hızı düzeyi ayarlamanıza, mevcut veritabanlarında ve kapsayıcılarda otomatik ölçeklendirmeyi etkinleştirmesine ve Azure Cosmos DB SDK 'lar aracılığıyla programlı destek sağlanmasına olanak tanır.
- Büyük miktarda belge (1GB fazla) ile sorgulama yaparken bir sorunu çözme, öykünücü iç hata durum kodu 500 ile başarısız olur.

### <a name="292"></a>2.9.2

- Bu sürüm, MongoDb uç noktası sürüm 3,2 desteğini etkinleştirirken bir hatayı düzeltir. Ayrıca, LOGMAN yerine WPR kullanarak sorun giderme amacıyla ETL izlemeleri oluşturmaya yönelik destek ekler.

### <a name="291"></a>2.9.1

- Bu sürüm, sorgu API 'SI desteğinin her iki sorununu düzeltir ve Windows Server 2012 gibi eski OSs ile uyumluluğu geri yükler.

### <a name="290"></a>2.9.0

- Bu sürüm, tutarlılığı tutarlı ön ek olarak ayarlama ve kullanıcılar ve izinler için maksimum sınırları artırma seçeneğini ekler.

### <a name="272"></a>2.7.2

- Bu sürüm, Cosmos öykünücüsüne MongoDB sürüm 3,6 sunucu desteği ekler. Hizmetin 3,6 sürümünü hedef alan bir MongoDB uç noktasını başlatmak için, "/EnableMongoDBEndpoint = 3.6" seçeneğiyle yönetici komut satırından öykünücü 'yı başlatın.

### <a name="270"></a>2.7.0

- Bu sürüm, .NET Core veya x86 .NET tabanlı istemcileri kullanırken kullanıcılardan SQL API hesabında sorgu yürütmesini önleyen bir gerileme düzeltir.

### <a name="246"></a>2.4.6

- Bu sürüm Azure Cosmos 2019 hizmetindeki özelliklerle birlikte Azure [Cosmos öykünücüsü ile yerel olarak geliştirme](local-emulator.md)bölümünde belirtilen özel durumlar ile eşlik sağlar. Ayrıca, komut satırı ve doğrudan mod bağlantısı kullanan SDK istemcileri için iç IP adresi geçersiz kılmaları aracılığıyla çağrıldığında öykünücü kapatmaya ilişkin çeşitli hataları düzeltir.

### <a name="243"></a>2.4.3

- MongoDB hizmetinin varsayılan olarak başlatılması devre dışı bırakıldı. Yalnızca SQL uç noktası varsayılan olarak etkindir. Kullanıcı, öykünücü "/EnableMongoDbEndpoint" komut satırı seçeneğini kullanarak uç noktayı el ile başlatmalıdır. Artık Gremlin, Cassandra ve Table gibi diğer tüm hizmet uç noktalarına benzer.
- Gremlin, Cassandra ve tablo uç noktalarının dış istemcilerden gelen istekleri doğru bir şekilde işleyemediğinde "/AllowNetworkAccess" ile Başlarken öykünücüsünde hata düzeltildi.
- Güvenlik duvarı kuralları ayarlarına doğrudan bağlantı bağlantı noktaları ekleyin.

### <a name="240"></a>2.4.0

- Ana bilgisayarda Pulse Istemcisi gibi ağ izleme uygulamaları varsa öykünücü ile ilgili bir sorun düzeltildi.
