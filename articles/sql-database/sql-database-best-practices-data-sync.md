---
title: Data Sync için en iyi deneyimler
description: Azure SQL Veri Eşitlemesini yapılandırmak ve çalıştırmak için en iyi uygulamalar hakkında bilgi edinin.
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: allenwux
ms.author: xiwu
ms.reviewer: carlrab
ms.date: 12/20/2018
ms.openlocfilehash: ee929fa227cb105b73bc929c13a768aabef37ce3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75771692"
---
# <a name="best-practices-for-sql-data-sync"></a>SQL Data Sync için en iyi deneyimler 

Bu makalede, Azure SQL Veri Eşitleme için en iyi uygulamalar açıklanmaktadır.

SQL Data Sync hizmetine genel bakış için bkz. [Azure SQL Data Sync ile birden fazla bulut ve şirket içi veritabanı arasında veri eşitleme](sql-database-sync-data.md).

> [!IMPORTANT]
> Azure SQL Veri Eşitlemesi şu anda Azure SQL Veritabanı Yönetilen Örneği'ni **desteklemez.**

## <a name="security-and-reliability"></a><a name="security-and-reliability"></a>Güvenlik ve güvenilirlik

### <a name="client-agent"></a>İstemci aracısı

-   Ağ hizmeti erişimi olan en az ayrıcalıklı kullanıcı hesabını kullanarak istemci aracısını yükleyin.  
-   İstemci aracısını şirket içi SQL Server bilgisayarı olmayan bir bilgisayara yükleyin.  
-   Birden fazla aracıyla şirket içi veritabanını kaydetmeyin.    
    -   Farklı eşitleme grupları için farklı tabloları eşitleseniz bile bunu önleyebilirsiniz.  
    -   Bir şirket içi veritabanını birden çok istemci aracısıyla kaydetmek, eşitleme gruplarından birini sildiğinizde zorluklar alabiliyor.

### <a name="database-accounts-with-least-required-privileges"></a>En az gerekli ayrıcalıklara sahip veritabanı hesapları

-   **Eşitleme kurulumu için.** Tablo Oluştur/Değiştir; Veritabanını Değiştir; Yordam Oluşturma; Seç / Şema değiştir; Kullanıcı Tanımlı Tür oluştur.

-   **Devam eden eşitleme için**. Eşitleme için seçilen tablolara ve eşitleme meta verileri ve izleme tablolarına select/ Ekle/ Güncelle/ Sil; Hizmet tarafından oluşturulan depolanan yordamlar üzerinde izin yürütmek; Kullanıcı tanımlı tablo türlerinde izin çalıştırın.

-   **Deprovisioning için**. Eşitleme nin tablolarında değiştirin; Eşitleme meta veri tablolarında seç/ Sil; Eşitleme izleme tabloları, depolanan yordamlar ve kullanıcı tanımlı türleri denetleme.

Azure SQL Veritabanı yalnızca tek bir kimlik bilgilerini destekler. Bu kısıtlama içinde bu görevleri gerçekleştirmek için aşağıdaki seçenekleri göz önünde bulundurun:

-   Farklı aşamalar için kimlik bilgilerini değiştirin (örneğin, kurulum için *kimlik bilgileri1* ve devam eden için *kimlik bilgileri2).*  
-   Kimlik bilgilerinin iznini değiştirin (diğer bir süre, eşitleme ayarlandıktan sonra izni değiştirin).

## <a name="setup"></a>Kurulum

### <a name="database-considerations-and-constraints"></a><a name="database-considerations-and-constraints"></a>Veritabanı hususları ve kısıtlamaları

#### <a name="sql-database-instance-size"></a>SQL Veritabanı örnek boyutu

Yeni bir SQL Veritabanı örneği oluşturduğunuzda, en büyük boyutu, dağıttığınız veritabanından her zaman daha büyük olacak şekilde ayarlayın. En büyük boyutu dağıtılan veritabanından daha büyük olarak ayarlamazsanız, eşitleme başarısız olur. SQL Veri Eşitlemesi otomatik büyüme sunmasa `ALTER DATABASE` da, oluşturulduktan sonra veritabanının boyutunu artırmak için komutu çalıştırabilirsiniz. SQL Veritabanı örnek boyutu sınırları içinde kaldığınızı sağlayın.

