---
title: Azure Cosmos DB MongoDB API için Azure CLI örnekleri
description: Azure Cosmos DB MongoDB API için Azure CLI örnekleri
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: sample
ms.date: 9/25/2019
ms.author: mjbrown
ms.openlocfilehash: f7807a4c2024e16f563adbcb46a5c60e5c542dda
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77524572"
---
# <a name="azure-cli-samples-for-azure-cosmos-db-mongodb-api"></a>Azure Cosmos DB MongoDB API için Azure CLI örnekleri

Aşağıdaki tablo, Azure Cosmos DB MongoDB API için Azure CLI komut dosyalarını örnekleyen bağlantılar içerir. Tüm Azure Cosmos DB CLI komutları için başvuru sayfaları [Azure CLI Başvurusu'nda](/cli/azure/cosmosdb)mevcuttur. Tüm Azure Cosmos DB CLI komut dosyası örnekleri [Azure Cosmos DB CLI GitHub Deposu'nda](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb)bulunabilir.

> [!NOTE]
> Şu anda PowerShell, CLI ve Kaynak Yöneticisi şablonlarını `*.documents.azure.com`kullanarak Azure Cosmos DB'nin MongoDB hesapları için API'sinin yalnızca 3.2 sürümünü (diğer bir deyişle, biçimdeki bitiş noktasını kullanan hesaplar) oluşturabilirsiniz. Hesapların 3,6 sürümünü oluşturmak için azure portalını kullanın.

| |  |
|---|---|
| [Azure Cosmos hesabı, veritabanı ve koleksiyonu oluşturma](scripts/cli/mongodb/create.md?toc=%2fcli%2fazure%2ftoc.json)| MongoDB API için bir Azure Cosmos DB hesabı, veritabanı ve koleksiyon oluşturur. |
| [İş buzunu değiştir](scripts/cli/mongodb/throughput.md?toc=%2fcli%2fazure%2ftoc.json) | Bir veritabanında ve koleksiyonda RU/s'yi güncelleştirin.|
| [Bölgeler ekleme veya başarısız](scripts/cli/common/regions.md?toc=%2fcli%2fazure%2ftoc.json) | Bir bölge ekleyin, önceliği değiştirin, el ile başarısız lık tetikle.|
| [Hesap anahtarları ve bağlantı dizeleri](scripts/cli/common/keys.md?toc=%2fcli%2fazure%2ftoc.json) | Hesap anahtarlarını, salt okunur tuşları, anahtarları yeniden oluşturma ve bağlantı dizelerini listele.|
| [IP güvenlik duvarı ile güvenli](scripts/cli/common/ipfirewall.md?toc=%2fcli%2fazure%2ftoc.json)| IP güvenlik duvarı yapılandırılmış bir Cosmos hesabı oluşturun.|
| [Hizmet bitiş noktalarıyla güvenli yeni hesap](scripts/cli/common/service-endpoints.md?toc=%2fcli%2fazure%2ftoc.json)| Bir Cosmos hesabı oluşturun ve hizmet bitiş noktalarıyla güvenli olun.|
| [Hizmet bitiş noktalarıyla varolan hesabı güvenli hale](scripts/cli/common/service-endpoints-ignore-missing-vnet.md?toc=%2fcli%2fazure%2ftoc.json)| Alt ağ sonunda yapılandırıldığında, hizmet bitiş noktalarıyla güvenli hale getirmek için bir Cosmos hesabını güncelleştirin.|
|||
