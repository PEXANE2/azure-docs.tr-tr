---
title: Örnek havuzları (önizleme)
description: Bu makalede, Azure SQL Veritabanı örnek havuzları (önizleme) açıklanmaktadır.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: bonova
ms.author: bonova
ms.reviewer: sstein, carlrab
ms.date: 09/05/2019
ms.openlocfilehash: c1e740fbfa4bf1e8a77a2d9d6060ab39dba7ae7b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79256217"
---
# <a name="what-are-sql-database-instance-pools-preview"></a>SQL Veritabanı örnek havuzları (önizleme) nedir?

Örnek havuzları, Azure SQL Veritabanı'nda küçük SQL örneklerini ölçekte buluta geçirmek için kullanışlı ve uygun maliyetli bir yol sağlayan yeni bir kaynaktır.

Örnek havuzları toplam geçiş gereksinimlerinize göre işlem kaynaklarını önceden sağlamanıza olanak tanır. Ardından, önceden sağlanan işlem düzeyinize ulaşana kadar tek tek yönetilen örnekleri dağıtabilirsiniz. Örneğin, 8 vCore ön sağlama ederseniz, iki 2 vCore ve bir 4 vCore örneği dağıtabilir ve ardından veritabanlarını bu örneklere geçirebilirsiniz. Örnek havuzların kullanılabilir hale getirilmeden önce, daha küçük ve daha az işlem yoğun iş yüklerinin buluta geçiş yaparken genellikle daha büyük yönetilen bir örnekte birleştirilmesi gerekir. Veritabanları gruplarını büyük bir örneğe geçirme gereksinimi genellikle dikkatli kapasite planlaması ve kaynak yönetimi, ek güvenlik konuları ve bazı ekstra veri birleştirme çalışmaları gerektirir.

Ayrıca, örnek havuzları yerel VNet tümleştirmesini destekler, böylece aynı alt ağda birden çok örnek havuzu ve birden çok tek örnek dağıtabilirsiniz.


## <a name="key-capabilities-of-instance-pools"></a>Örnek havuzlarının temel özellikleri

Örnek havuzları aşağıdaki avantajları sağlar:

1. 2 vCore örneğini barındırma yeteneği. *Yalnızca örnek havuzdaki örnekler için. \**
2. Öngörülebilir ve hızlı örnek dağıtım süresi (en fazla 5 dakika).
3. Minimum IP adresi tahsisi.

Aşağıdaki diyagram, sanal ağ alt ağında birden çok örneğin dağıtılmış olduğu bir örnek havuzunu göstermektedir.

![birden çok örnekiçeren örnek havuzu](./media/sql-database-instance-pools/instance-pools1.png)

Örnek havuzları, sanal makinenin bilgi işlem boyutunun havuz için ayrılan toplam vCore sayısına dayandığı aynı sanal makinede birden çok örneğin dağıtılmasını sağlar. Bu mimari, sanal makinenin 2 vCore (2 vCore örnekleri yalnızca havuzdaki örnekler için kullanılabilir) dahil olmak üzere desteklenen herhangi bir boyut alabilen birden çok örne *bölünmesine* olanak tanır.

