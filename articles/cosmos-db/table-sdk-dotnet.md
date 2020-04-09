---
title: Azure Cosmos DB Tablo API .NET SDK & Kaynakları
description: Sürüm tarihleri, emeklilik tarihleri ve her sürüm arasında yapılan değişiklikler dahil olmak üzere Azure Cosmos DB Tablo API'si hakkında her şeyi öğrenin.
author: sakash279
ms.author: akshanka
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.devlang: dotnet
ms.topic: reference
ms.date: 08/17/2018
ms.openlocfilehash: 9356ab0aec375a046a8f200838df296b2cf74bce
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80984989"
---
# <a name="azure-cosmos-db-table-net-api-download-and-release-notes"></a>Azure Cosmos DB Tablo .NET API: Notları indirin ve bırakın

> [!div class="op_single_selector"]
> * [.NET](table-sdk-dotnet.md)
> * [.NET Standard](table-sdk-dotnet-standard.md)
> * [Java](table-sdk-java.md)
> * [Node.js](table-sdk-nodejs.md)
> * [Python](table-sdk-python.md)

|   |   |
|---|---|
|**SDK indir**|[NuGet](https://www.nuget.org/packages/Microsoft.Azure.CosmosDB.Table)|
|**Hızlı Başlangıç**|[Azure Cosmos DB: .NET ve Tablo API'si ile bir uygulama oluşturma](create-table-dotnet.md)|
|**Öğretici**|[Azure Cosmos DB: .NET’te Tablo API’si ile geliştirme](tutorial-develop-table-dotnet.md)|
|**Geçerli desteklenen çerçeve**|[Microsoft .NET Framework 4.5.1](https://www.microsoft.com/en-us/download/details.aspx?id=40779)|

> [!IMPORTANT]
> .NET Framework SDK [Microsoft.Azure.CosmosDB.Table](https://www.nuget.org/packages/Microsoft.Azure.CosmosDB.Table) bakım modundadır ve yakında amortismana dahil edilir. Tablo API tarafından desteklenen en son özellikleri almaya devam etmek için lütfen yeni .NET Standart kitaplığı [Microsoft.Azure.Cosmos.Table'a](https://www.nuget.org/packages/Microsoft.Azure.Cosmos.Table) yükseltin.

> Önizleme sırasında bir Tablo API hesabı oluşturduysanız, genel kullanıma açık Tablo API SDK’ları ile çalışmak için lütfen [yeni Tablo API hesabı](create-table-dotnet.md#create-a-database-account) oluşturun.
>

## <a name="release-notes"></a>Sürüm notları

### <a name="212"></a><a name="2.1.2"/>2.1.2

* Hata düzeltmeleri

### <a name="210"></a><a name="2.1.0"/>2.1.0

* Hata düzeltmeleri

### <a name="200"></a><a name="2.0.0"/>2.0.0

* Çok bölgeli yazma desteği eklendi
* Microsoft.Azure.DocumentDB, Microsoft.OData.Core, Microsoft.OData.Edm, Microsoft.Spatial üzerindeki NuGet paketi bağımlılıkları düzeltildi

### <a name="113"></a><a name="1.1.3"/>1.1.3

* Microsoft.Azure.Storage.Common ve Microsoft.Azure.DocumentDB üzerindeki NuGet paketi bağımlılıkları düzeltildi.
* JsonConvert.DefaultSettings yapılandırıldığınızda tablo serileştirme hata giderir.

### <a name="111"></a><a name="1.1.1"/>1.1.1

* Doğrudan Modda yanlış biçimlendirilmiş ETAG'ler için doğrulama eklendi.
* Ağ Geçidi Modunda LINQ sorgu hatası düzeltildi.
* Synchronization CONTEXT ile iş parçacığı havuzunda artık eşzamanlılık API'leri çalıştırın.

### <a name="110"></a><a name="1.1.0"/>1.1.0

* TableQueryMaxItemCount, TableQueryEnableScan, TableQueryMaxDegreeOfParallelism ve TableQueryDevamıTokenLimitInKb'yi TableRequestOptions'a ekle
* Hata Düzeltmeleri

### <a name="100"></a><a name="1.0.0"/>1.0.0

* Genel kullanılabilirlik sürümü

### <a name="090-preview"></a><a name="0.1.0-preview"/>0.9.0-önizleme

* İlk önizleme yayını

## <a name="release-and-retirement-dates"></a>Serbest bırakma ve Emeklilik tarihleri

Microsoft, daha yeni/desteklenen bir sürüme geçişi kolaylaştırmak için SDK'nın emekliye geçmesinden en az **12 ay** önce bildirim sağlar.

`Microsoft.Azure.CosmosDB.Table` Kütüphane şu anda yalnızca .NET Framework için kullanılabilir ve bakım modundadır ve yakında amortismana hazır olacaktır. Yeni özellikler ve işlevler ve optimizasyonlar yalnızca .NET Standart kitaplığı [Microsoft.Azure.Cosmos.Table'a](https://www.nuget.org/packages/Microsoft.Azure.Cosmos.Table)eklenir, bu nedenle [Microsoft.Azure.Cosmos.Table'a](https://www.nuget.org/packages/Microsoft.Azure.Cosmos.Table)yükseltmeniz önerilir.

[WindowsAzure.Storage-PremiumTable](https://www.nuget.org/packages/WindowsAzure.Storage-PremiumTable/0.1.0-preview) önizleme paketi amortismana kaldırıldı. WindowsAzure.Storage-PremiumTable SDK 15 Kasım 2018 tarihinde kullanımdan kaldırılacak ve bu tarihte emekli SDK'ya yapılan taleplere izin verilmeyecektir. 

Emekli bir SDK kullanarak Azure Cosmos DB'ye yapılan tüm istekler hizmet tarafından reddedilir.
<br/>

| Sürüm | Yayın Tarihi | Emeklilik Tarihi |
| --- | --- | --- |
| [2.1.2](#2.1.2) |Eylül 16, 2019| |
| [2.1.0](#2.1.0) |22 Ocak 2019|01 Nisan 2020 |
| [2.0.0](#2.0.0) |Eylül 26, 2018|01 Mart 2020 |
| [1.1.3](#1.1.3) |17 Temmuz 2018|01 Aralık 2019 |
| [1.1.1](#1.1.1) |26 Mart 2018|01 Aralık 2019 |
| [1.1.0](#1.1.0) |Şubat 21, 2018|01 Aralık 2019 |
| [1.0.0](#1.0.0) |15 Kasım 2017|Kasım 15, 2019 |
| 0.9.0-önizleme |Kasım 11, 2017 |Kasım 11, 2019 |

## <a name="troubleshooting"></a>Sorun giderme

Hata alırsanız 

```
Unable to resolve dependency 'Microsoft.Azure.Storage.Common'. Source(s) used: 'nuget.org', 
'CliFallbackFolder', 'Microsoft Visual Studio Offline Packages', 'Microsoft Azure Service Fabric SDK'`
```

Microsoft.Azure.CosmosDB.Table NuGet paketini kullanmaya çalışırken, sorunu gidermek için iki seçeneğiniz vardır:

* Microsoft.Azure.CosmosDB.Table paketini ve bağımlılıklarını yüklemek için Paket Yönet Konsolu'nu kullanın. Bunu yapmak için, çözümünüz için Paket Yöneticisi Konsoluna aşağıdakileri yazın. 

    ```powershell
    Install-Package Microsoft.Azure.CosmosDB.Table -IncludePrerelease
    ```

    
* Tercih ettiğiniz NuGet paket yönetim aracını kullanarak Microsoft.Azure.Azure.CosmosDB.Table'ı yüklemeden önce Microsoft.Azure.Storage.Common NuGet paketini yükleyin.

## <a name="faq"></a>SSS

[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Ayrıca bkz.

Azure Cosmos DB Tablo API'si hakkında daha fazla bilgi edinmek için azure [cosmos DB Tablo API'ye giriş](table-introduction.md)konusuna bakın. 
