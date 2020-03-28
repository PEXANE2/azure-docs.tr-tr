---
title: Azure Cosmos DB için Azure PowerShell örnekleri - MongoDB API
description: Azure Cosmos DB'nin MongoDB için API'sinde çeşitli ortak görevleri gerçekleştirmek için Azure PowerShell örneklerini alın
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 12/02/2019
ms.author: mjbrown
ms.openlocfilehash: 810161268df405d84f6c190d4d7f3b67f2a1def8
ms.sourcegitcommit: 07d62796de0d1f9c0fa14bfcc425f852fdb08fb1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80366199"
---
# <a name="azure-powershell-samples-for-azure-cosmos-db-mongodb-api"></a>Azure Cosmos DB MongoDB API için Azure PowerShell örnekleri

Aşağıdaki tablo, MongoDB API için Azure Cosmos DB için Azure PowerShell komut dosyalarını örnekleyen bağlantılar içerir.

> [!NOTE]
> Şu anda PowerShell, CLI ve Kaynak Yöneticisi şablonlarını `*.documents.azure.com`kullanarak Azure Cosmos DB'nin MongoDB hesapları için API'sinin yalnızca 3.2 sürümünü (diğer bir deyişle, biçimdeki bitiş noktasını kullanan hesaplar) oluşturabilirsiniz. Hesapların 3,6 sürümünü oluşturmak için azure portalını kullanın.

> [!NOTE]
> Örneklerde [Az.CosmosDB](https://docs.microsoft.com/powershell/module/az.cosmosdb) yönetim cmdletleri kullanılmaktadır. Cmdletlerin `Az.CosmosDB` hala önizlemede olduğunu ve yayınlanmadan önce değişebileceğini lütfen unutmayın. Lütfen güncellemeleridüzenli `Az.CosmosDB` olarak kontrol edin.

| | |
|---|---|
|[Hesap, veritabanı ve koleksiyon oluşturma](scripts/powershell/mongodb/ps-mongodb-create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Azure Cosmos hesabı, veritabanı ve koleksiyonu oluşturur. |
|[Veritabanlarını veya koleksiyonları listeleyin veya alın](scripts/powershell/mongodb/ps-mongodb-list-get.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Veritabanı nı veya koleksiyonu listeleyin veya alın. |
|[RU/s alın](scripts/powershell/mongodb/ps-mongodb-ru-get.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bir veritabanı veya koleksiyon için RU/s alın. |
|[RU/s'yi güncelleştir](scripts/powershell/mongodb/ps-mongodb-ru-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bir veritabanı veya koleksiyon için RU/s'yi güncelleştirin. |
|[Hesabı güncelleştirme veya bölge ekleme](scripts/powershell/common/ps-account-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Cosmos hesabına bir bölge ekleyin. Diğer hesap özelliklerini değiştirmek için de kullanılabilir, ancak bunların bölgelere yapılan değişikliklerden ayrı olması gerekir. |
|[Başarısız önceliği veya tetikleyici failover'ı değiştirme](scripts/powershell/common/ps-account-failover-priority-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Azure Cosmos hesabının bölgesel hata önceliğini değiştirin veya el ile başarısızlığı tetikle. |
|[Hesap anahtarları veya bağlantı dizeleri](scripts/powershell/common/ps-account-keys-connection-strings.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Birincil ve ikincil anahtarları, bağlantı dizelerini alın veya Azure Cosmos hesabının hesap anahtarını yeniden oluşturun. |
|[IP Güvenlik Duvarı ile Cosmos Hesabı Oluşturma](scripts/powershell/common/ps-account-firewall-create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| IP Güvenlik Duvarı etkinleştirilmiş bir Azure Cosmos hesabı oluşturun. |
|||
