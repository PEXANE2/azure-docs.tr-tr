---
title: Yönetilen örnekler hakkında SSS
description: SQL veritabanı yönetilen örnek hakkında sık sorulan sorular (SSS)
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein, carlrab
ms.date: 07/16/2019
ms.openlocfilehash: 81f776428303ad5e6486ba52c1acdf70d051563e
ms.sourcegitcommit: f53cd24ca41e878b411d7787bd8aa911da4bc4ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2020
ms.locfileid: "75835009"
---
# <a name="sql-database-managed-instance-frequently-asked-questions-faq"></a>SQL veritabanı yönetilen örnek hakkında sık sorulan sorular (SSS)

Bu makale, [SQL veritabanı yönetilen örneği](sql-database-managed-instance.md)hakkında en yaygın soruların çoğunu içerir.

## <a name="supported-features"></a>Desteklenen özellikler

**Yönetilen örnekte desteklenen özelliklerin listesini nerede bulabilirim?**

Yönetilen örnekteki desteklenen özelliklerin listesi için bkz. [Azure SQL veritabanı SQL Server karşılaştırması](sql-database-features.md).

Azure SQL veritabanı yönetilen örneği ve şirket içi SQL Server arasındaki sözdizimi ve davranıştaki farklar için, bkz. [T-SQL farkları SQL Server](sql-database-managed-instance-transact-sql-information.md).


## <a name="tech-spec--resource-limits"></a>Teknik belirtim & kaynak limitleri
 
**Yönetilen örnek için teknik özellikleri ve kaynak sınırlarını nereden bulabilirim?**

