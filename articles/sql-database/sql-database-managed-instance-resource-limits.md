---
title: Kaynak sınırları - yönetilen örnek
description: Bu makalede, yönetilen örnekler için Azure SQL Veritabanı kaynak sınırlarına genel bir bakış sağlanmaktadır.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: bonova
ms.author: bonova
ms.reviewer: carlrab, jovanpop, sachinp, sstein
ms.date: 02/25/2020
ms.openlocfilehash: b2871ec87e4d7f337c26b3ff3de83c1c3c88aea2
ms.sourcegitcommit: 07d62796de0d1f9c0fa14bfcc425f852fdb08fb1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80365388"
---
# <a name="overview-azure-sql-database-managed-instance-resource-limits"></a>Genel Bakış Azure SQL Veritabanı yönetilen örnek kaynak sınırları

Bu makalede, Azure SQL Veritabanı yönetilen örnek için teknik özellikleri ve kaynak sınırları genel bir bakış sağlar ve bu sınırlar için bir artış istemek hakkında bilgi sağlar.

> [!NOTE]
> Desteklenen özelliklerdeki ve T-SQL deyimleri arasındaki farklar için [özellik farklılıkları](sql-database-features.md) ve [T-SQL deyimi desteğine](sql-database-managed-instance-transact-sql-information.md)bakın. Tek bir veritabanındaki hizmet katmanları ile yönetilen örnek arasındaki genel farklar için [Hizmet katmanı karşılaştırması'na](sql-database-service-tiers-general-purpose-business-critical.md#service-tier-comparison)bakın.

## <a name="hardware-generation-characteristics"></a>Donanım oluşturma özellikleri

Yönetilen örnek, temel altyapı ve mimariye bağlı özellikleri ve kaynak sınırları vardır. Azure SQL Veritabanı yönetilen örnek iki donanım nesilleri dağıtılabilir: Gen4 ve Gen5. Donanım nesillerini aşağıdaki tabloda açıklandığı gibi farklı özelliklere sahiptir:

|   | **Gen4** | **Gen5** |
| --- | --- | --- |
| Donanım | Intel E5-2673 v3 (Haswell) 2.4-GHz işlemciler, ekli SSD vCore = 1 PP (fiziksel çekirdek) | Intel E5-2673 v4 (Broadwell) 2.3-GHz ve Intel SP-8160 (Skylake) işlemciler, hızlı NVMe SSD, vCore=1 LP (hiper iş parçacığı) |
| Sanal çekirdek sayısı | 8, 16, 24 vCores | 4, 8, 16, 24, 32, 40, 64, 80 vCores |
| Maksimum bellek (bellek/çekirdek oranı) | vCore başına 7 GB<br/>Daha fazla bellek almak için daha fazla vCores ekleyin. | vCore başına 5,1 GB<br/>Daha fazla bellek almak için daha fazla vCores ekleyin. |
| Max Bellek IÇI OLTP bellek | Örnek limiti: vCore başına 1-1,5 GB| Örnek limiti: vCore başına 0,8 - 1,65 GB |
| Maksimum örnek ayrılmış depolama |  Genel Amaç: 8 TB<br/>İş Kritik: 1 TB | Genel Amaç: 8 TB<br/> İş Kritik 1 TB, 2 TB veya 4 TB çekirdek sayısına bağlı olarak |

> [!IMPORTANT]
> - Gen4 donanımı aşamalı olarak devre dışı ediliyor ve yeni dağıtımlar için artık kullanılamıyor. Tüm yeni yönetilen örnekler Gen5 donanımında dağıtılmalıdır.
> - Daha geniş bir vCore ve depolama ölçeklenebilirliği, hızlandırılmış ağ, en iyi IO performansı ve minimum gecikme süresini deneyimlemek için [yönetilen örneklerinizi Gen 5](sql-database-service-tiers-vcore.md) donanımına taşımayı düşünün.

### <a name="in-memory-oltp-available-space"></a>Bellek içi OLTP kullanılabilir alan 

[Business Critical](sql-database-service-tier-business-critical.md) hizmet katmanındaki bellek içi OLTP alanı nın miktarı vCore'ların ve donanım oluşturma sayısına bağlıdır. Aşağıdaki tabloda Bellek OLTP nesneleri için kullanılabilecek bellek sınırları listelenmiştir.

| Bellek içi OLTP alanı  | **Gen5** | **Gen4** |
| --- | --- | --- |
| 4 sanal çekirdek  | 3.14 GB | |   
| 8 sanal çekirdek  | 6,28 GB | 8 GB |
| 16 vCores | 15,77 GB | 20 GB |
| 24 vCores | 25,25 GB | 36 GB |
| 32 vCores | 37,94 GB | |
| 40 vCores | 52,23 GB | |
| 64 vCores | 99.9 GB    | |
| 80 vCores | 131,68 GB| |

## <a name="service-tier-characteristics"></a>Hizmet katmanı özellikleri

Yönetilen örneğin iki hizmet katmanı vardır: [Genel Amaç](sql-database-service-tier-general-purpose.md) ve İş [Açısından Kritik.](sql-database-service-tier-business-critical.md) Bu katmanlar, aşağıdaki tabloda açıklandığı gibi [farklı yetenekler](sql-database-service-tiers-general-purpose-business-critical.md)sağlar.

> [!Important]
> İş Açısından Kritik hizmet katmanı, salt okunur iş yükü için kullanılabilecek ek yerleşik örnek kopyası (ikincil yineleme) sağlar. Okuma-yazma sorgularını ve salt okunur/analitik/raporlama sorgularını birbirinden ayırabiliyorsanız, aynı fiyata iki kez vCore saves ve bellek elde elabilirsiniz. İkincil yineleme birincil örneğin birkaç saniye gerisinde kalabilir, bu nedenle tam geçerli veri durumuna ihtiyaç duymadan raporlama/analitik iş yükünü boşaltmak üzere tasarlanmıştır. Aşağıdaki tabloda, **salt sorguları** ikincil yinelemede yürütülen sorgular vardır.

| **Özellik** | **Genel Amaç** | **İş Kritik** |
| --- | --- | --- |
| Sanal çekirdek sayısı\* | Gen4: 8, 16, 24<br/>Gen5: 4, 8, 16, 24, 32, 40, 64, 80 | Gen4: 8, 16, 24 <br/> Gen5: 4, 8, 16, 24, 32, 40, 64, 80 <br/>\*Aynı sayıda vCore yalnızca okunur sorgular için ayrılmıştır. |
| Maksimum bellek | Gen4: 56 GB - 168 GB (7GB/vCore)<br/>Gen5: 20,4 GB - 408 GB (5,1 GB/vCore)<br/>Daha fazla bellek almak için daha fazla vCores ekleyin. | Gen4: 56 GB - 168 GB (7GB/vCore)<br/>Gen5: okuma yazma sorguları için 20,4 GB - 408 GB (5,1 GB/vCore)<br/>+ yalnızca okunan sorgular için ek 20,4 GB - 408 GB (5,1 GB/vCore).<br/>Daha fazla bellek almak için daha fazla vCores ekleyin. |
| Maksimum örnek depolama boyutu (ayrılmış) | - 4 vCores için 2 TB (Sadece Gen5)<br/>- Diğer boyutlar için 8 TB | Gen4: 1 TB <br/> Gen5: <br/>- 4, 8, 16 vCores için 1 TB<br/>- 24 vCores için 2 TB<br/>- 32, 40, 64, 80 vCores için 4 TB |
| Maks. veritabanı boyutu | Şu anda mevcut örnek boyutuna kadar (vCores sayısına bağlı olarak max 2 TB - 8 TB). | Şu anda kullanılabilir örnek boyutuna kadar (vCores sayısına bağlı olarak max 1 TB - 4 TB). |
| Maksimum tempDB boyutu | 24 GB/vCore (96 - 1.920 GB) ile sınırlıdır ve şu anda kullanılabilir örnek depolama boyutu.<br/>Daha fazla TempDB alanı elde etmek için daha fazla vCores ekleyin.<br/> Günlük dosya boyutu 120 GB ile sınırlıdır.| Şu anda kullanılabilir örnek depolama boyutuna kadar. |
| Örnek başına en fazla veritabanları sayısı | Örnek depolama boyutu sınırına ulaşılmadığı sürece 100. | Örnek depolama boyutu sınırına ulaşılmadığı sürece 100. |
| Örnek başına en fazla veritabanı dosyası sayısı | Örnek depolama boyutuna veya [Azure Premium Disk depolama alanı](sql-database-release-notes.md#exceeding-storage-space-with-small-database-files) sınırına ulaşılmadığı sürece 280'e kadar. | Örnek depolama boyutu sınırına ulaşılmadığı sürece veritabanı başına 32.767 dosya. |
| Maksimum veri dosyası boyutu | Şu anda kullanılabilen örnek depolama boyutu (max 2 TB - 8 TB) ve [Azure Premium Disk depolama alanı](sql-database-release-notes.md#exceeding-storage-space-with-small-database-files)ile sınırlıdır. | Şu anda kullanılabilir örnek depolama boyutuyla sınırlıdır (en fazla 1 TB - 4 TB). |
| Maksimum günlük dosya boyutu | 2 TB ile sınırlıdır ve şu anda kullanılabilir örnek depolama boyutu. | 2 TB ile sınırlıdır ve şu anda kullanılabilir örnek depolama boyutu. |
| Veri/Günlük IOPS (yaklaşık) | Örnek başına 30-40 K IOPS'ye kadar*, dosya başına 500 - 7500<br/>\*[Daha fazla IOPS almak için dosya boyutunu artırın](#file-io-characteristics-in-general-purpose-tier)| 10 K - 200 K (2500 IOPS/vCore)<br/>Daha iyi IO performansı elde etmek için daha fazla vCore ekleyin. |
| Günlük yazma iş tarihi sınırı (örnek başına) | VCore başına 3 MB/sn<br/>Maksimum 22 MB/sn | VCore başına 4 MB/sn<br/>Maksimum 48 MB/sn |
| Veri iş tartışımı (yaklaşık) | Dosya başına 100 - 250 MB/sn<br/>\*[Daha iyi IO performansı elde etmek için dosya boyutunu artırın](#file-io-characteristics-in-general-purpose-tier) | Sınırlı değil. |
| Depolama IO gecikmesi (yaklaşık) | 5-10 ms | 1-2 ms |
| Hafıza içi OLTP | Desteklenmiyor | Kullanılabilir, [boyutu vCore sayısına bağlıdır](#in-memory-oltp-available-space) |
| Maksimum seanslar | 30000 | 30000 |
| [Salt okunur yinelemeler](sql-database-read-scale-out.md) | 0 | 1 (fiyata dahil) |

> [!NOTE]
> - **Şu anda kullanılabilir örnek depolama boyutu,** ayrılmış örnek boyutu ile kullanılan depolama alanı arasındaki farktır.
> - Hem veri hem de kullanıcı ve sistem veritabanlarındaki günlük dosya boyutu, Max depolama boyutu sınırıyla karşılaştırılan örnek depolama boyutuna dahildir. Veritabanları tarafından kullanılan toplam alanı belirlemek için <a href="https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-master-files-transact-sql">sys.master_files</a> sistem görünümünü kullanın. Hata günlükleri kalıcı değildir ve boyuta dahil edilmez. Yedeklemeler depolama boyutuna dahil edilmez.
> - Genel Amaçlı katmandaki iş ortası ve IOPS, yönetilen örnekle açıkça sınırlandırılamayan [dosya boyutuna](#file-io-characteristics-in-general-purpose-tier) da bağlıdır.
> - Otomatik başarısız grupları kullanarak farklı Azure bölgesinde okunabilir başka bir yineleme oluşturabilirsiniz.
> - Max örnek IOPS dosya düzeni ve iş yükü dağıtımına bağlıdır. Örnek olarak, her biri 500 IOPS'ye sahip 7 x 1TB dosya ve 500 IOPS'ye sahip 7 küçük dosya (128 GB'dan küçük) oluşturursanız, iş yükünüz tüm dosyaları kullanabiliyorsa, her örnek başına 38500 IOPS (7x500+7x500) alabilirsiniz. Bir miktar IOPS'nin otomatik yedeklemeler için de kullanıldığını unutmayın.

> [!NOTE]
> [Bu makalede yönetilen örnek havuzlarında kaynak sınırları](sql-database-instance-pools.md#instance-pools-resource-limitations)hakkında daha fazla bilgi bulabilirsiniz.

### <a name="file-io-characteristics-in-general-purpose-tier"></a>Genel Amaçlı katmanda Dosya IO özellikleri

Genel Amaçlı hizmet katmanında her veritabanı dosyası, dosya boyutuna bağlı olarak özel IOPS ve iş elde edilir. Daha büyük dosyalar daha Fazla IOPS ve iş elde alıyorsanız. Veritabanı dosyalarının IO özellikleri aşağıdaki tabloda gösterilmiştir:

| Dosya boyutu | >=0 ve <=128 GiB | >128 ve <=256 GİB | >256 ve <= 512 GİB | >0,5 ve <=1 TiB    | >1 ve <=2 TiB    | >2 ve <=4 TiB | >4 ve <=8 TiB |
|---------------------|-------|-------|-------|-------|-------|-------|-------|
| Dosya başına IOPS       | 500   | 1100 | 2300              | 5000              | 7.500              | 7.500              | 12,500   |
| Dosya başına iş başı | 100 MiB/s | 125 MiB/s | 150 MiB/s | 200 MiB/s | 250 MiB/sn | 250 MiB/sn | 480 MiB/s | 

Bazı veritabanı dosyasında yüksek IO gecikmesi fark ederseniz veya IOPS/iş başına geçmenin sınıra ulaştığını görürseniz, [dosya boyutunu artırarak](https://techcommunity.microsoft.com/t5/Azure-SQL-Database/Increase-data-file-size-to-improve-HammerDB-workload-performance/ba-p/823337)performansı artırabilirsiniz.

Max log yazma iş başına 22 MB/s gibi örnek düzey sınırları da vardır, bu nedenle örnek iş ortası sınırına ulaştığınız için günlük dosyasında dosyaya erişemeyebilirsiniz.

## <a name="supported-regions"></a>Desteklenen bölgeler

Yönetilen örnekler yalnızca desteklenen [bölgelerde](https://azure.microsoft.com/global-infrastructure/services/?products=sql-database&regions=all)oluşturulabilir. Şu anda desteklenmeyen bir bölgede yönetilen bir örnek oluşturmak [için, Azure portalı üzerinden bir destek isteği gönderebilirsiniz.](quota-increase-request.md)

## <a name="supported-subscription-types"></a>Desteklenen abonelik türleri

Yönetilen örnek şu anda yalnızca aşağıdaki abonelik türlerinde dağıtımı destekler:

- [Kurumsal Anlaşma (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/)
- [Öde-as-you-go](https://azure.microsoft.com/offers/ms-azr-0003p/)
- [Bulut Hizmet Sağlayıcısı (CSP)](https://docs.microsoft.com/partner-center/csp-documents-and-learning-resources)
- [Kurumsal Geliştirme ve Test](https://azure.microsoft.com/offers/ms-azr-0148p/)
- [Kullandıkça Öde Geliştirme ve Test](https://azure.microsoft.com/offers/ms-azr-0023p/)
- [Visual Studio aboneleri için aylık Azure kredisine sahip abonelikler](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/)

## <a name="regional-resource-limitations"></a>Bölgesel kaynak sınırlamaları

Desteklenen abonelik türleri bölge başına sınırlı sayıda kaynak içerebilir. Yönetilen örneğin Azure bölgesi başına iki varsayılan sınırı vardır (bir abonelik türüne bağlı olarak [Azure portalında](quota-increase-request.md) özel bir destek isteği oluşturarak isteğe bağlı olarak artırılabilir:

- **Alt ağ sınırı**: Yönetilen örneklerin tek bir bölgede dağıtıldığı maksimum alt ağ sayısıdır.
- **vCore birim sınırı**: Tek bir bölgedeki tüm örneklerde dağıtılabilen maksimum vCore birimi sayısıdır. Bir GP vCore bir vCore birim kullanır ve bir BC vCore 4 vCore birimleri alır. VCore birim sınırı içinde olduğu sürece toplam örnek sayısı sınırlı değildir.

> [!Note]
> Bu sınırlar varsayılan ayarlardır ve teknik sınırlamalar değildir. Geçerli bölgede daha fazla yönetilen örneğe ihtiyacınız varsa, Azure [portalında](quota-increase-request.md) özel bir destek isteği oluşturarak sınırlar isteğe bağlı olarak artırılabilir. Alternatif olarak, destek istekleri göndermeden başka bir Azure bölgesinde yeni yönetilen örnekler oluşturabilirsiniz.

Aşağıdaki tablo, desteklenen abonelik türleri için **varsayılan bölgesel sınırları** gösterir (varsayılan sınırlar aşağıda açıklanan destek isteği kullanılarak genişletilebilir):

|Abonelik türü| Yönetilen örnek alt ağlarının maksimum sayısı | Maksimum vCore birim sayısı* |
| :---| :--- | :--- |
|Öde-as-you-go|3|320|
|CSP |8 (Bazı bölgelerde 15**)|960 (Bazı bölgelerde 1440**)|
|Öde-as-you-go Dev/Test|3|320|
|Kurumsal Geliştirme ve Test|3|320|
|Ea|8 (Bazı bölgelerde 15**)|960 (Bazı bölgelerde 1440**)|
|Visual Studio Enterprise|2 |64|
|Visual Studio Professional ve MSDN Platformları|2|32|

\*Planlama dağıtımlarında, Business Critical (BC) hizmet katmanının Genel Amaç (GP) hizmet katmanından dört (4) kat daha fazla vCore kapasitesi gerektirdiğini lütfen göz önünde bulundurun. Örneğin: 1 GP vCore = 1 vCore birimi ve 1 BC vCore = 4 vCore birimi. Varsayılan sınırlara göre tüketim çözümlemenizi basitleştirmek için, yönetilen örneklerin dağıtıldığı bölgedeki tüm alt ağlardaki vCore birimlerini özetleyin ve sonuçları abonelik türünüz için örnek birim sınırlarıyla karşılaştırın. **Maksimum vCore birim sınırı,** bir bölgedeki her abonelik için geçerlidir. Birden çok alt ağ arasında dağıtılan tüm vCore'ların toplamının daha düşük veya **maksimum vCore birimi sayısına**eşit olması dışında tek tek alt ağlar başına bir sınır yoktur.

\*\*Daha büyük alt ağ ve vCore limitleri aşağıdaki bölgelerde mevcuttur: Avustralya Doğu, Doğu ABD, Doğu ABD 2, Kuzey Avrupa, Güney Orta ABD, Güneydoğu Asya, İngiltere Güney, Batı Avrupa, Batı ABD 2.

## <a name="request-a-quota-increase-for-sql-managed-instance"></a>SQL yönetilen örnek için kota artışı isteği

Geçerli bölgelerinizde daha fazla yönetilen örneğe ihtiyacınız varsa, Azure portalını kullanarak kotayı genişletmek için bir destek isteği gönderin. Daha fazla bilgi için Azure [SQL Veritabanı için İstek kotası artışlarına](quota-increase-request.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

- Yönetilen örnek hakkında daha fazla bilgi için [bkz.](sql-database-managed-instance.md)
- Fiyatlandırma bilgileri için, [SQL Veritabanı yönetilen örnek fiyatlandırma](https://azure.microsoft.com/pricing/details/sql-database/managed/)bakın.
- Yönetilen ilk örneğini nasıl oluşturabilirsiniz öğrenmek için [hızlı başlangıç kılavuzuna](sql-database-managed-instance-get-started.md)bakın.
