---
title: Azure Cosmos DB Azure Cosmos DB değişiklik akışına erişme
description: Bu makalede, Azure Cosmos DB değişiklik akışını okuma ve erişme için kullanabileceğiniz farklı seçenekler açıklanmaktadır.
author: timsander1
ms.author: tisande
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/06/2020
ms.reviewer: sngun
ms.openlocfilehash: 266a13b7702f567e69129c0b4e92c4bd7c0f29ef
ms.sourcegitcommit: 999ccaf74347605e32505cbcfd6121163560a4ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82982505"
---
# <a name="reading-azure-cosmos-db-change-feed"></a>Azure Cosmos DB değişiklik akışını okuma

Aşağıdaki seçeneklerden herhangi birini kullanarak Azure Cosmos DB değişiklik akışı ile çalışabilirsiniz:

* Azure İşlevlerini kullanma
* Değişiklik akışı işlemcisini kullanma
* Azure Cosmos DB SQL API SDK’sını kullanma
* Değişiklik akışı çekme modelini kullanma (Önizleme)

## <a name="using-azure-functions"></a>Azure İşlevlerini kullanma

Azure Işlevleri, en basit ve önerilen seçenektir. Cosmos DB için bir Azure Işlevleri tetikleyicisi oluşturduğunuzda, bağlanacak kapsayıcıyı seçebilirsiniz ve kapsayıcıda her değişiklik yapıldığında Azure Işlevi tetiklenir. Tetikleyiciler, Azure Işlevleri portalı, Azure Cosmos DB portalı veya SDK 'lar aracılığıyla program aracılığıyla oluşturulabilir. Visual Studio ve VS Code Azure Işlevleri yazmak için destek sağlar ve platformlar arası geliştirme için Azure Işlevleri CLı 'yi de kullanabilirsiniz. Masaüstünüzdeki kodu yazabilir ve hata ayıklamanıza ve sonra işlevi tek bir tıklama ile dağıtmanıza de tıklayabilirsiniz. Daha fazla bilgi için bkz. [Azure işlevleri 'ni kullanarak sunucusuz veritabanı hesaplama](serverless-computing-database.md) ve [Azure işlevleri Ile değişiklik akışını kullanma](change-feed-functions.md)) makaleleri.

## <a name="using-the-change-feed-processor"></a>Değişiklik akışı işlemcisini kullanma

Değişiklik akışı işlemcisi karmaşıklığı gizler ve yine de değişiklik akışına ilişkin kapsamlı bir denetim sağlar. Kitaplık, işleme işlevinizin kitaplık tarafından çağrıldığı gözlemci düzeniyle uyar. Yüksek verimlilik değişikliği akışınız varsa, değişiklik akışını okumak için birden çok istemci örneği oluşturabilirsiniz. Değişiklik akışı işlemci kitaplığını kullandığınız için, bu mantığı uygulamak zorunda kalmadan yükü farklı istemciler arasında otomatik olarak böler. Tüm karmaşıklıklar kitaplık tarafından işlenir. Daha fazla bilgi için bkz. [değişiklik akışı işlemcisini kullanma](change-feed-processor.md). Değişiklik akışı işlemcisi, [Azure Cosmos DB SDK V3](https://github.com/Azure/azure-cosmos-dotnet-v3)'nin bir parçasıdır.

## <a name="using-the-azure-cosmos-db-sql-api-sdk"></a>Azure Cosmos DB SQL API SDK’sını kullanma

SDK ile, değişiklik akışında düşük düzey bir denetim edinirsiniz. Denetim noktasını yönetebilir, belirli bir mantıksal bölüm anahtarına erişebilir, vb. Birden çok okuyucularınız varsa, okuma yükünü farklı `ChangeFeedOptions` iş parçacıklarına veya farklı istemcilere dağıtmak için kullanabilirsiniz.

## <a name="using-the-change-feed-pull-model"></a>Değişiklik akışı çekme modelini kullanma

[Değişiklik akışı çekme modeli](change-feed-pull-model.md) , değişiklik akışını kendi hızınızda kullanmanıza ve paralel hale getirmek değişiklikleri feedranges ile işlemenize olanak tanır. Bir FeedRange, bölüm anahtarı değerlerini bir aralığa yaymıştır. Değişiklik akışı çekme modelini kullanarak belirli bir bölüm anahtarı için değişiklikleri işlemek de kolaydır.

> [!NOTE]
> Değişiklik akışı çekme modeli, şu anda yalnızca [Azure Cosmos DB .NET SDK sürümünde önizlemededir](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/3.9.0-preview) . Önizleme henüz diğer SDK sürümleri için kullanılamaz.

## <a name="change-feed-in-apis-for-cassandra-and-mongodb"></a>Cassandra ve MongoDB API 'Lerinde akışı değiştirme

Değişiklik akışı işlevselliği, MongoDB API 'sinde değişiklik akışı olarak ve Cassandra API ' de koşul ile sorgulama olarak ortaya çıkmış. MongoDB API 'SI için uygulama ayrıntıları hakkında daha fazla bilgi edinmek için bkz. [MongoDB için Azure Cosmos DB API 'sindeki akışları değiştirme](mongodb-change-streams.md).

Native Apache Cassandra değişiklik verilerini yakalama (CDC), belirli tabloları arşivleme için bayrakla işaretleme ve CDC günlüğü için yapılandırılabilir bir diske ulaşıldığında bu tablolara yazmaları reddetme mekanizması sağlar. Cassandra için Azure Cosmos DB API 'sindeki akış değiştirme özelliği, değişiklikleri CQL aracılığıyla koşullu olarak sorgulama yeteneğini geliştirir. Uygulama ayrıntıları hakkında daha fazla bilgi edinmek için bkz. [Cassandra için Azure Cosmos DB API 'sindeki akışı değiştirme](cassandra-change-feed.md).

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalelerde değişiklik akışı hakkında daha fazla bilgi edinmek için devam edebilirsiniz:

* [Değişiklik akışına genel bakış](change-feed.md)
* [Azure Işlevleri ile değişiklik akışını kullanma](change-feed-functions.md)
* [Değişiklik akışı işlemci kitaplığını kullanma](change-feed-processor.md)