Kullanılabilir donanım oluşturma özellikleri için bkz. [donanım oluşumlarında teknik farklılıklar](sql-database-managed-instance-resource-limits.md#hardware-generation-characteristics).
Kullanılabilir hizmet katmanları ve özellikleri için bkz. [hizmet katmanları arasındaki teknik farklılıklar](sql-database-managed-instance-resource-limits.md#service-tier-characteristics).

## <a name="known-issues--bugs"></a>Bilinen sorunlar & hataları

**Bilinen sorunları ve hataları nerede bulabilirim?**

Hatalar ve bilinen sorunlar için bkz. [bilinen sorunlar](sql-database-managed-instance-transact-sql-information.md#Issues).

## <a name="new-features"></a>Yeni özellikler

**En son özellikleri ve özellikleri genel önizlemede nerede bulabilirim?**

Yeni ve Önizleme özellikleri için bkz. [sürüm notları](/azure/sql-database/sql-database-release-notes?tabs=managed-instance).

## <a name="deployment-times"></a>Dağıtım süreleri 

**Örnek oluşturmak veya güncelleştirmek ya da bir veritabanını geri yüklemek için ne kadar süre sürer?**

Yeni yönetilen örnek oluşturmak için beklenen süre veya hizmet katmanını değiştirme (Vçekirdekler, depolama) çeşitli etkenlere bağlıdır. [Yönetim işlemlerine](/azure/sql-database/sql-database-managed-instance#managed-instance-management-operations) göz atın 

## <a name="naming-convention"></a>Adlandırma kuralı

**Yönetilen bir örnek şirket içi SQL Server aynı ada sahip olabilir mi?**

Yönetilen örnek, *Database.Windows.net*ile biten bir ada sahip olmalıdır. Varsayılan yerine başka bir DNS bölgesi kullanmak için, örneğin, **mı-diğer-adı**. contoso.com: 
- Bir diğer ad tanımlamak için CliConfig 'i kullanın. Araç yalnızca bir kayıt defteri ayarları sarmalayıcısıdır ve bu nedenle Grup İlkesi veya betiği kullanılarak da yapılabilir.
- *TrustServerCertificate = true* seçeneğiyle *CNAME* kullanın.

## <a name="move-db-from-mi"></a>DB 'yi mı 'dan taşı 

**Veritabanını yönetilen örnekten geri SQL Server veya Azure SQL veritabanı 'na nasıl taşıyabilirim?**

[Veritabanını BACPAC 'e aktarabilir](sql-database-export.md) ve ardından [bacpac dosyasını içeri aktarabilirsiniz]( sql-database-import.md). Veritabanınız 100 GB 'den küçükse bu önerilen bir yaklaşımdır.

Veritabanındaki tüm tablolarda birincil anahtarlar varsa işlemsel çoğaltma kullanılabilir.

Yönetilen örnekten alınan yerel `COPY_ONLY` yedeklemeleri, SQL Server kıyasla daha yüksek bir veritabanı sürümüne sahip olduğundan SQL Server geri yüklenemez.

## <a name="migrate-instance-db"></a>Örnek DB 'yi geçirme

**Örnek veritabanımı tek bir Azure SQL veritabanına nasıl geçirebilirim?**

Bir seçenek, [veritabanını BIR BACPAC 'e aktarmak](sql-database-export.md) ve ardından [bacpac dosyasını içeri aktaryıdır](sql-database-import.md). 

Veritabanınız 100 GB 'den küçükse bu önerilen yaklaşımdır. Veritabanındaki tüm tablolarda birincil anahtarlar varsa işlemsel çoğaltma kullanılabilir.

## <a name="gen-4-vs-gen-5"></a>Gen 4 vs Gen 5 

**Yönetilen örnek için gen 4 ve Gen 5 donanım oluşturma arasında seçim mi Nasıl yaparım??**

Bazı donanım oluşturma, diğer iş yükü türleri için daha iyi olduğundan iş yükünüze bağımlıdır. Performans konusunun basitleşmesi tercih edilirken, iş yükü performansını etkileyen donanım nesilleri arasında aşağıdaki farklılıklar vardır:
- Gen 4, fiziksel işlemcilere göre daha iyi bir işlem desteği sağlar ve bu, vCore işlemcilere bağlı olan gen 5 ' tir. İşlem yoğunluğu yoğun iş yükleri için daha avantajlı olabilir.
- Gen 5, uzak depolama için daha iyi bir GÇ bant genişliğine neden olan hızlandırılmış ağı destekler. Genel Amaçlı hizmeti katmanlarında GÇ yoğun iş yükleri için avantajlı olabilir. Gen 5, gen 4 ' e kıyasla daha hızlı SSD yerel disklerini kullanır. İş açısından kritik hizmet katmanlarında GÇ yoğun iş yükleri için avantajlı olabilir.

Belirli bir durumda hangi donanım oluşturmanın daha iyi çalışacağını öğrenmek için üretime yönelik olarak tasarlanan gerçek iş yüklerinin performansını test etmek önemle önerilir.

## <a name="switch-hardware-generation"></a>Donanım oluşturmayı Değiştir 

**Yönetilen örnek donanım oluşturma işlemi ile gen 4 ve Gen 5 arasında geçiş yapabilir miyim?**

Yönetilen örneğinizin sağlandığı bölgede her iki donanım nesli de kullanılabiliyorsa, donanım oluşturmaları arasında otomatik çevrimiçi geçiş mümkündür. Bu durumda, donanım oluşturma arasında nasıl geçiş yapılacağını açıklayan [blog gönderisine ait betiği](https://techcommunity.microsoft.com/t5/Azure-SQL-Database/Change-hardware-generation-on-Managed-Instance/ba-p/699824) kullanabilirsiniz.

Arka planda yeni bir yönetilen örnek sağlanacağı ve veritabanlarının sonunda hızlı bir yük devretmeyle, eski ve yeni örnek arasında otomatik olarak aktarılan, bu uzun süredir çalışan bir işlemdir. 

Aynı bölgede her iki donanım neslini de desteklenmiyorsa, donanım oluşturma işlemi değiştirilebilir, ancak el ile yapılması gerekir. Bu, istenen donanım oluşturma 'nın kullanılabildiği bölgede yeni bir örnek sağlamanızı ve eski ve yeni örnek arasında verileri el ile yedeklemeniz ve geri yüklemeniz gerekir.


## <a name="tune-performance"></a>Performansı ayarlama

**Yönetilen örneðimin performansı Nasıl yaparım? mi?**

Genel Amaçlı yönetilen örnek, performans için önemli veri ve günlük dosyası boyutu nedeniyle uzak depolama kullanır. Daha fazla bilgi için bkz. [genel amaçlı yönetilen örnek performansı üzerinde günlük dosyası boyutunun etkisi](https://medium.com/azure-sqldb-managed-instance/impact-of-log-file-size-on-general-purpose-managed-instance-performance-21ad170c823e).

Yoğun GÇ yoğunluklu iş yükleri için Gen 5 donanımı kullanmayı ve işlem yoğunluğu iş yükleri için gen 4 ' ü kullanmayı düşünün. Daha fazla bilgi için bkz. [Gen 4 ve Gen 5 arasında nasıl yaparım? seçme](#gen-4-vs-gen-5).

İş yükünüz çok sayıda küçük işlem içeriyorsa, bağlantı türünü proxy 'den yeniden yönlendirme moduna geçirmeyi düşünün.

## <a name="maximum-storage-size"></a>Maksimum depolama boyutu

**Yönetilen örnek için en büyük depolama boyutu nedir?**

Yönetilen örnek için depolama boyutu, seçilen hizmet katmanına (Genel Amaçlı veya İş Açısından Kritik) göre değişir. Bu hizmet katmanlarının depolama sınırlamaları için bkz. [hizmet katmanı özelliği](sql-database-service-tiers-general-purpose-business-critical.md).

## <a name="back-up-storage-cost"></a>Depolama maliyetini yedekleme 

**Yedekleme deposu yönetilen örnek depolamama göre mi kesilsin?**

Hayır, yedekleme depolama alanı yönetilen örnek depolama alanından kesilmiyor. Yedekleme depolaması, örnek depolama alanından bağımsızdır ve bu boyut sınırlı değildir. Yedekleme depolaması, örnek veritabanlarınızın yedeğinin tutulacağı zaman dilimi ile sınırlıdır ve 7 ile 35 gün arasında yapılandırılabilir. Ayrıntılar için bkz. [otomatik yedeklemeler](https://docs.microsoft.com/azure/sql-database/sql-database-automated-backups).

## <a name="track-billing"></a>Faturalandırmayı izle

**Yönetilen örneğim için faturalandırma maliyetimi izlemenin bir yolu var mı?**

Bunu [Azure maliyet yönetimi çözümünü](/azure/cost-management/)kullanarak yapabilirsiniz. [Azure Portal](https://portal.azure.com) **abonelikler** ' e gidin ve **Maliyet Analizi**' ni seçin. 

**Birikmiş maliyetler** seçeneğini kullanın ve ardından **kaynak türüne** göre `microsoft.sql/managedinstances`olarak filtreleyin. 
  
## <a name="inbound-nsg-rules"></a>Gelen NSG kuralları

**Yönetim bağlantı noktalarında gelen NSG kurallarını nasıl ayarlayabilirim?**

Yerleşik güvenlik duvarı özelliği, yalnızca Microsoft Yönetim/dağıtım makineleriyle ilişkili IP aralıklarından gelen bağlantılara izin vermek için kümedeki tüm sanal makinelerde Windows Güvenlik Duvarı 'nı yapılandırır ve güvenli yönetim iş istasyonları Ağ katmanında izinsiz erişime karşı koruma sağlar.

Hangi bağlantı noktalarının kullanıldığı aşağıda verilmiştir:

9000 ve 9003 bağlantı noktaları Service Fabric altyapısı tarafından kullanılır. Service Fabric birincil rol, sanal kümenin sağlıklı tutulması ve hedef durumunun Bileşen çoğaltmaları sayısı bakımından tutulmasını sağlamaktır.

1438, 1440 ve 1452 bağlantı noktaları düğüm Aracısı tarafından kullanılır. Düğüm Aracısı, küme içinde çalışan ve yönetim komutlarını yürütmek için denetim düzlemi tarafından kullanılan bir uygulamadır.

Ağ katmanındaki yerleşik güvenlik duvarına ek olarak, iletişim de sertifikalarla korunur.
  
Daha fazla bilgi ve yerleşik güvenlik duvarını doğrulama hakkında daha fazla bilgi için bkz. [Azure SQL veritabanı yönetilen örneği yerleşik güvenlik duvarı](sql-database-managed-instance-management-endpoint-verify-built-in-firewall.md).


## <a name="mitigate-network-risks"></a>Ağ risklerini azaltma  

**Ağ risklerini nasıl azaltabilirim?**

Herhangi bir ağ riskini azaltmak için müşterilerin bir dizi güvenlik ayarı ve denetimi uygulaması önerilir:

- Tüm veritabanlarında [Saydam veri şifrelemesi (TDE)](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-azure-sql) öğesini açın.
- Ortak dil çalışma zamanını (CLR) devre dışı bırakın. Bu, şirket içinde de önerilir.
- Yalnızca Azure Active Directory (AAD) kimlik doğrulaması kullanın.
- Düşük ayrıcalıklı DBA hesabıyla Access örneğine erişin.
- Sysadmin hesabı için JIT atlama kutusu erişimini yapılandırın.
- [SQL denetimini](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine)açın ve uyarı mekanizmalarıyla tümleştirin.
- [Tehdit algılamayı](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection) [Gelişmiş VERI güvenliği (ADS)](https://docs.microsoft.com/azure/sql-database/sql-database-advanced-data-security) paketinden açın.


## <a name="cost-saving-use-cases"></a>Maliyet kaydetme kullanım örnekleri

**Yönetilen örnekle birlikte kullanım durumlarını ve sonuçta elde edilen maliyet tasarrufunu nereden bulabilirim?**

Yönetilen örnek örnek olay incelemeleri:

- [Komatsu](https://customers.microsoft.com/story/komatsu-australia-manufacturing-azure)
- [KMD](https://customers.microsoft.com/en-ca/story/kmd-professional-services-azure-sql-database)
- [PowerDETAILS](https://customers.microsoft.com/story/powerdetails-partner-professional-services-azure-sql-database-managed-instance)
- [Allscripts](https://customers.microsoft.com/story/allscripts-partner-professional-services-azure)   
Azure SQL veritabanı yönetilen örneği dağıtmayla ilgili avantajları, maliyetleri ve riskleri daha iyi anlamak için, Forrester 'in bir incelemesi de vardır: [mı? un toplam ekonomik etkisi](https://azure.microsoft.com/resources/forrester-tei-sql-database-managed-instance).


## <a name="dns-refresh"></a>DNS yenileme 

**DNS yenileme yapabilir miyim?**

Şu anda yönetilen örnek için DNS sunucusu yapılandırmasını yenileme özelliği sağlamayız.

DNS yapılandırması sonunda yenilenir:

- DHCP kira süresi dolar.
- Platform yükseltmesi üzerinde.

Geçici bir çözüm olarak, yönetilen örneği 4 sanal çekirdekli bir sürüme indirip daha sonra tekrar yükseltin. Bunun, DNS yapılandırmasını yenilemeyi yan etkisi vardır.


## <a name="static-ip-address"></a>Statik IP adresi

**Yönetilen bir örnek statik IP adresine sahip olabilir mi?**

Nadir ancak gerekli durumlarda, yönetilen bir örnek için yeni bir sanal kümeye çevrimiçi geçiş yapmanız gerekebilir. Gerekirse, bu geçiş, teknoloji yığınımızda hizmetin güvenliğini ve güvenilirliğini artırmaya yönelik değişiklikler nedeniyle oluşur. Yeni bir sanal kümeye geçiş, yönetilen örnek ana bilgisayar adıyla eşlenen IP adresinin değiştirilmesine neden olur. Yönetilen örnek hizmeti statik IP adresi desteğini talep etmez ve normal bakım döngülerinin bir parçası olarak fark etmeden değiştirme hakkını saklı tutar.

Bu nedenle, IP adresinin gereksiz kapalı kalma süresine neden olabileceği için IP adresi 'nin dengeszlik düzeyini önemli bir şekilde ele geçirmesini kesinlikle önermiyoruz.

## <a name="moving-mi"></a>Mı taşınıyor?

**Yönetilen bir örneği veya VNet 'i başka bir kaynak grubuna taşıyabilir miyim?**

Hayır, bu geçerli platform kısıtlamasıdır. Yönetilen bir örnek oluşturulduktan sonra, yönetilen örneği veya VNet 'i başka bir kaynak grubuna veya aboneliğe taşımak desteklenmez.

## <a name="change-time-zone"></a>Saat dilimini değiştirme

**Mevcut bir yönetilen örnek için saat dilimini değiştirebilir miyim?**

Yönetilen bir örnek ilk kez sağlandığında saat dilimi yapılandırması ayarlanabilir. Mevcut yönetilen Örneğin saat dilimini değiştirme desteklenmiyor. Ayrıntılar için bkz. [saat dilimi sınırlamaları](sql-database-managed-instance-timezone.md#limitations).

Geçici çözümler, doğru saat dilimine sahip yeni bir yönetilen örnek oluşturmayı ve sonra el ile yedekleme ve geri yükleme işlemi gerçekleştirmeyi ya da önerdiğimiz bir süre sonra bir [çapraz örnek için geri yükleme](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/06/07/cross-instance-point-in-time-restore-in-azure-sql-database-managed-instance/)gerçekleştirmesini içerir.


## <a name="resolve-performance-issues"></a>Performans sorunlarını çözme

**Nasıl yaparım? yönetilen örneğimde performans sorunlarını çözmek mi istiyorsunuz?**

Yönetilen örnek ve SQL Server arasındaki bir performans karşılaştırması için iyi bir başlangıç noktası, [Azure SQL yönetilen örneği ve SQL Server makalesi arasında performans karşılaştırması Için en iyi](https://techcommunity.microsoft.com/t5/Azure-SQL-Database/The-best-practices-for-performance-comparison-between-Azure-SQL/ba-p/683210) uygulamadır.

Veri yükleme, zorunlu tam kurtarma modeli ve işlem günlüğü yazma aktarım hızı [sınırları](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-resource-limits#service-tier-characteristics) nedeniyle SQL Server yönetilen örnekte genellikle daha yavaştır. Bazen bu, geçici verileri Kullanıcı veritabanı yerine tempdb 'ye yükleyerek veya kümelenmiş columnstore ya da bellek için iyileştirilmiş tablolar kullanılarak gerçekleştirilebilir.


## <a name="restore-encrypted-backup"></a>Şifrelenmiş yedeklemeyi geri yükleme

**Şifrelenmiş veritabanımı yönetilen örneğe geri yükleyebilir miyim?**

Evet, yönetilen örneğe geri yükleyebilmek için veritabanınızın şifresini çözmeniz gerekmez. Şifrelenmiş yedekleme dosyasından veri okuyabilmeniz için, yönetilen örneğe kaynak sistemde şifreleme anahtarı koruyucusu olarak kullanılan bir sertifika/anahtar sağlamanız gerekir. Bunu yapmak için iki olası yol vardır:

- *Yönetilen örneğe sertifika koruyucuyu yükleyin*. Yalnızca PowerShell kullanılarak yapılabilir. [Örnek betik](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-migrate-tde-certificate) , tüm süreci açıklar.
- *Asimetrik anahtar koruyucuyu Azure Key Vault (AKV) ve işaret ile yönetilen örneğe yükleyin*. Bu yaklaşım, şifreleme anahtarını depolamak için aynı zamanda AKV tümleştirmesini kullanan, kendi anahtarını getir (BYOK) TDE kullanım örneğine benzer. Anahtarı şifreleme anahtar koruyucusu olarak kullanmak istemiyorsanız ve yalnızca anahtar yönetilen örnek için şifrelenmiş veritabanlarını geri yüklemek istiyorsanız, [BYOK TDE ayarlama](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-azure-sql#manage-transparent-data-encryption)yönergelerini izleyin ve *Seçili anahtarı varsayılan TDE koruyucusu yap*onay kutusunu işaretlemeyin.

Şifreleme koruyucusunu yönetilen örnek için kullanılabilir hale getirildikten sonra, standart veritabanı geri yükleme yordamıyla devam edebilirsiniz.

## <a name="migrate-from-single-db"></a>Tek bir DB 'den geçiş 

**Azure SQL veritabanı tekil veya elastik havuzdan yönetilen örneğe nasıl geçiş yapabilirim?**

Yönetilen örnek, Azure SQL veritabanı 'nın diğer dağıtım seçenekleri olarak işlem ve depolama boyutuna göre aynı performans düzeylerini sunmaktadır. Tek bir örnekteki verileri birleştirmek istiyorsanız veya yalnızca yönetilen örnekte yalnızca desteklenen bir özelliğe ihtiyacınız varsa, dışarı aktarma/içeri aktarma (BACPAC) işlevini kullanarak verilerinizi geçirebilirsiniz.