> [!IMPORTANT]
> SQL Data Sync, her veritabanıyla ek meta verileri depolar. Gerekli alanı hesaplarken bu meta verileri hesaba kattığınızdan emin olun. Eklenen ek yükün miktarı tabloların genişliğiyle (örneğin, dar tablolar daha fazla ek ek yükü gerektirir) ve trafik miktarıyla ilgilidir.

### <a name="table-considerations-and-constraints"></a><a name="table-considerations-and-constraints"></a>Tablo da dikkat edilmesi gerekenler ve kısıtlamalar

#### <a name="selecting-tables"></a>Tabloları seçme

Eşitleme grubuna veritabanında bulunan tüm tabloları eklemeniz gerekmez. Eşitleme grubuna eklediğiniz tablolar verimliliği ve maliyetleri etkiler. Yalnızca iş gereksinimleri gerektirdiğinde, tabloları ve bağımlı oldukları tabloları eşitleme grubuna ekleyin.

#### <a name="primary-keys"></a>Birincil anahtarlar

Eşitleme grubundaki her tablonun birincil anahtarı olmalıdır. SQL Veri Eşitleme hizmeti, birincil anahtarı olmayan bir tabloyu eşitleemez.

Üretimde SQL Data Sync kullanmadan önce, ilk ve sürekli eşitleme performansını test edin.

#### <a name="empty-tables-provide-the-best-performance"></a>Boş tablolar en iyi performansı sağlar

Boş tablolar başlatma zamanında en iyi performansı sağlar. Hedef tablo boşsa, Veri Eşitleme verileri yüklemek için toplu kesici uç kullanır. Aksi takdirde, Veri Eşitleme çakışmalarını denetlemek için satır satır karşılaştırma ve ekleme yapar. Ancak performans bir sorun değilse, zaten veri içeren tablolar arasında eşitleme ayarlayabilirsiniz.

### <a name="provisioning-destination-databases"></a><a name="provisioning-destination-databases"></a>Hedef veritabanlarını sağlama

SQL Data Sync temel veritabanı otomatik sağlama sağlar.

Bu bölümde, SQL Veri Eşitleme'de sağlama sınırlamaları anlatılmaktadır.

#### <a name="autoprovisioning-limitations"></a>Otomatik sağlama sınırlamaları

SQL Veri Eşitlemi otomatik sağlama için aşağıdaki sınırlamalar vardır:

-   Yalnızca hedef tabloda oluşturulan sütunları seçin. Eşitleme grubunun parçası olmayan sütunlar hedef tablolarda karşılanır.
-   Dizinler yalnızca seçili sütunlar için oluşturulur. Kaynak tablo dizininde eşitleme grubunun parçası olmayan sütunlar varsa, bu dizinler hedef tablolarda sağlanmış değildir.  
-   XML türü sütunlarında dizinler sağlanmış değil.  
-   ÇEK kısıtlamaları sağlanmıyor.  
-   Kaynak tablolardaki varolan tetikleyiciler sağlanmış değil.  
-   Görünümler ve depolanan yordamlar hedef veritabanında oluşturulmaz.
-   ON UPDATE CASCADE ve ON DELETE CASCADE eylemleri yabancı anahtar kısıtlamaları hedef tablolarda yeniden oluşturulmaz.
-   28'den büyük bir duyarlıkolan ondalık veya sayısal sütunlarınız varsa, SQL Veri Eşitleme eşitleme sırasında bir dönüşüm taşması sorunuyla karşılaşabilir. Ondalık veya sayısal sütunların kesinliğini 28 veya daha az ile sınırlamanızı öneririz.

#### <a name="recommendations"></a>Öneriler

-   Yalnızca hizmeti denerken SQL Veri Eşitleme otomatik sağlama özelliğini kullanın.  
-   Üretim için, veritabanı şema hükmü.

### <a name="where-to-locate-the-hub-database"></a><a name="locate-hub"></a>Hub veritabanını nerede bulabilirsiniz

