---
title: T-SQL farklarını çözme-geçiş
description: Azure SQL Veritabanında tam olarak desteklenmeyen Transact-SQL deyimleri
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 12/03/2018
ms.openlocfilehash: e0870ac9dc818ca07e149421b486136c76dd61a4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79208825"
---
# <a name="resolving-transact-sql-differences-during-migration-to-sql-database"></a>SQL Veritabanına geçiş sırasında Transact-SQL farklılıklarının çözümü

[Veritabanınızı](sql-database-single-database-migrate.md) SQL Server'dan Azure SQL Server'a geçirdiğinizde, VERITABANınızın SQL Server'ın geçirilemeden önce yeniden mühendislik gerektirdiğini keşfedebilirsiniz. Bu makalede, hem bu yeniden mühendislik gerçekleştirme ve yeniden mühendislik gerekli neden temel nedenleri anlamak size yardımcı olmak için rehberlik sağlar. Uyumsuzlukları algılamak için [Veri Geçiş Yardımcısı'nı (DMA)](https://www.microsoft.com/download/details.aspx?id=53595)kullanın.

## <a name="overview"></a>Genel Bakış

Uygulamaların kullandığı çoğu Transact-SQL özelliği hem Microsoft SQL Server hem de Azure SQL Veritabanı'nda tam olarak desteklenir. Örneğin, veri türleri, işleçler, dize, aritmetik, mantıksal ve imleç işlevleri gibi temel SQL bileşenleri SQL Server ve SQL Veritabanı'nda aynı şekilde çalışır. Ancak, DDL (veri tanımlı dil) ve DML (veri işleme dili) öğelerinde, yalnızca kısmen desteklenen T-SQL deyimleri ve sorgularla sonuçlanan birkaç T-SQL farkı vardır (bu makalede daha sonra tartışırız).

Buna ek olarak, Azure SQL Veritabanı özellikleri ana veritabanı ve işletim sistemine olan bağımlılıklardan yalıtmak üzere tasarlandığından, hiç desteklenmeyen bazı özellikler ve sözdizimi vardır. Bu nedenle, çoğu sunucu düzeyindeki etkinlikler SQL Veritabanı için uygun değildir. Sunucu düzeyindeki seçenekleri, işletim sistemi bileşenlerini yapılandırdıkları veya dosya sistemi yapılandırması belirtmeleri durumunda T-SQL deyimleri ve seçenekleri kullanılamaz. Bu tür özellikler gerektiğinde, sql veritabanından veya başka bir Azure özelliğinden veya hizmetinden uygun bir alternatif genellikle başka bir şekilde kullanılabilir.

Örneğin, yüksek kullanılabilirlik Azure SQL Veritabanı'nda [Her Zaman Kullanılabilirlik Gruplarına](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server)benzer bir teknoloji kullanılarak oluşturulur. Kullanılabilirlik gruplarıyla ilgili T-SQL deyimleri SQL Veritabanı tarafından desteklenmez ve Her Zaman Kullanılabilirlik Gruplarıyla ilgili dinamik yönetim görünümleri de desteklenmez.

SQL Veritabanı tarafından desteklenen ve desteklenmeyen özelliklerin listesi için [Azure SQL Veritabanı özellik karşılaştırması'na](sql-database-features.md)bakın. Bu sayfadaki liste, yönergeleri ve özellikleri makale ekler ve Transact-SQL deyimleri üzerinde duruluyor.

## <a name="transact-sql-syntax-statements-with-partial-differences"></a>Kısmi farklı transact-SQL sözdizimi deyimleri

Temel DDL (veri tanım dili dili) deyimleri kullanılabilir, ancak bazı DDL deyimlerinde disk yerleşimi ve desteklenmeyen özelliklerle ilgili uzantılar bulunur.

- CREATE ve ALTER DATABASE deyimlerinin üç düzineden fazla seçeneği vardır. İfadeler, yalnızca SQL Server için geçerli olan dosya yerleşimi, FILESTREAM ve servis aracısı seçeneklerini içerir. Geçiş yapmadan önce veritabanları oluştursanız bu önemli olmayabilir, ancak veritabanları oluşturan T-SQL kodunu aktarıyorsanız, kullandığınız tüm seçeneklerin desteklendirildikten emin olmak için [CREATE DATABASE (Azure SQL Database)](https://msdn.microsoft.com/library/dn268335.aspx) ile CREATE SERVER sözdizimi ile CREATE SERVER sözdizimini [(SQL Server Transact-SQL)](https://msdn.microsoft.com/library/ms176061.aspx) karşılaştırmanız gerekir. Azure SQL Veritabanı için CREATE DATABASE ayrıca yalnızca SQL Veritabanı'na uygulanan hizmet objektifi ve esnek ölçek seçeneklerine de sahiptir.
- CREATE ve ALTER TABLE deyimleri, FILESTREAM desteklenmediği için SQL Veritabanı'nda kullanılamayacak FileTable seçeneklerine sahiptir.
- CREATE ve ALTER giriş bildirimleri desteklenir, ancak SQL Veritabanı tüm seçenekleri sunmaz. Veritabanınızı daha taşınabilir hale getirmek için, SQL Veritabanı mümkün olduğunca oturum açma yerine içerdiği veritabanı kullanıcılarını kullanmayı teşvik eder. Daha fazla bilgi için [CREATE/ALTER Gİrİş](https://docs.microsoft.com/sql/t-sql/statements/alter-login-transact-sql) VE [Gİrİşİ YÖNET ve gİrİşi ve kullanıcıları yönet.](sql-database-manage-logins.md)

## <a name="transact-sql-syntax-not-supported-in-azure-sql-database"></a>Azure SQL Veritabanı'nda metişlem-SQL sözdizimi desteklenmiyor

 [Azure SQL Veritabanı özellik karşılaştırmasında](sql-database-features.md)açıklanan desteklenmeyen özelliklerle ilgili Transact-SQL deyimleri ek olarak, aşağıdaki ifadeler ve deyim grupları desteklenmez. Bu nedenle, geçirilecek veritabanınız aşağıdaki özelliklerden birini kullanıyorsa, bu T-SQL özelliklerini ve deyimlerini ortadan kaldırmak için T-SQL'inizi yeniden tasarla.

- Harmanlanmış sistem nesneleri
- İlgili bağlantı: Bitiş noktası deyimleri. SQL Veritabanı Windows kimlik doğrulamasını desteklemez ancak ona benzer olan Azure Active Directory kimlik doğrulamasını destekler. Bazı kimlik doğrulaması türleri için SSMS'nin en son sürümü gerekir. Daha fazla bilgi için bkz. [Azure Active Directory Kimlik Doğrulamasını Kullanarak SQL Veritabanına veya SQL Veri Ambarına Bağlanma](sql-database-aad-authentication.md).
- Üç veya dört bölüm adı kullanan veritabanları arası sorgular. (Salt okunur veritabanları arası sorgular [elastik veritabanı sorgusu](sql-database-elastic-query-overview.md) kullanılarak desteklenir.)
- Veritabanları arası sahiplik zinciri, `TRUSTWORTHY` ayarı
- `EXECUTE AS LOGIN` Bunun yerine "EXECUTE AS USER" kullanın.
- Genişletilebilir anahtar yönetimi haricinde şifreleme desteklenir
- Olay: Olaylar, olay bildirimleri, sorgu bildirimleri
- Dosya yerleşimi: Microsoft Azure tarafından otomatik olarak yönetilen veritabanı dosya yerleşimi, boyutu ve veritabanı dosyalarıyla ilgili sözdizimi.
- Yüksek kullanılabilirlik: Microsoft Azure hesabınız aracılığıyla yönetilen yüksek kullanılabilirlik ile ilgili sözdizimi. Buna yedekleme, geri yükleme, Her Zaman Açık, veritabanı yansıtması, günlük aktarma ve kurtarma modları için söz dizimleri dahildir.
- Günlük okuyucu: SQL Veritabanı'nda bulunmayan günlük okuyucusuna dayanan sözdizimi: Çoğaltmayı İtme, Veri Yakalamayı Değiştir. SQL Veritabanı gönderme temelli çoğaltma gönderisinin abonesi olabilir.
- İşlevler: `fn_get_sql`, `fn_virtualfilestats`, `fn_virtualservernodes`
- Donanım: Donanımla ilgili sunucu ayarlarıyla ilgili sözdizimi: bellek, alt iş parçacıkları, CPU yakınlığı, izleme bayrakları gibi. Bunun yerine hizmet katmanları ve işlem boyutlarını kullanın.
- `KILL STATS JOB`
- `OPENQUERY`, `OPENROWSET` `OPENDATASOURCE`, ve dört parçalı adlar
- .NET Framework: SQL Server ile CLR entegrasyonu
- Anlamsal arama
- Sunucu kimlik bilgileri: Bunun yerine [veritabanı kapsamı yla ilgili kimlik bilgilerini](https://msdn.microsoft.com/library/mt270260.aspx) kullanın.
- Sunucu düzeyindeöğeler: Sunucu `sys.login_token`rolleri, . `GRANT`, `REVOKE` ve `DENY` sunucu düzeyi izinler kullanılamaz ancak bazıları veritabanı düzeyi izinlerle değiştirilmiştir. Sunucu düzeyi kullanışlı DMV'lerden bazıları, eşdeğer veritabanı düzeyi DMV'lerine sahiptir.
- `SET REMOTE_PROC_TRANSACTIONS`
- `SHUTDOWN`
- `sp_addmessage`
- `sp_configure` seçenekleri ve `RECONFIGURE`. Bazı seçenekler [ALTER DATABASE SCOPED CONFIGURATION](https://msdn.microsoft.com/library/mt629158.aspx) ile kullanılabilir.
- `sp_helpuser`
- `sp_migrate_user_to_contained`
- SQL Server Agent: SQL Server Agent veya MSDB veritabanına dayanan sözdizimi: uyarılar, operatörler, merkezi yönetim sunucuları. Bunun yerine Azure PowerShell gibi betik uygulamaları kullanın.
- SQL Server denetimi: Bunun yerine SQL Veritabanı denetimi kullanın.
- SQL Server izleme
- İzleme bayrakları: Bazı izleme bayrakları öğeleri uyumluluk modlarına taşınmıştır.
- Transact-SQL hata ayıklama
- Tetikleyiciler: Sunucu kapsamlı veya oturum açma tetikleyicileri
- `USE` deyimi: Veritabanı bağlamını farklı bir veritabanıyla değiştirmek için yeni veritabanıyla yeni bir bağlantı kurmanız gerekir.

## <a name="full-transact-sql-reference"></a>Tam Transact-SQL başvurusu

Transact-SQL dilbilgisi, kullanımı ve örnekleri hakkında daha fazla bilgi için SQL Server Books Online'da [Transact-SQL Reference (Database Engine)](https://msdn.microsoft.com/library/bb510741.aspx) adresine bakın.

### <a name="about-the-applies-to-tags"></a>"Uygulandığı öğe" etiketleri hakkında

Transact-SQL başvurusu, 2008'den günümüze SQL Server sürümleriyle ilgili makaleleri içerir. Makale başlığının altında, dört SQL Server platformlarını listeleyen ve uygulanabilirliği gösteren bir simge çubuğu vardır. Örneğin kullanılabilirlik grupları SQL Server 2012'de tanıtılmıştır.  [CREATE AVAILABILITY GROUP](https://msdn.microsoft.com/library/ff878399.aspx) makalesi, deyimin SQL Server için geçerli olduğunu gösterir **(2012'den itibaren)**. Deyim SQL Server 2008, SQL Server 2008 R2, Azure SQL Veritabanı, Azure SQL Veri Ambarı veya Paralel Veri Ambarı için geçerli değildir.

Bazı durumlarda, bir makalenin genel konusu bir üründe kullanılabilir, ancak ürünler arasında küçük farklılıklar vardır. Farklar maddenin orta noktalarında uygun olarak belirtilir. Bazı durumlarda, bir makalenin genel konusu bir üründe kullanılabilir, ancak ürünler arasında küçük farklılıklar vardır. Farklar maddenin orta noktalarında uygun olarak belirtilir. Örneğin CREATE TRIGGER makalesi SQL Veritabanı'nda kullanılabilir. Ancak sunucu düzeyinde tetikleyiciler için **TÜM SERVER** seçeneği, sunucu düzeyinde tetikleyicilerin SQL Veritabanı'nda kullanılamayacağını gösterir. Bunun yerine veritabanı düzeyinde tetikleyicileri kullanın.

## <a name="next-steps"></a>Sonraki adımlar

SQL Veritabanı tarafından desteklenen ve desteklenmeyen özelliklerin listesi için [Azure SQL Veritabanı özellik karşılaştırması'na](sql-database-features.md)bakın. Bu sayfadaki liste, yönergeleri ve özellikleri makale ekler ve Transact-SQL deyimleri üzerinde duruluyor.
