---
title: Yüksek kullanılabilirlik
description: Azure SQL Veritabanı hizmeti nin yüksek kullanılabilirlik özellikleri ve özellikleri hakkında bilgi edinin
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: sashan
ms.author: sashan
ms.reviewer: carlrab, sashan
ms.date: 04/02/2020
ms.openlocfilehash: 1c4ed77112e8c06db1946d756239e02cb187f3ef
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80618457"
---
# <a name="high-availability-and-azure-sql-database"></a>Yüksek kullanılabilirlik ve Azure SQL Veritabanı

Azure SQL Veritabanı'ndaki Yüksek Kullanılabilirlik mimarisinin amacı, veritabanınızın en az %99,99 oranında çalışır durumda olduğunu garanti etmektir (Farklı katmanlar için belirli SLA ile ilgili daha fazla bilgi için lütfen, bakım işlemlerinin ve kesintilerin etkisinden endişe etmeden [Azure SQL Veritabanı için SLA'ya](https://azure.microsoft.com/support/legal/sla/sql-database/)bakın. Azure, yama, yedekleme, Windows ve SQL yükseltmeleri gibi kritik hizmet görevlerinin yanı sıra temel donanım, yazılım veya ağ hataları gibi planlanmamış olayları otomatik olarak işler.  Altta yatan SQL örneği yamalı veya başarısız olduğunda, uygulamanızda [yeniden deneme mantığı kullanırsanız,](sql-database-develop-overview.md#resiliency) kesinti süresi fark edilmez. Azure SQL Veritabanı, verilerinizin her zaman kullanılabilir olmasını sağlayan en kritik durumlarda bile hızlı bir şekilde kurtarılabilir.

Yüksek kullanılabilirlik çözümü, taahhüt edilen verilerin hatalar nedeniyle asla kaybedilmemesini, bakım işlemlerinin iş yükünüzü etkilememesini ve veritabanının yazılım mimarinizde tek bir hata noktası olmamasını sağlamak için tasarlanmıştır. Veritabanı yükseltilirken veya korunurken iş yükünü durdurmanızı gerektirecek bakım pencereleri veya kapalı saatler yoktur. 

Azure SQL Veritabanı'nda kullanılan iki yüksek kullanılabilirlikli mimari model vardır:

- İşlem ve depolamanın ayrılmasını temel alan standart kullanılabilirlik modeli.  Uzak depolama katmanının yüksek kullanılabilirliğine ve güvenilirliğine dayanır. Bu mimari, bakım faaliyetleri sırasında bazı performans bozulmalarını tolere edebilen bütçe odaklı iş uygulamalarını hedefler.
- Veritabanı altyapısı işlemleri kümesini temel alan premium kullanılabilirlik modeli. Her zaman kullanılabilir veritabanı altyapı düğümleri bir çoğunluk olduğu gerçeğine dayanır. Bu mimari, yüksek IO performansı, yüksek işlem oranı ile kritik uygulamaları hedefler ve bakım faaliyetleri sırasında iş yükünüz üzerinde en az performans etkisini garanti eder.

Azure SQL Veritabanı, SQL Server Database Engine ve Windows OS'nin en son kararlı sürümünde çalışır ve çoğu kullanıcı yükseltmelerin sürekli olarak gerçekleştirildiğini fark etmez.

## <a name="basic-standard-and-general-purpose-service-tier-availability"></a>Temel, Standart ve Genel Amaçlı hizmet katmanı kullanılabilirliği

Bu hizmet katmanları standart kullanılabilirlik mimarisinden yararlanır. Aşağıdaki şekil, ayrılmış işlem ve depolama katmanları ile dört farklı düğümleri gösterir.

![İşlem ve depolamanın ayrılması](media/sql-database-high-availability/general-purpose-service-tier.png)

Standart kullanılabilirlik modeli iki katmaniçerir:

- `sqlservr.exe` İşlemi çalıştıran ve TempDB, ekteki SSD'deki örnek veritabanları ve bellekte önbellek, arabellek havuzu ve sütun deposu havuzu gibi yalnızca geçici ve önbelleğe alınmış veriler içeren durumsuz bir bilgi işlem katmanı. Bu stateless düğüm, başharfe `sqlservr.exe`basılan, düğümün durumunu denetleyen ve gerekirse başka bir düğümde başarısız olan Azure Hizmet Dokusu tarafından işletilmektedir.
- Azure Blob depolama alanında depolanan veritabanı dosyaları (.mdf/.ldf) içeren özel bir veri katmanı. Azure blob depolama yerleşik veri kullanılabilirliği ve artıklık özelliğine sahiptir. SQL Server işlemi çökse bile, veri dosyasındaki veya sayfadaki her kaydın korunacağını garanti eder.

Veritabanı altyapısı veya işletim sistemi yükseltildiğinde veya bir hata algılandığında, Azure Service Fabric, devletsiz SQL Server işlemini yeterli boş kapasiteye sahip başka bir durumsuz bilgi işlem düğümüne taşır. Azure Blob depolama sundaki veriler taşınmadan etkilenmez ve veri/günlük dosyaları yeni başlatılan SQL Server işlemine eklenir. Bu işlem %99,99 kullanılabilirliği garanti eder, ancak yeni SQL Server örneğinin soğuk önbellekle başlamasından bu yana geçiş sırasında bazı performans düşükleri görülebilir.

## <a name="premium-and-business-critical-service-tier-availability"></a>Premium ve Business Critical hizmet katmanı kullanılabilirliği

Premium ve Business Critical hizmet katmanları, tek bir düğümüzerinde bilgi işlem kaynakları (SQL Server Database Engine process) ve depolamayı (yerel olarak bağlı SSD) entegre eden Premium kullanılabilirlik modelinden yararlanır. Yüksek kullanılabilirlik, üç ila dört düğümlü küme oluşturarak hem bilgi işlem hem de depolamayı ek düğümlere kopyalayarak elde edilir. 

![Veritabanı altyapı düğümleri kümesi](media/sql-database-high-availability/business-critical-service-tier.png)

Altta yatan veritabanı dosyaları (.mdf/.ldf), iş yükünüze çok düşük gecikmeli IO sağlamak için ekli SSD depolama alanına yerleştirilir. Yüksek kullanılabilirlik, SQL Server Always [On Availability Groups'a](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server)benzer bir teknoloji kullanılarak uygulanır. Küme, okuma yazma müşteri iş yükleri için erişilebilir tek bir birincil yineleme (SQL Server işlemi) ve veri kopyalarını içeren en fazla üç ikincil yineleme (bilgi işlem ve depolama) içerir. Birincil düğüm, ikincil düğümdeki değişiklikleri sürekli olarak sırayla iter ve her hareketi gerçekleştirmeden önce verilerin en az bir ikincil yinelemeyle eşitlenmesine izin verilir. Bu işlem, birincil düğüm herhangi bir nedenle çökerse, her zaman üzerinde başarısız olmak için tam senkronize düğüm olduğunu garanti eder. Hata, Azure Hizmet Kumaşı tarafından başlatılır. İkincil yineleme yeni birincil düğüm haline geldikten sonra, kümenin yeterli düğüme (çoğunluk kümesi) sahip olduğundan emin olmak için başka bir ikincil yineleme oluşturulur. Başarısız bir sonraki hata tamamlandıktan sonra, SQL bağlantıları otomatik olarak yeni birincil düğüme yönlendirilir.

Ek bir avantaj olarak, premium kullanılabilirlik modeli salt okunur SQL bağlantılarını ikincil yinelemelerden birine yönlendirme olanağını içerir. Bu özellik [Ölçekoku-Out](sql-database-read-scale-out.md)olarak adlandırılır. It provides 100% additional compute capacity at no extra charge to off-load read-only operations, such as analytical workloads, from the primary replica.

## <a name="hyperscale-service-tier-availability"></a>Hiper ölçekli hizmet katmanı kullanılabilirliği

Hyperscale hizmet katmanı mimarisi [Dağıtılmış işlevler mimarisinde](https://docs.microsoft.com/azure/sql-database/sql-database-service-tier-hyperscale#distributed-functions-architecture)açıklanmıştır. 

![Hiper ölçekli fonksiyonel mimari](./media/sql-database-hyperscale/hyperscale-architecture.png)

Hyperscale'deki kullanılabilirlik modeli dört katmandan oluşur:

- `sqlservr.exe` İşlemleri çalıştıran ve bağlı SSD'de rbpex önbelleği, TempDB, model veritabanı vb. gibi yalnızca geçici ve önbelleğe alınmış verileri içeren ve bellekte önbellek, arabellek havuzu ve sütun deposu havuzunu planlayan durumsuz bir bilgi işlem katmanı. Bu durum suz katman birincil işlem yineleme ve isteğe bağlı olarak başarısız hedefler olarak hizmet verebilir ikincil işlem yinelemeleri bir dizi içerir.
- Sayfa sunucuları tarafından oluşturulan durumsuz bir depolama katmanı. Bu katman, işlem yinelemeleri `sqlservr.exe` üzerinde çalışan işlemler için dağıtılmış depolama motorudur. Her sayfa sunucusu, ekli SSD'deki RBPEX önbelleğini ve bellekte önbelleğe alınmış veri sayfalarını kaplamak gibi yalnızca geçici ve önbelleğe alınmış veriler içerir. Her sayfa sunucusu, yük dengeleme, artıklık ve yüksek kullanılabilirlik sağlamak için etkin bir yapılandırmada eşleştirilmiş bir sayfa sunucusuna sahiptir.
- Günlük hizmet işlemini çalıştıran bilgi işlem düğümü, hareket günlüğü iniş bölgesi ve işlem günlüğü uzun vadeli depolama tarafından oluşturulan durumlu bir hareket günlüğü depolama katmanı. İniş bölgesi ve uzun süreli depolama alanı, işlem günlüğü için kullanılabilirlik ve [artıklık](https://docs.microsoft.com/azure/storage/common/storage-redundancy) sağlayan ve taahhüt edilen hareketler için veri dayanıklılığını sağlayan Azure Depolama'yı kullanır.
- Azure Depolama'da depolanan ve sayfa sunucuları tarafından güncelleştirilen veritabanı dosyalarının (.mdf/.ndf) bulunduğu özel bir veri depolama katmanı. Bu katman, Azure Depolama'nın veri kullanılabilirliği ve [artıklık](https://docs.microsoft.com/azure/storage/common/storage-redundancy) özelliklerini kullanır. Bir veri dosyasındaki her sayfa, Hyperscale mimarisinin diğer katmanlarındaki işlemler kilitlense veya bilgi işlem düğümleri başarısız olsa bile korunacağını garanti eder.

Tüm Hiper ölçek katmanlarındaki hesaplama düğümleri, her düğümün durumunu denetleyen ve gerektiğinde kullanılabilir sağlıklı düğümlere başarısız olan Azure Hizmet Kumaşı'nda çalışır.

Hyperscale'de yüksek kullanılabilirlik hakkında daha fazla bilgi için [Hyperscale'de Veritabanı Yüksek Kullanılabilirliği'ne](https://docs.microsoft.com/azure/sql-database/sql-database-service-tier-hyperscale#database-high-availability-in-hyperscale)bakın.

## <a name="zone-redundant-configuration"></a>Bölge yedekli yapılandırma

Varsayılan olarak, premium kullanılabilirlik modeli için düğüm kümesi aynı veri merkezinde oluşturulur. [Azure Kullanılabilirlik Bölgeleri'nin](../availability-zones/az-overview.md)piyasaya sürülmesiyle, SQL Veritabanı İş Açısından Kritik veritabanının farklı yinelemelerini aynı bölgedeki farklı kullanılabilirlik bölgelerine yerleyebilir. Tek bir hata noktasını ortadan kaldırmak için, denetim halkası da birden çok bölge arasında üç ağ geçidi halkası (GW) olarak çoğaltılır. Belirli bir ağ geçidi halkasına [yönlendirme, Azure Trafik Yöneticisi](../traffic-manager/traffic-manager-overview.md) (ATM) tarafından denetlenir. Premium veya Business Critical hizmet katmanlarındaki bölge gereksiz yapılandırması ek veritabanı artıklığı oluşturmadığından, bunu ek ücret ödemeden etkinleştirebilirsiniz. Bölge gereksiz yapılandırmasını seçerek, Premium veya İş Açısından Kritik veritabanlarınızı uygulama mantığında herhangi bir değişiklik yapmadan, felaket veri merkezi kesintileri de dahil olmak üzere çok daha büyük bir hata kümesine karşı esnek hale getirebilirsiniz. Ayrıca, varolan Premium veya İş Açısından Kritik veritabanlarını veya havuzları bölge gereksiz yapılandırmasına da dönüştürebilirsiniz.

Bölge yedekli veritabanları arasında bir miktar mesafe olan farklı veri merkezlerinde yinelemeler olduğundan, artan ağ gecikmesi işleme süresini artırabilir ve böylece bazı OLTP iş yüklerinin performansını etkileyebilir. Bölge artıklığı ayarını devre dışı bırakarak her zaman tek bölgeli yapılandırmaya dönebilirsiniz. Bu işlem, normal hizmet katmanı yükseltmesine benzer bir çevrimiçi işlemdir. İşlemin sonunda, veritabanı veya havuz bir bölge gereksiz halkadan tek bir bölge halkasına veya tam tersi geçirilir.

> [!IMPORTANT]
> Bölge yedekveritabanları ve esnek havuzlar şu anda yalnızca belirli bölgelerdeki Premium ve İş Açısından Kritik hizmet katmanlarında desteklenir. İş Açısından Kritik katmanı nı kullanırken, bölge gereksiz yapılandırması yalnızca Gen5 işlem donanımı seçildiğinde kullanılabilir. Bölge gereksiz veritabanlarını destekleyen bölgeler le ilgili güncel bilgiler için, [bölgeye göre Hizmetler desteğine](../availability-zones/az-overview.md#services-support-by-region)bakın.  
> Bu özellik Yönetilen örnekte kullanılamaz.

Yüksek kullanılabilirlik mimarisinin bölge yedekli sürümü aşağıdaki diyagramla gösterilmiştir:

![yüksek kullanılabilirlik mimari bölge yedekli](./media/sql-database-high-availability/zone-redundant-business-critical-service-tier.png)

## <a name="accelerated-database-recovery-adr"></a>Hızlandırılmış Veritabanı Kurtarma (ADR)

[Hızlandırılmış Veritabanı Kurtarma (ADR),](sql-database-accelerated-database-recovery.md) özellikle uzun süren işlemlerin varlığında veritabanı kullanılabilirliğini büyük ölçüde iyileştiren yeni bir SQL veritabanı altyapısı özelliğidir. ADR şu anda tek veritabanları, elastik havuzlar ve Azure SQL Veri Ambarı için kullanılabilir.

## <a name="testing-application-fault-resiliency"></a>Uygulama hatası dayanıklılığını test etme

Yüksek kullanılabilirlik, veritabanı uygulamanız için saydam olarak çalışan Azure SQL Veritabanı platformunun temel bir parçasıdır. Ancak, planlı veya planlanmamış olaylar sırasında başlatılan otomatik başarısız operasyonların, siz üretime dağıtmadan önce uygulamayı nasıl etkileyeceğini test etmek isteyebileceğinibiliyoruz. Bir veritabanını veya elastik bir havuzu yeniden başlatmak için özel bir API çağırabilirsiniz, bu da bir hata başlatmayı tetikler. Bölge gereksiz veritabanı veya elastik havuz söz konusu olduğunda, API çağrısı istemci bağlantılarını eski birincil kullanılabilirlik bölgesinden farklı bir Kullanılabilirlik Bölgesi'ndeki yeni birincil birincile yönlendirmesine neden olur. Bu nedenle, failover'ın varolan veritabanı oturumlarını nasıl etkilediğini test etmeye ek olarak, ağ gecikmesindeki değişiklikler nedeniyle uçlardan uca performansı değiştirip değiştirmedığını da doğrulayabilirsiniz. Yeniden başlatma işlemi müdahaleci olduğundan ve bunların çok sayıda platformu stres olabilir, sadece bir failover arama her 30 dakikada bir veritabanı veya elastik havuz için izin verilir. 

REST API veya PowerShell kullanılarak bir hata başlatılabilir. REST API için Veritabanı [failover](https://docs.microsoft.com/rest/api/sql/databases(failover)/failover) ve [Elastic pool failover'a](https://docs.microsoft.com/rest/api/sql/elasticpools(failover)/failover)bakın. PowerShell için [Invoke-AzSqlDatabaseFailover](https://docs.microsoft.com/powershell/module/az.sql/invoke-azsqldatabasefailover) ve [Invoke-AzSqlElasticPoolFailover'e](https://docs.microsoft.com/powershell/module/az.sql/invoke-azsqlelasticpoolfailover)bakın. REST API [aramaları, az rest](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest#az-rest) komutu kullanılarak Azure CLI'den de yapılabilir.

> [!IMPORTANT]
> Failover komutu şu anda Hyperscale hizmet katmanında ve Yönetilen Örnek için kullanılamıyor.

## <a name="conclusion"></a>Sonuç

Azure SQL Veritabanı, Azure platformuyla derinden entegre edilmiş yerleşik yüksek kullanılabilirlik çözümüne sahiptir. Hata algılama ve kurtarma için Service Fabric'e, veri koruması için Azure Blob depolamasına ve daha yüksek hata toleransı için Kullanılabilirlik Bölgeleri'ne bağlıdır. Buna ek olarak, Azure SQL veritabanı çoğaltma ve başarısızlık için SQL Server'dan Her Zaman Kullanılabilirlik Grubu teknolojisini kullanır. Bu teknolojilerin birleşimi, uygulamaların karma depolama modelinin avantajlarını tam olarak fark etmesini ve en zorlu SLA'ları desteklemesini sağlar.

## <a name="next-steps"></a>Sonraki adımlar

- Azure [Kullanılabilirlik Bölgeleri](../availability-zones/az-overview.md) hakkında bilgi edinin
- Servis [Kumaşı](../service-fabric/service-fabric-overview.md) hakkında bilgi edinin
- Azure [Trafik Yöneticisi](../traffic-manager/traffic-manager-overview.md) hakkında bilgi edinin
- Yüksek kullanılabilirlik ve olağanüstü durum kurtarma için daha fazla seçenek için [İş Sürekliliği'ne](sql-database-business-continuity.md) bakın