#### <a name="enterprise-to-cloud-scenario"></a>Kurumsaldan buluta senaryo

Gecikmeyi en aza indirmek için hub veritabanını eşitleme grubunun veritabanı trafiğinin en yoğun olduğu yoğunlığa yakın tutun.

#### <a name="cloud-to-cloud-scenario"></a>Bulut-bulut senaryosu

-   Eşitleme grubundaki tüm veritabanları tek bir veri merkezindeyken, hub aynı veri merkezinde bulunmalıdır. Bu yapılandırma gecikme süresini ve veri merkezleri arasındaki veri aktarım maliyetini azaltır.
-   Eşitleme grubundaki veritabanları birden çok veri merkezinde olduğunda, hub veritabanlarının ve veritabanı trafiğinin çoğuyla aynı veri merkezinde bulunmalıdır.

#### <a name="mixed-scenarios"></a>Karışık senaryolar

Önceki yönergeleri, kurumsaldan buluta ve buluttan buluta senaryoların bir karışımı olan karmaşık eşitleme grubu yapılandırmalarına uygulayın.

## <a name="sync"></a>Sync

### <a name="avoid-slow-and-costly-initial-sync"></a><a name="avoid-a-slow-and-costly-initial-synchronization"></a>Yavaş ve maliyetli ilk eşitlemeden kaçının

Bu bölümde, eşitleme grubunun ilk eşitlemeyi tartışıyoruz. İlk eşitlemenin daha uzun sürmesini ve gerekenden daha pahalı olmasını önlemeye nasıl yardımcı oylamayı öğrenin.

#### <a name="how-initial-sync-works"></a>İlk eşitleme nasıl çalışır?

Eşitleme grubu oluşturduğunuzda, yalnızca bir veritabanındaki verilerle başlayın. Birden çok veritabanında veriniz varsa, SQL Veri Eşitlemesi her satırı çözülmesi gereken bir çakışma olarak ele alar. Bu çakışma çözümü, ilk eşitlemenin yavaş gitmesine neden olur. Birden çok veritabanında veri niz varsa, veritabanı boyutuna bağlı olarak ilk eşitleme birkaç gün ile birkaç ay arasında sürebilir.

Veritabanları farklı veri merkezlerindeyse, her satır farklı veri merkezleri arasında seyahat etmelidir. Bu, ilk eşitlemenin maliyetini artırır.

#### <a name="recommendation"></a>Öneri

Mümkünse, eşitleme grubunun veritabanlarından yalnızca birinde verilerle başlayın.

### <a name="design-to-avoid-sync-loops"></a><a name="design-to-avoid-synchronization-loops"></a>Eşitleme döngülerinden kaçınmak için tasarım

Eşitleme grubu içinde dairesel başvurular olduğunda eşitleme döngüsü oluşur. Bu senaryoda, bir veritabanındaki her değişiklik eşitleme grubundaki veritabanları aracılığıyla sonsuz ve dairesel olarak çoğaltılır.   

Performans düşüşüne neden oldukları ve maliyetleri önemli ölçüde artırabileceğinden eşitleme döngülerinden kaçındığınızdan emin olun.

### <a name="changes-that-fail-to-propagate"></a><a name="handling-changes-that-fail-to-propagate"></a>Yayılmayı başaramayan değişiklikler

#### <a name="reasons-that-changes-fail-to-propagate"></a>Değişikliklerin yayılmaması nın nedenleri

Değişiklikler aşağıdaki nedenlerden biri için yayılmayı başaramayabilir:

-   Şema/datatype uyumsuzluğu.
-   Null olmayan sütunlara null ekleme.
-   Yabancı anahtar kısıtlamalarını ihlal etmek.

#### <a name="what-happens-when-changes-fail-to-propagate"></a>Değişiklikler yayılmayı başaramazsa ne olur?

-   Eşitleme grubu, **uyarı** durumunda olduğunu gösterir.
-   Ayrıntılar portal UI günlük görüntüleyicide listelenir.
-   Sorun 45 gün boyunca çözülmezse, veritabanı güncelliğini yitirir.

