---
title: MongoDB için Azure Cosmos DB'nin API'sine giriş
description: Azure Cosmos DB'nin MongoDB için API'sini kullanarak büyük miktarda veriyi depolamak ve sorgulamak için Azure Cosmos DB'yi nasıl kullanabileceğinizi öğrenin.
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: overview
ms.date: 10/1/2019
author: sivethe
ms.author: sivethe
ms.openlocfilehash: 670020d276b9d4a868f24eb4a3f522581060adca
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "72754987"
---
# <a name="azure-cosmos-dbs-api-for-mongodb"></a>MongoDB için Azure Cosmos DB API'si

[Azure Cosmos DB](introduction.md), Microsoft'un görev açısından kritik uygulamalar için genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Azure Cosmos DB, [kullanıma hazır genel dağıtım](distribute-data-globally.md), dünya çapında [aktarım hızı ve depolama için esnek ölçeklendirme](partition-data.md), 99. yüzdebirlik dilimde tek haneli milisaniyelik gecikme süreleri ve garantili yüksek kullanılabilirlik olanakları sunar ve bunların tümü [sektör lideri SLA’lar](https://azure.microsoft.com/support/legal/sla/cosmos-db/) ile desteklenir. Azure Cosmos DB, şema ve dizin yönetimiyle ilgilenmenize gerek kalmadan [otomatik olarak verilerin dizinini oluşturur](https://www.vldb.org/pvldb/vol8/p1668-shukla.pdf). Çok modelli olan bu hizmet belge, anahtar-değer, grafik ve sütunlu veri modellerini destekler. Varsayılan olarak, SQL API kullanarak Cosmos DB ile etkileşim kurabilirsiniz. Ayrıca, Cosmos DB hizmeti Cassandra, MongoDB, Gremlin ve Azure Tablo Depolama dahil olmak üzere yaygın NoSQL API'leri için tel protokolleri uygular. Bu, Cosmos veritabanınızla etkileşimkurmak için tanıdık NoSQL istemci sürücülerinizi ve araçlarınızı kullanmanıza olanak tanır.

## <a name="wire-protocol-compatibility"></a>Tel protokolü uyumluluğu

Azure Cosmos DB, Cassandra, MongoDB, Gremlin ve Azure Tablolar Depolama gibi yaygın NoSQL veritabanlarının tel protokollerini uygular. Tel protokollerinin Cosmos DB içinde doğrudan ve verimli bir şekilde yerel bir şekilde uygulanmasını sağlayarak, Mevcut istemci SDK'ları, sürücüleri ve NoSQL veritabanlarının araçlarının Cosmos DB ile şeffaf bir şekilde etkileşimkurmasını sağlar. Cosmos DB, NoSQL veritabanlarının herhangi biri için kablo uyumlu API'ler sağlamak için veritabanlarının herhangi bir kaynak kodunu kullanmaz.

Varsayılan olarak, Azure Cosmos DB'nin MongoDB için API'si kullanılarak oluşturulan yeni hesaplar, MongoDB havale protokolünün 3.6 sürümüyle uyumludur. Bu protokol sürümünü anlayan herhangi bir MongoDB istemci sürücüsü, cosmos DB'ye yerel olarak bağlanabilmeli.

![MongoDB için Azure Cosmos DB API'si](./media/mongodb-introduction/cosmosdb-mongodb.png)

## <a name="key-benefits"></a>Önemli avantajlar

Cosmos DB'nin temel yararları tam olarak yönetilen, küresel olarak dağıtılan, bir hizmet olarak veritabanı [olarak burada](introduction.md)açıklanmıştır. Ayrıca, popüler NoSQL API'lerinin tel protokollerini yerel olarak uygulayarak Cosmos DB aşağıdaki avantajları sağlar:

* Uygulama mantığınızın önemli bir bölümünü korurken uygulamanızı cosmos DB'ye kolayca geçirin.
* Uygulamanızı taşınabilir tutun ve bulut satıcısı-agnostik kalmaya devam edin.
* Cosmos DB tarafından desteklenen ortak NoSQL API'leri için sektör lideri, finansal destekli SLA'ları alın.
* Cosmos veritabanlarınız için sağlanan veri ve depolamayı ihtiyaca göre elastik olarak ölçeklendirin ve yalnızca ihtiyacınız olan iş ve depolama için ödeme leyin. Bu önemli maliyet tasarrufu yol açar.
* Anahtar teslimi, çoklu ana çoğaltma ile küresel dağıtım.

## <a name="cosmos-dbs-api-for-mongodb"></a>MongoDB için Cosmos DB's API

Azure Cosmos DB'yi kullanmak veya yeni bir hesap oluşturmak için bir Azure Cosmos hesabı oluşturmak ve mevcut MongoDB uygulamanızı geçirmek için hızlı başlangıçları izleyin:

* [Mevcut bir MongoDB Düğümü.js web uygulamasını geçirin.](create-mongodb-nodejs.md)
* [MongoDB ve .NET SDK için Azure Cosmos DB'nin API'yi kullanarak bir web uygulaması oluşturun](create-mongodb-dotnet.md)
* [MongoDB ve Java SDK için Azure Cosmos DB'nin API'yi kullanarak bir konsol uygulaması oluşturun](create-mongodb-java.md)

## <a name="next-steps"></a>Sonraki adımlar

İşte başlamanıza yardımcı olacak birkaç ipucu:

* Hesap bağlantısı dize bilgilerinizi nasıl alacağınızı öğrenmek [için Bir MongoDB uygulamasını Azure Cosmos DB](connect-mongodb-account.md) öğreticisine bağlayın.
* Cosmos veritabanınızla Studio 3T'deki MongoDB uygulamanız arasında nasıl bağlantı kuracağımıöğrenmek için [Azure Cosmos DB öğreticisiyle Studio 3T'yi kullanın.](mongodb-mongochef.md)
* Verilerinizi bir Cosmos veritabanına almak için [MongoDB verilerini Azure Cosmos DB](mongodb-migrate.md) öğreticisine aktarın.
* [Robo 3T'yi](mongodb-robomongo.md)kullanarak Cosmos hesabına bağlanın.
* Genel olarak dağıtılan uygulamalar için okuma tercihlerini nasıl [yapılandıracaklarını](../cosmos-db/tutorial-global-distribution-mongodb.md)öğrenin.

<sup>Not: Bu makalede, Azure Cosmos DB'nin MongoDB veritabanlarıyla tel protokolü uyumluluğu sağlayan bir özelliği açıklanmaktadır. Microsoft, bu hizmeti sağlamak için MongoDB veritabanlarını çalıştırmaz. Azure Cosmos DB, MongoDB, Inc. şirketine bağlı değildir.</sup>
