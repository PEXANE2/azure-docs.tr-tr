---
title: Azure SQL yönetilen örnek havuzu nedir?
titleSuffix: Azure SQL Managed Instance
description: Daha küçük SQL Server veritabanlarını ölçekli buluta geçirmek için kullanışlı ve uygun maliyetli bir yol sağlayan Azure SQL yönetilen örnek havuzları (Önizleme) hakkında bilgi edinin ve birden çok SQL yönetilen örneği yönetin.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: bonova
ms.author: bonova
ms.reviewer: sstein, carlrab
ms.date: 09/05/2019
ms.openlocfilehash: 21ac62f78a8eb599fb0e2c86d41e6452365b80b8
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84045154"
---
# <a name="what-is-an-azure-sql-managed-instance-pool-preview"></a>Azure SQL yönetilen örnek Havuzu (Önizleme) nedir?
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Azure SQL yönetilen örneğindeki örnek havuzlar, daha küçük SQL örneklerini buluta ölçeklendirmenin uygun ve ekonomik bir yolunu sunar.

Örnek havuzları toplam geçiş gereksinimlerinize göre işlem kaynaklarını önceden sağlamanıza olanak tanır. Daha sonra, önceden sağlanmış işlem düzeyinize kadar birkaç ayrı SQL yönetilen örneği dağıtabilirsiniz. Örneğin, 8 sanal çekirdekleri önceden sağlarsanız iki 2 sanal çekirdek ve bir 4 sanal çekirdek örneği dağıtabilir ve ardından veritabanlarını bu örneklere geçirebilirsiniz. Örnek havuzlarından önce, daha küçük ve daha az işlem yoğunluklu iş yükleri, buluta geçiş yaparken genellikle daha büyük bir yönetilen örneğe birleştirilmelidir. Veritabanı gruplarının büyük bir örneğe geçirilmesi gerekir, genellikle dikkatli kapasite planlama ve kaynak İdaresi, ek güvenlik konuları ve bazı ek veri birleştirme örnek düzeyinde çalışır.

Ayrıca, örnek havuzlar aynı alt ağda birden çok örnek havuzu ve birden çok tek örneği dağıtabilmeniz için yerel VNet tümleştirmesini destekler.

## <a name="key-capabilities"></a>Temel işlevler

Örnek havuzlar aşağıdaki avantajları sağlar:

1. 2 sanal çekirdek örnekleri barındırma özelliği. * \* Yalnızca örnek havuzlardaki örnekler için*.
2. Öngörülebilir ve hızlı örnek dağıtım süresi (5 dakikaya kadar).
3. En az IP adresi ayırması.

Aşağıdaki diyagramda, bir sanal ağ alt ağı içinde dağıtılan birden çok SQL yönetilen örneği olan bir örnek havuzu gösterilmektedir.

![birden çok örneğe sahip örnek havuzu](./media/instance-pools-overview/instance-pools1.png)

Örnek havuzlar, sanal makinenin işlem boyutunun, havuz için ayrılan toplam sanal çekirdek sayısına bağlı olarak aynı sanal makinede birden çok örneğinin dağıtılmasını sağlar. Bu mimari, sanal makinenin birden çok örneğe *bölümlenmesini* sağlar. Bu, 2 sanal çekirdek (2 sanal çekirdek örnekleri yalnızca havuzlardaki örneklerde kullanılabilir) dahil olmak üzere, desteklenen bir boyut olabilir.