> [!NOTE]
> Bu değişiklikler asla yayılmaz. Bu senaryoda kurtarmanın tek yolu eşitleme grubunu yeniden oluşturmaktır.

#### <a name="recommendation"></a>Öneri

Eşitleme grubunu ve veritabanı durumunu portal ve günlük arabirimi üzerinden düzenli olarak izleyin.


## <a name="maintenance"></a>Bakım

### <a name="avoid-out-of-date-databases-and-sync-groups"></a><a name="avoid-out-of-date-databases-and-sync-groups"></a>Güncel olmayan veritabanlarından ve eşitleme gruplarından kaçının

Eşitleme grubundaki bir eşitleme grubu veya veritabanı güncelliğini yitirebilir. Eşitleme grubunun durumu **güncel olmadığında,** çalışmayı durdurur. Bir veritabanının durumu **güncel**olmadığında, veriler kaybolabilir. Bu senaryodan kurtulmaya çalışmak yerine kaçınmak en iyisidir.

#### <a name="avoid-out-of-date-databases"></a>Güncel olmayan veritabanlarından kaçının

Bir veritabanının durumu, 45 gün veya daha uzun süredir çevrimdışı olduğunda **güncel** olmayan bir veritabanı olarak ayarlanır. Veritabanında **güncel olmayan** bir durumu önlemek için, veritabanlarının hiçbirinin 45 gün veya daha uzun süre çevrimdışı olmadığından emin olun.

#### <a name="avoid-out-of-date-sync-groups"></a>Güncel olmayan eşitleme gruplarından kaçının

Eşitleme grubundaki herhangi bir değişikliğin eşitleme grubunun geri kalanına 45 gün veya daha uzun süre yayılmaması durumunda eşitleme grubunun durumu **güncel olmayan** olarak ayarlanır. Eşitleme grubunda **güncel olmayan** bir durumu önlemek için eşitleme grubunun geçmiş günlüğünü düzenli olarak denetleyin. Tüm çakışmaların çözüldüğünden ve değişikliklerin eşitgrup veritabanlarında başarıyla yayÝlýlýlýlýlýlýndan emin olun.

Eşitleme grubu, şu nedenlerden biriyle değişiklik uygulayamayabilir:

-   Tablolar arasında şema uyumsuzluğu.
-   Tablolar arasındaki veri uyumsuzluğu.
-   Null değerlerine izin vermeyen bir sütuna null değeri olan bir satır ekleme.
-   Yabancı anahtar kısıtlamasını ihlal eden bir değere sahip bir satırı güncelleştirme.

Güncel olmayan eşitleme gruplarını önlemek için:

-   Başarısız satırlarda bulunan değerlere izin vermek için şemayı güncelleştirin.
-   Başarısız satırlarda bulunan değerleri içerecek şekilde yabancı anahtar değerlerini güncelleştirin.
-   Başarısız satırdaki veri değerlerini, hedef veritabanındaki şema veya yabancı anahtarlarla uyumlu olacak şekilde güncelleştirin.

### <a name="avoid-deprovisioning-issues"></a><a name="avoid-deprovisioning-issues"></a>Sorunları çözmekten kaçının

Bazı durumlarda, bir veritabanını istemci aracısıyla kayıt dışı etmek eşitlemenin başarısız olmasına neden olabilir.

#### <a name="scenario"></a>Senaryo

1. Eşitleme grubu A, bir SQL Veritabanı örneği ve yerel aracı 1 ile ilişkili şirket içi SQL Server veritabanı kullanılarak oluşturulmuştur.
2. Aynı şirket içi veritabanı yerel aracı 2'ye kaydedilir (bu aracı herhangi bir eşitleme grubuyla ilişkili değildir).
3. Şirket içi veritabanını yerel aracı 2'den kaydetmek, şirket içi veritabanı için eşitleme grubu A için izleme ve meta tabloları kaldırır.
4. Eşitleme grubu A işlemleri, bu hataile başarısız oldu: "Veritabanı eşitleme için sağlanmadığından veya eşitleme yapılandırma tablolarına izinvermedikdiye geçerli işlem tamamlanamadı."

