---
title: Azure Cosmos DB-MongoDB API 'SI için Azure PowerShell örnekleri
description: MongoDB için Azure Cosmos DB API 'sindeki çeşitli ortak görevleri gerçekleştirmek için Azure PowerShell örnekleri alın
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 12/02/2019
ms.author: mjbrown
ms.openlocfilehash: 810161268df405d84f6c190d4d7f3b67f2a1def8
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80366199"
---
# <a name="azure-powershell-samples-for-azure-cosmos-db-mongodb-api"></a>Azure Cosmos DB MongoDB API 'SI için Azure PowerShell örnekleri

Aşağıdaki tabloda, MongoDB API 'SI için Azure Cosmos DB örnek Azure PowerShell betiklerinin bağlantıları yer almaktadır.

> [!NOTE]
> Şu anda, PowerShell, CLı ve Kaynak Yöneticisi şablonlarını kullanarak, MongoDB hesapları için Azure Cosmos DB API `*.documents.azure.com`'sinin yalnızca 3,2 sürümünü (yani, bir uç noktasını kullanan hesaplar) oluşturabilirsiniz. Hesapların 3,6 sürümünü oluşturmak için bunun yerine Azure portal kullanın.

> [!NOTE]
> Örnekler [az. CosmosDB](https://docs.microsoft.com/powershell/module/az.cosmosdb) Management cmdlet 'lerini kullanır. `Az.CosmosDB` Cmdlet 'lerin hala önizlemededir ve yayından önce değişebileceğini lütfen unutmayın. Lütfen `Az.CosmosDB` düzenli olarak güncelleştirmeleri denetleyin.

| | |
|---|---|
|[Hesap, veritabanı ve koleksiyon oluşturma](scripts/powershell/mongodb/ps-mongodb-create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Azure Cosmos hesabı, veritabanı ve koleksiyonu oluşturur. |
|[Veritabanları veya koleksiyonları listeleme veya edinme](scripts/powershell/mongodb/ps-mongodb-list-get.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Veritabanını veya toplamayı listeleyin veya alın. |
|[RU/s 'yi al](scripts/powershell/mongodb/ps-mongodb-ru-get.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bir veritabanı veya koleksiyon için RU/s 'yi alın. |
|[RU/s 'yi Güncelleştir](scripts/powershell/mongodb/ps-mongodb-ru-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bir veritabanı veya koleksiyon için RU/s 'yi güncelleştirin. |
|[Hesap güncelleştirme veya bölge ekleme](scripts/powershell/common/ps-account-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Cosmos hesabına bölge ekleyin. Diğer hesap özelliklerini değiştirmek için de kullanılabilir, ancak bunlar bölgelerin değişikliklerinden ayrı olmalıdır. |
|[Yük devretme önceliğini değiştirme veya yük devretmeyi tetikleme](scripts/powershell/common/ps-account-failover-priority-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Azure Cosmos hesabının bölgesel yük devretme önceliğini değiştirin veya el ile yük devretme tetikleyin. |
|[Hesap anahtarları veya bağlantı dizeleri](scripts/powershell/common/ps-account-keys-connection-strings.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Birincil ve ikincil anahtarları, bağlantı dizelerini alın veya bir Azure Cosmos hesabının hesap anahtarını yeniden oluşturun. |
|[IP güvenlik duvarı ile Cosmos hesabı oluşturma](scripts/powershell/common/ps-account-firewall-create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| IP Güvenlik Duvarı etkinken bir Azure Cosmos hesabı oluşturun. |
|||
