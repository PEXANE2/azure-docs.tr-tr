---
title: Azure Cosmos öykünücüsü indirme ve sürüm notları
description: Azure Cosmos öykünücü sürüm notlarını okuyun ve indirin.
ms.service: cosmos-db
ms.topic: tutorial
author: markjbrown
ms.author: mjbrown
ms.date: 06/20/2019
ms.openlocfilehash: 587c730dfa436760d42e614c2dabee117f3b61d3
ms.sourcegitcommit: 71db032bd5680c9287a7867b923bf6471ba8f6be
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2019
ms.locfileid: "71018813"
---
# <a name="use-the-azure-cosmos-emulator-for-local-development-and-testing"></a>Yerel geliştirme ve test için Azure Cosmos öykünücüsünü kullanma

Bu makalede, Azure Cosmos öykünücü sürüm notları her sürümde yapılan Özellik güncelleştirmelerinin bir listesi ile gösterilir. Ayrıca, indirme ve kullanma öykünücünün en son sürümünü de listeler.

## <a name="download"></a>İndirme

| | |
|---------|---------|
|**MSI indirme**|[Microsoft İndirme Merkezi](https://aka.ms/cosmosdb-emulator)|
|**Kullanmaya başlama**|[Azure Cosmos öykünücüsü ile yerel olarak geliştirme](local-emulator.md)|

## <a name="release-notes"></a>Sürüm notları

### <a name="246"></a>2.4.6

- Bu sürüm Azure Cosmos 2019 hizmetindeki özelliklerle birlikte Azure [Cosmos öykünücüsü ile yerel olarak geliştirme](local-emulator.md)bölümünde belirtilen özel durumlar ile eşlik sağlar. Ayrıca, komut satırı ve doğrudan mod bağlantısı kullanan SDK istemcileri için iç IP adresi geçersiz kılmaları aracılığıyla çağrıldığında öykünücü kapatmaya ilişkin çeşitli hataları düzeltir.

### <a name="243"></a>2.4.3

- MongoDB hizmetinin varsayılan olarak başlatılması devre dışı bırakıldı. Yalnızca SQL uç noktası varsayılan olarak etkindir. Kullanıcı, öykünücü "/EnableMongoDbEndpoint" komut satırı seçeneğini kullanarak uç noktayı el ile başlatmalıdır. Artık Gremlin, Cassandra ve Table gibi diğer tüm hizmet uç noktalarına benzer.
- Gremlin, Cassandra ve tablo uç noktalarının dış istemcilerden gelen istekleri doğru bir şekilde işleyemediğinde "/AllowNetworkAccess" ile Başlarken öykünücüsünde hata düzeltildi.
- Güvenlik duvarı kuralları ayarlarına doğrudan bağlantı bağlantı noktaları ekleyin.

### <a name="240"></a>2.4.0

- Ana bilgisayarda Pulse Istemcisi gibi ağ izleme uygulamaları varsa öykünücü ile ilgili bir sorun düzeltildi.
