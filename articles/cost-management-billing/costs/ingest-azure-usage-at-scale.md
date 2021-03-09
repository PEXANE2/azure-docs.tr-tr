---
title: Dışarı aktarmalar ile büyük maliyetli veri kümelerini düzenli olarak alma
description: Bu makale, Azure maliyet yönetimi 'nden dışarı aktarmalar ile büyük miktarlarda verileri düzenli olarak dışarı aktarmanıza yardımcı olur.
author: bandersmsft
ms.author: banders
ms.date: 03/08/2021
ms.topic: conceptual
ms.service: cost-management-billing
ms.subservice: cost-management
ms.reviewer: adwise
ms.openlocfilehash: 465225341bdffc984ac6cbc82ba94eb656ad60df
ms.sourcegitcommit: 15d27661c1c03bf84d3974a675c7bd11a0e086e6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2021
ms.locfileid: "102509703"
---
# <a name="retrieve-large-cost-datasets-recurringly-with-exports"></a>Dışarı aktarmalar ile büyük maliyetli veri kümelerini düzenli olarak alma

Bu makale, Azure maliyet yönetimi 'nden dışarı aktarmalar ile büyük miktarlarda verileri düzenli olarak dışarı aktarmanıza yardımcı olur. Dışarı aktarma, toplu olmayan maliyet verilerini almak için önerilen yoldur. Özellikle Kullanım Ayrıntıları API’sini kullanarak çağırıp indirilemeyecek kadar büyük kullanım dosyaları olduğunda bu yoldan faydalanabilirsiniz. Dışarı aktarılan veriler, seçtiğiniz Azure Depolama hesabına yerleştirilir. Buradan, verileri kendi sistemlerinize yükleyip gereken şekilde analiz edebilirsiniz. Dışarı aktarmaları Azure portalında yapılandırmak için bkz. [Verileri dışarı aktarma](tutorial-export-acm-data.md).

Belirli kapsamlardaki dışarı aktarmaları otomatikleştirmek istiyorsanız sonraki bölümde yer alan örnek API isteğiyle çalışmaya başlamanız faydalı olabilir. Genel ortam yapılandırmanızın bir parçası olarak otomatik dışarı aktarmalar oluşturmak için Dışarı Aktarmalar API’sini kullanabilirsiniz. Otomatik dışarı aktarmalar, ihtiyaç duyduğunuz verilere sahip olmanızı sağlar. Azure kullanımınızı genişlettikçe kendi kuruluşunuzun sistemlerinde kullanabilirsiniz.

## <a name="common-export-configurations"></a>Ortak dışarı aktarma yapılandırmaları

İlk dışarı aktarmanızı oluşturmadan önce, bunu sağlamak için gereken senaryoyu ve yapılandırma seçeneklerini dikkate alın. Aşağıdaki dışarı aktarma seçeneklerini göz önünde bulundurun:

- **Yinelenme**: Dışarı aktarma işlerinin ne sıklıkla çalıştırıldığını ve bir dosyanın Azure Depolama hesabınıza yerleştirilme zamanını belirler. Günlük, Haftalık ve Aylık arasında seçim yapın. Yinelenmenizi, kuruluşunuzun şirket içindeki sistemi tarafından kullanılan veri içeri aktarma işleriyle eşleşecek şekilde yapılandırmayı deneyin.
- **Yinelenme Süresi**: Dışarı aktarmanın ne kadar süre boyunca geçerli kalacağını belirler. Dosyalar yalnızca yinelenme süresi boyunca dışarı aktarılır.
- **Zaman Dilimi**: Belirli bir çalıştırma üzerinde dışarı aktarma tarafından oluşturulan veri miktarını belirler. Yaygın seçenekler MonthToDate ve WeekToDate’tir.
- **StartDate**: Dışarı aktarma zamanlamasının başlamasını istediğiniz zamanı yapılandırır. Dışarı aktarma ilk önce StartDate tarihinde, daha sonra Yinelenmenize göre oluşturulur.
- **Tür** - Üç tür dışarı aktarma vardır:
  - ActualCost - Belirli bir döneme ait toplam kullanımı ve maliyetleri, faturanıza yansıtıldığı şekilde gösterir.
  - AmortizedCost - Belirli bir döneme ait toplam kullanımı ve maliyetleri, amortismanların uygun rezervasyon satın alma maliyetlerine uygulanmış şekilde gösterir.
  - Kullanım - 20 Temmuz 2020’den önce oluşturulan tüm dışarı aktarmalar Kullanım türündedir. Tüm zamanlanmış dışarı aktarmalarınızı ActualCost veya AmortizedCost olarak güncelleştirin.
- **Sütunlar**: Dışarı aktarma dosyanıza dahil etmek istediğiniz veri alanlarını tanımlar. Bunlar, Kullanım Ayrıntıları API’sinde yer alan dosyalarla ilişkilidir. Daha fazla bilgi için bkz. [Kullanım Ayrıntıları API’si](/rest/api/consumption/usagedetails/list).

