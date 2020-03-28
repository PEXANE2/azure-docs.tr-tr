---
title: Azure Cosmos Emülatör indirme ve sürüm notları
description: Azure Cosmos emülatörü sürüm notlarını farklı sürümler için alın ve bilgileri indirin.
ms.service: cosmos-db
ms.topic: tutorial
author: milismsft
ms.author: adrianmi
ms.date: 06/20/2019
ms.openlocfilehash: 4dffe169908d0dd3effa4e46140b5f6696805a3e
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77168643"
---
# <a name="azure-cosmos-emulator---release-notes-and-download-information"></a>Azure Cosmos Emülatörü - Notları ve indirme bilgilerini yayın

Bu makalede, azure cosmos emülatörü sürüm notları, her sürümde yapılan özellik güncelleştirmelerinin bir listesini gösterir. Ayrıca indirmek ve kullanmak için emülatör en son sürümünü listeler.

## <a name="download"></a>İndirme

| | |
|---------|---------|
|**MSI indir**|[Microsoft İndirme Merkezi](https://aka.ms/cosmosdb-emulator)|
|**Kullanmaya başlayın**|[Azure Cosmos emülatörü ile yerel olarak geliştirin](local-emulator.md)|

## <a name="release-notes"></a>Sürüm notları

### <a name="291"></a>2.9.1

- Bu sürüm, sorgu API desteğindeki çift sorunları giderir ve Windows Server 2012 gibi eski İş Su İşleriyle uyumluluğu geri yükler.

### <a name="290"></a>2.9.0

- Bu sürüm tutarlı önek için tutarlılık ayarlamak ve kullanıcılar ve izinler için maksimum sınırları artırmak için seçenek ekler.

### <a name="272"></a>2.7.2

- Bu sürüm Cosmos Emulator MongoDB sürüm 3.6 sunucu desteği ekler. Hizmetin 3.6 sürümünü hedefleyen bir MongoDB bitiş noktası başlatmak için emülatöre Yönetici komut satırından "/EnableMongoDBEndpoint=3.6" seçeneğiyle başlayın.

### <a name="270"></a>2.7.0

- Bu sürüm, kullanıcıların .NET core veya x86 .NET tabanlı istemcileri kullanırken emülatörden SQL API hesabına karşı sorgu yürütmesini engelleyen bir gerilemeyi giderir.

### <a name="246"></a>2.4.6

- Bu sürüm, Azure [Cosmos emülatörü ile yerel olarak geliştir'de](local-emulator.md)belirtilen istisnalar dışında, Temmuz 2019 itibarıyla Azure Cosmos hizmetindeki özelliklerle eşitlik sağlar. Ayrıca, doğrudan mod bağlantısı kullanarak SDK istemcileri için komut satırı ve dahili IP adresi geçersiz kılmaları üzerinden çağrıldığınızda emülatör kapatma ile ilgili birkaç hata giderir.

### <a name="243"></a>2.4.3

- Varsayılan olarak MongoDB hizmetini başlatarak devre dışı bırakılır. Varsayılan olarak yalnızca SQL bitiş noktası etkinleştirilir. Kullanıcı, emülatörün "/EnableMongoDbEndpoint" komut satırı seçeneğini kullanarak bitiş noktasını el ile başlatmalıdır. Şimdi, Gremlin, Cassandra ve Table gibi diğer tüm servis uç noktaları gibi.
- Gremlin, Cassandra ve Table uç noktalarının harici istemcilerden gelen istekleri düzgün şekilde işlemediği "/AllowNetworkAccess" ile başlarken emülatördeki bir hata düzeltildi.
- Güvenlik Duvarı Kuralları ayarlarına doğrudan bağlantı bağlantı noktaları ekleyin.

### <a name="240"></a>2.4.0

- Emülatör, Pulse Client gibi ağ izleme uygulamaları ana bilgisayarda bulunduğunda başlatılamamasıyla ilgili bir sorun giderildi.
