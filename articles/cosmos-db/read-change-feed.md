---
title: Azure Cosmos DB Azure Cosmos DB değişiklik akışına erişme
description: Bu makalede, Azure Cosmos DB Azure Cosmos DB ' de değişiklik akışını okuma ve erişme için kullanabileceğiniz farklı seçenekler açıklanmaktadır.
author: TheovanKraay
ms.author: thvankra
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/25/2019
ms.openlocfilehash: fc7e78a44d03af8952c1e178a3e92b1ee0c6fe66
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "74688123"
---
# <a name="reading-azure-cosmos-db-change-feed"></a>Azure Cosmos DB değişiklik akışını okuma

Aşağıdaki seçeneklerden herhangi birini kullanarak Azure Cosmos DB değişiklik akışı ile çalışabilirsiniz:

* Azure İşlevlerini kullanma
* Değişiklik akışı işlemci kitaplığını kullanma
* Azure Cosmos DB SQL API SDK’sını kullanma

## <a name="using-azure-functions"></a>Azure İşlevlerini kullanma

Azure Işlevleri, en basit ve önerilen seçenektir. Cosmos DB için bir Azure Işlevleri tetikleyicisi oluşturduğunuzda, bağlanacak kapsayıcıyı seçebilirsiniz ve kapsayıcıda her değişiklik yapıldığında Azure Işlevi tetiklenir. Tetikleyiciler, Azure Işlevleri portalı, Azure Cosmos DB portalı veya SDK 'lar aracılığıyla program aracılığıyla oluşturulabilir. Visual Studio ve VS Code Azure Işlevleri yazmak için destek sağlar ve platformlar arası geliştirme için Azure Işlevleri CLı 'yi de kullanabilirsiniz. Masaüstünüzdeki kodu yazabilir ve hata ayıklamanıza ve sonra işlevi tek bir tıklama ile dağıtmanıza de tıklayabilirsiniz. Daha fazla bilgi için bkz. [Azure işlevleri 'ni kullanarak sunucusuz veritabanı hesaplama](serverless-computing-database.md) ve [Azure işlevleri Ile değişiklik akışını kullanma](change-feed-functions.md)) makaleleri.

## <a name="using-the-change-feed-processor-library"></a>Değişiklik akışı işlemci kitaplığını kullanma

Değişiklik akışı işlemci kitaplığı karmaşıklığı gizler ve yine de değişiklik akışına ilişkin kapsamlı bir denetim sağlar. Kitaplık, işleme işlevinizin kitaplık tarafından çağrıldığı gözlemci düzeniyle uyar. Yüksek verimlilik değişikliği akışınız varsa, değişiklik akışını okumak için birden çok istemci örneği oluşturabilirsiniz. Değişiklik akışı işlemci kitaplığını kullandığınız için, bu mantığı uygulamak zorunda kalmadan yükü farklı istemciler arasında otomatik olarak böler. Tüm karmaşıklıklar kitaplık tarafından işlenir. Kendi yük dengeleyicinizi kullanmak istiyorsanız, `IPartitionLoadBalancingStrategy` değişiklik akışını işlemek için özel bir bölüm stratejisi uygulayabilirsiniz. Daha fazla bilgi edinmek için bkz. [değişiklik akışı işlemci Kitaplığı kullanma](change-feed-processor.md).

## <a name="using-the-azure-cosmos-db-sql-api-sdk"></a>Azure Cosmos DB SQL API SDK’sını kullanma

SDK ile, değişiklik akışında düşük düzey bir denetim edinirsiniz. Denetim noktasını yönetebilir, belirli bir mantıksal bölüm anahtarına erişebilir, vb. Birden çok okuyucularınız varsa, okuma yükünü farklı `ChangeFeedOptions` iş parçacıklarına veya farklı istemcilere dağıtmak için kullanabilirsiniz.

## <a name="change-feed-in-apis-for-cassandra-and-mongodb"></a>Cassandra ve MongoDB API 'Lerinde akışı değiştirme

Değişiklik akışı işlevselliği, MongoDB API 'sinde değişiklik akışı olarak ve Cassandra API ' de koşul ile sorgulama olarak ortaya çıkmış. MongoDB API 'SI için uygulama ayrıntıları hakkında daha fazla bilgi edinmek için bkz. [MongoDB için Azure Cosmos DB API 'sindeki akışları değiştirme](mongodb-change-streams.md).

Native Apache Cassandra değişiklik verilerini yakalama (CDC), belirli tabloları arşivleme için bayrakla işaretleme ve CDC günlüğü için yapılandırılabilir bir diske ulaşıldığında bu tablolara yazmaları reddetme mekanizması sağlar. Cassandra için Azure Cosmos DB API 'sindeki akış değiştirme özelliği, değişiklikleri CQL aracılığıyla koşullu olarak sorgulama yeteneğini geliştirir. Uygulama ayrıntıları hakkında daha fazla bilgi edinmek için bkz. [Cassandra için Azure Cosmos DB API 'sindeki akışı değiştirme](cassandra-change-feed.md).

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalelerde değişiklik akışı hakkında daha fazla bilgi edinmek için devam edebilirsiniz:

* [Değişiklik akışına genel bakış](change-feed.md)
* [Azure Işlevleri ile değişiklik akışını kullanma](change-feed-functions.md)
* [Değişiklik akışı işlemci kitaplığını kullanma](change-feed-processor.md)