## <a name="create-a-daily-month-to-date-export-for-a-subscription"></a>Bir abonelik için günlük ay başından bugüne kadar dışarı aktarması oluşturma

İstek URL’si: `PUT https://management.azure.com/{scope}/providers/Microsoft.CostManagement/exports/{exportName}?api-version=2020-06-01`

```json
{
  "properties": {
    "schedule": {
      "status": "Active",
      "recurrence": "Daily",
      "recurrencePeriod": {
        "from": "2020-06-01T00:00:00Z",
        "to": "2020-10-31T00:00:00Z"
      }
    },
    "format": "Csv",
    "deliveryInfo": {
      "destination": {
        "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/MYDEVTESTRG/providers/Microsoft.Storage/storageAccounts/{yourStorageAccount} ",
        "container": "{yourContainer}",
        "rootFolderPath": "{yourDirectory}"
      }
    },
    "definition": {
      "type": "ActualCost",
      "timeframe": "MonthToDate",
      "dataSet": {
        "granularity": "Daily",
        "configuration": {
          "columns": [
            "Date",
            "MeterId",
            "ResourceId",
            "ResourceLocation",
            "Quantity"
          ]
        }
      }
    }
}
```

## <a name="copy-large-azure-storage-blobs"></a>Büyük Azure depolama bloblarını Kopyala

Azure kullanım ayrıntılarınız için Azure depolama hesaplarınıza blob 'lar olarak dışarı aktarmaları zamanlamak için maliyet yönetimi 'ni kullanabilirsiniz. Sonuçta elde edilen blob boyutları gigabayt 'tan fazla olabilir. Azure maliyet yönetimi ekibi, büyük Azure depolama bloblarını kopyalamayı test etmek için Azure depolama ekibi ile birlikte çalışmıştır. Sonuçlar aşağıdaki bölümlerde belgelenmiştir. Depolama bloblarını bir Azure bölgesinden diğerine kopyaladığınızda benzer sonuçlar elde edebilirsiniz.

Ekip, performansını test etmek için, ABD Batı bölgesindeki depolama hesaplarından blob 'ları aynı ve diğer bölgelere aktardı. Ekip, Güney Doğu Asya bölgesindeki depolama hesaplarına saniyede 2 GB ile aynı bölgedeki saniyede 150 MB 'ye kadar ölçülen hızları ölçdü.

### <a name="test-configuration"></a>Test yapılandırması

Takım, blob aktarım hızlarını ölçmek için, NuGet aracılığıyla Azure veri taşıma kitaplığı 'nın (DLD) en son sürümüne (v 2.0.1'i) başvuran basit bir .NET konsol uygulaması oluşturdu. DLCı, Azure depolama ekibi tarafından verilen ve aktarım hizmetlerine programlı erişimi kolaylaştıran bir SDK 'dir. Daha sonra, birden çok bölgede standart v2 depolama hesapları oluşturup Batı ABD kaynak bölge olarak kullanır. Bunlar, her biri 10 2 GB blok Blobları bulunan kapsayıcılar ile ilgili depolama hesaplarını doldurmaktadır. Bu kişiler, DLD 'nin _Transfermanager. CopyDirectoryAsync ()_ metodunu _Copymethod. ServiceSideSyncCopy_ seçeneğiyle kullanarak kapsayıcıları diğer depolama hesaplarına kopyalamıştır. Sınamalar, 12 çekirdek ve 1-GbE ağ ile Windows 10 çalıştıran bir bilgisayarda yürütülür.

Kullanılan uygulama ayarları:

- _TransferManager.Configurışlar. Paralleloperations_  =  _Environment. ProcessorCount \* 32_. Takım, genel üretilen iş üzerinde en etkili olması için ayarı buldu. Çekirdek sayısının, test istemcisi için en iyi verimi sağlayan 32 katından oluşan bir değer.
- _ServicePointManager. DefaultConnectionLimit = int. MaxValue_. En yüksek değere ayarlanması, yukarıdaki _Paralleloperations_ ayarına aktarım paralelliği tam denetimini etkin bir şekilde geçirir.
- _TransferManager.Configurışlar. Blok boyutu = 4.194.304_. 4 MB ile aktarım ücretleri üzerinde bir etkisi olduğundan, test için en iyi hale getiriyordu.

