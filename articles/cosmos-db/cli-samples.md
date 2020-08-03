---
title: Azure Cosmos DB için Azure CLı örnekleri
description: Azure Cosmos DB için Azure CLı örnekleri
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: sample
ms.date: 07/29/2020
ms.author: mjbrown
ms.custom: devx-track-azurecli
ms.openlocfilehash: 954215f04525e850151fdad93af6e7272b41b3df
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87498472"
---
# <a name="azure-cli-samples-for-azure-cosmos-db"></a>Azure Cosmos DB için Azure CLI örnekleri

Aşağıdaki tablo, Azure Cosmos DB’ye yönelik örnek Azure CLI betiklerinin bağlantılarını içerir. API 'ye özel örneklere gitmek için sağdaki bağlantıları kullanın. Ortak örnekler tüm API 'lerde aynıdır. Tüm Azure Cosmos DB CLı komutlarına yönelik başvuru sayfaları, [Azure CLI başvurusunda](/cli/azure/cosmosdb)bulunabilir. Azure Cosmos DB CLı betik örnekleri, [Azure Cosmos DB CLI GitHub deposunda](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb)da bulunabilir.

Bu örnekler için Azure CLı sürüm 2.9.1 veya üzeri gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse bkz. [Azure CLI 'Yı yüklemek](/cli/azure/install-azure-cli)

## <a name="common-samples"></a>Ortak örnekler

Bu örnekler tüm Azure Cosmos DB API 'Leri için geçerlidir

|Görev | Açıklama |
|---|---|
| [Yük devretme bölgesi ekleme veya](scripts/cli/common/regions.md?toc=%2fcli%2fazure%2ftoc.json) | Bir bölge ekleyin, yük devretme önceliğini değiştirin, el ile yük devretme tetikleyin.|
| [Hesap anahtarları ve bağlantı dizeleri](scripts/cli/common/keys.md?toc=%2fcli%2fazure%2ftoc.json) | Hesap anahtarlarını listeleyin, salt okunurdur, anahtarları yeniden oluşturun ve bağlantı dizelerini listeleyin.|
| [IP güvenlik duvarıyla güvenli hale getirme](scripts/cli/common/ipfirewall.md?toc=%2fcli%2fazure%2ftoc.json)| IP güvenlik duvarı yapılandırılmış bir Cosmos hesabı oluşturun.|
| [Hizmet uç noktaları ile güvenli yeni hesap](scripts/cli/common/service-endpoints.md?toc=%2fcli%2fazure%2ftoc.json)| Hizmet uç noktaları ile Cosmos hesabı oluşturun ve güvenli hale getirin.|
| [Mevcut hesabı hizmet uç noktalarıyla güvenli hale getirin](scripts/cli/common/service-endpoints-ignore-missing-vnet.md?toc=%2fcli%2fazure%2ftoc.json)| Alt ağ sonunda yapılandırıldığında hizmet uç noktalarıyla güvenli hale getirmek için Cosmos hesabını güncelleştirin.|
|||

## <a name="core-sql-api-samples"></a>Çekirdek (SQL) API örnekleri

|Görev | Açıklama |
|---|---|
| [Azure Cosmos hesabı, veritabanı ve kapsayıcısı oluşturma](scripts/cli/sql/create.md?toc=%2fcli%2fazure%2ftoc.json)| Çekirdek (SQL) API 'SI için bir Azure Cosmos DB hesabı, veritabanı ve kapsayıcı oluşturur. |
| [Otomatik ölçeklendirme ile bir Azure Cosmos hesabı, veritabanı ve kapsayıcısı oluşturma](scripts/cli/sql/autoscale.md?toc=%2fcli%2fazure%2ftoc.json)| Çekirdek (SQL) API 'SI için otomatik ölçeklendirmeyi olan bir Azure Cosmos DB hesabı, veritabanı ve kapsayıcı oluşturur. |
| [Aktarım hızını değiştirme](scripts/cli/sql/throughput.md?toc=%2fcli%2fazure%2ftoc.json) | Bir veritabanında ve kapsayıcıda RU/s 'yi güncelleştirin.|
| [Kaynakları silinmeye karşı kilitle](scripts/cli/sql/lock.md?toc=%2fcli%2fazure%2ftoc.json)| Kaynakların kaynak kilitleri ile silinmesini engelleyin.|
|||

## <a name="mongodb-api-samples"></a>MongoDB API örnekleri

|Görev | Açıklama |
|---|---|
| [Azure Cosmos hesabı, veritabanı ve koleksiyonu oluşturma](scripts/cli/mongodb/create.md?toc=%2fcli%2fazure%2ftoc.json)| MongoDB API 'SI için bir Azure Cosmos DB hesabı, veritabanı ve koleksiyonu oluşturur. |
| [Paylaşılan verimlilik içeren bir Azure Cosmos hesabı, otomatik ölçeklendirme ve iki koleksiyon ile veritabanı oluşturma](scripts/cli/mongodb/autoscale.md?toc=%2fcli%2fazure%2ftoc.json)| MongoDB API 'SI için paylaşılan iş hızına sahip bir Azure Cosmos DB hesap, otomatik ölçeklendirme ve iki koleksiyon içeren bir veritabanı oluşturur. |
| [Aktarım hızını değiştirme](scripts/cli/mongodb/throughput.md?toc=%2fcli%2fazure%2ftoc.json) | Bir veritabanında ve koleksiyonda RU/s 'yi güncelleştirin.|
| [Kaynakları silinmeye karşı kilitle](scripts/cli/mongodb/lock.md?toc=%2fcli%2fazure%2ftoc.json)| Kaynakların kaynak kilitleri ile silinmesini engelleyin.|
|||

## <a name="cassandra-api-samples"></a>Cassandra API örnekleri

|Görev | Açıklama |
|---|---|
| [Azure Cosmos hesabı, anahtar alanı ve tablo oluşturma](scripts/cli/cassandra/create.md?toc=%2fcli%2fazure%2ftoc.json)| Cassandra API için Azure Cosmos DB bir hesap, anahtar uzayı ve tablo oluşturur. |
| [Otomatik ölçeklendirme ile bir Azure Cosmos hesabı, anahtar alanı ve tablo oluşturma](scripts/cli/cassandra/autoscale.md?toc=%2fcli%2fazure%2ftoc.json)| Cassandra API için otomatik ölçeklendirmeyi içeren Azure Cosmos DB bir hesap, anahtar uzayı ve tablo oluşturur. |
| [Aktarım hızını değiştirme](scripts/cli/cassandra/throughput.md?toc=%2fcli%2fazure%2ftoc.json) | Anahtar alanı ve tablodaki RU/s 'yi güncelleştirin.|
| [Kaynakları silinmeye karşı kilitle](scripts/cli/cassandra/lock.md?toc=%2fcli%2fazure%2ftoc.json)| Kaynakların kaynak kilitleri ile silinmesini engelleyin.|
|||

## <a name="gremlin-api-samples"></a>Gremlin API örnekleri

|Görev | Açıklama |
|---|---|
| [Azure Cosmos hesabı, veritabanı ve grafik oluşturma](scripts/cli/gremlin/create.md?toc=%2fcli%2fazure%2ftoc.json)| Gremlin API için bir Azure Cosmos DB hesabı, veritabanı ve grafik oluşturur. |
| [Otomatik ölçeklendirme ile bir Azure Cosmos hesabı, veritabanı ve grafik oluşturma](scripts/cli/gremlin/autoscale.md?toc=%2fcli%2fazure%2ftoc.json)| Gremlin API için otomatik ölçeklendirmeyi olan bir Azure Cosmos DB hesabı, veritabanı ve grafik oluşturur. |
| [Aktarım hızını değiştirme](scripts/cli/gremlin/throughput.md?toc=%2fcli%2fazure%2ftoc.json) | Bir veritabanında ve grafikte RU/s 'yi güncelleştirin.|
| [Kaynakları silinmeye karşı kilitle](scripts/cli/gremlin/lock.md?toc=%2fcli%2fazure%2ftoc.json)| Kaynakların kaynak kilitleri ile silinmesini engelleyin.|
|||

## <a name="table-api-samples"></a>Tablo API'si örnekleri

|Görev | Açıklama |
|---|---|
| [Azure Cosmos hesabı ve tablosu oluşturma](scripts/cli/table/create.md?toc=%2fcli%2fazure%2ftoc.json)| Tablo API'si için bir Azure Cosmos DB hesabı ve tablo oluşturur. |
| [Otomatik ölçeklendirme ile bir Azure Cosmos hesabı ve tablosu oluşturma](scripts/cli/table/autoscale.md?toc=%2fcli%2fazure%2ftoc.json)| Tablo API'si için otomatik ölçeklendirmeyi olan bir Azure Cosmos DB hesabı ve tablo oluşturur. |
| [Aktarım hızını değiştirme](scripts/cli/table/throughput.md?toc=%2fcli%2fazure%2ftoc.json) | Bir tablodaki RU/s 'yi güncelleştirin.|
| [Kaynakları silinmeye karşı kilitle](scripts/cli/table/lock.md?toc=%2fcli%2fazure%2ftoc.json)| Kaynakların kaynak kilitleri ile silinmesini engelleyin.|
|||
