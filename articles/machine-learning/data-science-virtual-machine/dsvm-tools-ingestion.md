---
title: Veri alma araçları
titleSuffix: Azure Data Science Virtual Machine
description: Veri Bilimi Sanal Makine'ye önceden yüklenmiş veri alma araçları ve yardımcı programları hakkında bilgi edinin.
keywords: veri bilimi araçları, veri bilimi sanal makinesi, veri bilimi için araçlar, linux veri bilimi
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: lobrien
ms.author: laobri
ms.topic: conceptual
ms.date: 12/12/2019
ms.openlocfilehash: d86858f8d7f09628457b718ca3c481934d720081
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79270062"
---
# <a name="data-science-virtual-machine-data-ingestion-tools"></a>Data Science Sanal Makine veri alma araçları

Bir veri bilimi veya AI projesinin ilk teknik adımlarından biri olarak, kullanılacak veri kümelerini tanımlamalı ve bunları analiz ortamınıza getirmelisiniz. Data Science Virtual Machine (DSVM), farklı kaynaklardan gelen verileri DSVM'de yerel olarak analitik veri depolamaya veya bulutta veya şirket içinde bir veri platformuna getirmek için araçlar ve kütüphaneler sağlar.

Aşağıda, DSVM'de kullanılabilen bazı veri hareket araçları verebvardır.

## <a name="adlcopy"></a>AdlCopy

|    |           |
| ------------- | ------------- |
| Bu nedir?   | Azure Blob depolamadan Azure Veri Gölü Deposu'na veri kopyalamaaracı. Ayrıca, iki Azure Veri Gölü Deposu hesabı arasındaki verileri de kopyalayabilir.      |
| Desteklenen DSVM sürümleri      | Windows      |
| Tipik kullanımlar      | Azure Blob depolamasından Azure Veri Gölü Deposu'na birden çok blob alma.      |
|  Nasıl kullanılır / çalıştırın?    |   Yardım almak için `adlcopy` bir komut istemi açın ve yazın.    |
| Örneklere bağlantılar      | [AdlCopy kullanma](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-copy-data-azure-storage-blob)      |
| DSVM ile ilgili araçlar      | AzCopy, Azure CLI     |

## <a name="azure-cli"></a>Azure CLI

|    |           |
| ------------- | ------------- |
| Bu nedir?   | Azure için bir yönetim aracı. Ayrıca, Azure Blob depolama ve Azure Veri Gölü Deposu gibi Azure veri platformlarından veri taşımak için komut fiilleri içerir.     |
| Desteklenen DSVM sürümleri      | Windows, Linux     |
| Tipik kullanımlar      | Azure Depolama ve Azure Veri Gölü Deposu'na veri alma ve verme.      |
|  Nasıl kullanılır / çalıştırın?    |   Yardım almak için `az` bir komut istemi açın ve yazın.    |
| Örneklere bağlantılar      | [Azure CLI’yı kullanma](https://docs.microsoft.com/cli/azure)     |
| DSVM ile ilgili araçlar      | AzCopy, AdlCopy      |


## <a name="azcopy"></a>AzCopy

|    |           |
| ------------- | ------------- |
| Bu nedir?   | Yerel dosyalara, Azure Blob depolamasına, dosyalara ve tablolara veri kopyalamak için bir araçtır.      |
| Desteklenen DSVM sürümleri      | Windows      |
| Tipik kullanımlar      | Dosyaları Azure Blob depolama alanına kopyalama ve hesaplar arasındaki blob'ları kopyalama.      |
|  Nasıl kullanılır / çalıştırın?    |   Yardım almak için `azcopy` bir komut istemi açın ve yazın.    |
| Örneklere bağlantılar      | [Windows üzerinde AzCopy](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy)      |
| DSVM ile ilgili araçlar      | AdlCopy     |


## <a name="azure-cosmos-db-data-migration-tool"></a>Azure Cosmos DB Veri Geçiş aracı

|    |           |
| ------------- | ------------- |
| Bu nedir?   | Çeşitli kaynaklardan verileri buluttaki Bir NoSQL veritabanı olan Azure Cosmos DB'ye aktarma aracı. Bu kaynaklar arasında JSON dosyaları, CSV dosyaları, SQL, MongoDB, Azure Tablo depolama, Amazon DinamoDB ve Azure Cosmos DB SQL API koleksiyonları yer alır.      |
| Desteklenen DSVM sürümleri      | Windows      |
| Tipik kullanımlar      | VM'den CosmosDB'ye dosya alma, Azure tablo depolamadan CosmosDB'ye veri alma ve Microsoft SQL Server veritabanından CosmosDB'ye veri alma.     |
|  Nasıl kullanılır / çalıştırın?    |   Komut satırı sürümünü kullanmak için bir komut `dt`istemi açın ve yazın. GUI aracını kullanmak için bir komut `dtui`istemi açın ve yazın.    |
| Örneklere bağlantılar      | [CosmosDB Alma verileri](https://docs.microsoft.com/azure/cosmos-db/import-data)      |
| DSVM ile ilgili araçlar      | AzCopy, AdlCopy      |

## <a name="azure-storage-explorer"></a>Azure Storage Gezgini

|    |           |
| ------------- | ------------- |
| Bu nedir?   | Azure bulutunda depolanan dosyalarla etkileşim kurmak için Grafik Kullanıcı Arabirimi. |
| Desteklenen DSVM sürümleri      | Windows      |
| Tipik kullanımlar      | DSVM'den veri alma ve dışa aktarma.    |
|  Nasıl kullanılır / çalıştırın?    | Başlat menüsünde "Azure Depolama Gezgini"ni arayın. |
| Örneklere bağlantılar      | [Azure Storage Gezgini](vm-do-ten-things.md#access-azure-data-and-analytics-services)      |


## <a name="bcp"></a>bcp

|    |           |
| ------------- | ------------- |
| Bu nedir?   | SQL Server ve bir veri dosyası arasındaki verileri kopyalamak için SQL Server aracı.      |
| Desteklenen DSVM sürümleri      | Windows      |
| Tipik kullanımlar      | Bir CSV dosyasını SQL Server tablosuna alma ve bir DOSYAYA SQL Server tablosu verme.      |
|  Nasıl kullanılır / çalıştırın?    |   Yardım almak için `bcp` bir komut istemi açın ve yazın.    |
| Örneklere bağlantılar      | [bcp yardımcı programı](https://docs.microsoft.com/sql/tools/bcp-utility)      |
| DSVM ile ilgili araçlar      | SQL Server, sqlcmd      |

## <a name="blobfuse"></a>damla bezi

|    |           |
| ------------- | ------------- |
| Bu nedir?   | Linux dosya sistemine Azure Blob depolama konteyneri takmak için bir araç.      |
| Desteklenen DSVM sürümleri      | Linux      |
| Tipik kullanımlar      | Okuma ve bir kapta lekeler için yazma.      |
|  Nasıl kullanılır ve çalıştırın?    |   Bir terminalde _blobfuse_ çalıştırın.    |
| Örneklere bağlantılar      | [GitHub üzerinde blobfuse](https://github.com/Azure/azure-storage-fuse)      |
| DSVM ile ilgili araçlar      | Azure CLI      |
