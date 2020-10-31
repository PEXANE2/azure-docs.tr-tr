---
title: Azure Cosmos DB Cassandra API için Azure CLı örnekleri
description: Azure Cosmos DB Cassandra API için Azure CLı örnekleri
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: sample
ms.date: 10/13/2020
ms.author: mjbrown
ms.custom: devx-track-azurecli
ms.openlocfilehash: 9de87341f57c42aa6a4fa39465c9311887056075
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93074038"
---
# <a name="azure-cli-samples-for-azure-cosmos-db-cassandra-api"></a>Azure Cosmos DB Cassandra API için Azure CLı örnekleri
[!INCLUDE[appliesto-cassandra-api](includes/appliesto-cassandra-api.md)]

Aşağıdaki tablo, Azure Cosmos DB’ye yönelik örnek Azure CLI betiklerinin bağlantılarını içerir. API 'ye özel örneklere gitmek için sağdaki bağlantıları kullanın. Ortak örnekler tüm API 'lerde aynıdır. Tüm Azure Cosmos DB CLı komutlarına yönelik başvuru sayfaları, [Azure CLI başvurusunda](/cli/azure/cosmosdb)bulunabilir. Azure Cosmos DB CLı betik örnekleri, [Azure Cosmos DB CLI GitHub deposunda](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb)da bulunabilir.

Bu örnekler için Azure CLı sürüm 2.12.1 veya üzeri gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse bkz. [Azure CLI 'Yı yüklemek](/cli/azure/install-azure-cli)

## <a name="common-samples"></a>Ortak örnekler

Bu örnekler tüm Azure Cosmos DB API 'Leri için geçerlidir

|Görev | Açıklama |
|---|---|
| [Yük devretme bölgesi ekleme veya](scripts/cli/common/regions.md?toc=%2fcli%2fazure%2ftoc.json) | Bir bölge ekleyin, yük devretme önceliğini değiştirin, el ile yük devretme tetikleyin.|
| [Hesap anahtarları ve bağlantı dizeleri](scripts/cli/common/keys.md?toc=%2fcli%2fazure%2ftoc.json) | Hesap anahtarlarını listeleyin, salt okunurdur, anahtarları yeniden oluşturun ve bağlantı dizelerini listeleyin.|
| [IP güvenlik duvarıyla güvenli hale getirme](scripts/cli/common/ipfirewall.md?toc=%2fcli%2fazure%2ftoc.json)| IP güvenlik duvarı yapılandırılmış bir Cosmos hesabı oluşturun.|
| [Hizmet uç noktaları ile güvenli yeni hesap](scripts/cli/common/service-endpoints.md?toc=%2fcli%2fazure%2ftoc.json)| Hizmet uç noktaları ile Cosmos hesabı oluşturun ve güvenli hale getirin.|
| [Mevcut hesabı hizmet uç noktalarıyla güvenli hale getirin](scripts/cli/common/service-endpoints-ignore-missing-vnet.md?toc=%2fcli%2fazure%2ftoc.json)| Alt ağ sonunda yapılandırıldığında hizmet uç noktalarıyla güvenli hale getirmek için Cosmos hesabını güncelleştirin.|
|||

## <a name="cassandra-api-samples"></a>Cassandra API örnekleri

|Görev | Açıklama |
|---|---|
| [Azure Cosmos hesabı, anahtar alanı ve tablo oluşturma](scripts/cli/cassandra/create.md?toc=%2fcli%2fazure%2ftoc.json)| Cassandra API için Azure Cosmos DB bir hesap, anahtar uzayı ve tablo oluşturur. |
| [Otomatik ölçeklendirme ile bir Azure Cosmos hesabı, anahtar alanı ve tablo oluşturma](scripts/cli/cassandra/autoscale.md?toc=%2fcli%2fazure%2ftoc.json)| Cassandra API için otomatik ölçeklendirmeyi içeren Azure Cosmos DB bir hesap, anahtar uzayı ve tablo oluşturur. |
| [Aktarım hızı işlemleri](scripts/cli/cassandra/throughput.md?toc=%2fcli%2fazure%2ftoc.json) | Anahtar alanı ve tablodaki otomatik ölçeklendirme ve standart verimlilik arasında okuma, güncelleştirme ve geçirme.|
| [Kaynakları silinmeye karşı kilitle](scripts/cli/cassandra/lock.md?toc=%2fcli%2fazure%2ftoc.json)| Kaynakların kaynak kilitleri ile silinmesini engelleyin.|
|||
