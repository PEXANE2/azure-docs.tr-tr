---
title: 'Azure Cosmos DB: Toplu yürütme .NET API, SDK & kaynakları'
description: Sürüm tarihleri, emeklilik tarihleri ve Azure Cosmos DB toplu yürütme .NET SDK'nın her sürümü arasında yapılan değişiklikler dahil olmak üzere toplu uygulayıcı .NET API ve SDK hakkında her şeyi öğrenin.
author: tknandu
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: reference
ms.date: 01/16/2020
ms.author: ramkris
ms.openlocfilehash: 1a8040fc397b526b540ce9343baa985cab49e2b4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76169408"
---
# <a name="net-bulk-executor-library-download-information"></a>.NET toplu uygulayıcı kitaplık: Bilgileri karşıdan yükleme 

> [!div class="op_single_selector"]
> * [.NET](sql-api-sdk-dotnet.md)
> * [.NET Değişim Akışı](sql-api-sdk-dotnet-changefeed.md)
> * [.NET Core](sql-api-sdk-dotnet-core.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Async Java](sql-api-sdk-async-java.md)
> * [Java](sql-api-sdk-java.md)
> * [Python](sql-api-sdk-python.md)
> * [Geri kalanı](https://docs.microsoft.com/rest/api/cosmos-db/)
> * [REST Kaynak Sağlayıcısı](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/)
> * [SQL](sql-api-query-reference.md)
> * [Toplu uygulayıcı - .NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [Toplu uygulayıcı - Java](sql-api-sdk-bulk-executor-java.md)

| |  |
|---|---|
| **Açıklama**| .Net toplu yürütücü kitaplığı, istemci uygulamalarının Azure Cosmos DB hesaplarında toplu işlemler gerçekleştirmesine olanak tanır. Bu kitaplık BulkImport, BulkUpdate ve BulkDelete ad alanları sağlar. BulkImport modülü, bir koleksiyon için sağlanan verimin maksimum ölçüde tüketilmesi için belgeleri optimize edilmiş bir şekilde toplayabilir. BulkUpdate modülü, Azure Cosmos kapsayıcılarında varolan verileri düzeltme ekinde veyare olarak güncelleyebilir. BulkDelete modülü, bir koleksiyon için sağlanan verimin maksimum ölçüde tüketilmesi için belgeleri en iyi şekilde silebilir.|
|**SDK indir**| [NuGet](https://www.nuget.org/packages/Microsoft.Azure.CosmosDB.BulkExecutor/) |
| **GitHub'da toplu yürütme kitaplığı**| [GitHub](https://github.com/Azure/azure-cosmosdb-bulkexecutor-dotnet-getting-started)|
|**API belgeleri**|[.NET API başvuru belgeleri](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor?view=azure-dotnet)|
|**Kullanmaya başlayın**|[Toplu yürütme kitaplığı .NET SDK ile başlayın](bulk-executor-dot-net.md)|
| **Geçerli desteklenen çerçeve**| Microsoft .NET Framework 4.5.2, 4.6.1 ve .NET Standart 2.0 |

> [!NOTE]
> Toplu uygulayıcı kullanıyorsanız, lütfen SDK'da yerleşik toplu uygulayıcısı olan [.NET SDK'nın](tutorial-sql-api-dotnet-bulk-import.md)3.x son sürümüne bakın. 

## <a name="release-notes"></a>Sürüm notları

### <a name="241-preview"></a><a name="2.4.1-preview"/>2.4.1-önizleme

* Tümsiletler dahil toplam süreyi doğru bir şekilde ölçmek için BulkDelete yanıtında TotalElapsedTime düzeltildi.

### <a name="240-preview"></a><a name="2.4.0-preview"/>2.4.0-önizleme

* SDK bağımlılığı >= 2,5.1 olarak değiştirildi

### <a name="230-preview2"></a><a name="2.3.0-preview2"/>2.3.0-önizleme2

* Vertiklerde ve kenarlarda ttl kabul etmek için grafik toplu uygulayıcısı için destek eklendi

### <a name="220-preview2"></a><a name="2.2.0-preview2"/>2.2.0-önizleme2

* Ağ Geçidi modunda çalışırken Azure Cosmos DB'nin elastik ölçekleme sırasında özel durumlara neden olan bir sorun giderildi. Bu düzeltme işlevsel olarak 1.4.1 sürümüne eşdeğer hale getirir.

### <a name="210-preview2"></a><a name="2.1.0-preview2"/>2.1.0-önizleme2

* SQL API hesapları için BulkDelete desteği eklendi bölüm anahtarı kabul etmek, belge id tuples silmek için. Bu değişiklik işlevsel olarak 1.4.0 sürümüne eşdeğer hale getirir.

### <a name="200-preview2"></a><a name="2.0.0-preview2"/>2.0.0-önizleme2

* .NET Standard 2.0'ı desteklemek için MongoBulkExecutor dahil. Bu özellik, hedef çerçeve olarak .NET Standard 2.0'ı destekleyen bir özellik ile işlevsel olarak 1.3.0 sürümüne eşdeğer dir.

### <a name="200-preview"></a><a name="2.0.0-preview"/>2.0.0-önizleme

* .NET Core uygulamalarıile toplu yürütme kitaplığı çalışması için desteklenen hedef çerçevelerden biri olarak .NET Standard 2.0 eklendi.

### <a name="188"></a><a name="1.8.8"/>1.8.8

* Dolgu ekleyerek ve bazı durumlarda maksimum belge boyutu sınırını aşarak belge boyutunu beklenmedik bir şekilde artıran MongoBulkExecutor'da bir sorun giderildi.

### <a name="187"></a><a name="1.8.7"/>1.8.7

* Koleksiyon bölüm anahtar yollarını iç içe geçtiğinde BulkDeleteAsync ile ilgili bir sorun giderildi.

### <a name="186"></a><a name="1.8.6"/>1.8.6

* MongoBulkExecutor şimdi IDisposable uygular ve kullanıldıktan sonra bertaraf edilmesi bekleniyor.

### <a name="185"></a><a name="1.8.5"/>1.8.5

* SDK sürümündeki kilit kaldırıldı. Paket artık SDK >= 2.5.1'e bağlıdır.

### <a name="184"></a><a name="1.8.4"/>1.8.4

* Sayısal değerlere sahip POCO nesnelerinin bir listesiyle BulkImport'u ararken tanımlayıcıların sabit kullanımı.

### <a name="183"></a><a name="1.8.3"/>1.8.3

* Tümsiletler dahil toplam süreyi doğru bir şekilde ölçmek için BulkDelete yanıtında TotalElapsedTime düzeltildi.

### <a name="182"></a><a name="1.8.2"/>1.8.2

* Belirli senaryolarda yüksek CPU tüketimi düzeltildi.
* İzleme şimdi TraceSource kullanır. Kullanıcılar `BulkExecutorTrace` kaynak için dinleyici tanımlayabilirsiniz.
* 2Mb boyutuna yakın belge gönderirken kilitlenmeye neden olabilecek nadir bir senaryo düzeltildi.

### <a name="160"></a><a name="1.6.0"/>1.6.0

* Azure Cosmos DB .NET SDK'nın (2.4.0) en son sürümünü kullanacak şekilde toplu uygulayıcısı güncelleştirildi

### <a name="150"></a><a name="1.5.0"/>1.5.0

* Vertiklerde ve kenarlarda ttl kabul etmek için grafik toplu uygulayıcısı için destek eklendi

### <a name="141"></a><a name="1.4.1"/>1.4.1

* Ağ Geçidi modunda çalışırken Azure Cosmos DB'nin elastik ölçekleme sırasında özel durumlara neden olan bir sorun giderildi.

### <a name="140"></a><a name="1.4.0"/>1.4.0

* SQL API hesapları için BulkDelete desteği eklendi bölüm anahtarı kabul etmek, belge id tuples silmek için.

### <a name="130"></a><a name="1.3.0"/>1.3.0

* Toplu yürütücü tarafından kullanılan kullanıcı aracısında biçimlendirme sorununa neden olan bir sorun giderildi.

### <a name="120"></a><a name="1.2.0"/>1.2.0

* Depolama özel durumlar atmadan geçerli kapasiteyi aştığında Cosmos konteynerinin elastik ölçeklemeye şeffaf bir şekilde uyum sağlamak için toplu uygulayıcı alma ve güncelleme API'lerinde iyileştirme yapıldı.

### <a name="112"></a><a name="1.1.2"/>1.1.2

* DocumentDB .NET SDK bağımlılığını sürüm 2.1.3'e yükseltildi.

### <a name="111"></a><a name="1.1.1"/>1.1.1

* Sabit koleksiyonlara içe aktarırken toplu uygulayıcının JSRT hatası atmasını sağlayan bir sorun giderildi.

### <a name="110"></a><a name="1.1.0"/>1.1.0

* Azure Cosmos DB SQL API hesapları için BulkDelete işlemi için destek eklendi.
* Azure Cosmos DB'nin MongoDB için API'si olan hesaplar için BulkImport işlemi için destek eklendi.
* DocumentDB .NET SDK bağımlılığını sürüm 2.0.0'a yükseltildi. 

### <a name="102"></a><a name="1.0.2"/>1.0.2

* Azure Cosmos DB Gremlin API hesapları için BulkImport işlemi için destek eklendi.

### <a name="101"></a><a name="1.0.1"/>1.0.1

* Azure Cosmos DB SQL API hesapları için BulkImport işleminde küçük hata düzeltmesi.

### <a name="100"></a><a name="1.0.0"/>1.0.0

* Azure Cosmos DB SQL API hesapları için BulkImport ve BulkUpdate işlemleri için destek eklendi.

## <a name="next-steps"></a>Sonraki adımlar

Toplu uygulayıcı Java kitaplığı hakkında bilgi edinmek için aşağıdaki makaleye bakın:

[Java toplu yürütme kütüphanesi SDK ve sürüm bilgileri](sql-api-sdk-bulk-executor-java.md)
