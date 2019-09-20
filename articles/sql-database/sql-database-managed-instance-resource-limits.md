---
title: Azure SQL veritabanı kaynak limitleri-yönetilen örnek | Microsoft Docs
description: Bu makalede yönetilen örnekler için Azure SQL veritabanı kaynak sınırlarına genel bir bakış sunulmaktadır.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: bonova
ms.author: bonova
ms.reviewer: carlrab, jovanpop, sachinp, sstein
ms.date: 09/16/2019
ms.openlocfilehash: d0356ff61ec8073e7fe69c3b09cbbdd8845fb787
ms.sourcegitcommit: 2ed6e731ffc614f1691f1578ed26a67de46ed9c2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2019
ms.locfileid: "71128905"
---
# <a name="overview-azure-sql-database-managed-instance-resource-limits"></a>Azure SQL veritabanı yönetilen örneği kaynak sınırlarına genel bakış

Bu makalede, Azure SQL veritabanı yönetilen örneği için teknik özelliklere ve kaynak sınırlarına genel bir bakış sağlanır ve bu sınırlara bir artış isteme hakkında bilgi sağlanır.

> [!NOTE]
> Desteklenen özellikler ve T-SQL deyimlerindeki farklar için bkz. [özellik farklılıkları](sql-database-features.md) ve [t-SQL deyimi desteği](sql-database-managed-instance-transact-sql-information.md). Tek veritabanı ve yönetilen örnekteki hizmet katmanları arasında genel farklıara için bkz. [hizmet katmanı karşılaştırması](sql-database-service-tiers-general-purpose-business-critical.md#service-tier-comparison).

## <a name="instance-level-resource-limits"></a>Örnek düzeyi kaynak sınırları

Yönetilen örnek, temel altyapıyı ve mimarisine bağlı olan özelliklere ve kaynak sınırlarına sahiptir. Sınırlar, donanım oluşturma ve hizmet katmanına bağlıdır.

### <a name="hardware-generation-characteristics"></a>Donanım oluşturma özellikleri

Azure SQL veritabanı yönetilen örneği iki donanım nesile dağıtılabilir: 4. nesil ve 5. nesil. Aşağıdaki tabloda açıklandığı gibi donanım nesilleri farklı özelliklere sahiptir:

|   | **4. nesil** | **5. nesil** |
| --- | --- | --- |
| Donanım | Intel E5-2673 v3 (Haswell) 2,4-GHz işlemciler, ekli SSD sanal çekirdek = 1 PP (fiziksel çekirdek) | Intel E5-2673 v4 (çok Iyi) 2,3-GHz işlemcileri, Fast NVMe SSD, sanal çekirdek = 1 LP (hiper iş parçacığı) |
| Sanal çekirdek sayısı | 8, 16, 24 sanal çekirdek | 4, 8, 16, 24, 32, 40, 64, 80 Vçekirdekler |
| Maksimum bellek (bellek/çekirdek oranı) | Sanal çekirdek başına 7 GB<br/>Daha fazla bellek almak için daha fazla sanal çekirdek ekleyin. | vCore başına 5,1 GB<br/>Daha fazla bellek almak için daha fazla sanal çekirdek ekleyin. |
| Maks. bellek Içi OLTP belleği | Örnek sınırı: Sanal çekirdek başına 3 GB<br/>Veritabanı limitleri:<br/> -8 çekirdek: veritabanı başına 8 GB<br/> -16 çekirdek: veritabanı başına 20 GB<br/> -24 çekirdek: veritabanı başına 36 GB | Örnek sınırı: vCore başına 2,5 GB<br/>Veritabanı limitleri:<br/> -8 çekirdek: veritabanı başına 13 GB<br/> -16 çekirdek: veritabanı başına 32 GB |
| En büyük örnek ayrılmış depolama alanı |  Genel Amaçlı: 8 TB<br/>İş Açısından Kritik: 1 | Genel Amaçlı: 8 TB<br/> Çekirdek sayısına bağlı olarak 1 TB, 2 TB veya 4 TB İş Açısından Kritik |

> [!IMPORTANT]
> - 4\. nesil donanım kullanıma alınıyor. 5. nesil donanımında yeni yönetilen örnekler dağıtmanız önerilir.
> - Şu anda 4. nesil donanım hala yalnızca aşağıdaki bölgelerde kullanılabilir: Kuzey Avrupa, Batı Avrupa, Doğu ABD, Orta Güney ABD, Orta Kuzey ABD, Batı ABD 2, Orta ABD, Kanada Orta, Güney Hindistan, Güneydoğu Asya ve Kore Orta.

### <a name="service-tier-characteristics"></a>Hizmet katmanı özellikleri

Yönetilen örnek iki hizmet katmanına sahiptir: [Genel amaçlı](sql-database-service-tier-general-purpose.md) ve [iş açısından kritik](sql-database-service-tier-business-critical.md). Bu katmanlar, aşağıdaki tabloda açıklandığı gibi [farklı yetenekler](sql-database-service-tiers-general-purpose-business-critical.md)sağlar:

| **Özelliği** | **Genel Amaçlı** | **İş Açısından Kritik** |
| --- | --- | --- |
| Sanal çekirdek sayısı\* | 4\. nesil 8, 16, 24<br/>5\. nesil 4, 8, 16, 24, 32, 40, 64, 80 | 4\. nesil 8, 16, 24 <br/> 5\. nesil 4, 8, 16, 24, 32, 40, 64, 80 |
| Maksimum bellek | 4\. nesil 56 GB-168 GB (7GB/sanal çekirdek)<br/>5\. nesil 20,4 GB-408 GB (5.1 GB/sanal çekirdek)<br/>Daha fazla bellek almak için daha fazla sanal çekirdek ekleyin. | 4\. nesil 56 GB-168 GB (7GB/sanal çekirdek)<br/>5\. nesil 20,4 GB-408 GB (5.1 GB/sanal çekirdek)<br/>Daha fazla bellek almak için daha fazla sanal çekirdek ekleyin. |
| En büyük örnek depolama boyutu (ayrılmış) | 4 sanal çekirdek için-2 TB (yalnızca 5. nesil)<br/>-8 TB diğer boyutlar için | 4\. nesil 1 TB <br/> 5\. nesil <br/>-1 TB, 4, 8, 16 sanal çekirdek<br/>-2 TB, 24 sanal çekirdek için<br/>-4 TB 32, 40, 64, 80 sanal çekirdekler |
| Maks. veritabanı boyutu | Şu anda kullanılabilir örnek boyutu (sanal çekirdek sayısına bağlı olarak en fazla 2 TB-8 TB). | Şu anda kullanılabilir örnek boyutu (sanal çekirdek sayısına bağlı olarak en fazla 1 TB-4 TB). |
| En fazla tempDB boyutu | 24 GB/vCore (96-1.920 GB) ile sınırlıdır ve şu anda kullanılabilir örnek boyutu.<br/>Daha fazla TempDB alanı almak için daha fazla sanal çekirdek ekleyin. | Şu anda kullanılabilir örnek boyutu. TempDB günlük dosyası boyutu şu anda 24 GB/sanal çekirdek ile sınırlıdır. |
| Örnek başına en fazla veritabanı sayısı | 100, örnek depolama boyutu sınırına ulaşılmadığı müddetçe. | 100, örnek depolama boyutu sınırına ulaşılmadığı müddetçe. |
| Örnek başına en fazla veritabanı dosyası sayısı | Örnek depolama boyutu veya [Azure Premium disk depolama ayırma alanı](sql-database-managed-instance-transact-sql-information.md#exceeding-storage-space-with-small-database-files) sınırına ulaşılmadığı takdirde 280 'e kadar. | örnek depolama boyutu sınırına ulaşılmadığı takdirde veritabanı başına 32.767 dosya. |
| En büyük dosya boyutu | 8 TB ile sınırlıdır, şu anda kullanılabilir örnek boyutu (maksimum 2 TB-8 TB) ve [Azure Premium disk depolama ayırma alanı](sql-database-managed-instance-transact-sql-information.md#exceeding-storage-space-with-small-database-files). | 4 TB ile sınırlıdır ve şu anda kullanılabilir örnek boyutu (1 TB-4 TB 'a kadar). |
| Günlük dosyası boyutu üst sınırı | 2 TB, şu anda kullanılabilir örnek boyutu ve [Azure Premium disk depolama ayırma alanı](sql-database-managed-instance-transact-sql-information.md#exceeding-storage-space-with-small-database-files)ile sınırlıdır. | 2 TB ile sınırlıdır ve şu anda kullanılabilir örnek boyutu. |
| Veri/günlük ıOPS (yaklaşık) | dosya başına 500-7.500<br/>\*[Daha fazla ıOPS almak için dosya boyutunu artırın](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage-performance#premium-storage-disk-sizes)| 5,5 k-110 K (1375/vCore)<br/>Daha iyi GÇ performansı almak için daha fazla sanal çekirdek ekleyin. |
| Günlük yazma verimlilik sınırı (örnek başına) | Sanal çekirdek başına 3 MB/s<br/>En fazla 22 MB/sn | vCore başına 4 MB/s<br/>En fazla 48 MB/sn |
| Veri işleme (yaklaşık) | dosya başına 100-250 MB/s<br/>\*[Daha iyi GÇ performansı almak için dosya boyutunu artırın](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage-performance#premium-storage-disk-sizes) | Sınırlı değildir. |
| Depolama GÇ gecikmesi (yaklaşık) | 5-10 MS | 1-2 MS |
| Bellek içi OLTP | Desteklenmiyor | Kullanılabilir |
| En fazla oturum sayısı | 30000 | 30000 |
| [Salt okuma çoğaltmaları](sql-database-read-scale-out.md) | 0 | 1 (fiyata dahildir) |

> [!NOTE]
> - Kullanıcı ve sistem veritabanlarındaki veri ve günlük dosyası boyutu, en büyük depolama boyutu sınırı ile karşılaştırılan örnek depolama boyutuna dahildir. Veritabanlarına göre kullanılan toplam alanı öğrenmek için <a href="https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-master-files-transact-sql">sys. master_files</a> sistem görünümünü kullanın. Hata günlükleri kalıcı değil ve boyutuna dahil edilmez. Yedeklemeler depolama boyutuna dahil değildir.
> - Aktarım hızı ve ıOPS ayrıca yönetilen örnekle açıkça sınırlı olmayan sayfa boyutuna bağlıdır.
> Farklı Azure bölgesinde, otomatik yük devretme grupları kullanarak başka bir okunabilir çoğaltma oluşturabilirsiniz.

> [!NOTE]
> [Bu makaledeki yönetilen örnek havuzlardaki kaynak limitleri](sql-database-instance-pools.md#instance-pools-resource-limitations)hakkında daha fazla bilgi bulabilirsiniz.

## <a name="supported-regions"></a>Desteklenen bölgeler

Yönetilen örnekler yalnızca [desteklenen bölgelerde](https://azure.microsoft.com/global-infrastructure/services/?products=sql-database&regions=all)oluşturulabilir. Şu anda desteklenmeyen bir bölgede yönetilen bir örnek oluşturmak için, [Azure Portal aracılığıyla bir destek isteği gönderebilirsiniz](#obtaining-a-larger-quota-for-sql-managed-instance).

## <a name="supported-subscription-types"></a>Desteklenen abonelik türleri

Yönetilen örnek şu anda yalnızca aşağıdaki abonelik türlerinde dağıtımı desteklemektedir:

- [Kurumsal Anlaşma (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/)
- [Kullandıkça öde](https://azure.microsoft.com/offers/ms-azr-0003p/)
- [Bulut hizmeti sağlayıcısı (CSP)](https://docs.microsoft.com/partner-center/csp-documents-and-learning-resources)
- [Kurumsal Geliştirme ve Test](https://azure.microsoft.com/offers/ms-azr-0148p/)
- [Kullandıkça Öde Geliştirme ve Test](https://azure.microsoft.com/offers/ms-azr-0023p/)
- [Visual Studio aboneleri için aylık Azure kredisine sahip abonelikler](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/)

## <a name="regional-resource-limitations"></a>Bölgesel kaynak sınırlamaları

Desteklenen Abonelik türleri, bölge başına sınırlı sayıda kaynak içerebilir. Yönetilen örnek, Azure bölgesi başına iki varsayılan sınıra sahiptir (Bu, Azure portal) bir abonelik türü türüne bağlı olarak bir özel [destek isteği](#obtaining-a-larger-quota-for-sql-managed-instance)oluşturarak isteğe bağlı olarak artırılabilir:

- **Alt ağ sınırı**: Yönetilen örneklerin tek bir bölgede dağıtıldığı en fazla alt ağ sayısı.
- **sanal çekirdek birim sınırı**: Tek bir bölgedeki tüm örneklerde dağıtılabilecek en fazla vCore birimi sayısı. Bir GP sanal çekirdeği bir vCore birimi kullanır ve bir BC sanal çekirdek 4 sanal çekirdek birimi alır. Toplam örnek sayısı, sanal çekirdek birim sınırının içinde olduğu sürece sınırlı değildir.

> [!Note]
> Bu sınırlar varsayılan ayarlar değildir ve teknik sınırlamalardır. Geçerli bölgede daha fazla yönetilen örneğe ihtiyacınız varsa Azure portal özel bir [destek isteği](#obtaining-a-larger-quota-for-sql-managed-instance) oluşturularak sınırlar artırılabilir. Alternatif olarak, destek istekleri göndermeden başka bir Azure bölgesinde yeni yönetilen örnekler oluşturabilirsiniz.

Aşağıdaki tabloda desteklenen Abonelik türleri için **varsayılan bölgesel sınırlar** gösterilmektedir (varsayılan sınırlar aşağıda açıklanan destek isteği kullanılarak genişletilebilir):

|Abonelik türü| En fazla yönetilen örnek alt ağ sayısı | Maksimum vCore birimi sayısı * |
| :---| :--- | :--- |
|Kullandıkça öde|3|320|
|CSP |8 (bazı bölgelerde 15 * *)|960 (bazı bölgelerde 1440 * *)|
|Kullandıkça Öde geliştirme ve test|3|320|
|Kurumsal Geliştirme ve Test|3|320|
|EA|8 (bazı bölgelerde 15 * *)|960 (bazı bölgelerde 1440 * *)|
|Visual Studio Enterprise|2 |64|
|Visual Studio Professional ve MSDN Platformları|2|32|

\*Dağıtımları planlama bölümünde, lütfen İş Açısından Kritik (BC) hizmet katmanının dört (4) kat daha fazla sanal çekirdek kapasitesi Genel Amaçlı (GP) hizmet katmanından gerektirdiğini göz önünde bulundurun. Örneğin: 1 GP sanal çekirdek = 1 sanal çekirdek birim ve 1 BC sanal çekirdek = 4 sanal çekirdek birim. Tüketim analizinizi varsayılan sınırlara karşı basitleştirmek için, yönetilen örneklerin dağıtıldığı bölgedeki tüm alt ağlarda vCore birimlerini özetleyin ve sonuçları abonelik türü için örnek birim sınırlarıyla karşılaştırın. Bir bölgedeki her abonelik için **en fazla vCore birimi** sınırı geçerlidir. Birden çok alt ağ arasında dağıtılan tüm sanal çekirdekler toplamı, **en fazla sanal çekirdek birimi sayısına**eşit veya daha düşük olmalıdır.

\*\*Daha büyük alt ağ ve sanal çekirdek limitleri aşağıdaki bölgelerde kullanılabilir: Avustralya Doğu, Doğu ABD, Doğu ABD 2, Kuzey Avrupa, Orta Güney ABD, Güneydoğu Asya, UK Güney, Batı Avrupa, Batı ABD 2.

## <a name="obtaining-a-larger-quota-for-sql-managed-instance"></a>SQL yönetilen örneği için daha büyük bir kota alma

Geçerli Bölgelerinizdeki daha fazla yönetilen örneğe ihtiyacınız varsa, Azure portal kullanarak kotayı uzatmak için bir destek isteği gönderin.
Daha büyük bir kota elde etme işlemini başlatmak için:

1. **Yardım ve destek**' i açın ve **Yeni destek isteği**' ne tıklayın.

   ![Yardım ve Destek](media/sql-database-managed-instance-resource-limits/help-and-support.png)
2. Yeni destek isteği için temel bilgiler sekmesinde:
   - **Sorun türü**için **hizmet ve abonelik sınırları (kotalar)** öğesini seçin.
   - **Abonelik** bölümünde aboneliğinizi seçin.
   - **Kota türü**Için, **SQL veritabanı yönetilen örneği**' ni seçin.
   - **Destek planı**için destek planınızı seçin.

     ![Sorun türü kotası](media/sql-database-managed-instance-resource-limits/issue-type-quota.png)

3. **İleri**’ye tıklayın.
4. Yeni destek isteği için **sorun sekmesinde** :
   - **Önem derecesi**için sorunun önem derecesini seçin.
   - **Ayrıntılar**için, sorununuz hakkında hata iletileri de dahil olmak üzere ek bilgiler sağlayın.
   - **Karşıya dosya yükleme**için, daha fazla bilgi içeren bir dosya ekleyin (4 MB 'a kadar).

     ![Sorun ayrıntıları](media/sql-database-managed-instance-resource-limits/problem-details.png)

     > [!IMPORTANT]
     > Geçerli bir istek şunları içermelidir:
     > - Abonelik sınırının artırılması gereken bölge.
     > - Kota artırdıktan sonra mevcut alt ağlardaki hizmet katmanı başına gereken sanal çekirdek sayısı (mevcut alt ağlardan herhangi birinin genişletilmesi gerekiyorsa).
     > - Yeni alt ağlar içinde gerekli yeni alt ağ sayısı ve hizmet katmanı başına toplam Vcore sayısı (yeni alt ağlarda yönetilen örnekler dağıtmanız gerekiyorsa).

5. **İleri**’ye tıklayın.
6. Yeni destek isteği için kişi bilgileri sekmesinde, tercih edilen iletişim yöntemini (e-posta veya telefon) ve iletişim ayrıntılarını girin.
7. **Oluştur**’a tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

- Yönetilen örnek hakkında daha fazla bilgi için bkz. [yönetilen örnek nedir?](sql-database-managed-instance.md).
- Fiyatlandırma bilgileri için bkz. [SQL veritabanı yönetilen örnek fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/managed/).
- İlk yönetilen örneğinizi oluşturmayı öğrenmek için [hızlı başlangıç kılavuzuna](sql-database-managed-instance-get-started.md)bakın.
