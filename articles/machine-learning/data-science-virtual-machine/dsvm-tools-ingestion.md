---
title: Veri alma araçları
titleSuffix: Azure Data Science Virtual Machine
description: Veri Bilimi Sanal Makinesi önceden yüklenmiş veri alma araçları ve yardımcı programları hakkında bilgi edinin.
keywords: veri bilimi araçları, veri bilimi sanal makinesi, veri bilimi için araçlar, linux veri bilimi
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: vijetajo
ms.author: vijetaj
ms.topic: conceptual
ms.date: 09/11/2017
ms.openlocfilehash: 2d3d37bcb6ee4bae82d82cf676211e9e1546cfad
ms.sourcegitcommit: 532335f703ac7f6e1d2cc1b155c69fc258816ede
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70191992"
---
# <a name="data-science-virtual-machine-data-ingestion-tools"></a>Veri bilimi sanal makinesi veri alma araçları

Bir veri bilimi veya AI projesindeki ilk teknik adımlardan biri olarak kullanılacak veri kümelerini tanımlamalısınız ve bunları analiz ortamınıza getirebilirsiniz. Veri Bilimi Sanal Makinesi (DSVM), farklı kaynaklardan gelen verileri DSVM 'de yerel olarak veya bulutta ya da şirket içinde bulunan bir veri platformuna getirmek için araçlar ve kitaplıklar sağlar.

DSVM 'de bulunan bazı veri taşıma araçları aşağıda verilmiştir.

## <a name="adlcopy"></a>AdlCopy

|    |           |
| ------------- | ------------- |
| Nedir?   | Verileri Azure Blob depolamadan Azure Data Lake Store 'e kopyalamak için bir araç. Ayrıca, verileri iki Azure Data Lake Store hesapları arasında da kopyalayabilirsiniz.      |
| Desteklenen DSVM sürümleri      | Windows      |
| Tipik kullanımları      | Azure Blob depolamadan birden çok blob 'u Azure Data Lake Store içine aktarma.      |
|  Kullanma / çalıştırın nasıl?    |   Yardım almak için bir komut istemi `adlcopy` açın ve yazın.    |
| Örneklere bağlantılar      | [AdlCopy’yi kullanma](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-copy-data-azure-storage-blob)      |
| DSVM 'deki ilgili araçlar      | AzCopy, Azure CLı     |

## <a name="azure-cli"></a>Azure CLI

|    |           |
| ------------- | ------------- |
| Nedir?   | Azure için bir yönetim aracı. Ayrıca Azure Blob depolama ve Azure Data Lake Store gibi Azure veri platformlarından verileri taşımak için komut fiilleri de içerir.     |
| Desteklenen DSVM sürümleri      | Windows, Linux     |
| Tipik kullanımları      | Azure depolama ve Azure Data Lake Store verileri içeri ve dışarı aktarma.      |
|  Kullanma / çalıştırın nasıl?    |   Yardım almak için bir komut istemi `az` açın ve yazın.    |
| Örneklere bağlantılar      | [Azure CLI’yi kullanma](https://docs.microsoft.com/cli/azure)     |
| DSVM 'deki ilgili araçlar      | AzCopy, AdlCopy      |


## <a name="azcopy"></a>AzCopy

|    |           |
| ------------- | ------------- |
| Nedir?   | Yerel dosyalara, Azure Blob depolama, dosyalar ve tablolar arasında veri kopyalamak için bir araç.      |
| Desteklenen DSVM sürümleri      | Windows      |
| Tipik kullanımları      | Azure Blob depolama alanına dosya kopyalama ve BLOB 'ları hesaplar arasında kopyalama.      |
|  Kullanma / çalıştırın nasıl?    |   Yardım almak için bir komut istemi `azcopy` açın ve yazın.    |
| Örneklere bağlantılar      | [Windows üzerinde AzCopy](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy)      |
| DSVM 'deki ilgili araçlar      | AdlCopy     |


## <a name="azure-cosmos-db-data-migration-tool"></a>Azure Cosmos DB veri geçiş aracı

|    |           |
| ------------- | ------------- |
| Nedir?   | Çeşitli kaynaklardan verileri Azure Cosmos DB içine aktarmak için araç. Bu kaynaklar JSON dosyalarını, CSV dosyalarını, SQL, MongoDB, Azure Tablo depolama, Amazon DynamoDB ve Azure Cosmos DB SQL API koleksiyonlarını içerir.      |
| Desteklenen DSVM sürümleri      | Windows      |
| Tipik kullanımları      | Bir VM 'den CosmosDB 'ye dosya aktarma, Azure Tablo depolama alanından CosmosDB 'ye veri aktarma ve bir Microsoft SQL Server veritabanından CosmosDB 'ye veri aktarma.     |
|  Kullanma / çalıştırın nasıl?    |   Komut satırı sürümünü kullanmak için bir komut istemi açın ve yazın `dt`. GUI aracını kullanmak için bir komut istemi açın ve yazın `dtui`.    |
| Örneklere bağlantılar      | [CosmosDB verileri Içeri aktarma](https://docs.microsoft.com/azure/cosmos-db/import-data)      |
| DSVM 'deki ilgili araçlar      | AzCopy, AdlCopy      |


## <a name="bcp"></a>bcp

|    |           |
| ------------- | ------------- |
| Nedir?   | SQL Server ve bir veri dosyası arasında veri kopyalamak için SQL Server Aracı'nı tıklatın.      |
| Desteklenen DSVM sürümleri      | Windows      |
| Tipik kullanımları      | Bir CSV dosyasını SQL Server tablosuna aktarma ve bir SQL Server tablosunu bir dosyaya aktarma.      |
|  Kullanma / çalıştırın nasıl?    |   Yardım almak için bir komut istemi `bcp` açın ve yazın.    |
| Örneklere bağlantılar      | [BCP yardımcı programı](https://docs.microsoft.com/sql/tools/bcp-utility)      |
| DSVM 'deki ilgili araçlar      | SQL Server'ı sqlcmd      |

## <a name="blobfuse"></a>Blobfuse

|    |           |
| ------------- | ------------- |
| Nedir?   | Linux dosya sistemine bir Azure Blob depolama kapsayıcısını bağlamak için bir araç.      |
| Desteklenen DSVM sürümleri      | Linux      |
| Tipik kullanımları      | Bir kapsayıcıdaki Blobları okuma ve yazma.      |
|  Nasıl kullanılır ve çalıştırılır?    |   Çalıştırma _blobfuse_ bir terminal konumunda.    |
| Örneklere bağlantılar      | [github'da blobfuse](https://github.com/Azure/azure-storage-fuse)      |
| DSVM 'deki ilgili araçlar      | Azure CLI      |


## <a name="microsoft-data-management-gateway"></a>Microsoft Veri Yönetimi ağ geçidi

|    |           |
| ------------- | ------------- |
| Nedir?   | Bağlanmak için bir aracı veri kaynakları, bulut hizmetlerine tüketim için şirket içi.      |
| Desteklenen DSVM sürümleri      | Windows      |
| Tipik kullanımları      | Bir VM, bir şirket içi veri kaynağına bağlanma.      |
|  Kullanma / çalıştırın nasıl?    |   Başlat menüsünden Microsoft Veri Yönetimi ağ geçidini açın.    |
| Örneklere bağlantılar      | [Veri Yönetimi Ağ Geçidi](https://msdn.microsoft.com/library/dn879362.aspx)      |
| DSVM 'deki ilgili araçlar      | AzCopy, AdlCopy, bcp    |