İlk dağıtımdan sonra, havuzdaki örneklerde bulunan yönetim işlemleri çok daha hızlıdır. Bunun nedeni, bir [sanal kümenin](connectivity-architecture-overview.md#high-level-connectivity-architecture) dağıtılması veya uzantısının (adanmış sanal makine kümesi) yönetilen örneği sağlamanın bir parçası olmaması nedeniyle oluşur.

Bir havuzdaki tüm örnekler aynı sanal makineyi paylaştığından, Toplam IP ayırması dağıtılan örnek sayısına bağlı değildir, bu da dar IP aralığına sahip alt ağlarda dağıtım için uygundur.

Her havuzun yalnızca dokuz IP adresi sabit bir IP ayırması vardır (alt ağda, kendi ihtiyaçları için ayrılan beş IP adresi dahil değildir). Ayrıntılar için bkz. [tek örnekler için alt ağ boyutu gereksinimleri](vnet-subnet-determine-size.md).

## <a name="application-scenarios"></a>Uygulama senaryoları

Aşağıdaki listede, örnek havuzlarının göz önünde bulundurulması gereken başlıca kullanım durumları verilmiştir:

- *BIR SQL örnekleri grubunun* , çoğunluğu daha küçük bir boyut (örneğin, 2 veya 4 sanal çekirdek) olduğu bir kez geçirilmesi.
- *Öngörülebilir ve kısa örnek oluşturma veya ölçeklendirmenin* önemli olduğu senaryolar. Örneğin, yeni bir kiracının örnek düzeyi yetenekler gerektiren çok kiracılı SaaS uygulama ortamında dağıtılması.
- *Sabit maliyetli* veya *harcama limitine* sahip olan senaryolar önemli olur. Örneğin, bir sabit (veya seyrek değişen) boyutun paylaşılan geliştirme-test veya tanıtım ortamları çalıştırmak, gerektiğinde yönetilen örnekleri düzenli olarak dağıtırsınız.
- VNet alt ağında *en az IP adresi ayırmanın* önemli olduğu senaryolar. Bir havuzdaki tüm örnekler bir sanal makineyi paylaşıyor, bu nedenle ayrılan IP adreslerinin sayısı tek örnek olması durumunda daha düşük.

## <a name="architecture"></a>Mimari

Örnek havuzlarının normal (*tek*) yönetilen örneklere benzer bir mimarisi vardır.  [Azure sanal ağları (VNet) içindeki dağıtımları](../../virtual-network/virtual-network-for-azure-services.md)desteklemek   ve müşterilere yalıtım ve güvenlik sağlamak için, örnek havuzları [sanal kümelere](connectivity-architecture-overview.md#high-level-connectivity-architecture)da güvenir. Sanal kümeler, müşterinin sanal ağ alt ağı içinde dağıtılan ayrılmış bir yalıtılmış sanal makine kümesini temsil eder.

İki dağıtım modeli arasındaki temel fark, örnek havuzların [Windows Iş nesneleri](https://docs.microsoft.com/windows/desktop/ProcThread/job-objects)kullanılarak yönetilen aynı sanal makine düğümünde birden çok SQL Server işlem dağıtımına izin verişleridir, tek örnekler her zaman bir sanal makine düğümünde yer alır.

Aşağıdaki diyagramda, aynı alt ağda dağıtılan bir örnek havuz ve iki tekil örnek gösterilmektedir ve her iki dağıtım modeli için de ana mimari ayrıntıları gösterilmektedir:

![örnek havuzu ve iki tekil örnek](./media/instance-pools-overview/instance-pools2.png)

Her örnek havuz, altında ayrı bir sanal küme oluşturur. Bir havuz içindeki örnekler ve aynı alt ağda dağıtılan tek örnekler, performans öngörülebilirlik sağlayan SQL Server işlemlerine ve ağ geçidi bileşenlerine ayrılan işlem kaynaklarını paylaşmaz.

## <a name="resource-limitations"></a>Kaynak sınırlamaları

Örnek havuzlarıyla ve havuzların içindeki örneklerle ilgili çeşitli kaynat sınırlamaları vardır:

- Örnek havuzları yalnızca 5. nesil donanımında kullanılabilir.
- Bir havuz içindeki SQL yönetilen örnekleri ayrılmış CPU ve RAM 'e sahiptir, bu nedenle tüm örneklerde toplanan sanal çekirdek sayısı, havuza ayrılan Vçekirdeklerin sayısından küçük veya bu sayıya eşit olmalıdır.
- Tüm [örnek düzeyi sınırları](resource-limits.md#service-tier-characteristics) , bir havuz içinde oluşturulan örneklere uygulanır.
- Örnek düzeyi sınırlarına ek olarak, *örnek havuzu düzeyinde*uygulanan iki sınır da vardır:
  - Havuz başına toplam depolama boyutu (8 TB).
  - Havuz başına toplam veritabanı sayısı (100).

Tüm örneklerin tamamında toplam depolama alanı ayırma ve veritabanı sayısı, örnek havuzlarının açığa çıkarılan sınırlara eşit veya daha düşük olmalıdır.

- Örnek havuzları 8, 16, 24, 32, 40, 64 ve 80 sanal çekirdekleri destekler.
- Havuzlar içindeki yönetilen örnekler 2, 4, 8, 16, 24, 32, 40, 64 ve 80 sanal çekirdekleri destekler.
- Havuzlar içindeki yönetilen örnekler, şunlar dışında 32 GB ile 8 TB arasında depolama boyutlarını destekler:
  - 2 sanal çekirdek örneği 32 GB ile 640 GB arasındaki boyutları destekler
  - 4 sanal çekirdek örneği 32 GB ile 2 TB arasında boyutları destekler

[Hizmet katmanı özelliği](resource-limits.md#service-tier-characteristics) örnek havuzu kaynağıyla ilişkilendirilir, bu nedenle havuzdaki tüm örneklerin havuzun hizmet katmanıyla aynı hizmet katmanı olması gerekir. Şu anda yalnızca Genel Amaçlı hizmet katmanı kullanılabilir (geçerli Önizlemedeki sınırlamalar hakkında aşağıdaki bölüme bakın).

### <a name="public-preview-limitations"></a>Genel önizleme sınırlamaları

Genel Önizleme aşağıdaki sınırlamalara sahiptir:

- Şu anda yalnızca Genel Amaçlı hizmet katmanı kullanılabilir.
- Örnek havuzlar genel önizleme sırasında ölçeklendirilemez, böylece dağıtım önemli olduğundan dikkatli bir kapasite planlaması yapmanız önerilir.
- Örnek havuzu oluşturma ve yapılandırma için Azure portal desteği henüz kullanılamıyor. Örnek havuzlardaki tüm işlemler yalnızca PowerShell aracılığıyla desteklenir. Önceden oluşturulmuş havuzda ilk örnek dağıtımı yalnızca PowerShell üzerinden de desteklenir. Bir havuza dağıtıldıktan sonra, SQL yönetilen Örnekler Azure portal kullanılarak güncelleştirilebilecek.
- Havuzun dışında oluşturulan SQL yönetilen örnekleri mevcut bir havuza taşınamaz ve bir havuz içinde oluşturulan örnekler, tek bir örnek veya başka bir havuz olarak dışına taşınamaz.
- [Ayrılan kapasite](../database/reserved-capacity-overview.md) örneği fiyatlandırması kullanılamıyor.

## <a name="sql-features-supported"></a>Desteklenen SQL özellikleri

Havuzlarda oluşturulan SQL yönetilen örnekleri, [tek yönetilen örneklerde desteklenen uyumluluk düzeylerini ve özellikleri](sql-managed-instance-paas-overview.md#sql-features-supported)destekler.

Bir havuzda dağıtılan her SQL yönetilen örneği SQL aracısının ayrı bir örneğine sahiptir.

Belirli değerleri (örneğin, örnek düzeyi harmanlama, saat dilimi, veri trafiği için ortak uç nokta, yük devretme grupları) seçmenizi gerektiren isteğe bağlı özellikler veya özellikler, örnek düzeyinde yapılandırılır ve bir havuzdaki her örnek için farklı olabilir.

## <a name="performance-considerations"></a>Performansla ilgili önemli noktalar

Havuzlardaki SQL yönetilen örnekleri ayrılmış sanal çekirdek ve RAM 'e sahip olsa da, yerel disk (tempdb kullanımı için) ve ağ kaynakları paylaşır. Büyük olasılıkla, havuzda birden çok örnek aynı anda yüksek kaynak tüketimine sahip olursa *gürültülü komşu* efektini denemek mümkündür. Bu davranışı gözlemlerseniz, bu örnekleri daha büyük bir havuza veya tek örnek olarak dağıtmaya göz önünde bulundurun.

## <a name="security-considerations"></a>Güvenlikle ilgili dikkat edilmesi gerekenler

Bir havuzda dağıtılan örnekler aynı sanal makineyi paylaştığından, daha yüksek güvenlik riskleri sunan özellikleri devre dışı bırakmayı veya bu özelliklere yönelik erişim izinlerini güvenli bir şekilde kontrol etmek isteyebilirsiniz. Örneğin, CLR tümleştirmesi, yerel yedekleme ve geri yükleme, veritabanı e-postası vb.

## <a name="instance-pool-support-requests"></a>Örnek havuzu destek istekleri

[Azure Portal](https://portal.azure.com)örnek havuzları için destek istekleri oluşturun ve yönetin.

Örnek havuzu dağıtımıyla ilgili sorunlar yaşıyorsanız (oluşturma veya silme), **sorun alt türü** alanında **örnek havuzlar** belirttiğinizden emin olun.

![örnek havuzları destek isteği](./media/instance-pools-overview/support-request.png)

Tek SQL yönetilen örneklerle veya bir havuz içindeki veritabanlarıyla ilgili sorunlar yaşıyorsanız, Azure SQL yönetilen örnekleri için normal bir destek bileti oluşturmanız gerekir.

Daha büyük SQL yönetilen örnek dağıtımları oluşturmak için (örnek havuzlarla birlikte veya olmadan), daha büyük bir bölgesel kota edinmeniz gerekebilir. Daha fazla bilgi için bkz. [Azure SQL veritabanı Için istek kotası artışları](../database/quota-increase-request.md). Örnek havuzlarının dağıtım mantığı, kotayı daha fazla artırmadan yeni kaynaklar oluşturmanıza izin verilip verilmeyeceğini öğrenmek için havuz düzeyindeki toplam vCore tüketimini *havuzunuzla* karşılaştırır.

## <a name="instance-pool-billing"></a>Örnek havuzu Faturalaması

Örnek havuzları, işlem ve depolamayı bağımsız olarak ölçeklendirmeye olanak tanır. Müşteriler, sanal çekirdekler ile ölçülen havuz kaynağıyla ilişkili işlem için ve gigabayt cinsinden ölçülen her örnekle ilişkili depolama alanı için ödeme yapar (her örnek için ilk 32 GB ücretsizdir).

Havuzun sanal çekirdek fiyatı, bu havuzda kaç örnek dağıtıldığına bakılmaksızın ücretlendirilir.

Işlem fiyatı (sanal çekirdekler cinsinden ölçülür) için, iki fiyatlandırma seçeneği mevcuttur:

  1. *Lisans dahil*: SQL lisansı fiyatları dahildir. Bu, Yazılım Güvencesi kapsamındaki mevcut SQL Server lisanslarını uygulamalarını seçen müşteriler içindir.
  2. *Azure hibrit avantajı*: SQL Server için Azure hibrit avantajı içeren daha düşük bir fiyat. Müşteriler, Yazılım Güvencesi kapsamındaki mevcut SQL Server lisanslarını kullanarak bu fiyatı kabul edebilir. Uygunluk ve diğer ayrıntılar için bkz. [Azure hibrit avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/).

Bir havuzdaki tek tek örnekler için farklı fiyatlandırma seçeneklerinin ayarlanması mümkün değildir. Üst havuzdaki tüm örnekler, lisans dahil fiyattan veya Azure Hibrit Avantajı fiyattan olmalıdır. Havuzun lisans modeli, havuz oluşturulduktan sonra değiştirilebilir.

> [!IMPORTANT]
> Havuzu için havuzdan farklı olan bir lisans modeli belirtirseniz, havuz fiyatı kullanılır ve örnek düzeyi değeri yok sayılır.

[Geliştirme ve test avantajına uygun abonelikler](https://azure.microsoft.com/pricing/dev-test/)üzerinde örnek havuzları oluşturursanız, Azure SQL yönetilen örneği 'nde yüzde 55 ' ye varan indirimli ücretler alırsınız.

Örnek havuzu fiyatlandırmasıyla ilgili tam Ayrıntılar için, [SQL yönetilen örnek fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/sql-database/managed/) *örnek havuzlar* bölümüne bakın.

## <a name="next-steps"></a>Sonraki adımlar

- Örnek havuzlarla çalışmaya başlamak için bkz. [SQL yönetilen örnek havuzları nasıl yapılır Kılavuzu](instance-pools-configure.md).
- İlk SQL yönetilen örneğinizi oluşturmayı öğrenmek için bkz. [Hızlı Başlangıç Kılavuzu](instance-create-quickstart.md).
- Özellikler ve karşılaştırma listesi için bkz. [SQL ortak özellikleri](../database/features-comparison.md).
- VNet yapılandırması hakkında daha fazla bilgi için bkz. [SQL yönetilen örnek VNET yapılandırması](connectivity-architecture-overview.md).
- SQL yönetilen örneği oluşturan ve bir veritabanını bir yedekleme dosyasından geri yükleyen hızlı başlangıç için bkz. [SQL yönetilen örneği oluşturma](instance-create-quickstart.md).
- Geçiş için Azure veritabanı geçiş hizmeti 'ni (DMS) kullanan bir öğretici için bkz. [DMS kullanarak SQL yönetilen örnek geçişi](../../dms/tutorial-sql-server-to-managed-instance.md).
- Yerleşik sorun giderme zekası ile SQL yönetilen örnek veritabanı performansının gelişmiş izlenmesi için bkz. [Azure SQL Analytics kullanarak Azure SQL yönetilen örneğini izleme](../../azure-monitor/insights/azure-sql.md).
- Fiyatlandırma bilgileri için bkz. [SQL veritabanı yönetilen örnek fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/managed/).
