---
title: Azure Cosmos DB Tablo API'sine Giriş
description: Azure Tabloları API'sini kullanarak büyük hacimli anahtar değerli verileri düşük gecikme li olarak depolamak ve sorgulamak için Azure Cosmos DB'yi nasıl kullanabileceğinizi öğrenin.
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.topic: overview
ms.date: 07/26/2019
ms.author: sngun
ms.openlocfilehash: 5b2e2c51eaa878ba0ce8bc31c001575acebe6919
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79240159"
---
# <a name="introduction-to-azure-cosmos-db-table-api"></a>Azure Cosmos DB: Tablo API’sine Giriş

[Azure Cosmos DB](introduction.md), Azure Tablo depolaması için yazılmış olan ve aşağıdaki gibi üst düzey özelliklere ihtiyaç duyan uygulamalar için Tablo API'sini sunar:

* [Anahtar teslimi küresel dağıtım.](distribute-data-globally.md)
* Dünya genelinde [adanmış aktarım hızı](partition-data.md).
* 99 yüzdebirlikte tek basamaklı milisaniyelik gecikme süresi.
* Garantili yüksek kullanılabilirlik.
* Otomatik ikincil dizin oluşturma.

Azure Tablo depolama için yazılmış uygulamalar herhangi bir kod değişikliği olmadan Tablo API'sini kullanarak Azure Cosmos DB'ye geçirilebilir ve üst düzey özelliklerden yararlanabilir. Tablo API’si, .NET, Java, Python ve Node.js ile kullanılabilecek istemci SDK’larına sahiptir.

> [!IMPORTANT]
> .NET Framework SDK [Microsoft.Azure.CosmosDB.Table](https://www.nuget.org/packages/Microsoft.Azure.CosmosDB.Table) bakım modundadır ve yakında amortismana dahil edilir. Tablo API tarafından desteklenen en son özellikleri almaya devam etmek için lütfen yeni .NET Standart kitaplığı [Microsoft.Azure.Cosmos.Table'a](https://www.nuget.org/packages/Microsoft.Azure.Cosmos.Table) yükseltin.

## <a name="table-offerings"></a>Tablo teklifleri
Şu anda Azure Tablo Depolama hizmetini kullanıyorsanız, Azure Cosmos DB Tablo API’sine geçerek aşağıdaki avantajlara sahip olabilirsiniz:

| | Azure Tablo depolama | Azure Cosmos DB Tablo API’si |
| --- | --- | --- |
| Gecikme süresi | Hızlıdır, ancak gecikme süresi için üst sınır yoktur. | Okuma ve yazma için tek basamaklı milisaniye gecikmesi, dünyanın herhangi bir yerinde, herhangi bir ölçekte, 99 yüzdelik olarak okuma ve yazma için <10 ms gecikme ile desteklenen. |
| Aktarım hızı | Değişken aktarım hızı modeli. Tabloların 20.000 işlem/sn'lik bir ölçeklenebilirlik sınırı vardır. | SLA'lar ile desteklenen [tablo başına adanmış, ayrılmış aktarım hızı](request-units.md) ile yüksek düzeyde ölçeklenebilir. Hesapların aktarım hızı açısından üst sınırı yoktur ve tablo başına saniyede 10 milyondan fazla işlem desteklenir. |
| Genel dağıtım | Yüksek kullanılabilirlik için isteğe bağlı okunabilir bir ikincil okuma bölgesi olan tek bölge. Yük devretme başlatamazsınız. | Bir bölgeden herhangi bir bölgeye [anahtar teslimi küresel dağıtım.](distribute-data-globally.md) Her zaman, dünyanın her yerinde [otomatik ve el ile yük devretme](high-availability.md) desteği. Herhangi bir bölgenin yazma işlemlerini kabul etmesini sağlamak için çok yönlü yetenek. |
| Dizin Oluşturma | Yalnızca PartitionKey ve RowKey’de birincil dizin. İkincil dizin yok. | Tüm özelliklerde varsayılan olarak, dizin yönetimi olmadan otomatik ve tam dizin oluşturma. |
| Sorgu | Sorgu yürütme birincil anahtar için dizini kullanır, aksi durumda tarar. | Sorgular, hızlı sorgu süreleri için özelliklerde otomatik dizin oluşturma avantajından yararlanabilir. |
| Tutarlılık | Birincil bölge içinde güçlü. İkincil bölge içinde nihai. | Uygulama gereksinimlerinize bağlı olarak kullanılabilirliği, gecikmeyi, iş ortalığını ve tutarlılığı kapatmak için [iyi tanımlanmış beş tutarlılık düzeyi.](consistency-levels.md) |
| Fiyatlandırma | Depolama açısından iyileştirilmiş. | Aktarım hızı açısından iyileştirilmiş. |
| SLA’lar | Çoğaltma stratejisine bağlı olarak %99,9 ila %99,99 kullanılabilirlik. | %99,999'u okuma kullanılabilirliği, %99,99'u tek bölgeli hesaplarda yazma kullanılabilirliği ve %99,999'u çok bölgeli hesaplarda kullanılabilirlik yazmaktadır. Kullanılabilirlik, gecikme süresi, iş artışı ve tutarlılığı kapsayan [kapsamlı SLA'lar.](https://azure.microsoft.com/support/legal/sla/cosmos-db/) |

## <a name="get-started"></a>Kullanmaya başlayın

[Azure portalındaki](https://portal.azure.com) yeni Azure Cosmos DB hesabı. Ardından [.NET kullanarak Tablo API'sı için hızlı başlangıç](create-table-dotnet.md) makalemizi inceleyin. 

> [!IMPORTANT]
> Önizleme sırasında bir Tablo API hesabı oluşturduysanız, genel kullanıma açık Tablo API SDK’ları ile çalışmak için lütfen [yeni Tablo API hesabı](create-table-dotnet.md#create-a-database-account) oluşturun.
>

## <a name="next-steps"></a>Sonraki adımlar

İşte başlamanıza yardımcı olacak birkaç ipucu:
* [Tablo API'sini kullanarak bir .NET uygulaması derleme](create-table-dotnet.md)
* [.NET’te Tablo API’siyle geliştirme](tutorial-develop-table-dotnet.md)
* [Tablo API’sini kullanarak tablo verilerini sorgulama](tutorial-query-table.md)
* [Tablo API'sini kullanarak Azure Cosmos DB genel dağıtımını ayarlamayı öğrenin](tutorial-global-distribution-table.md)
* [Azure Cosmos DB Tablosu .NET Standart SDK](table-sdk-dotnet-standard.md)
* [Azure Cosmos DB Tablosu .NET SDK](table-sdk-dotnet.md)
* [Azure Cosmos DB Tablo Java SDK](table-sdk-java.md)
* [Azure Cosmos DB Tablo Düğümü.js SDK](table-sdk-nodejs.md)
* [Python için Azure Cosmos DB Tablosu SDK'sı](table-sdk-python.md)