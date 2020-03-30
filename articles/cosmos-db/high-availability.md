---
title: Azure Cosmos DB'de yüksek kullanılabilirlik
description: Bu makalede, Azure Cosmos DB'nin yüksek kullanılabilirlik sağladığı açıklanmaktadır
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/06/2019
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: 0f024bac535ed792d8480c991e470cf5d85932b8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79247429"
---
# <a name="high-availability-with-azure-cosmos-db"></a>Azure Cosmos DB ile yüksek kullanılabilirlik

Azure Cosmos DB, verilerinizi Cosmos hesabınızla ilişkili tüm Azure bölgelerinde saydam olarak çoğaltır. Cosmos DB, verileriniz için aşağıdaki resimde gösterildiği gibi birden çok fazlalık katmanı kullanır:

![Fiziksel bölümleme](./media/high-availability/cosmosdb-data-redundancy.png)

- Cosmos kapsayıcıları içindeki veriler [yatay olarak bölümlenmiştir.](partitioning-overview.md)

- Her bölge içinde, her bölüm, tüm yinelemeler çoğaltılır ve çoğaltılmış çoğaltmaçoğunluğu tarafından kararlı bir şekilde işlenen bir yineleme kümesi tarafından korunur. Yinelemeler 10-20 hata etki alanına dağıtılır.

- Tüm bölgeler deki her bölüm çoğaltılır. Her bölge bir Cosmos kapsayıcının tüm veri bölümlerini içerir ve okuma ları kabul edebilir ve hizmet edebilir.  

Cosmos hesabınız *N* Azure bölgelerine dağıtılırsa, tüm verilerinizin en az *N* x 4 kopyası olur. Düşük gecikmeli veri erişimi sağlamanın ve Cosmos hesabınızla ilişkili bölgeler arasında yazma/okuma iş lerinin ölçeklendirilmesinin yanı sıra, daha fazla bölgeye (daha yüksek *N)* sahip olmak kullanılabilirliği daha da artırır.  

## <a name="slas-for-availability"></a>Kullanılabilirlik için SLA'lar

Küresel olarak dağıtılan bir veritabanı olarak Cosmos DB, yüzde 99'luk yüzdelik artışta üretim, gecikme süresi, tutarlılık ve yüksek kullanılabilirliği kapsayan kapsamlı SLA'lar sağlar. Aşağıdaki tablo, Cosmos DB tarafından tek ve çok bölgeli hesaplar için sağlanan yüksek kullanılabilirlik garantilerini göstermektedir. Yüksek kullanılabilirlik için Cosmos hesaplarınızı her zaman birden çok yazma bölgesine sahip olacak şekilde yapılandırın.

|İşlem türü  | Tek bölge |Çok bölgeli (tek bölge yazıyor)|Çok bölgeli (çok bölgeli yazma) |
|---------|---------|---------|-------|
|Yazar    | 99,99    |99,99   |99,999|
|Okur     | 99,99    |99,999  |99,999|

> [!NOTE]
> Uygulamada, sınırlı bayatlık, oturum, tutarlı önek ve nihai tutarlılık modelleri için gerçek yazma kullanılabilirliği yayınlanan SLA'lardan önemli ölçüde daha yüksektir. Tüm tutarlılık düzeyleri için gerçek okuma kullanılabilirliği, yayınlanan SLA'lardan önemli ölçüde daha yüksektir.

## <a name="high-availability-with-cosmos-db-in-the-event-of-regional-outages"></a>Bölgesel kesinti durumlarında Cosmos DB ile yüksek kullanılabilirlik

Bölgesel kesintiler oldukça yaygındır ve Azure Cosmos DB veritabanınızın her zaman yüksek kullanılabilirliğe sahip olduğundan emin olur. Aşağıdaki ayrıntılar, Cosmos hesap yapılandırmanıza bağlı olarak bir kesinti sırasında Cosmos DB davranışını yakalar:

- Cosmos DB ile, bir yazma işlemi istemciye kabul edilmeden önce, veriler yazma işlemlerini kabul eden bölge içindeki bir çok yineleme tarafından kararlı bir şekilde işlenir.

