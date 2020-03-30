---
title: Azure Cosmos DB'de Azure Tablo Depolama desteği
description: Aynı tablo veri modelini bir işlemi paylaşarak Azure Cosmos DB Tablo API'si ve Azure Depolama Tablolarının birlikte nasıl çalıştığını öğrenin
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.topic: overview
ms.date: 12/02/2019
author: sakash279
ms.author: akshanka
ms.reviewer: sngun
ms.openlocfilehash: 82397e49e473b8660dfada54a0e05cafe0da4120
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "76770678"
---
# <a name="developing-with-azure-cosmos-db-table-api-and-azure-table-storage"></a>Azure Cosmos DB Tablo API'si ve Azure Tablo depolaması ile geliştirme

Azure Cosmos DB Tablo API'si ve Azure Tablo depolaması aynı tablo veri modelini paylaşır ve SDK'ları aracılığıyla aynı oluşturma, silme, güncelleştirme ve sorgulama işlemlerini gösterir.

[!INCLUDE [storage-table-cosmos-comparison](../../includes/storage-table-cosmos-comparison.md)]

## <a name="developing-with-the-azure-cosmos-db-table-api"></a>Azure Cosmos DB Tablo API'siyle geliştirme

Şu anda [Azure Cosmos DB Tablo API'sinin](table-introduction.md) geliştirme için kullanılabilen dört SDK'sı vardır: 

* [Microsoft.Azure.Cosmos.Table](https://www.nuget.org/packages/Microsoft.Azure.Cosmos.Table): .NET SDK. Bu kitaplık .NET Standard'ı hedefler ve ortak [Windows Azure Depolama SDK](https://www.nuget.org/packages/WindowsAzure.Storage)ile aynı sınıflara ve yöntem imzalarına sahiptir, ancak Tablo API'sini kullanarak Azure Cosmos DB hesaplarına bağlanma özelliğine de sahiptir. .NET Framework library [Microsoft.Azure.CosmosDB.Table](https://www.nuget.org/packages/Microsoft.Azure.CosmosDB.Table/) kullanıcılarının [microsoft.Azure.Cosmos.Table'a](https://www.nuget.org/packages/Microsoft.Azure.Cosmos.Table) bakım modunda olduğu için yükseltmeleri önerilir ve yakında amortismana dahil edilecektir.

* [Python SDK](table-sdk-python.md): Yeni Azure Cosmos DB Python SDK, Python'daki Azure Tablo depolamasını destekleyen tek SDK'dır. Bu SDK hem Azure Tablo depolaması hem de Azure Cosmos DB Tablo API'sine bağlanır.

* [Java SDK](table-sdk-java.md): Bu Azure Depolama SDK,Tablo API'sini kullanarak Azure Cosmos DB hesaplarına bağlanma özelliğine sahiptir.

* [Düğüm.js SDK](table-sdk-nodejs.md): Bu Azure Depolama SDK, Tablo API'sini kullanarak Azure Cosmos DB hesaplarına bağlanma özelliğine sahiptir.


Tablo API'siyle çalışma hakkındaki ek bilgileri [SSS: Tablo API'siyle geliştirme](faq.md#table) makalesinde bulabilirsiniz.

## <a name="developing-with-azure-table-storage"></a>Azure Tablo depolamasıyla geliştirme

Azure Tablo depolamasının geliştirme için kullanılabilen SDK'ları vardır:

- [WindowsAzure.Storage .NET SDK'sı](https://www.nuget.org/packages/WindowsAzure.Storage/). Bu kitaplık, depolama Tablo hizmetiyle çalışmanıza olanak tanır.
- [Python SDK](https://github.com/Azure/azure-cosmos-table-python). Python için Azure Cosmos DB Tablo SDK Tablo Depolama hizmetini destekler (Çünkü Azure Tablo Depolama ve Cosmos DB'nin Tablo API'si aynı özellikleri ve işlevleri paylaşır ve SDK geliştirme çabalarımızı faktörize etmek amacıyla bu SDK'yı kullanmanızı öneririz).
- [Java için Azure Depolama SDK.](https://github.com/azure/azure-storage-java) Bu Azure Depolama SDK'sı, Azure Tablo depolamasını kullanmak için Java'da bir istemci kitaplığı sağlar.
- [Düğüm.js SDK](https://github.com/Azure/azure-storage-node). Bu SDK, depolama Tablo hizmetini kullanmak için bir Node.js paketi ve tarayıcıyla uyumlu bir JavaScript istemci kitaplığı sağlar.
- [AzureRmStorageTable PowerShell modülü](https://www.powershellgallery.com/packages/AzureRmStorageTable). Bu PowerShell modülünün depolama Tablolarıyla çalışmak için cmdlet'leri vardır.
- [C++ için Azure Depolama İstemcisi Kitaplığı](https://github.com/Azure/azure-storage-cpp/). Bu kitaplık Azure Depolama için uygulamalar oluşturmanıza olanak tanır.
- [Ruby için Azure Depolama Tablosu İstemci Kitaplığı](https://github.com/azure/azure-storage-ruby/tree/master/table). Bu proje, Azure depolaması Tablo hizmetlerine erişmeyi kolaylaştıran bir Ruby paketi sağlar.
- [Azure Depolama Tablosu PHP İstemci Kitaplığı](https://github.com/Azure/azure-storage-php/tree/master/azure-storage-table). Bu proje, Azure depolaması Tablo hizmetlerine erişmeyi kolaylaştıran bir PHP istemci kitaplığı sağlar.


   





