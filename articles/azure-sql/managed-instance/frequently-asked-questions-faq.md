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
ms.openlocfilehash: 88f92117dc07fc241ca714851956e386cd10d617
ms.sourcegitcommit: e995f770a0182a93c4e664e60c025e5ba66d6a45
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86135033"
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

  
## <a name="networking-requirements"></a>Ağ gereksinimleri 

**Yönetilen örnek alt ağında geçerli gelen/giden NSG kısıtlamaları nelerdir?**

Gerekli NSG ve UDR kuralları [burada](connectivity-architecture-overview.md#mandatory-inbound-security-rules-with-service-aided-subnet-configuration)belgelenmiştir ve hizmet tarafından otomatik olarak ayarlanır.
Lütfen bu kuralların yalnızca hizmeti sürdürmek için ihtiyaç duyduğumuz tek olduğunu aklınızda bulundurun. Yönetilen örneğe bağlanmak ve farklı özellikler kullanmak için, bakımını yapmanız gereken ek, özelliğe özgü kurallar ayarlamanız gerekir.

**Yönetim bağlantı noktalarında gelen NSG kurallarını nasıl ayarlayabilirim?**

SQL yönetilen örneği, yönetim bağlantı noktalarında kuralların ayarlamamasından sorumludur. Bu, [hizmet destekli alt ağ yapılandırması](connectivity-architecture-overview.md#service-aided-subnet-configuration)adlı işlevlerle elde edilir.
Bu, bir SLA 'nın yerine getirilmesi için yönetim trafiğinin kesintisiz olarak akmasını sağlamaktır.

**Gelen yönetim trafiği için kullanılan kaynak IP aralıklarını alabilir miyim?**

Evet. [Ağ İzleyicisi akış günlüklerini yapılandırarak](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#analyze-traffic-to-or-from-a-network-security-group)ağlar güvenlik grubunuzdan gelen trafiği çözümleyebilirsiniz.

**NSG 'yi veri uç noktasına (bağlantı noktası 1433) erişimi denetlemek için ayarlayabilir miyim?**

Evet. Yönetilen bir örnek sağlandıktan sonra, 1433 numaralı bağlantı noktasına gelen erişimi denetleyen NSG 'yi ayarlayabilirsiniz. IP aralığını mümkün olduğunca daraltmak önerilir.

**NVA veya şirket içi güvenlik duvarını, FQDN 'Lere göre giden yönetim trafiğini filtrelemek için ayarlayabilir miyim?**

Hayır. Bu, birkaç nedenden dolayı desteklenmez:
-   Gelen yönetim isteğine yanıtı temsil eden yönlendirme trafiği asimetrik olur ve çalışmayabilir.
-   Depolama alanına giden yönlendirme trafiği aktarım hızı kısıtlamalarından ve gecikmeden etkilenerek beklenen hizmet kalitesini ve kullanılabilirliğini sağlayamayacağız.
-   Deneyim temelinde, bu yapılandırmalarda hata yaşılır ve supportable değildir.

**Giden yönetim dışı trafik için NVA veya güvenlik duvarını ayarlayabilir miyim?**

Evet. Bunu başarmanın en kolay yolu, trafiği NVA aracılığıyla yönlendirmek için yönetilen örnek alt ağıyla ilişkili bir UDR 'ye 0/0 kuralı eklemektir.
 
**Yönetilen bir örnek için kaç IP adresine ihtiyacım var?**

Alt ağda yeterli sayıda kullanılabilir [IP adresi](connectivity-architecture-overview.md#network-requirements)olmalıdır. SQL yönetilen örneği için VNet alt ağ boyutunu belirlemekte, bkz. [yönetilen örnek için gereken alt ağ boyutunu ve aralığını belirleme](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-determine-size-vnet-subnet). 

**Örnek güncelleştirme işlemini gerçekleştirmek için yeterli IP adresi yoksa ne olacak?**

Yönetilen örneğinizin sağlandığı alt ağda yeterli [IP adresi](connectivity-architecture-overview.md#network-requirements) yoksa, içinde yeni bir alt ağ ve yeni bir yönetilen örnek oluşturmanız gerekir. Ayrıca, yeni alt ağın daha fazla IP adresi ayrılmış olarak oluşturulmasını önerdiğimiz için, gelecekteki güncelleştirme işlemleri benzer durumlardan kaçınacaktır. Yeni örnek sağlandıktan sonra, eski ve yeni örnekler arasında verileri el ile yedekleyebilir veya geri yükleyebilir ya da çapraz örnek [zaman içinde geri yükleme](point-in-time-restore.md?tabs=azure-powershell)gerçekleştirebilirsiniz.

**Yönetilen bir örnek oluşturmak için boş bir alt ağa ihtiyacım var mı?**

Hayır. Boş bir alt ağ ya da zaten yönetilen örnek içeren bir alt ağ kullanabilirsiniz. 

**Alt ağ adres aralığını değiştirebilir miyim?**

İçinde yönetilen örnekler varsa. Bu bir Azure ağ altyapısı kısıtlamasıdır. Yalnızca [boş bir alt ağa ek adres alanı ekleme](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-subnet#change-subnet-settings)izni verilir. 

**Yönetilen örnekten başka bir alt ağa taşıyabilir miyim?**

Hayır. Bu, geçerli bir yönetilen örnek tasarım kısıtlamasıdır. Ancak, başka bir alt ağda yeni bir örnek sağlayabilir ve verileri, eski ve yeni örnek arasında el ile yedekleyebilir ya da çapraz örnek [zaman içinde geri yükleme](point-in-time-restore.md?tabs=azure-powershell)gerçekleştirebilirsiniz.

**Yönetilen bir örnek oluşturmak için boş bir sanal ağa ihtiyacım var mı?**

Bu gerekli değildir. Azure [SQL yönetilen örneği için bir sanal ağ oluşturabilir](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-create-vnet-subnet) veya [var olan bir sanal ağı Azure SQL yönetilen örneği için yapılandırabilirsiniz](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-configure-vnet-subnet).

**Yönetilen bir örneği bir alt ağdaki diğer hizmetlere yerleştirebilir miyim?**

Hayır. Şu anda, yönetilen örneği zaten diğer kaynak türlerini içeren bir alt ağda yerleştirmeyi desteklemiyoruz.

## <a name="connectivity"></a>Bağlantı 

**Yönetilen örneğime IP adresini kullanarak bağlanabilir miyim?**

Hayır, bu desteklenmiyor. Yönetilen bir örneğin ana bilgisayar adı, yönetilen örneğin sanal kümesinin önünde yük dengeleyiciye eşlenir. Bir sanal küme birden çok yönetilen örneği barındırabildiğinden, bir bağlantı, adını belirtmeden doğru yönetilen örneğe yönlendirilemez.
SQL yönetilen örnek sanal küme mimarisi hakkında daha fazla bilgi için bkz. [sanal küme bağlantısı mimarisi](connectivity-architecture-overview.md#virtual-cluster-connectivity-architecture).

**Yönetilen bir örnek statik IP adresine sahip olabilir mi?**

Bu şu anda desteklenmiyor.

Nadir ancak gerekli durumlarda, yönetilen bir örnek için yeni bir sanal kümeye çevrimiçi geçiş yapmanız gerekebilir. Gerekirse, bu geçiş, teknoloji yığınımızda hizmetin güvenliğini ve güvenilirliğini artırmaya yönelik değişiklikler nedeniyle oluşur. Yeni bir sanal kümeye geçiş, yönetilen örnek ana bilgisayar adıyla eşlenen IP adresinin değiştirilmesine neden olur. Yönetilen örnek hizmeti statik IP adresi desteğini talep etmez ve normal bakım döngülerinin bir parçası olarak fark etmeden değiştirme hakkını saklı tutar.

Bu nedenle, IP adresinin gereksiz kapalı kalma süresine neden olabileceği için IP adresi 'nin dengeszlik düzeyini önemli bir şekilde ele geçirmesini kesinlikle önermiyoruz.

**Yönetilen örnek genel bir uç noktaya sahip mi?**

Evet. Yönetilen örnek, varsayılan olarak yalnızca hizmet yönetimi için kullanılan genel bir uç noktaya sahiptir, ancak müşteri bu hizmeti veri erişimi için de etkinleştirebilir. Daha ayrıntılı bilgi için bkz. [genel uç NOKTALARLA SQL yönetilen örneği kullanma](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-public-endpoint-securely). Ortak uç noktayı yapılandırmak için, [SQL yönetilen örneği 'nde ortak uç noktayı yapılandırma](public-endpoint-configure.md)bölümüne gidin.

**Yönetilen örnek denetimi genel uç noktaya nasıl erişebilir?**

Yönetilen örnek, hem ağ hem de uygulama düzeyinde genel uç noktaya erişimi denetler.

Yönetim ve Dağıtım Hizmetleri, bir dış yük dengeleyiciye eşlenen bir [Yönetim uç noktası](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-connectivity-architecture#management-endpoint) kullanarak yönetilen örneğe bağlanır. Trafik yalnızca yönetilen örneğin yönetim bileşenlerinin kullandığı önceden tanımlanmış bir bağlantı noktası kümesi üzerinde alındığında düğümlere yönlendirilir. Düğümlerdeki yerleşik bir güvenlik duvarı yalnızca Microsoft IP aralıklarından gelen trafiğe izin verecek şekilde ayarlanmıştır. Sertifikalar, Yönetim bileşenleri ile yönetim düzlemi arasındaki iletişimin hepsini karşılıklı olarak doğrular. Daha ayrıntılı bilgi için bkz. [SQL yönetilen örneği Için bağlantı mimarisi](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-connectivity-architecture#virtual-cluster-connectivity-architecture).

**Yönetilen örnek veritabanlarındaki verilere erişmek için genel uç noktasını kullanabilir miyim?**

Evet. Müşterinin, [Azure Portal](public-endpoint-configure.md#enabling-public-endpoint-for-a-managed-instance-in-the-azure-portal)PowerShell/ARM 'den ortak uç nokta veri erişimini etkinleştirmesi  /  [PowerShell](public-endpoint-configure.md#enabling-public-endpoint-for-a-managed-instance-using-powershell) ve NSG 'yi, veri bağlantı noktasına (bağlantı noktası numarası 3342) erişimi kilitleyecek şekilde yapılandırması gerekir. Daha fazla bilgi için bkz. [Azure SQL yönetilen örneği 'nde ortak uç noktayı yapılandırma](public-endpoint-configure.md) ve [genel uç noktayla GÜVENLI şekilde Azure SQL yönetilen örneği kullanma](public-endpoint-overview.md). 

**SQL Data Endpoint için özel bir bağlantı noktası belirtebilir miyim?**

Hayır, bu seçenek kullanılamaz.  Özel veri uç noktası için, yönetilen örnek varsayılan 1433 numaralı bağlantı noktası numarasını kullanır ve genel veri uç noktası için, yönetilen örnek varsayılan bağlantı noktası 3342 numarasını kullanır.

**Farklı bölgelere yerleştirilmiş yönetilen örnekleri bağlamak için önerilen yol nedir?**

Hızlı rota devresi eşlemesi bunu yapmanın tercih edilen yoludur. Bu, iç yük dengeleyici ile ilgili [kısıtlama](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview)nedeniyle desteklenmeyen bölgeler arası sanal ağ eşlemesi ile birlikte kullanılamaz.

Hızlı rota devresi eşlemesi mümkün değilse, siteden siteye VPN bağlantısı ([Azure Portal](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal), [POWERSHELL](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell), [Azure CLI](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli)) oluşturmak diğer tek seçenektir.


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


## <a name="dns"></a>DNS

**SQL yönetilen örneği için özel bir DNS yapılandırabilir miyim?**

Evet. Bkz. [Azure SQL yönetilen örneği Için özel DNS yapılandırma](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-custom-dns).

**DNS yenileme yapabilir miyim?**

Şu anda SQL yönetilen örneği için DNS sunucusu yapılandırmasını yenileme özelliği sağlamayız.

DNS yapılandırması sonunda yenilenir:

- DHCP kira süresi dolar.
- Platform yükseltmesi üzerinde.

Geçici bir çözüm olarak, SQL yönetilen örneği 4 sanal çekirdeğe indirgekten sonra yeniden yükseltin. Bunun, DNS yapılandırmasını yenilemeyi yan etkisi vardır.


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

## <a name="purchasing-models-and-benefits"></a>Modelleri ve avantajları satın alma

**SQL yönetilen örneği için hangi satın alma modelleri kullanılabilir?**

SQL yönetilen örneği, [sanal çekirdek tabanlı satın alma modeli](sql-managed-instance-paas-overview.md#vcore-based-purchasing-model)sunar.

**SQL yönetilen örneği için hangi maliyet avantajları mevcuttur?**

Azure SQL avantajları ile maliyetleri aşağıdaki yollarla kaydedebilirsiniz:
-   Şirket içi lisanslarda mevcut yatırımlarını en üst düzeye çıkarın ve [Azure hibrit avantajı](https://docs.microsoft.com/azure/azure-sql/azure-hybrid-benefit?tabs=azure-powershell)yüzde 55 ' ye kadar tasarruf edin. 
-   İşlem kaynakları için bir ayırmaya işleyin ve [ayrılmış örnek avantajı](https://docs.microsoft.com/azure/sql-database/sql-database-reserved-capacity)ile yüzde 33 ' a kadar tasarruf edin. Bunu, yüzde 82 ' e varan tasarruf için Azure hibrit avantajı ile birleştirin. 
-   Devam eden geliştirme ve test iş yükleriniz için indirimli ücretler sunan [Azure geliştirme ve test fiyatlandırma avantajına](https://azure.microsoft.com/pricing/dev-test/) göre yüzde 55 ' a varan bir ücret kazanın.

**Ayrılmış örnek avantajı kimler için uygun?**

Ayrılmış örnek avantajına uygun olması için, abonelik türü bir kurumsal anlaşma (teklif numaraları: MS-AZR-0017P veya MS-AZR-0148P) veya kullandıkça öde fiyatlandırması (teklif numaraları: MS-AZR-0003P veya MS-AZR-0023P) içeren tek bir anlaşma olmalıdır. Ayırmalar hakkında daha fazla bilgi için bkz. [ayrılmış örnek avantajı](https://docs.microsoft.com/azure/sql-database/sql-database-reserved-capacity). 

**Rezervasyonları iptal edebilir, Exchange veya para iadesi yapılabilir mi?**

Belirli sınırlamalara sahip rezervasyonları iptal edebilir, Exchange veya iade edebilirsiniz. Daha fazla bilgi için bkz. [Azure Ayrılmış Sanal Makine Örnekleri için self servis değişimler ve para iadeleri](https://docs.microsoft.com/azure/cost-management-billing/reservations/exchange-and-refund-azure-reservations).

## <a name="billing-for-managed-instance-and-backup-storage"></a>Yönetilen örnek ve yedekleme depolaması için faturalandırma

**SQL yönetilen örnek fiyatlandırma seçenekleri nelerdir?**

Yönetilen örnek fiyatlandırma seçeneklerini araştırmak için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/azure-sql/sql-managed-instance/single/).

**Yönetilen örneğimin fatura maliyetini nasıl izleyebilirim?**

Bunu [Azure maliyet yönetimi çözümünü](https://docs.microsoft.com/azure/cost-management-billing/)kullanarak yapabilirsiniz. [Azure Portal](https://portal.azure.com) **abonelikler** ' e gidin ve **Maliyet Analizi**' ni seçin. 

**Birikmiş maliyetler** seçeneğini kullanın ve ardından **kaynak türüne** göre filtreleyin `microsoft.sql/managedinstances` .

**Ne kadar otomatik yedekleme maliyetlidir?**

Yedekleme Bekletme dönemi kümesinden bağımsız olarak satın alınan ayrılmış veri depolama alanı olarak eşit miktarda ücretsiz yedekleme depolama alanı elde edersiniz. Yedekleme depolama tüketiğiniz ayrılan boş yedekleme depolama alanı içindeyse, yönetilen örnekteki otomatik yedeklemelerin sizin için ek maliyeti olmaz ve bu nedenle ücretsiz olarak ücretlendirilecektir. Yedekleme depolama alanının üzerinde boş alanın kullanımı aşıldıktan sonra ABD bölgelerindeki $0,20-$0,24 arasındaki maliyetler ve bölgenizdeki Ayrıntılar için fiyatlandırma sayfasına bakın. Daha fazla ayrıntı için bkz. [yedekleme depolama tüketimi açıklanmıştı](https://techcommunity.microsoft.com/t5/azure-sql-database/backup-storage-consumption-on-managed-instance-explained/ba-p/1390923).

**Yedekleme depolama tüketimin fatura maliyetini nasıl izleyebilirim?**

Azure Portal aracılığıyla yedekleme depolama maliyetini izleyebilirsiniz. Yönergeler için bkz. [otomatik yedeklemeler Için izleme maliyetleri](https://docs.microsoft.com/azure/azure-sql/database/automated-backups-overview?tabs=managed-instance#monitor-costs). 

**Yönetilen örnekteki yedekleme depolama maliyetlerimi nasıl iyileştirebilirim?**

Yedekleme depolama maliyetlerinizi iyileştirmek için bkz. [SQL yönetilen örneği üzerinde ince yedekleme ayarları](https://techcommunity.microsoft.com/t5/azure-sql-database/fine-tuning-backup-storage-costs-on-managed-instance/ba-p/1390935).

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

## <a name="azure-feedback-and-support"></a>Azure geri bildirim ve destek

**SQL yönetilen örnek geliştirmeleri için fikirlerinizi nereden bırakabilirim?**

Yeni bir yönetilen örnek özelliği için oy verebilir veya [SQL yönetilen örnek geri bildirim forumuna](https://feedback.azure.com/forums/915676-sql-managed-instance)oylama üzerine yeni bir geliştirme fikrini koyabilirsiniz. Bu şekilde, ürün geliştirmeye katkıda bulunabilir ve olası geliştirmelerimizi önceliklendirmemize yardımcı olabilirsiniz.

**Azure destek isteği 'ni nasıl oluşturabilirim?**

Azure destek isteği oluşturmayı öğrenmek için bkz. [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request).

