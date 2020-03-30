---
title: Azure Cosmos DB Tablo API .NET Standart SDK & Kaynakları
description: Sürüm tarihleri, emeklilik tarihleri ve her sürüm arasında yapılan değişiklikler de dahil olmak üzere Azure Cosmos DB Tablo API ve .NET Standart SDK hakkında her şeyi öğrenin.
author: sakash279
ms.author: akshanka
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.devlang: dotnet
ms.topic: reference
ms.date: 03/18/2019
ms.openlocfilehash: 48fd85e27feb3d0c7f7f722dbbd502be55684385
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76771583"
---
# <a name="azure-cosmos-db-table-net-standard-api-download-and-release-notes"></a>Azure Cosmos DB Tablo .NET Standart API: Notları indirin ve yayınla
> [!div class="op_single_selector"]
> 
> * [.NET](table-sdk-dotnet.md)
> * [.NET Standard](table-sdk-dotnet-standard.md)
> * [Java](table-sdk-java.md)
> * [Node.js](table-sdk-nodejs.md)
> * [Python](table-sdk-python.md)

|   |   |
|---|---|
|**SDK indir**|[NuGet](https://www.nuget.org/packages/Microsoft.Azure.Cosmos.Table)|
|**Örnek**|[Cosmos DB Tablo API .NET Örnek](https://github.com/Azure-Samples/azure-cosmos-table-dotnet-core-getting-started)|
|**Quickstart**|[Quickstart](create-table-dotnet.md)|
|**Öğretici**|[Öğretici](tutorial-develop-table-dotnet.md)|
|**Geçerli desteklenen çerçeve**|[Microsoft .NET Standart 2.0](https://www.nuget.org/packages/NETStandard.Library)|
|**Rapor Sorunu**|[Rapor Sorunu](https://github.com/Azure/azure-cosmos-table-dotnet/issues)|

## <a name="release-notes-for-200-series"></a>2.0.0 serisi için yayın notları
2.0.0 serisi, performans iyileştirmeleri ve Cosmos DB bitiş noktasına ad alanı birleştirme ile [Microsoft.Azure.Cosmos'a](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/)olan bağımlılığı nı alır.

### <a name="200-preview"></a><a name="2.0.0-preview"/>2.0.0-önizleme
* [microsoft.Azure.Cosmos](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/)bağımlılığı alır 2.0.0 Tablo SDK ilk önizleme, performans iyileştirmeleri ve Cosmos DB bitiş noktası için ad alanı konsolidasyonu ile. Kamu API'si aynı kalır.

## <a name="release-notes-for-100-series"></a>1.0.0 serisi için yayın notları
1.0.0 serisi [Microsoft.Azure.DocumentDB.Core'a](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core/)bağımlı dır.

### <a name="105"></a><a name="1.0.5"/>1.0.5
* Cosmos DB Tablo API ile iletişim kurmak için Rest Executor'ı kullanmak için TableClientConfiguration altında yeni config tanıtın

### <a name="105-preview"></a><a name="1.0.5-preview"/>1.0.5-önizleme
* Hata düzeltmeleri

### <a name="104"></a><a name="1.0.4"/>1.0.4
* Hata düzeltmeleri
* RestExecutorConfiguration için HttpClientTimeout seçeneğini sağlayın.

### <a name="104-preview"></a><a name="1.0.4-preview"/>1.0.4-önizleme
* Hata düzeltmeleri
* RestExecutorConfiguration için HttpClientTimeout seçeneğini sağlayın.

### <a name="101"></a><a name="1.0.1"/>1.0.1
* Hata düzeltmeleri

### <a name="100"></a><a name="1.0.0"/>1.0.0
* Genel kullanılabilirlik sürümü

### <a name="0110-preview"></a><a name="0.11.0-preview"/>0.11.0-önizleme
* CloudTableClient'ın nasıl yapılandırılabildiği konusunda değişiklikler yapıldı. Şimdi inşaat sırasında bir TableClientConfiguration nesne alır. TableClientConfiguration, hedef uç noktanın Cosmos DB Table API veya Azure Depolama Tablosu API'sı olup olmadığına bağlı olarak istemci davranışını yapılandırmak için farklı özellikler sağlar.
* Özel bir sütunda sıralanmış sırada sonuçları döndürmek için TableQuery'ye destek eklendi. Bu özellik yalnızca Cosmos DB Tablo uç noktalarında desteklenir.
* Çeşitli sonuç türlerinde İstek Ücretlerini ortaya çıkarmak için destek eklendi. Bu özellik yalnızca Cosmos DB Tablo uç noktalarında desteklenir.

### <a name="0101-preview"></a><a name="0.10.1-preview"/>0.10.1-önizleme
* Azure Depolama Tablosu uç noktalarına karşı SAS belirteci, TablePermissions, ServiceProperties ve ServiceStats işlemleri için destek ekleyin. 
   > [!NOTE]
   > Önceki Azure Depolama Tablosu SDK'larında istemci tarafı şifreleme gibi bazı işlevler henüz desteklenmez.

### <a name="0100-preview"></a><a name="0.10.0-preview"/>0.10.0-önizleme
* Azure Depolama Tablosu bitiş noktalarına karşı temel CRUD, toplu iş ve sorgu işlemleri için destek ekleyin. 
   > [!NOTE]
   > Önceki Azure Depolama Tablosu SDK'larında istemci tarafı şifreleme gibi bazı işlevler henüz desteklenmez.

### <a name="091-preview"></a><a name="0.9.1-preview"/>0.9.1-önizleme
* Azure Cosmos DB Tablo .NET Standart SDK, Cosmos DB'deki Tablo veri modeline verimli erişim sağlayan bir çapraz platform .NET kitaplığıdır. Bu ilk sürüm, [.NET Framework için Cosmos DB Table SDK](table-sdk-dotnet.md)gibi benzer API'lere sahip Tablo ve Entity CRUD + Query işlevlerinin tam kümesini destekler. 
   > [!NOTE]
   >  Azure Depolama Tablosu bitiş noktaları henüz 0.9.1 önizleme sürümünde desteklenmedi.

## <a name="release-and-retirement-dates"></a>Serbest bırakma ve Emeklilik tarihleri
Microsoft, daha yeni/desteklenen bir sürüme geçişi kolaylaştırmak için SDK'nın emekliye geçmesinden en az **12 ay** önce bildirim sağlar.

Bu çapraz platform .NET Standart kitaplığı [Microsoft.Azure.Cosmos.Table](https://www.nuget.org/packages/Microsoft.Azure.Cosmos.Table) .NET Framework kitaplığı [Microsoft.Azure.CosmosDB.Table](https://www.nuget.org/packages/Microsoft.Azure.CosmosDB.Table)yerini alacaktır.

### <a name="200-series"></a>2.0.0 serisi
| Sürüm | Yayın Tarihi | Emeklilik Tarihi |
| --- | --- | --- |
| [2.0.0-önizleme](#2.0.0-preview) |Auguest 22, 2019 |--- |

### <a name="100-series"></a>1.0.0 serisi
| Sürüm | Yayın Tarihi | Emeklilik Tarihi |
| --- | --- | --- |
| [1.0.5](#1.0.5) |Eylül 13, 2019 |--- |
| [1.0.5-önizleme](#1.0.5-preview) |Auguest 20, 2019 |--- |
| [1.0.4](#1.0.4) |Auguest 12, 2019 |--- |
| [1.0.4-önizleme](#1.0.4-preview) |Temmuz 26, 2019 |--- |
| 1.0.2-önizleme |Mayıs 2, 2019 |--- |
| [1.0.1](#1.0.1) |19 Nisan 2019 |--- |
| [1.0.0](#1.0.0) |Mart 13, 2019 |--- |
| [0.11.0-önizleme](#0.11.0-preview) |Mart 5, 2019 |--- |
| [0.10.1-önizleme](#0.10.1-preview) |22 Ocak 2019 |--- |
| [0.10.0-önizleme](#0.10.0-preview) |18 Aralık 2018 |--- |
| [0.9.1-önizleme](#0.9.1-preview) |Ekim 18, 2018 |--- |


## <a name="faq"></a>SSS

[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Ayrıca bkz.
Azure Cosmos DB Tablo API'si hakkında daha fazla bilgi edinmek için azure [cosmos DB Tablo API'ye giriş](table-introduction.md)konusuna bakın.
