---
title: Sık sorulan sorular (SSS)
titleSuffix: Azure SQL Managed Instance
description: Azure SQL yönetilen örnek hakkında sık sorulan sorular (SSS)
services: sql-database
ms.service: sql-managed-instance
ms.subservice: operations
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein, carlrab
ms.date: 03/17/2020
ms.openlocfilehash: 9295c6e1daaad6346581b959a9b94a7ab74da44c
ms.sourcegitcommit: 537c539344ee44b07862f317d453267f2b7b2ca6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2020
ms.locfileid: "84708867"
---
# <a name="azure-sql-managed-instance-frequently-asked-questions-faq"></a>Azure SQL yönetilen örnek hakkında sık sorulan sorular (SSS)
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Bu makale, [Azure SQL yönetilen örneği](sql-managed-instance-paas-overview.md)hakkında en sık sorulan soruları içerir.

## <a name="supported-features"></a>Desteklenen özellikler

**SQL yönetilen örneği 'nde desteklenen özelliklerin listesini nerede bulabilirim?**

SQL yönetilen örneğindeki desteklenen özelliklerin listesi için bkz. [Azure SQL yönetilen örnek özellikleri](../database/features-comparison.md).

Azure SQL yönetilen örneği ve SQL Server arasındaki sözdizimi ve davranıştaki farklar için, bkz. [T-SQL farkları SQL Server](transact-sql-tsql-differences-sql-server.md).


## <a name="tech-spec--resource-limits"></a>Teknik belirtim & kaynak limitleri
 
**SQL yönetilen örneği için teknik özellikleri ve kaynak sınırlarını nereden bulabilirim?**

