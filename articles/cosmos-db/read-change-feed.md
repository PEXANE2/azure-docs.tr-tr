---
title: Azure Cosmos DB Azure Cosmos DB'de değişiklik akışına erişim
description: Bu makalede, Azure Cosmos DB Azure Cosmos DB'deki değişiklik akışını okumak ve erişmek için kullanılabilen farklı seçenekler açıklanmaktadır.
author: TheovanKraay
ms.author: thvankra
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/25/2019
ms.openlocfilehash: fc7e78a44d03af8952c1e178a3e92b1ee0c6fe66
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74688123"
---
# <a name="reading-azure-cosmos-db-change-feed"></a>Azure Cosmos DB değişiklik akışını okuma

Aşağıdaki seçeneklerden herhangi birini kullanarak Azure Cosmos DB değiştirme akışıyla çalışabilirsiniz:

* Azure İşlevlerini kullanma
* Değişiklik akışı işlemci kitaplığını kullanma
* Azure Cosmos DB SQL API SDK’sını kullanma

## <a name="using-azure-functions"></a>Azure İşlevlerini kullanma

Azure İşlevler en basit ve önerilen seçenektir. Cosmos DB için bir Azure İşlevler tetikleyicisi oluşturduğunuzda, bağlanacak kapsayıcıyı seçebilirsiniz ve kapsayıcıda bir değişiklik olduğunda Azure İşlevi tetiklenir. Tetikleyiciler, Azure İşlevler portalı, Azure Cosmos DB portalı veya SDK'larla programlı olarak kullanılarak oluşturulabilir. Visual Studio ve VS Code, Azure İşlevlerini yazmak için destek sağlar ve hatta platformlar arası geliştirme için Azure İşlevleri CLI'yi kullanabilirsiniz. Kodu masaüstünüze yazıp hata ayıklayabilir ve ardından işlevi tek bir tıklatarak dağıtabilirsiniz. Daha fazla bilgi edinmek için Azure İşlevlerini ve [Azure İşlevleriyle değişiklik akışını kullanarak](change-feed-functions.md) [Serverless veritabanı hesaplamasına](serverless-computing-database.md) bakın.

## <a name="using-the-change-feed-processor-library"></a>Değişiklik akışı işlemci kitaplığını kullanma

Değişiklik akışı işlemci kitaplığı karmaşıklığı gizler ve yine de değişiklik akışı tam bir denetim sağlar. Kitaplık, işleme işlevinizin kitaplık tarafından çağrıldığı gözlemci deseni izler. Yüksek iş akışı değişikliği akışınız varsa, değişiklik akışını okumak için birden çok istemciyi anında atabilirsiniz. Besleme işlemci kitaplığını değiştir'i kullandığınızdan, bu mantığı uygulamak zorunda kalmadan yükü otomatik olarak farklı istemciler arasında böler. Tüm karmaşıklık kütüphane tarafından ele alınır. Kendi yük dengeleyiciniz olmasını istiyorsanız, özet `IPartitionLoadBalancingStrategy` akışı değiştirme işlemini işlemek için özel bir bölümleme stratejisi uygulayabilirsiniz. Daha fazla bilgi edinmek [için, besleme işlemci kitaplığını değiştir'i kullanarak](change-feed-processor.md)bkz.

## <a name="using-the-azure-cosmos-db-sql-api-sdk"></a>Azure Cosmos DB SQL API SDK’sını kullanma

SDK ile, değişim akışı düşük düzeyde kontrol olsun. Denetim noktasını yönetebilir, belirli bir mantıksal bölüm anahtarına vb. erişebilirsiniz. Birden çok okuyucunuz varsa, `ChangeFeedOptions` okuma yükünü farklı iş parçacıklarına veya farklı istemcilere dağıtmak için kullanabilirsiniz.

## <a name="change-feed-in-apis-for-cassandra-and-mongodb"></a>Cassandra ve MongoDB için API'lerde besleme değiştirme

Değişiklik akışı işlevi, MongoDB API ve Sorgu'da Cassandra API'de yüklem içeren değişim akışı olarak su yüzüne çıkar. MongoDB API'nin uygulama ayrıntıları hakkında daha fazla bilgi edinmek [için MongoDB için Azure Cosmos DB API'deki Değiştir akışlarına](mongodb-change-streams.md)bakın.

Yerli Apache Cassandra değişim veri yakalama (CDC), arşivleme için belirli tabloları bayrak için bir mekanizma yanı sıra CDC günlüğü için yapılandırılabilir boyutu-on-disk ulaştıktan sonra bu tablolara yazma ları reddederek sağlar. Cassandra için Azure Cosmos DB API'deki değişiklik akışı özelliği, değişiklikleri CQL üzerinden yüklemle sorgulama olanağını geliştirir. Uygulama ayrıntıları hakkında daha fazla bilgi edinmek [için Cassandra için Azure Cosmos DB API'sındaki akış değiştir'e](cassandra-change-feed.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Şimdi aşağıdaki makalelerde değişiklik akışı hakkında daha fazla bilgi edinmek için devam edebilirsiniz:

* [Değişiklik akışına genel bakış](change-feed.md)
* [Azure İşlevleriyle değişiklik akışını kullanma](change-feed-functions.md)
* [Değişiklik beslemeişlemci kitaplığını kullanma](change-feed-processor.md)