#### <a name="solution"></a>Çözüm

Bu senaryoyu önlemek için, birden fazla aracıyla bir veritabanı nı kaydetmeyin.

Bu senaryodan kurtarmak için:

1. Veritabanını ait olduğu her eşitleme grubundan kaldırın.  
2. Veritabanını kaldırdığınız her eşitleme grubuna geri ekleyin.  
3. Etkilenen her eşitleme grubunu dağıtın (bu eylem veritabanını karşılar).  

### <a name="modifying-a-sync-group"></a><a name="modifying-your-sync-group"></a>Eşitleme grubunu değiştirme

Bir eşitleme grubundan bir veritabanıkaldırmaya çalışmayın ve ardından değişikliklerden birini dağıtmadan eşitleme grubunu edin.

Bunun yerine, önce bir eşitleme grubundan bir veritabanı kaldırın. Ardından, değişikliği dağıtın ve hükmün tamamlanmasını bekleyin. Deprovisioning tamamlandığında, eşitleme grubunu ve değişiklikleri dağıtabilirsiniz.

Bir veritabanını kaldırmayı denerseniz ve sonra değişikliklerden birini dağıtmadan eşitleme grubunu düzenlediyseniz, bir veya diğer işlem başarısız olur. Portal arabirimi tutarsız olabilir. Bu durumda, doğru durumu geri yüklemek için sayfayı yenileyin.

### <a name="avoid-schema-refresh-timeout"></a>Şema yenileme zaman anına kaçınmak

Eşitleme niz gereken karmaşık bir şema varsa, eşitleme meta veri veritabanında daha düşük bir SKU varsa şema yenilemesi sırasında bir işlem zaman ile karşılaşabilirsiniz (örnek: temel). 

#### <a name="solution"></a>Çözüm

Bu sorunu azaltmak için, lütfen eşitleme meta veri veritabanınızı S3 gibi daha yüksek bir SKU'ya sahip olacak şekilde ölçeklendirin. 

## <a name="next-steps"></a>Sonraki adımlar
SQL Veri Eşitlemi hakkında daha fazla bilgi için bkz:

-   Genel Bakış - [Azure SQL Veri Eşitlemi ile verileri birden çok bulut ve şirket içi veritabanları nda eşitleme](sql-database-sync-data.md)
-   Veri Eşitlemeyi Ayarlama
    - Portalda - [Öğretici: Azure SQL Veritabanı ve SQL Server arasında verileri şirket içinde eşitlemek için SQL Veri Eşitlemeyi'ni ayarlayın](sql-database-get-started-sql-data-sync.md)
    - PowerShell ile
        -  [PowerShell kullanarak birden çok Azure SQL veritabanı arasında eşitleme](scripts/sql-database-sync-data-between-sql-databases.md)
        -  [PowerShell kullanarak bir Azure SQL Veritabanı ile SQL Server şirket içi veritabanı arasında eşitleme](scripts/sql-database-sync-data-between-azure-onprem.md)
-   Veri Eşitleme Aracısı - [Azure SQL Veri Eşitleme için Veri Eşitleme Aracısı](sql-database-data-sync-agent.md)
-   Monitör - [Azure Monitor günlükleriyle SQL Veri Eşitlemesini Izleyin](sql-database-sync-monitor-oms.md)
-   Sorun Giderme - [Azure SQL Veri Eşitleme ile sorun giderme](sql-database-troubleshoot-data-sync.md)
-   Eşitleme şeasını güncelleştirme
    -   Transact-SQL ile - [Azure SQL Veri Eşitleme'deki şema değişikliklerinin çoğaltılması otomatikleştirin](sql-database-update-sync-schema.md)
    -   PowerShell ile - [Varolan bir eşitleme grubunda eşitleme şemasını güncellemek için PowerShell'i kullanın](scripts/sql-database-sync-update-schema.md)

SQL Veritabanı hakkında daha fazla bilgi için bkz:

-   [SQL Veritabanına genel bakış](sql-database-technical-overview.md)
-   [Veritabanı yaşam döngüsü yönetimi](https://msdn.microsoft.com/library/jj907294.aspx)
