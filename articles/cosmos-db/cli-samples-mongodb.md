---
title: Azure Cosmos DB MongoDB API 'SI için Azure CLı örnekleri
description: Azure Cosmos DB MongoDB API 'SI için Azure CLı örnekleri
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: how-to
ms.date: 06/03/2020
ms.author: mjbrown
ms.openlocfilehash: 7536a2e3fc32aeb330aad52625a5946e856d6646
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85556037"
---
# <a name="azure-cli-samples-for-azure-cosmos-db-mongodb-api"></a>Azure Cosmos DB MongoDB API 'SI için Azure CLı örnekleri

Aşağıdaki tabloda Azure Cosmos DB MongoDB API 'SI için örnek Azure CLı betikleri bağlantıları yer almaktadır. Tüm Azure Cosmos DB CLı komutlarına yönelik başvuru sayfaları, [Azure CLI başvurusunda](/cli/azure/cosmosdb)bulunabilir. Tüm Azure Cosmos DB CLı betiği örnekleri [Azure Cosmos DB CLI GitHub deposunda](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb)bulunabilir.

> [!NOTE]
> Şu anda, `*.documents.azure.com` PowerShell, CLI ve Kaynak Yöneticisi şablonlarını kullanarak, MongoDB hesapları için Azure Cosmos DB API 'sinin yalnızca 3,2 sürümünü (yani, bir uç noktasını kullanan hesaplar) oluşturabilirsiniz. Hesapların 3,6 sürümünü oluşturmak için bunun yerine Azure portal kullanın.

|Görev | Description |
|---|---|
| [Azure Cosmos hesabı, veritabanı ve koleksiyonu oluşturma](scripts/cli/mongodb/create.md?toc=%2fcli%2fazure%2ftoc.json)| MongoDB API 'SI için bir Azure Cosmos DB hesabı, veritabanı ve koleksiyonu oluşturur. |
| [Aktarım hızını değiştirme](scripts/cli/mongodb/throughput.md?toc=%2fcli%2fazure%2ftoc.json) | Bir veritabanında ve koleksiyonda RU/s 'yi güncelleştirin.|
| [Yük devretme bölgesi ekleme veya](scripts/cli/common/regions.md?toc=%2fcli%2fazure%2ftoc.json) | Bir bölge ekleyin, yük devretme önceliğini değiştirin, el ile yük devretme tetikleyin.|
| [Hesap anahtarları ve bağlantı dizeleri](scripts/cli/common/keys.md?toc=%2fcli%2fazure%2ftoc.json) | Hesap anahtarlarını listeleyin, salt okunurdur, anahtarları yeniden oluşturun ve bağlantı dizelerini listeleyin.|
| [IP güvenlik duvarıyla güvenli hale getirme](scripts/cli/common/ipfirewall.md?toc=%2fcli%2fazure%2ftoc.json)| IP güvenlik duvarı yapılandırılmış bir Cosmos hesabı oluşturun.|
| [Hizmet uç noktaları ile güvenli yeni hesap](scripts/cli/common/service-endpoints.md?toc=%2fcli%2fazure%2ftoc.json)| Hizmet uç noktaları ile Cosmos hesabı oluşturun ve güvenli hale getirin.|
| [Mevcut hesabı hizmet uç noktalarıyla güvenli hale getirin](scripts/cli/common/service-endpoints-ignore-missing-vnet.md?toc=%2fcli%2fazure%2ftoc.json)| Alt ağ sonunda yapılandırıldığında hizmet uç noktalarıyla güvenli hale getirmek için Cosmos hesabını güncelleştirin.|
| [Kaynakları silinmeye karşı kilitle](scripts/cli/mongodb/lock.md?toc=%2fcli%2fazure%2ftoc.json)| Kaynakların kaynak kilitleri ile silinmesini engelleyin.|
|||