Kullanılabilir donanım oluşturma özellikleri için bkz. [donanım oluşumlarında teknik farklılıklar](resource-limits.md#hardware-generation-characteristics).
Kullanılabilir hizmet katmanları ve özellikleri için bkz. [hizmet katmanları arasındaki teknik farklılıklar](resource-limits.md#service-tier-characteristics).

## <a name="known-issues--bugs"></a>Bilinen sorunlar & hataları

**Bilinen sorunları ve hataları nerede bulabilirim?**

Hatalar ve bilinen sorunlar için bkz. [bilinen sorunlar](../database/doc-changes-updates-release-notes.md#known-issues).

## <a name="new-features"></a>Yeni özellikler

**En son özellikleri ve özellikleri genel önizlemede nerede bulabilirim?**

Yeni ve Önizleme özellikleri için bkz. [sürüm notları](../database/doc-changes-updates-release-notes.md?tabs=managed-instance).

## <a name="deployment-times"></a>Dağıtım süreleri 

**Örneği oluşturmak veya güncelleştirmek ya da bir veritabanını geri yüklemek için ne kadar süre sürer?**

Yönetilen bir örnek oluşturmak için beklenen süre veya hizmet katmanını değiştirme (Vçekirdekler, depolama) çeşitli etkenlere bağlıdır. [Yönetim işlemlerine](/azure/sql-database/sql-database-managed-instance#managed-instance-management-operations)göz atın. 

## <a name="naming-conventions"></a>Adlandırma kuralları

**Yönetilen bir örnek SQL Server şirket içi örnekle aynı ada sahip olabilir mi?**

Yönetilen örnek adının değiştirilmesi desteklenmiyor.

Yönetilen örnek için varsayılan DNS bölgesi *. Database.Windows.net* değiştirilebilir. 

Varsayılan yerine başka bir DNS bölgesi kullanmak için, örneğin *. contoso.com*: 
- Bir diğer ad tanımlamak için CliConfig 'i kullanın. Araç yalnızca bir kayıt defteri ayarları sarmalayıcısı, bu yüzden Grup İlkesi veya bir komut dosyası kullanılarak yapılabilir.
- *TrustServerCertificate = true* seçeneğiyle *CNAME* kullanın.

## <a name="move-a-database-from-sql-managed-instance"></a>SQL yönetilen örneğinden bir veritabanını taşıma 

**SQL yönetilen örneğinden bir veritabanını SQL Server veya Azure SQL veritabanı 'na geri nasıl taşıyabilirim?**

[Bir veritabanını BACPAC 'e aktarabilir](../database/database-export.md) ve ardından [bacpac dosyasını içeri aktarabilirsiniz](../database/database-import.md). Veritabanınız 100 GB 'den küçükse bu önerilen yaklaşımdır.

Veritabanındaki tüm tablolarda birincil anahtarlar varsa işlemsel çoğaltma kullanılabilir.

SQL yönetilen `COPY_ONLY` örneği SQL Server kıyasla daha yüksek bir veritabanı sürümüne sahip olduğundan SQL yönetilen örneğinden alınan yerel yedeklemeler SQL Server geri yüklenemez.

## <a name="migrate-an-instance-database"></a>Örnek veritabanını geçirme

**Örnek veritabanımı Azure SQL veritabanı 'na nasıl geçirebilirim?**

Bir seçenek, [veritabanını BIR BACPAC 'e aktarmak](../database/database-export.md) ve ardından [bacpac dosyasını içeri aktaryıdır](../database/database-import.md). 

Veritabanınız 100 GB 'den küçükse bu önerilen yaklaşımdır. Veritabanındaki tüm tablolarda birincil anahtarlar varsa işlemsel çoğaltma kullanılabilir.

## <a name="switch-hardware-generation"></a>Donanım oluşturmayı Değiştir 

**SQL yönetilen örnek donanım oluşturma işlemi ile gen 4 ve Gen 5 arasında geçiş yapabilir miyim?**

SQL yönetilen örneğinin sağlandığı bölgede her iki donanım nesli de kullanılabiliyorsa, donanım nesilleri arasında otomatik çevrimiçi geçiş mümkündür. Bu durumda, [sanal çekirdek modeline genel bakış sayfasını](../database/service-tiers-vcore.md)denetleyerek, donanım nesilleri arasında nasıl geçiş yapılacağını açıklayan bir denetim yapabilirsiniz.

Bu uzun süredir çalışan bir işlemdir. yeni bir yönetilen örnek, arka planda ve veritabanları otomatik olarak eski ve yeni örnekler arasında, işlem sonunda hızlı bir yük devretmeyle hazırlanacaktır. 

**Aynı bölgede her iki donanım kuşnesi de desteklenmiyorsa ne olacak?**

Aynı bölgede her iki donanım neslini de desteklenmiyorsa, donanım oluşturma işlemi değiştirilebilir, ancak el ile yapılması gerekir. Bu, istenen donanım oluşturma 'nın kullanılabildiği bölgede yeni bir örnek sağlamanızı ve eski ve yeni örnekler arasında verileri el ile yedeklemeniz ve geri yüklemeniz gerekir.

**Güncelleştirme işlemini gerçekleştirmek için yeterli IP adresi yoksa ne olacak?**

Yönetilen örneğinizin sağlandığı alt ağda yeterli IP adresi yoksa, içinde yeni bir alt ağ ve yeni bir yönetilen örnek oluşturmanız gerekir. Ayrıca, yeni alt ağın daha fazla IP adresi ayrılmış olarak oluşturulmasını önerdiğimiz için, gelecekteki güncelleştirme işlemleri benzer durumlardan kaçınacaktır. (Uygun alt ağ boyutu için, [VNET alt ağının boyutunu belirlemeyi](vnet-subnet-determine-size.md)denetleyin.) Yeni örnek sağlandıktan sonra, eski ve yeni örnekler arasında verileri el ile yedekleyebilir veya geri yükleyebilir ya da çapraz örnek [zaman içinde geri yükleme](point-in-time-restore.md?tabs=azure-powershell)gerçekleştirebilirsiniz. 


## <a name="tune-performance"></a>Performansı ayarlama

**Nasıl yaparım? SQL yönetilen örneği 'nin performansı yapılsın mı?**

Genel Amaçlı katmanındaki SQL yönetilen örneği, uzak depolama kullanır, bu nedenle veri ve günlük dosyalarının boyutu performansı önemli değildir. Daha fazla bilgi için bkz. [SQL yönetilen örnek performansı genel amaçlı günlük dosyası boyutunun etkisi](https://medium.com/azure-sqldb-managed-instance/impact-of-log-file-size-on-general-purpose-managed-instance-performance-21ad170c823e).

İş yükünüz çok sayıda küçük işlem içeriyorsa, bağlantı türünü proxy 'den yeniden yönlendirme moduna geçirmeyi düşünün.

## <a name="maximum-storage-size"></a>Maksimum depolama boyutu

**SQL yönetilen örneği için en büyük depolama boyutu nedir?**

SQL yönetilen örneği için depolama boyutu, seçilen hizmet katmanına (Genel Amaçlı veya İş Açısından Kritik) göre değişir. Bu hizmet katmanlarının depolama sınırlamaları için bkz. [hizmet katmanı özellikleri](../database/service-tiers-general-purpose-business-critical.md).

## <a name="backup-storage-cost"></a>Yedekleme depolama maliyeti 

**Yedekleme deposu SQL yönetilen örnek depolamama göre mi kesilsin?**

Hayır, yedekleme depolaması SQL yönetilen örnek depolama alanınızda kesilmiyor. Yedekleme depolaması, örnek depolama alanından bağımsızdır ve bu boyut sınırlı değildir. Yedekleme depolaması, örnek veritabanlarınızın yedeğinin tutulacağı zaman dilimi ile sınırlıdır ve 7 ile 35 gün arasında yapılandırılabilir. Ayrıntılar için bkz. [otomatik yedeklemeler](../database/automated-backups-overview.md).

## <a name="track-billing"></a>Faturalandırmayı izle

**SQL yönetilen örneği için faturalandırma maliyetimi izlemenin bir yolu var mı?**

Bunu [Azure maliyet yönetimi çözümünü](/azure/cost-management/)kullanarak yapabilirsiniz. [Azure Portal](https://portal.azure.com) **abonelikler** ' e gidin ve **Maliyet Analizi**' ni seçin. 

**Birikmiş maliyetler** seçeneğini kullanın ve ardından **kaynak türüne** göre filtreleyin `microsoft.sql/managedinstances` . 
  
## <a name="inbound-nsg-rules"></a>Gelen NSG kuralları

**Yönetim bağlantı noktalarında gelen NSG kurallarını nasıl ayarlayabilirim?**

SQL yönetilen örnek denetim düzlemi, yönetim bağlantı noktalarını koruyan NSG kurallarını korur.

İçin aşağıdaki yönetim bağlantı noktaları kullanılır:

9000 ve 9003 bağlantı noktaları Azure Service Fabric altyapısı tarafından kullanılır. Service Fabric birincil rol, sanal kümenin sağlıklı tutulması ve hedef durumunun Bileşen çoğaltmaları sayısına göre kalmasını sağlamaktır.

1438, 1440 ve 1452 bağlantı noktaları düğüm Aracısı tarafından kullanılır. Düğüm Aracısı, küme içinde çalışan ve yönetim komutlarını yürütmek için denetim düzlemi tarafından kullanılan bir uygulamadır.

NSG kurallarına ek olarak, yerleşik güvenlik duvarı ağ katmanındaki örneği korur. Uygulama katmanında iletişim, sertifikalarla korunur.

Daha fazla bilgi edinmek ve yerleşik güvenlik duvarını nasıl doğrulayacağınızı öğrenmek için bkz. [Azure SQL yönetilen örneği yerleşik güvenlik duvarı](management-endpoint-verify-built-in-firewall.md).


## <a name="mitigate-data-exfiltration-risks"></a>Veri kaybı riskini azaltma  

**Veri kaybı riskini nasıl azaltabilirim?**

Herhangi bir veri savunma riskini azaltmak için müşterilerin bir dizi güvenlik ayarı ve denetimi uygulaması önerilir:

- Tüm veritabanlarında [Saydam veri şifrelemesi (TDE)](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-azure-sql) öğesini açın.
- Ortak dil çalışma zamanını (CLR) devre dışı bırakın. Bu, şirket içinde de önerilir.
- Yalnızca Azure Active Directory (Azure AD) kimlik doğrulaması kullanın.
- Düşük ayrıcalıklı bir DBA hesabıyla örneğe erişin.
- Sysadmin hesabı için JıT atlama kutusu erişimini yapılandırın.
- [SQL denetimini](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine)açın ve uyarı mekanizmalarıyla tümleştirin.
- [Tehdit algılamayı](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection) [Gelişmiş VERI güvenliği (ADS)](https://docs.microsoft.com/azure/sql-database/sql-database-advanced-data-security) paketinden açın.


## <a name="cost-saving-use-cases"></a>Maliyet tasarrufu kullanım örnekleri

**SQL yönetilen örneği ile kullanım durumlarını ve sonuçta elde edilen maliyet tasarrufunu nereden bulabilirim?**

SQL yönetilen örnek olay incelemeleri:

- [Komatsu](https://customers.microsoft.com/story/komatsu-australia-manufacturing-azure)
- [KMD](https://customers.microsoft.com/en-ca/story/kmd-professional-services-azure-sql-database)
- [PowerDETAILS](https://customers.microsoft.com/story/powerdetails-partner-professional-services-azure-sql-database-managed-instance)
- [Allscripts](https://customers.microsoft.com/story/allscripts-partner-professional-services-azure)

Azure SQL yönetilen örneği dağıtmayla ilgili avantajları, maliyetleri ve riskleri daha iyi anlamak için, [Microsoft Azure SQL veritabanı yönetilen örneğin toplam ekonomik etkisi](https://azure.microsoft.com/resources/forrester-tei-sql-database-managed-instance)de vardır.


## <a name="dns-refresh"></a>DNS yenileme 

**DNS yenileme yapabilir miyim?**

Şu anda SQL yönetilen örneği için DNS sunucusu yapılandırmasını yenileme özelliği sağlamayız.

DNS yapılandırması sonunda yenilenir:

- DHCP kira süresi dolar.
- Platform yükseltmesi üzerinde.

Geçici bir çözüm olarak, SQL yönetilen örneği 4 sanal çekirdeğe indirgekten sonra yeniden yükseltin. Bunun, DNS yapılandırmasını yenilemeyi yan etkisi vardır.


## <a name="ip-address"></a>IP adresi

**Bir IP adresi kullanarak SQL yönetilen örneğine bağlanabilir miyim?**

SQL yönetilen örneğine bir IP adresi kullanılarak bağlanma desteklenmiyor. SQL yönetilen örnek ana bilgisayar adı, SQL yönetilen örnek sanal kümesinin önünde bir yük dengeleyiciye eşlenir. Bir sanal küme birden çok yönetilen örneği barındırabildiğinden, bağlantılar açıkça belirtilmeden, doğru yönetilen örneğe yönlendirilemez.

SQL yönetilen örnek sanal küme mimarisi hakkında daha fazla bilgi için bkz. [sanal küme bağlantısı mimarisi](connectivity-architecture-overview.md#virtual-cluster-connectivity-architecture).

**SQL yönetilen örneği statik bir IP adresine sahip olabilir mi?**

Nadir ancak gerekli durumlarda, SQL yönetilen örneği 'nin çevrimiçi geçişini yeni bir sanal kümeye yapmanız gerekebilir. Gerekirse, bu geçiş, teknoloji yığınımızda hizmetin güvenliğini ve güvenilirliğini artırmaya yönelik değişiklikler nedeniyle oluşur. Yeni bir sanal kümeye geçiş, SQL yönetilen örnek ana bilgisayar adıyla eşlenen IP adresini değiştirmeye neden olur. SQL yönetilen örnek hizmeti statik IP adresi desteğini talep etmez ve normal bakım döngülerinin bir parçası olarak fark etmeden değiştirme hakkını saklı tutar.

Bu nedenle, IP adresinin gereksiz kapalı kalma süresine neden olabileceği için IP adresi 'nin dengeszlik düzeyini önemli bir şekilde ele geçirmesini kesinlikle önermiyoruz.

## <a name="change-time-zone"></a>Saat dilimini değiştirme

**Mevcut bir yönetilen örnek için saat dilimini değiştirebilir miyim?**

Yönetilen bir örnek ilk kez sağlandığında saat dilimi yapılandırması ayarlanabilir. Mevcut bir yönetilen Örneğin saat dilimini değiştirme desteklenmiyor. Ayrıntılar için bkz. [saat dilimi sınırlamaları](timezones-overview.md#limitations).

Geçici çözümler, doğru saat dilimine sahip yeni bir yönetilen örnek oluşturma ve ardından el ile yedekleme ve geri yükleme gerçekleştirme ya da önerdiğimiz bir [çapraz örnek zaman geri yükleme](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/06/07/cross-instance-point-in-time-restore-in-azure-sql-database-managed-instance/)gerçekleştirme işlemlerini içerir.


## <a name="resolve-performance-issues"></a>Performans sorunlarını çözme

**Nasıl yaparım? SQL yönetilen örneği ile performans sorunlarını çözmek mi istiyorsunuz?**

SQL yönetilen örneği ve SQL Server arasındaki performans karşılaştırması için iyi bir başlangıç noktası, [Azure SQL yönetilen örneği ve SQL Server arasında performans karşılaştırması Için en iyi](https://techcommunity.microsoft.com/t5/Azure-SQL-Database/The-best-practices-for-performance-comparison-between-Azure-SQL/ba-p/683210)uygulamadır.

Veri yükleme, zorunlu tam kurtarma modeli ve işlem günlüğü yazma aktarım hızı [sınırları](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-resource-limits#service-tier-characteristics) nedenıyle, SQL yönetilen SQL Server örneği üzerinde genellikle daha yavaştır. Bazen bu, geçici verileri Kullanıcı veritabanı yerine tempdb 'ye yükleyerek veya kümelenmiş columnstore ya da bellek için iyileştirilmiş tablolar kullanarak geçici olarak çalışabilir.


## <a name="restore-encrypted-backup"></a>Şifrelenmiş yedeklemeyi geri yükleme

**Şifrelenmiş veritabanımı SQL yönetilen örneğine geri yükleyebilir miyim?**

Evet, SQL yönetilen örneğine geri yüklemek için veritabanınızın şifresini çözmeniz gerekmez. Şifrelenmiş yedekleme dosyasından verileri okuyabilmeniz için kaynak sistemde SQL yönetilen örneği için şifreleme anahtarı koruyucusu olarak kullanılan bir sertifika/anahtar sağlamanız gerekir. Bunu yapmak için iki olası yol vardır:

- *Sertifika koruyucuyu SQL yönetilen örneğine yükleyin*. Yalnızca PowerShell kullanılarak yapılabilir. [Örnek betik](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-migrate-tde-certificate) , tüm süreci açıklar.
- *Azure Key Vault için asimetrik anahtar koruyucusunu karşıya yükleyin ve SQL yönetilen örneği üzerine gelin*. Bu yaklaşım, şifreleme anahtarını depolamak için Key Vault tümleştirmesinin kullanıldığı, kendi anahtarını getir (BYOK) TDE kullanım örneğine benzer. Anahtarı şifreleme anahtar koruyucusu olarak kullanmak istemiyorsanız ve yalnızca SQL yönetilen örneği için anahtarı şifrelenmiş veritabanlarını geri yüklemek istiyorsanız, [BYOK TDE ayarlama](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-azure-sql#manage-transparent-data-encryption)yönergelerini izleyin ve **Seçili anahtarı varsayılan TDE koruyucusu yap**onay kutusunu işaretlemeyin.

Şifreleme koruyucusunu SQL yönetilen örneği için kullanılabilir hale getirildikten sonra standart veritabanı geri yükleme yordamıyla devam edebilirsiniz.

## <a name="migrate-from-sql-database"></a>SQL veritabanından geçiş 

**Azure SQL veritabanından SQL yönetilen örneğine nasıl geçiş yapabilirim?**

SQL yönetilen örneği, Azure SQL veritabanı olarak işlem ve depolama boyutu başına aynı performans düzeylerini sunar. Tek bir örnekteki verileri birleştirmek istiyorsanız veya yalnızca SQL yönetilen örneği 'nde yalnızca desteklenen bir özelliğe ihtiyacınız varsa, dışarı aktarma/içeri aktarma (BACPAC) işlevini kullanarak verilerinizi geçirebilirsiniz.

## <a name="password-policy"></a>Parola ilkesi 

**SQL yönetilen örnek SQL oturumları için hangi parola ilkeleri uygulandı?**

SQL oturum açmaları için SQL yönetilen örnek parola ilkesi, yönetilen örneği tutan sanal kümeyi oluşturan VM 'lere uygulanan Azure platform ilkelerini devralır. Bu ayarlar Azure tarafından tanımlandığından ve yönetilen örnek tarafından devralındığından, bu ayarlardan herhangi birini değiştirmek mümkün değildir.

 > [!IMPORTANT]
 > Azure platformu, bu ilkelere bağlı hizmetleri bilgilendirmeden ilke gereksinimlerini değiştirebilir.

**Geçerli Azure platformu ilkeleri nelerdir?**

Her oturum açma sırasında parolasını ayarlaması ve en yüksek yaşına ulaştıktan sonra parolasını değiştirmesi gerekir.

| **İlke** | **Güvenlik Ayarı** |
| --- | --- |
| Maksimum parola yaşı | 42 gün |
| En az parola yaşı | 1 gün |
| Minimum parola uzunluğu | 10 karakter |
| Parolanın karmaşıklık gereksinimlerini karşılaması gerekir | Etkin |

**Oturum açma düzeyindeki SQL yönetilen örneği 'nde parola karmaşıklığını ve kullanım süresini devre dışı bırakmak mümkün mü?**

Evet, oturum açma düzeyinde CHECK_POLICY ve CHECK_EXPIRATION alanlarını denetlemek mümkündür. Aşağıdaki T-SQL komutunu yürüterek geçerli ayarları denetleyebilirsiniz:

```sql
SELECT *
FROM sys.sql_logins
```

Bundan sonra, şunu yürüterek belirtilen oturum açma ayarlarını değiştirebilirsiniz:

```sql
ALTER LOGIN <login_name> WITH CHECK_POLICY = OFF;
ALTER LOGIN <login_name> WITH CHECK_EXPIRATION = OFF;
```

(' test ' öğesini istenen oturum açma adı ile değiştirin ve ilke ile süre sonu değerlerini ayarlayın)
