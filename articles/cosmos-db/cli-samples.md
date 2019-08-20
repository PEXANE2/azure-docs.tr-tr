---
title: Azure Cosmos DB için Azure CLı örnekleri
description: Azure CLI Örnekleri - Azure Cosmos DB hesapları, veritabanları, kapsayıcıları, bölgeleri ve güvenlik duvarları oluşturun ve yönetin.
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 10/26/2018
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: d7ce5e2c12feeee770de8acfd5df81d340e0a7d0
ms.sourcegitcommit: e42c778d38fd623f2ff8850bb6b1718cdb37309f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69615566"
---
# <a name="azure-cli-samples-for-azure-cosmos-db"></a>Azure Cosmos DB için Azure CLI örnekleri

Aşağıdaki tablo, Azure Cosmos DB’ye yönelik örnek Azure CLI betiklerinin bağlantılarını içerir. Tüm Azure Cosmos DB CLI komutlarına ait başvuru sayfalarına [Azure CLI Başvurusu](/cli/azure/cosmosdb) sayfasından erişilebilir.

| |  |
|---|---|
|**Azure Cosmos DB hesabı, veritabanı ve kapsayıcıları oluşturma**||
| [SQL API kullanarak Azure Cosmos DB hesabı oluşturma](scripts/create-database-account-collections-cli.md?toc=%2fcli%2fazure%2ftoc.json)| Tek bir Azure Cosmos DB hesabı, veritabanı ve kapsayıcısı oluşturur. |
| [MongoDB için Cosmos DB API 'sini kullanarak Azure Cosmos DB hesabı oluşturma](scripts/create-mongodb-database-account-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Tek bir Azure Cosmos DB hesabı, veritabanı ve bir koleksiyon oluşturur. |
| [Gremlin API kullanarak Azure Cosmos DB hesabı oluşturma](scripts/create-gremlin-database-account-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Tek bir Azure Cosmos DB hesabı, veritabanı ve grafik oluşturur. |
| [Cassandra API kullanarak Azure Cosmos DB hesabı oluşturma](scripts/create-cassandra-database-account-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Tek bir Azure Cosmos DB hesabı ve veritabanı oluşturur. |
| [Tablo API'si kullanarak Azure Cosmos DB hesabı oluşturma](scripts/create-table-database-account-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Tek bir Azure Cosmos DB hesabı, veritabanı ve tablo oluşturur. |
|**Azure Cosmos DB’yi ölçeklendirme**||
| [Kapsayıcının aktarım hızını ölçeklendirme](scripts/scale-collection-throughput-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Kapsayıcıda sağlanan aktarım hızını değiştirir.|
| [Azure Cosmos veritabanı hesabını birden çok bölgede çoğaltma ve yük devretme önceliklerini yapılandırma](scripts/scale-multiregion-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Hesap verilerini, belirtilmiş yük devretme önceliğine göre birden fazla bölgeye genel olarak çoğaltır.|
|**Güvenli Azure Cosmos DB**||
| [Hesap anahtarlarını alma](scripts/secure-get-account-key-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Hesap için birincil ve ikincil ana yazma anahtarlarıyla birincil ve ikincil salt okunur anahtarları alır.|
| [MongoDB için Azure Cosmos DB API 'siyle yapılandırılmış Cosmos hesabı bağlantı dizesini alın](scripts/secure-mongo-connection-string-cli.md?toc=%2fcli%2fazure%2ftoc.json) | MongoDB uygulamasını Azure Cosmos DB hesabınıza bağlamak için bağlantı dizesini alır.|
| [Hesap anahtarlarını yeniden oluşturma](scripts/secure-regenerate-key-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Hesap anahtarlarını yeniden oluşturun.|
| [Güvenlik duvarı oluşturma](scripts/create-firewall-cli.md?toc=%2fcli%2fazure%2ftoc.json)| Onaylanmış bir makine kümesinden ve/veya bulut hizmetlerinden hesaba erişimi kısıtlayan bir gelen IP erişim denetimi ilkesi oluşturun.|
|**Yüksek kullanılabilirlik, olağanüstü durum kurtarma, yedekleme ve geri yükleme**||
| [Yük devretme ilkesini yapılandırma](scripts/ha-failover-policy-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Hesabın çoğaltıldığı her bölgenin yük devretme önceliğini ayarlar.|
|**Azure Cosmos DB’yi kaynaklara bağlama**||
| [Azure Cosmos DB’ye bir web uygulaması bağlama](../app-service/scripts/cli-connect-to-documentdb.md?toc=%2fcli%2fazure%2ftoc.json)|Azure Cosmos veritabanı ve Azure Web uygulaması oluşturun ve bağlayın.|
|||