Daha fazla bilgi ve örnek kod için, [sonraki adımlar](#next-steps) bölümündeki bağlantılar bölümüne bakın.

### <a name="test-results"></a>Test sonuçları

| **Sınama numarası** | **Bölgeye** | **Bloblar** | **Süre (saniye)** | **MB/s** | **Açıklamalar** |
| --- | --- | --- | --- | --- | --- |
| 1 | WestUS | 2 GB x 10 | 10 | 2.000 |   |
| 2 | WestUS2 | 2 GB x 10 | 33 | 600 |   |
| 3 | EastUS | 2 GB x 10 | 67 | 300 |   |
| 4 | EastUS | 2 GB x 10 x 4 | 99 | 200 | 8 depolama hesabı kullanan 4 Batı-4 Doğu ortalama aktarım sayısı |
| 6 | EastUS | 2 GB x 10 x 4 | 92 | 870 | 1 depolama hesabından diğerine 4 paralel aktarım |
| 5 | EastUS | 2G x 10 x 8 | 148 | 135 | 8 depolama hesabı kullanan 8 paralel aktarım: aktarım başına 4 Batı-4x2 Doğu ortalaması |
| 7 | Doğu Asya | 2 GB x 10 | 133 | 150 |   |
| 8 | Doğu Asya | 2 GB x 10 x 4 | 444 | 180 | 1 depolama hesabından diğerine 4 paralel aktarım |

### <a name="sync-transfer-characteristics"></a>Eşitleme aktarım özellikleri

Kullanım için uygun olan DML ile kullanılan hizmet tarafı eşitleme aktarımının özelliklerinden bazıları şunlardır:

- DML, tek bir Blobu veya bir dizini aktarabilir. Dizin aktarımı için blob ön ekiyle eşleştirmek üzere bir arama kalıbı kullanabilirsiniz.
- Blok Blobu aktarımları paralel olarak gerçekleşir. Tüm işlemler aktarım işleminin sonuna doğru tamamlanmıştır. Ayrı blob blokları paralel olarak aktarılır.
- Aktarım, istemcide zaman uyumsuz olarak yürütülür. Aktarım durumu, bir _Transfercontext_ nesnesinde tanımlanılabilecek bir yönteme geri çağırma aracılığıyla düzenli olarak kullanılabilir.
- Aktarım, ilerlemesi sırasında denetim noktaları oluşturur ve bir _Transfercheckpoint_ nesnesi gösterir. Nesnesi, _Transfercontext_ nesnesi aracılığıyla en son denetim noktasını temsil eder. _Transfercheckpoint_ bir aktarım iptal edildiğinde/durdurulmadan önce kaydedilirse, aktarım noktasından yedi güne kadar devam edebilir. Aktarım yalnızca en son sürümü değil, herhangi bir kontrol noktasından devam edebilir.
- Aktarım istemcisi işlemi sonlandırıldı ve denetim noktası özelliği uygulanmaksızın yeniden başlatılırsa.
  - Herhangi bir blob aktarımı tamamlanmadan önce, aktarım yeniden başlatılır.
  - Blobların bazıları tamamlandıktan sonra, aktarım yalnızca tamamlanmış Bloblar için yeniden başlatılır.
- İstemci yürütmesini duraklatmak aktarımları duraklatır.
- Blob aktarımı özelliği, istemciyi geçici hatalardan soyutlar. Örneğin, depolama hesabı azaltma normalde aktarım başarısız olmasına neden olmaz, ancak aktarımı yavaşlatır.
- Hizmet tarafı aktarımları, CPU ve bellek için düşük istemci kaynak kullanımına, bazı ağ bant genişliğine ve bağlantılarına sahiptir.

### <a name="async-transfer-characteristics"></a>Zaman uyumsuz aktarım özellikleri

_Transfermanager. CopyDirectoryAsync ()_ metodunu _Copymethod. ServiceSideAsyncCopy_ seçeneğiyle çağırabilirsiniz. İstemci perspektifinden eşitleme aktarım mekanizmasına benzer, ancak işlemdeki aşağıdaki farklılıklarla aynı şekilde çalışır:

- Aktarım oranları, eşdeğer eşitleme aktarımından çok daha yavaştır (genellikle 10 MB/s veya daha az).
- Aktarım, istemci işlemi sonlansa bile devam eder.
- Kontrol noktaları desteklenmekle birlikte, bir _Transferkontrol noktası_ kullanarak bir aktarımı sürdürmek denetim noktası zamanında devam ettirilmez, ancak aktarımın geçerli durumunda kalmaz.

### <a name="test-summary"></a>Test Özeti

Azure Blob depolama, hizmet tarafı eşitleme aktarımı özelliği ile yüksek küresel aktarım hızlarını destekler. .NET uygulamalarında özelliğinin kullanılması, veri taşıma kitaplığı kullanılarak basittir. Maliyet yönetimi dışarı aktarmaları, yüzlerce gigabayt veriyi bir saatten daha az bir yerde depolama hesabına güvenle kopyalamak için mümkündür.

## <a name="next-steps"></a>Sonraki adımlar

- [Microsoft Azure depolama veri taşıma kitaplığı](https://github.com/Azure/azure-storage-net-data-movement) kaynağına bakın.
- Veri [taşıma kitaplığıyla veri aktarma](../../storage/common/storage-use-data-movement-library.md).
- Bkz. [Azuredmlbackup örnek uygulama](https://github.com/markjbrown/AzureDmlBackup) kaynak örneği.
- [Azure Blob depolama Ile yüksek Iş üretimini](https://azure.microsoft.com/blog/high-throughput-with-azure-blob-storage)okuyun.