- Çok yazma bölgeleri ile yapılandırılan çok bölgeli hesaplar hem yazma hem de okuma için son derece kullanılabilir olacaktır. Bölgesel arızalar anlık bir şekilde gerçekleşir ve uygulamadan herhangi bir değişiklik gerektirmez.

- Tek bölgeli hesaplar, bölgesel bir kesinti sonrasında kullanılabilirliğini kaybedebilir. Her zaman en az **iki bölge** (tercihen, en az iki yazma bölgesi) cosmos hesabınızla her zaman yüksek kullanılabilirlik sağlamak için ayarlamanız önerilir.

- **Tek yazma lı çok bölgeli hesaplar (bölge kesintisi yazma):**
  - Yazma bölgesi kesintisi sırasında Cosmos hesabı, Azure Cosmos hesabında **otomatik olarak başarısız** lığa izin verildiğinde ikinci bir bölgeyi otomatik olarak yeni birincil yazma bölgesi olarak tanıtacaktır. Etkinleştirildiğinde, başarısızlık belirttiğiniz bölge önceliği sırasına göre başka bir bölgeye gelir.
  - Müşteriler ayrıca el **ile başarısız** olmayı ve cosmos yazma uç nokta URL'lerini kendileri inşa edilmiş bir aracı kullanarak izlemeyi de seçebilirler. Karmaşık ve gelişmiş sistem durumu izleme gereksinimleri olan müşteriler için, yazma bölgesinde bir hata olması durumunda bu durum azaltılmış RTO sağlayabilir.
  - Daha önce etkilenen bölge yeniden çevrimiçi olduğunda, bölge başarısız olduğunda çoğaltılmamış olan tüm yazma verileri [çakışma akışı](how-to-manage-conflicts.md#read-from-conflict-feed)aracılığıyla kullanılabilir hale getirilir. Uygulamalar çakışma akışını okuyabilir, çakışmaları uygulamaya özgü mantığa göre çözebilir ve güncelleştirilmiş verileri uygun şekilde Azure Cosmos kapsayıcısına geri yazabilir.
  - Daha önce etkilenen yazma bölgesi kurtarıldıktan sonra, okuma bölgesi olarak otomatik olarak kullanılabilir hale gelir. Yazma bölgesi olarak kurtarılan bölgeye geri dönebilirsiniz. [Azure CLI veya Azure portalını](how-to-manage-database-account.md#manual-failover)kullanarak bölgeler arasında geçiş yapabilirsiniz. Yazma bölgesini değiştirmeden önce, sırasında veya sonrasında **veri veya kullanılabilirlik kaybı yoktur** ve uygulamanız son derece kullanılabilir olmaya devam etmektedir.

- **Tek yazma lı çok günlükli hesaplar (okuma bölgesi kesintisi):**
  - Okuma bölgesi kesintisi sırasında, bu hesaplar okuma ve yazma için son derece kullanılabilir olmaya devam edecektir.
  - Etkilenen bölgenin bağlantısı otomatik olarak kesilir ve çevrimdışı işaretlenir. [Azure Cosmos DB SDK'lar](sql-api-sdk-dotnet.md) okuma çağrılarını tercih edilen bölge listesindeki bir sonraki kullanılabilir bölgeye yönlendirir.
  - Tercih edilen bölge listesindeki bölgelerin hiçbiri kullanılabilir durumda değilse çağrılar otomatik olarak geçerli yazma bölgesine döner.
  - Okuma bölgesi kesintisini işlemek için uygulama kodunuzda değişiklik gerekmez. Sonuç olarak, etkilenen bölge yeniden çevrimiçi olduğunda, daha önce etkilenen okuma bölgesi geçerli yazma bölgesiyle otomatik olarak eşitlenir ve okuma isteklerini sunmak için yeniden kullanılabilir olacaktır.
  - Sonraki okumalar kurtarılan bölgeye yönlendirilir ve bunun için uygulamanızın kodunda değişiklik yapılması gerekmez. Daha önce başarısız olan bir bölgenin hem başarısız olması hem de yeniden katılması sırasında, okuma tutarlılık garantileri Cosmos DB tarafından onurlandırılamaya devam eder.

- Azure bölgesinin kalıcı olarak geri kazanılabildiği nadir ve talihsiz bir olayda bile, çok bölgeli Cosmos hesabınız *Güçlü* tutarlılıkla yapılandırılırsa veri kaybı olmaz. Kalıcı olarak geri kazanılamayan bir yazma bölgesi, sınırlı bayatlık tutarlılığı ile yapılandırılan çok bölgeli bir Cosmos hesabı olması durumunda, olası veri kaybı penceresi K=100.000 güncelleştirmeve T=5 dakikanın bulunduğu bayatlık penceresiyle *(K* veya *T)* sınırlıdır. Oturum, tutarlı önek ve nihai tutarlılık düzeyleri için, olası veri kaybı penceresi en fazla 15 dakika ile sınırlıdır. Azure Cosmos DB için RTO ve RPO hedefleri hakkında daha fazla bilgi için [Tutarlılık düzeyleri ve veri dayanıklılığı](consistency-levels-tradeoffs.md#rto) hakkında bilgi

## <a name="availability-zone-support"></a>Kullanılabilirlik Bölgesi desteği

Bölgeler arası esnekliğe ek olarak, Azure Cosmos veritabanınızla ilişkilendirilecek bir bölge seçerken **bölge fazlalıÄ** ını da kısmızını etkinleştirebilirsiniz.

Kullanılabilirlik Bölgesi desteği yle Azure Cosmos DB, yinelemelerin bölge hataları sırasında yüksek kullanılabilirlik ve esneklik sağlamak için belirli bir bölge içindeki birden çok bölgeye yerleştirilmesini sağlar. Bu yapılandırmada gecikme ve diğer SLA'larda değişiklik yoktur. Tek bir bölge hatası durumunda, bölge artıklığı RPO=0 ile tam veri dayanıklılığı ve RTO=0 ile kullanılabilirlik sağlar.

Bölge [artıklığı, çok büyük çoğaltma](how-to-multi-master.md) özelliğine ek bir *özelliktir.* Bölgesel esneklik elde etmek için bölge fazlalığı tek başına güvenilemez. Örneğin, bölgesel kesintiler veya bölgeler arasında düşük gecikme erişimi durumunda, bölge artıklığına ek olarak birden çok yazma bölgesi olması önerilir.

Azure Cosmos hesabınız için çok bölgeli yazıları yapılandırırken, ek ücret ödemeden bölge artıklığına dahil olabilirsiniz. Aksi takdirde, bölge fazlalık desteği için fiyatlandırma ile ilgili aşağıdaki nota bakın. Azure Cosmos hesabınızın varolan bir bölgesinde bölge artıklığını, bölgeyi kaldırarak ve bölge artıklığı etkinleştirilmiş şekilde geri ekleyerek bölge artıklığını etkinleştirebilirsiniz.

Bu özellik aşağıdaki Azure bölgelerinde kullanılabilir:

- Güney Birleşik Krallık

- Güneydoğu Asya

- Doğu ABD

- Doğu ABD 2

- Orta ABD

- Batı Avrupa

- Batı ABD 2

> [!NOTE]
> Tek bir bölge Azure Cosmos hesabı için Kullanılabilirlik Bölgeleri etkinleştirmek, hesabınıza ek bir bölge eklemeye eşdeğer ücretlere neden olur. Fiyatlandırma yla ilgili ayrıntılar için [Azure Cosmos DB makalelerinde](optimize-cost-regions.md) [fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/cosmos-db/) ve çok bölgeli maliyete bakın.

Aşağıdaki tablo, çeşitli hesap yapılandırmalarının yüksek kullanılabilirlik yeteneğini özetley:

|KPI  |Kullanılabilirlik Bölgeleri olmayan tek bölge (AZ Dışı)  |Kullanılabilirlik Bölgeleri olan tek bölge (AZ)  |Kullanılabilirlik Bölgeleri (AZ, 2 bölge) ile çok bölgeli yazılar - En çok önerilen ayar |
|---------|---------|---------|---------|
|Yazma durumu SLA | %99,99 | %99,99 | %99,999 |
|Kullanılabilirlik SLA okuyun  | %99,99 | %99,99 | %99,999 |
|Fiyat | Tek bölge faturalandırma oranı | Tek bölge Kullanılabilirlik Bölgesi faturalandırma oranı | Çok bölgeli faturalandırma oranı |
|Bölge hataları – veri kaybı | Veri kaybı | Veri kaybı yok | Veri kaybı yok |
|Bölge hataları – kullanılabilirlik | Kullanılabilirlik kaybı | Kullanılabilirlik kaybı yok | Kullanılabilirlik kaybı yok |
|Gecikme yi oku | Çapraz bölge | Çapraz bölge | Düşük |
|Gecikme sonu yazma | Çapraz bölge | Çapraz bölge | Düşük |
|Bölgesel kesinti – veri kaybı | Veri kaybı |  Veri kaybı | Veri kaybı <br/><br/> Çok ana ve birden fazla bölge ile sınırlı bayatlık tutarlılığı kullanırken, veri kaybı hesabınızda yapılandırılan sınırlı bayatlık la sınırlıdır <br /><br />Bölgesel bir kesinti sırasında, birden çok bölgeyle güçlü tutarlılık yapılandırarak veri kaybını önleyebilirsiniz. Bu seçenek, kullanılabilirliği ve performansı etkileyen dengelerle birlikte gelir. Yalnızca tek bölgeli yazılar için yapılandırılan hesaplarda yapılandırılabilir. |
|Bölgesel kesinti – kullanılabilirlik | Kullanılabilirlik kaybı | Kullanılabilirlik kaybı | Kullanılabilirlik kaybı yok |
|Aktarım hızı | X RU/s sağlanan iş | X RU/s sağlanan iş | 2X RU/s sağlanan iş <br/><br/> Bu yapılandırma modu, iki bölge olduğundan, Kullanılabilirlik Bölgeleri olan tek bir bölgeyle karşılaştırıldığında iki kat fazla iş elde etme gerektirir. |

> [!NOTE]
> Çok bölgeli bir Azure Cosmos hesabı için Kullanılabilirlik Bölgesi desteğini etkinleştirmek için, hesabın çok büyük yazmaları etkinleştirilmiş olması gerekir.

Yeni veya varolan Azure Cosmos hesaplarına bölge eklerken bölge artıklığını etkinleştirebilirsiniz. Azure Cosmos hesabınızda bölge artıklığını etkinleştirmek `isZoneRedundant` `true` için bayrağı belirli bir konuma ayarlamanız gerekir. Bu bayrağı konumlar özelliği içinde ayarlayabilirsiniz. Örneğin, aşağıdaki powershell snippet "Güneydoğu Asya" bölgesi için bölge artıklığı sağlar:

```powershell
$locations = @(
    @{ "locationName"="Southeast Asia"; "failoverPriority"=0; "isZoneRedundant"= "true" },
    @{ "locationName"="East US"; "failoverPriority"=1 }
)
```

Aşağıdaki komut, "EastUS" ve "WestUS2" bölgeleri için bölge artıklığının nasıl etkinleştirilen gösteriş olduğunu gösterir:

```azurecli-interactive
az cosmosdb create \
  --name mycosmosdbaccount \
  --resource-group myResourceGroup \
  --kind GlobalDocumentDB \
  --default-consistency-level Session \
  --locations regionName=EastUS failoverPriority=0 isZoneRedundant=True \
  --locations regionName=WestUS2 failoverPriority=1 isZoneRedundant=True
```

Azure Cosmos hesabı oluştururken Azure portalını kullanarak Kullanılabilirlik Bölgeleri'ni etkinleştirebilirsiniz. Bir hesap oluşturduğunuzda, Coğrafi **artıklığı**, Çok **bölgeli Yazmaları**etkinleştirdiğinizden emin olun ve Kullanılabilirlik Bölgeleri'nin desteklendiği bir bölge seçin:

![Azure portalını kullanarak Kullanılabilirlik Bölgelerini Etkinleştirme](./media/high-availability/enable-availability-zones-using-portal.png) 

## <a name="building-highly-available-applications"></a>Yüksek kullanılabilir uygulamalar oluşturma

- Yüksek yazma ve okuma kullanılabilirliği sağlamak için Cosmos hesabınızı birden çok yazma bölgesine sahip en az iki bölgeye yayılacak şekilde yapılandırın. Bu yapılandırma, SLA'lar tarafından desteklenen hem okuma hem de yazma için en yüksek kullanılabilirlik, en düşük gecikme ve en iyi ölçeklenebilirlik sağlar. Daha fazla bilgi edinmek için [Cosmos hesabınızı birden çok yazma bölgesiyle nasıl yapılandırılatırılabilirsiniz.](tutorial-global-distribution-sql-api.md)

- Tek yazma lı bir bölgeyle yapılandırılan çok bölgeli Cosmos hesapları için [Azure CLI veya Azure portalı kullanılarak otomatik olarak başarısız](how-to-manage-database-account.md#automatic-failover)olmasını etkinleştirin. Otomatik arızayı etkinleştirdikten sonra, bölgesel bir felaket olduğunda, Cosmos DB otomatik olarak hesabınızda başarısız olur.  

- Cosmos hesabınız yüksek oranda kullanılabilir olsa bile, uygulamanız yüksek oranda kullanılabilir kalacak şekilde doğru şekilde tasarlanamayabilir. Uygulama testinizin veya olağanüstü durum kurtarma (DR) matkaplarınızın bir parçası olarak, uygulamanızın uçtan uca yüksek kullanılabilirliğini test etmek için, hesap için otomatik arızayı geçici olarak devre dışı kalarak, [Azure CLI veya Azure portalını kullanarak manuel başarısızlığı](how-to-manage-database-account.md#manual-failover)başlatın ve ardından uygulamanızın başarısız olmasını izleyin. Tamamlandıktan sonra, birincil bölgeye geri başarısız olabilir ve hesap için otomatik hata yı geri yükleyebilirsiniz.

- Genel olarak dağıtılan bir veritabanı ortamında, bölge çapında bir kesintinin varlığında tutarlılık düzeyi ile veri dayanıklılığı arasında doğrudan bir ilişki vardır. İş sürekliliği planınızı geliştirirken, uygulamanın rahatsız edici bir olaydan sonra tamamen iyileşmesi için kabul edilebilir maksimum zamanı anlamanız gerekir. Bir uygulamanın tam olarak kurtarılması için gereken süre, kurtarma süresi hedefi (RTO) olarak bilinir. Ayrıca, uygulamanın yıkıcı bir olaydan sonra kurtarma yaparken kaybetmeyi tolere edebileceği en son veri güncelleştirmelerinin en uzun süresini anlamanız gerekir. Kaybetmeyi göze alabileceğiniz güncelleştirme süresi kurtarma noktası hedefini (RPO) olarak bilinir. Azure Cosmos DB için RPO ve RTO'yu görmek için [Tutarlılık düzeylerine ve veri dayanıklılığına](consistency-levels-tradeoffs.md#rto) bakın

## <a name="next-steps"></a>Sonraki adımlar

Daha sonra aşağıdaki makaleleri okuyabilirsiniz:

- [Çeşitli tutarlılık düzeyleri için kullanılabilirlik ve performans dengeleri](consistency-levels-tradeoffs.md)
- [Sağlanan aktarım hızını küresel olarak ölçeklendirme](scaling-throughput.md)
- [Genel dağıtım - başlık altında](global-dist-under-the-hood.md)
- [Azure Cosmos DB'de tutarlılık düzeyleri](consistency-levels.md)
- [Cosmos hesabınızı birden çok yazma bölgesiyle yapılandırma](how-to-multi-master.md)
