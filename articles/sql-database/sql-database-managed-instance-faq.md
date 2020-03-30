---
title: Yönetilen örnekler hakkında SSS
description: SQL Veritabanı yönetilen örnek sık sorulan sorular (SSS)
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein, carlrab
ms.date: 03/17/2020
ms.openlocfilehash: 393d67b200a4f8d44cb001b3a7e2e491209e9d58
ms.sourcegitcommit: 07d62796de0d1f9c0fa14bfcc425f852fdb08fb1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80364161"
---
# <a name="sql-database-managed-instance-frequently-asked-questions-faq"></a>SQL Veritabanı yönetilen örnek sık sorulan sorular (SSS)

Bu makalede, [SQL Veritabanı yönetilen örnek](sql-database-managed-instance.md)hakkında en sık sorulan soruların çoğunu içerir.

## <a name="supported-features"></a>Desteklenen özellikler

**Yönetilen örnekte desteklenen özelliklerin listesini nerede bulabilirim?**

Yönetilen örnekte desteklenen özelliklerin listesi için Azure [SQL Veritabanı ile SQL Server'a](sql-database-features.md)bakın.

Azure SQL Veritabanı yönetilen örnek ve şirket içi SQL Server arasındaki sözdizimi ve davranış farklılıkları [için, SQL Server'dan T-SQL farklılıklarına](sql-database-managed-instance-transact-sql-information.md)bakın.


## <a name="tech-spec--resource-limits"></a>Teknoloji spec kaynak limitleri &
 
**Yönetilen örnek için teknik özellikleri ve kaynak sınırlarını nerede bulabilirim?**

Kullanılabilir donanım oluşturma özellikleri için, [donanım nesillerinde teknik farklılıklar](sql-database-managed-instance-resource-limits.md#hardware-generation-characteristics)bakın.
Kullanılabilir hizmet katmanları ve özellikleri [için, hizmet katmanları arasındaki teknik farklılıklara](sql-database-managed-instance-resource-limits.md#service-tier-characteristics)bakın.

## <a name="known-issues--bugs"></a>Hataları& bilinen sorunlar

**Bilinen sorunları ve hataları nerede bulabilirim?**

Hatalar ve bilinen sorunlar için [bilinen sorunlara](sql-database-release-notes.md#known-issues)bakın.

## <a name="new-features"></a>Yeni özellikler

**Genel önizlemede en son özellikleri ve özellikleri nerede bulabilirim?**

Yeni ve önizleme özellikleri için [sürüm notlarına](sql-database-release-notes.md?tabs=managed-instance)bakın.

## <a name="deployment-times"></a>Dağıtım süreleri 

**Örnek oluşturmak veya güncelleştirmek veya bir veritabanını geri yüklemek ne kadar zaman alır?**

Yeni yönetilen örnek oluşturmak veya hizmet katmanını (vCores, depolama) değiştirmek için beklenen süre çeşitli etkenlere bağlıdır. [Yönetim işlemlerine](/azure/sql-database/sql-database-managed-instance#managed-instance-management-operations) bir göz atın 

## <a name="naming-convention"></a>Adlandırma kuralı

**Yönetilen bir örnek, şirket içi SQL Server ile aynı ada sahip olabilir mi?**

Yönetilen örnek adı değiştirilmez.

Yönetilen örnek varsayılan DNS zone *.database.windows.net* değiştirilebilir. 

Varsayılan yerine başka bir DNS bölgesi kullanmak için, örneğin, *.contoso.com*: 
- Takma ad tanımlamak için CliConfig'i kullanın. Araç sadece bir kayıt defteri ayarları sarıcı, bu nedenle grup ilkesi veya komut dosyası kullanılarak da yapılabilir.
- *TrustServerCertificate=true* seçeneği ile *CNAME'yi* kullanın.

## <a name="move-db-from-mi"></a>DB'yi MI'den taşıyın 

**Veritabanını yönetilen örnekten SQL Server veya Azure SQL Veritabanı'na nasıl taşıyabilirim?**

[Veritabanını BACPAC'a dışa aktarıp](sql-database-export.md) [BACPAC dosyasını içe aktarabilirsiniz.]( sql-database-import.md) Veritabanınız 100 GB'tan küçükse, bu önerilen bir yaklaşımdır.

Veritabanındaki tüm tabloların birincil anahtarları varsa işlem çoğaltma kullanılabilir.

Yönetilen `COPY_ONLY` örnekten alınan yerel yedeklemeler SQL Server'a geri yüklenemez, çünkü yönetilen örnek SQL Server'a göre daha yüksek bir veritabanı sürümüne sahiptir.

## <a name="migrate-instance-db"></a>Geçiş örneği DB

**Örnek veritabanımı tek bir Azure SQL Veritabanına nasıl geçirebilirim?**

Bir seçenek [bir BACPAC](sql-database-export.md) veritabanı dışa aktarmak ve sonra [BACPAC dosyasını almaktır.](sql-database-import.md) 

Veritabanınız 100 GB'tan küçükse, bu önerilen yaklaşımdır. Veritabanındaki tüm tabloların birincil anahtarları varsa işlem çoğaltma kullanılabilir.

## <a name="switch-hardware-generation"></a>Donanım oluşturma yı değiştirme 

**Yönetilen örnek donanım neslimi Gen 4 ve Gen 5 arasında çevrimiçi olarak değiştirebilir miyim?**

Yönetilen örneğinizin sağlandığı bölgede her iki donanım nesli de mevcutsa, donanım nesilleri arasında otomatik çevrimiçi geçiş mümkündür. Bu durumda, donanım nesilleri arasında nasıl geçiş yapılacağını açıklayan [vCore modeline genel bakış sayfasını](sql-database-service-tiers-vcore.md) denetleyebilirsiniz.

Bu, arka planda yeni yönetilen bir örnek sağlanacaktır ve veritabanları otomatik olarak işlemin sonunda hızlı bir hata ile eski ve yeni örnek arasında aktarılacak gibi uzun süren bir işlemdir. 

Her iki donanım nesli de aynı bölgede desteklenmiyorsa, donanım oluşturmayı değiştirmek mümkündür, ancak el ile yapılması gerekir. Bu, istenen donanım oluşturmanın kullanılabildiği bölgede yeni bir örnek sağlamanızı ve eski ve yeni örnek arasındaki verileri el ile yedeklemenizi ve geri yüklemenizi gerektirir.


## <a name="tune-performance"></a>Performansı ayarlama

**Yönetilen örneğimin performansını nasıl akort edebilirim?**

Genel Amaç yönetilen örnek, performans için önemli olan veri ve günlük dosyalarının boyutu nedeniyle uzak depolama yı kullanır. Daha fazla bilgi için bkz: [Günlük dosya boyutunun Genel Amaçlı Yönetilen Örnek performansıüzerindeki etkisi.](https://medium.com/azure-sqldb-managed-instance/impact-of-log-file-size-on-general-purpose-managed-instance-performance-21ad170c823e)

İş yükünüz çok sayıda küçük işlemden oluşuyorsa, bağlantı türünü proxy'den yeniden yönlendirme moduna geçmeyi düşünün.

## <a name="maximum-storage-size"></a>Maksimum depolama boyutu

**Yönetilen örnek için maksimum depolama boyutu nedir?**

Yönetilen örnek için depolama boyutu, seçili hizmet katmanına (Genel Amaç veya İş Açısından Kritik) bağlıdır. Bu hizmet katmanlarının depolama sınırlamaları için [Hizmet katmanı özelliğine](sql-database-service-tiers-general-purpose-business-critical.md)bakın.

## <a name="back-up-storage-cost"></a>Depolama maliyetini yedekleme 

**Yedek depolama yönetilen örnek depolamamdan düşülür mü?**

Hayır, yedek depolama yönetilen örnek depolama alanınızdan düşülmez. Yedekleme depolama örneği depolama alanından bağımsızdır ve boyutu sınırlı değildir. Yedekleme depolama, 7 ila 35 gün arasında yapılandırılabilir örnek veritabanlarınızın yedeklemesini korumak için süreyle sınırlıdır. Ayrıntılar [için, Otomatik yedeklemelere](https://docs.microsoft.com/azure/sql-database/sql-database-automated-backups)bakın.

## <a name="track-billing"></a>Faturalandırmayı izleme

**Yönetilen örneğim için faturalandırma maliyetimi izlemenin bir yolu var mı?**

Bunu Azure Maliyet [Yönetimi çözümlerini](/azure/cost-management/)kullanarak yapabilirsiniz. [Azure portalındaki](https://portal.azure.com) **Aboneliklere** gidin ve **Maliyet Analizi'ni**seçin. 

Birikmiş **maliyetler** seçeneğini kullanın ve **kaynak türüne** göre filtreleme . `microsoft.sql/managedinstances` 
  
## <a name="inbound-nsg-rules"></a>Gelen NSG kuralları

**Yönetim bağlantı noktalarında gelen NSG kurallarını nasıl ayarlayabilirim?**

Yönetilen örnek kontrol düzlemi, yönetim bağlantı noktalarını koruyan NSG kurallarını korur.

Yönetim bağlantı noktaları nın ne için kullanıldığı aşağıda açıklanmaktadır:

9000 ve 9003 portları Service Fabric altyapısı tarafından kullanılmaktadır. Service Fabric birincil rolü sanal küme sağlıklı tutmak ve bileşen yinelemesayısı açısından hedef durumu tutmaktır.

1438, 1440 ve 1452 numaralı bağlantı noktaları düğüm aracısı tarafından kullanılır. Düğüm aracısı küme içinde çalışan ve yönetim komutları yürütmek için denetim düzlemi tarafından kullanılan bir uygulamadır.

NSG kurallarına ek olarak yerleşik güvenlik duvarı ağ katmanındaki örneği korur. Uygulama katmanında iletişim sertifikalarla korunur.
  
Daha fazla bilgi ve yerleşik güvenlik duvarını nasıl doğrulayınız için Azure [SQL Veritabanı'nın yerleşik güvenlik duvarı tarafından yönetilen örneğine](sql-database-managed-instance-management-endpoint-verify-built-in-firewall.md)bakın.


## <a name="mitigate-data-exfiltration-risks"></a>Veri sızma risklerini azaltmak  

**Veri sızma risklerini nasıl azaltabilirim?**

Veri sızma risklerini azaltmak için, müşterilerin bir dizi güvenlik ayarı ve denetimi uygulaması önerilir:

- Tüm veritabanlarında [Saydam Veri Şifreleme 'yi (TDE)](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-azure-sql) açın.
- Ortak Dil Çalışma Süresini (CLR) kapatın. Bu, şirket içinde de önerilir.
- Yalnızca Azure Etkin Dizini (AAD) kimlik doğrulamasını kullanın.
- Düşük ayrıcalıklı DBA hesabı yla erişim örneği.
- Sysadmin hesabı için JiT atlama kutusu erişimini yapılandırın.
- SQL [denetimini](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine)açın ve uyarı mekanizmalarıyla tümleştirin.
- [Gelişmiş Veri Güvenliği (ADS)](https://docs.microsoft.com/azure/sql-database/sql-database-advanced-data-security) paketinden [Tehdit Algılama'yı](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection) açın.


## <a name="cost-saving-use-cases"></a>Maliyet tasarrufu kullanım örnekleri

**Yönetilen örnekle kullanım örneklerini ve ortaya çıkan maliyet tasarruflarını nerede bulabilirim?**

Yönetilen örnek vaka çalışmaları:

- [Komatsu](https://customers.microsoft.com/story/komatsu-australia-manufacturing-azure)
- [KMD](https://customers.microsoft.com/en-ca/story/kmd-professional-services-azure-sql-database)
- [PowerDETAILS](https://customers.microsoft.com/story/powerdetails-partner-professional-services-azure-sql-database-managed-instance)
- [Allscripts](https://customers.microsoft.com/story/allscripts-partner-professional-services-azure)  
Azure SQL Veritabanı yönetilen örneğini dağıtmayla ilgili yararları, maliyetleri ve riskleri daha iyi anlamak için forrester'ın çalışması da vardır: [MI'nin Toplam Ekonomik Etkisi.](https://azure.microsoft.com/resources/forrester-tei-sql-database-managed-instance)


## <a name="dns-refresh"></a>DNS Yenileme 

**DNS yenileme yapabilir miyim?**

Şu anda, yönetilen örneğin DNS sunucu yapılandırmasını yenilemek için bir özellik sunmayız.

DNS yapılandırması sonunda yenilenir:

- DHCP kira süresi dolduğunda.
- Platform yükseltmede.

Geçici çözüm olarak, yönetilen örneği 4 vCore'a düşürün ve daha sonra yeniden yükseltin. Bu DNS yapılandırmasını yenilemenin bir yan etkisi vardır.


## <a name="ip-address"></a>IP adresi

**IP adresini kullanarak yönetilen örneğe bağlanabilir miyim?**

IP adresini kullanarak yönetilen örneğe bağlanma desteklenmez. Yönetilen örnek ana bilgisayar adı eşlemleri yönetilen örnek sanal küme nin önüne bakiyeyi yüklemek için. Bir sanal küme birden çok yönetilen örneği barındırabildiği için, bağlantı adını belirtmeden uygun yönetilen örneğe yönlendirilemedi.

Yönetilen örnek sanal küme mimarisi hakkında daha fazla bilgi için [Sanal küme bağlantı mimarisine](sql-database-managed-instance-connectivity-architecture.md#virtual-cluster-connectivity-architecture)bakın.

**Yönetilen bir örnekstatik bir IP adresine sahip olabilir mi?**

Nadir ama gerekli durumlarda, yönetilen bir örneğin yeni bir sanal kümeye çevrimiçi geçiş yapması gerekebilir. Gerekirse, bu geçiş, hizmetin güvenliğini ve güvenilirliğini artırmayı amaçlayan teknoloji yığınımızdaki değişikliklerden kaynaklanıyor. Yeni bir sanal kümeye geçiş, yönetilen örnek ana bilgisayar adına eşlenen IP adresinin değiştirilmesiyle sonuçlanır. Yönetilen örnek hizmeti statik IP adresi desteği talep etmez ve düzenli bakım döngülerinin bir parçası olarak bildirimde bulunmaksızın değiştirme hakkını saklı tutar.

Bu nedenle, gereksiz kapalı kalma süresine neden olabileceğinden IP adresinin işe alınamaması konusunda güçlü bir şekilde öneriliyoruz.

## <a name="change-time-zone"></a>Saat dilimini değiştirme

**Varolan yönetilen bir örneğin saat dilimini değiştirebilir miyim?**

Yönetilen bir örnek ilk kez sağlandığında saat dilimi yapılandırması ayarlanabilir. Varolan yönetilen örneğin saat dilimini değiştirme desteklenmez. Ayrıntılar için [saat dilimi sınırlamaları'na](sql-database-managed-instance-timezone.md#limitations)bakın.

Geçici geçici çözüm, uygun saat dilimiyle yeni yönetilen bir örnek oluşturmayı ve ardından el ile yedekleme ve geri yükleme gerçekleştirmeyi veya önerdiğimiz şeyi, [zaman içinde çapraz](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/06/07/cross-instance-point-in-time-restore-in-azure-sql-database-managed-instance/)bir geri yükleme gerçekleştirmeyi içerir.


## <a name="resolve-performance-issues"></a>Performans sorunlarını çözme

**Yönetilen örneğimle performans sorunlarını nasıl çözebilirim?**

Yönetilen örnek ve SQL Server arasında bir performans karşılaştırması için iyi bir başlangıç noktası, Azure SQL yönetilen örneği ile SQL Server makalesi [arasında performans karşılaştırması için en iyi uygulamalardır.](https://techcommunity.microsoft.com/t5/Azure-SQL-Database/The-best-practices-for-performance-comparison-between-Azure-SQL/ba-p/683210)

Veri yükleme genellikle zorunlu tam kurtarma modeli ve işlem günlüğü yazma iş kaynağı [sınırları](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-resource-limits#service-tier-characteristics) nedeniyle SQL Server daha yönetilen örnek daha yavaştır. Bazen, bu geçici verileri kullanıcı veritabanı yerine tempdb'ye yükleyerek veya kümelenmiş sütun deposu veya bellek için optimize edilmiş tablolar kullanarak çözülebilir.


## <a name="restore-encrypted-backup"></a>Şifreli yedeklemeyi geri yükleme

**Şifrelenmiş veritabanımı yönetilen örneğe geri yükleyebilir miyim?**

Evet, veritabanınızı yönetilen örneğe geri yükleyebilmek için veritabanınızın şifresini çözmeniz gerekmez. Şifreli yedekleme dosyasındaki verileri okuyabilmek için kaynak sistemde şifreleme anahtarı koruyucusu olarak kullanılan bir sertifika/anahtarı yönetilen örneğe sağlamanız gerekir. Bunu yapmanın iki olası yolu vardır:

- *Sertifika koruyucuyu yönetilen örne yükleyin.* Sadece PowerShell kullanılarak yapılabilir. [Örnek komut dosyası](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-migrate-tde-certificate) tüm işlemi açıklar.
- *Asimetrik anahtar koruyucusu Azure Key Vault'a (AKV) yükleyin ve yönetilen örneği işaret edin.* Bu yaklaşım, şifreleme anahtarını depolamak için AKV tümleştirmesini de kullanan kendi anahtarını getir (BYOK) TDE kullanım örneğini andırır. Anahtarı şifreleme anahtarı koruyucusu olarak kullanmak istemiyorsanız ve yalnızca şifreli veritabanı(lar)'ı geri yüklemek için yönetilen örnek için anahtarı kullanılabilir hale getirmek istiyorsanız, [BYOK TDE'yi ayarlama](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-azure-sql#manage-transparent-data-encryption)yönergelerini izleyin ve onay kutusunu denetlemeyin *varsayılan TDE koruyucusu yapın*.

Şifreleme koruyucusunu yönetilen örnek için kullanılabilir hale getirince, standart veritabanı geri yükleme yordamını uygulayabilirsiniz.

## <a name="migrate-from-single-db"></a>Tek DB'den geçiş 

**Yönetilen örneğin Azure SQL Veritabanı tek veya esnek havuzundan nasıl geçirebilirim?**

Yönetilen örnek, Azure SQL Veritabanı'nın diğer dağıtım seçenekleriyle aynı performans düzeylerini hesaplama ve depolama boyutu başına sunar. Verileri tek bir örnekte birleştirmek istiyorsanız veya yalnızca yönetilen örnekte desteklenen bir özelliğe ihtiyacınız varsa, dışa aktarma/alma (BACPAC) işlevlerini kullanarak verilerinizi geçirebilirsiniz.