Havuzdaki örneklerdeki yönetim işlemleri, havuz ilk dağıtıldıktan sonra çok daha hızlıdır. [Sanal kümenin](sql-database-managed-instance-connectivity-architecture.md#high-level-connectivity-architecture) (özel sanal makine kümesi) dağıtımı veya uzantısı yönetilen örneği sağlamanın bir parçası olmadığından, bu işlemler daha hızlıdır.

Havuzdaki tüm örnekler aynı sanal makineyi paylaştığından, toplam IP ayırması dağıtılan örnek sayısına bağlı değildir ve bu da dar IP aralığına sahip alt ağlarda dağıtım için uygundur.

Her havuzda yalnızca dokuz IP adresi (alt ağda kendi ihtiyaçları için ayrılmış beş IP adresi hariç) sabit bir IP ayırması vardır. Ayrıntılar [için, tek bir örnek için alt ağ boyutu gereksinimlerine](sql-database-managed-instance-determine-size-vnet-subnet.md)bakın.

## <a name="application-scenarios-for-instance-pools"></a>Örneğin havuzlar için uygulama senaryoları

Aşağıdaki liste, örnek havuzlarının dikkate alınması gereken ana kullanım örneklerini sağlar:

- Çoğunluğun daha küçük boyut (örneğin 2 veya 4 vCores) olduğu aynı anda *bir grup SQL örneği* geçişi.
- *Öngörülebilir ve kısa örnek oluşturma veya ölçeklemenin* önemli olduğu senaryolar. Örneğin, yeni bir kiracının örnek düzey yetenekleri gerektiren çok kiracılı Bir SaaS uygulama ortamında dağıtımı.
- Sabit bir *maliyet* veya *harcama sınırına* sahip olmak önemli senaryolar. Örneğin, gerektiğinde yönetilen örnekleri düzenli aralıklarla dağıttığınız sabit (veya seyrek değişen) boyutta paylaşılan geliştirme testi veya demo ortamlarını çalıştırmak.
- VNet alt netinde *en az IP adresi tahsisinin* önemli olduğu senaryolar. Havuzdaki tüm örnekler sanal bir makineyi paylaşır, bu nedenle ayrılan IP adreslerinin sayısı tek örnektekinden daha düşüktür.


## <a name="architecture-of-instance-pools"></a>Örnek havuzlarının mimarisi

Örnek havuzları normal yönetilen örneklere *(tek örnek)* benzer mimariye sahiptir. Azure [Sanal Ağlar (VNets)](../virtual-network/virtual-network-for-azure-services.md#deploy-azure-services-into-virtual-networks) içindeki dağıtımları desteklemek ve müşteriler için yalıtım ve güvenlik sağlamak için örnek havuzları da [sanal kümelere](sql-database-managed-instance-connectivity-architecture.md#high-level-connectivity-architecture)güvenir. Sanal kümeler, müşterinin sanal ağ alt ağında dağıtılan özel bir yalıtılmış sanal makine kümesini temsil eder.

İki dağıtım modeli arasındaki temel fark, örnek [havuzlarının, Windows İş Nesneleri](https://docs.microsoft.com/windows/desktop/ProcThread/job-objects)kullanılarak yönetilen aynı sanal makine düğümünde birden çok SQL Server işlem dağıtımına izin verirken, tek örnekler sanal makine düğümünde her zaman tek başına dır.

Aşağıdaki diyagram, aynı alt ağda dağıtılan bir örnek havuzunu ve iki ayrı örneği gösterir ve her iki dağıtım modelinin ana mimari ayrıntılarını gösterir:

![örnek havuzu ve iki ayrı örnek](./media/sql-database-instance-pools/instance-pools2.png)

Her örnek havuzu altında ayrı bir sanal küme oluşturur. Havuzdaki örnekler ve aynı alt ağda dağıtılan tek örnekler, SQL Server işlemlerine ve ağ geçidi bileşenlerine ayrılan bilgi işlem kaynaklarını paylaşmaz, bu performans öngörülebilirliği sağlar.

## <a name="instance-pools-resource-limitations"></a>Örnek havuzlar kaynak sınırlamaları

Örnek havuzlarıyla ve havuzların içindeki örneklerle ilgili çeşitli kaynat sınırlamaları vardır:

- Örnek havuzları yalnızca Gen5 donanımında kullanılabilir.
- Havuzdaki örneklerde özel CPU ve RAM vardır, bu nedenle tüm örneklerde toplanan vCore sayısı havuza ayrılan vCore sayısından daha az veya eşit olmalıdır.
- Tüm [örnek düzeyi sınırları](sql-database-managed-instance-resource-limits.md#service-tier-characteristics) havuz içinde oluşturulan örnekler için geçerlidir.
- Örnek düzey sınırlarına ek *olarak, örnek havuz düzeyinde*de iki sınır vardır:
  - Havuz başına toplam depolama boyutu (8 TB).
  - Havuz başına toplam veritabanı sayısı (100).

Tüm örneklerdeki toplam depolama alanı ayırma ve veritabanlarının sayısı, örnek havuzlarıtarafından maruz kalan sınırlara daha düşük veya eşit olmalıdır.

- Örnek havuzları 8, 16, 24, 32, 40, 64 ve 80 vCores'u destekler.
- Havuzlar içinde yönetilen örnekler 2, 4, 8, 16, 24, 32, 40, 64 ve 80 vCores destekler.
- Havuzlar içinde yönetilen örnekler, 32 GB ile 8 TB arasındaki depolama boyutlarını destekler:
  - 2 vCore örneği 32 GB ile 640 GB arasındaki boyutları destekler
  - 4 vCore örneği 32 GB ile 2 TB arasındaki boyutları destekler

[Hizmet katmanı özelliği](sql-database-managed-instance-resource-limits.md#service-tier-characteristics) örnek havuz kaynağıyla ilişkilidir, bu nedenle havuzdaki tüm örnekler havuzun hizmet katmanıyla aynı hizmet katmanı olmalıdır. Şu anda yalnızca Genel Amaçlı hizmet katmanı kullanılabilir (geçerli önizlemedeki sınırlamalar ile ilgili aşağıdaki bölüme bakın).

### <a name="public-preview-limitations"></a>Genel önizleme sınırlamaları

Genel önizlemeaşağıdaki sınırlamaları vardır:

- Şu anda yalnızca Genel Amaçlı hizmet katmanı kullanılabilir.
- Örnek havuzları genel önizleme sırasında ölçeklendirilemez, bu nedenle dağıtım önemli olmadan önce dikkatli kapasite planlaması yapılır.
- Örneğin havuz oluşturma ve yapılandırma için Azure portal desteği henüz kullanılamıyor. Örnek havuzdaki tüm işlemler yalnızca PowerShell üzerinden desteklenir. Önceden oluşturulmuş bir havuzda ilk örnek dağıtımı yalnızca PowerShell üzerinden de desteklenir. Bir havuza dağıtıldıktan sonra yönetilen örnekler Azure portalı kullanılarak güncellenebilir.
- Havuzun dışında oluşturulan yönetilen örnekler varolan bir havuza taşınamaz ve havuz içinde oluşturulan örnekler tek bir örnek olarak veya başka bir havuza taşınamaz.
- Ayrılmış örnek fiyatlandırması (lisans dahil veya Azure Karma Avantajı ile birlikte) kullanılamaz.

## <a name="sql-features-supported"></a>Desteklenen SQL özellikleri

Havuzlarda oluşturulan örnekler, [aynı uyumluluk düzeylerini ve tek yönetilen örneklerde desteklenen özellikleri](sql-database-managed-instance.md#sql-features-supported)destekler.

Bir havuzda dağıtılan her yönetilen örnek, ayrı bir SQL Agent örneği vardır.

Belirli değerleri seçmeniz gereken isteğe bağlı özellikler veya özellikler (örneğin düzey harmanlama, saat dilimi, veri trafiği için genel bitiş noktası, başarısız gruplar gibi) örnek düzeyinde yapılandırılır ve havuzdaki her örnek için farklı olabilir.

## <a name="performance-considerations"></a>Performansla ilgili önemli noktalar

Havuzlar içinde yönetilen örnekleri özel vCore ve RAM olmasına rağmen, yerel disk (tempdb kullanımı için) ve ağ kaynaklarını paylaşır. Bu olası değildir, ancak havuzda birden fazla örnek aynı anda yüksek kaynak tüketimi varsa *gürültülü komşu* etkisi yaşamak mümkündür. Bu davranışı gözlemlerseniz, bu örnekleri daha büyük bir havuza veya tek örnek olarak dağıtmayı düşünün.

## <a name="security-considerations"></a>Güvenlik konuları

Havuzda dağıtılan örnekler aynı sanal makineyi paylaştığından, daha yüksek güvenlik riskleri ne kadar yüksek güvenlik riskleri doğuran özellikleri devre dışı bırakmayı veya bu özelliklere erişim izinlerini sıkı bir şekilde denetlemeyi düşünebilirsiniz. Örneğin, CLR entegrasyonu, yerel yedekleme ve geri yükleme, veritabanı e-posta, vb.

## <a name="instance-pool-support-requests"></a>Örnek havuz destek istekleri

[Azure portalındaki](https://portal.azure.com)örneğin havuzları için destek istekleri oluşturun ve yönetin.

Örnek havuzu dağıtımıyla (oluşturma veya silme) ilgili sorunlar yaşıyorsanız, **Sorun alt türü** alanında Örnek **Havuzları** belirttiğinden emin olun.

![örnek havuzları destek isteği](./media/sql-database-instance-pools/support-request.png)

Havuzdaki tek örnekler veya veritabanlarıyla ilgili sorunlar yaşıyorsanız, Azure SQL Veritabanı yönetilen örnekleri için düzenli bir destek bileti oluşturmanız gerekir.

Daha büyük yönetilen örnek dağıtımları oluşturmak için (örnek havuzları olsun veya olmasın), daha büyük bir bölgesel kota almanız gerekebilir. Daha fazla bilgi için Azure [SQL Veritabanı için İstek kotası artışlarına](quota-increase-request.md)bakın. Örnek havuzları kullanıyorsanız, dağıtım mantığı, kotanızı daha fazla artırmadan yeni kaynaklar oluşturmanıza izin verilip verilmediğini belirlemek için *havuz düzeyindeki* toplam vCore tüketimini kotanızla karşılaştırır.

## <a name="instance-pool-billing"></a>Örnek havuz faturalandırma

Örnek havuzları, hesaplama ve depolamayı bağımsız olarak ölçekleme sağlar. Müşteriler vCores cinsinden ölçülen havuz kaynağı yla ilişkili bilgi işlem için ödeme ve gigabaytlarla ölçülen her örnekle ilişkili depolama (ilk 32 GB her örnek için ücretsizdir).

bir havuz için vCore fiyatı, bu havuzda kaç örnek dağıtıldıklarına bakılmaksızın ücretlendirilir.

İşlem fiyatı (vCorecinsinden ölçülen) için iki fiyatlandırma seçeneği mevcuttur:

  1. *Lisans dahil*: SQL lisanslarının fiyatı dahildir. Bu, Yazılım Güvencesi ile varolan SQL Server lisanslarını uygulamamayı seçen müşteriler içindir.
  2. *Azure Karma Avantajı*: SQL Server için Azure Karma Avantajı içeren indirimli fiyat. Müşteriler, Yazılım Güvencesi ile mevcut SQL Server lisanslarını kullanarak bu fiyatı tercih edebilirler. Uygunluk ve diğer ayrıntılar için [Azure Karma Avantajı'na](https://azure.microsoft.com/pricing/hybrid-benefit/)bakın.

Havuzdaki tek tek örnekler için farklı fiyatlandırma seçenekleri ayarlamak mümkün değildir. Üst havuzdaki tüm örnekler, Lisans Dahil fiyatıveya Azure Karma Avantajı fiyatında olmalıdır. Havuz oluşturulduktan sonra havuzun lisans modeli değiştirilebilir.

> [!IMPORTANT]
> Havuzdakinden farklı bir örnek için bir Lisans Modeli belirtirseniz, havuz fiyatı kullanılır ve örnek düzeyi değeri yoksayılır.

[Geliştirme testi avantajına uygun aboneliklerde](https://azure.microsoft.com/pricing/dev-test/)örnek havuzları oluşturursanız, Azure SQL yönetilen örneğinde otomatik olarak yüzde 55'e varan indirimli fiyatlar alırsınız.

Örnek havuzu fiyatlandırması hakkında tüm ayrıntılar için, [yönetilen örnek fiyatlandırma sayfasındaki](https://azure.microsoft.com/pricing/details/sql-database/managed/)örnek *havuzları* bölümüne bakın.

## <a name="next-steps"></a>Sonraki adımlar

- Örnek havuzları ile başlamak için, [SQL Veritabanı örnek havuzları nasıl yapılır kılavuzuna](sql-database-instance-pools-how-to.md)bakın.
- Yönetilen ilk örneğini nasıl oluşturabilirsiniz öğrenmek için [Quickstart kılavuzuna](sql-database-managed-instance-get-started.md)bakın.
- Özellikler ve karşılaştırma listesi için [SQL ortak özelliklerine](sql-database-features.md)bakın.
- VNet yapılandırması hakkında daha fazla bilgi için [yönetilen örnek VNet](sql-database-managed-instance-connectivity-architecture.md)yapılandırması'na bakın.
- Yönetilen bir örnek oluşturan ve bir yedekleme dosyasından veritabanını geri yükleyen hızlı [bir](sql-database-managed-instance-get-started.md)başlangıç için bkz.
- Geçiş için Azure Veritabanı Geçiş Hizmeti'ni (DMS) kullanan bir öğretici için, [Yönetilen Örnek Geçişi'ne bakın.](../dms/tutorial-sql-server-to-managed-instance.md)
- Yerleşik sorun giderme zekasıyla yönetilen örnek veritabanı performansının gelişmiş izlenmesi için Azure [SQL Analytics'i kullanarak Azure SQL Veritabanını Izleyin](../azure-monitor/insights/azure-sql.md)bölümüne bakın.
- Fiyatlandırma bilgileri için, [SQL Veritabanı yönetilen örnek fiyatlandırma](https://azure.microsoft.com/pricing/details/sql-database/managed/)bakın.
