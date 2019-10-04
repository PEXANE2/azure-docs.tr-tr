---
title: Azure SQL veritabanı yönetilen örnek T-SQL farkları | Microsoft Docs
description: Bu makalede, Azure SQL veritabanı ve SQL Server yönetilen bir örnek arasındaki T-SQL farklılıkları ele alınmaktadır
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein, carlrab, bonova
ms.date: 08/12/2019
ms.custom: seoapril2019
ms.openlocfilehash: 704c1cdf95424bffa19e0946d13fa45d1b520753
ms.sourcegitcommit: 4d177e6d273bba8af03a00e8bb9fe51a447196d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71959946"
---
# <a name="managed-instance-t-sql-differences-limitations-and-known-issues"></a>Yönetilen örnek T-SQL farkları, sınırlamaları ve bilinen sorunlar

Bu makalede, Azure SQL veritabanı yönetilen örneği ve şirket içi SQL Server veritabanı altyapısı arasındaki söz dizimi ve davranış farklılıkları özetlenmektedir ve açıklanmaktadır. Yönetilen örnek dağıtım seçeneği, şirket içi SQL Server veritabanı altyapısı ile yüksek uyumluluk sağlar. SQL Server veritabanı altyapısı özelliklerinin çoğu yönetilen bir örnekte desteklenir.

![Geçiş](./media/sql-database-managed-instance/migration.png)

Yönetilen örnekte tanıtılan bazı PaaS sınırlamaları ve SQL Server karşılaştırıldığında bazı davranış değişiklikleri vardır. Farklar aşağıdaki kategorilere ayrılmıştır:<a name="Differences"></a>

- [Kullanılabilirlik](#availability) , [her zaman açık](#always-on-availability) ve [yedeklemelerdeki](#backup)farkları içerir.
- [Güvenlik](#security) , [Denetim](#auditing), [sertifika](#certificates), [kimlik bilgileri](#credential), [şifreleme sağlayıcıları](#cryptographic-providers), [oturum açmalar ve kullanıcılar](#logins-and-users)ve [hizmet anahtarı ile hizmet ana anahtarı](#service-key-and-service-master-key)arasındaki farkları içerir.
- [Yapılandırma](#configuration) , [arabellek havuzu genişletme](#buffer-pool-extension), [harmanlama](#collation), [Uyumluluk düzeyleri](#compatibility-levels), [veritabanı yansıtma](#database-mirroring), [veritabanı seçenekleri](#database-options), [SQL Server Agent](#sql-server-agent)ve [tablo seçeneklerindeki](#tables)farklılıkları içerir.
- [İşlevler](#functionalities) şunlardır [bulk INSERT/OPENROWSET](#bulk-insert--openrowset), [clr](#clr), [DBCC](#dbcc), [Dağıtılmış işlemler](#distributed-transactions), [genişletilmiş olaylar](#extended-events), [dış kitaplıklar](#external-libraries), [FILESTREAM ve FileTable](#filestream-and-filetable), [tam metin Anlamsal arama](#full-text-semantic-search), [bağlı sunucular](#linked-servers), [PolyBase](#polybase), [çoğaltma](#replication), [geri yükleme](#restore-statement), [Hizmet Aracısı](#service-broker), [saklı yordamlar, işlevler ve Tetikleyiciler](#stored-procedures-functions-and-triggers).
- Sanal ağlar ve alt ağ yapılandırması gibi [ortam ayarları](#Environment) .

Bu özelliklerin çoğu mimari kısıtlamalardır ve hizmet özelliklerini temsil eder.

Bu sayfada Ayrıca, gelecekte çözümlenecek olan yönetilen örnekte bulunan [bilinen geçici sorunlar](#Issues) açıklanmaktadır.

## <a name="availability"></a>Kullanılabilirlik

### <a name="always-on-availability"></a>Her zaman açık

[Yüksek kullanılabilirlik](sql-database-high-availability.md) yönetilen örnekte yerleşiktir ve kullanıcılar tarafından denetlenemez. Aşağıdaki deyimler desteklenmez:

- [UÇ NOKTA OLUŞTUR... DATABASE_MIRRORING IÇIN](https://docs.microsoft.com/sql/t-sql/statements/create-endpoint-transact-sql)
- [KULLANıLABILIRLIK GRUBU OLUŞTUR](https://docs.microsoft.com/sql/t-sql/statements/create-availability-group-transact-sql)
- [KULLANıLABILIRLIK GRUBUNU DEĞIŞTIR](https://docs.microsoft.com/sql/t-sql/statements/alter-availability-group-transact-sql)
- [KULLANıLABILIRLIK GRUBU BıRAKMA](https://docs.microsoft.com/sql/t-sql/statements/drop-availability-group-transact-sql)
- [Alter database](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql) ifadesinin [set HADR](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-set-hadr) yan tümcesi

### <a name="backup"></a>Yedeklenmiş

Yönetilen örneklerin otomatik yedeklemeleri vardır, bu sayede kullanıcılar `COPY_ONLY` yedeklemesi için tam veritabanı oluşturabilir. Fark, günlük ve dosya anlık görüntüsü yedeklemeleri desteklenmez.

- Yönetilen bir örnek ile, bir örnek veritabanını yalnızca bir Azure Blob depolama hesabına yedekleyebilirsiniz:
  - Yalnızca `BACKUP TO URL` desteklenir.
  - `FILE`, `TAPE` ve yedekleme cihazları desteklenmez.
- Genel `WITH` seçeneklerinin çoğu desteklenir.
  - `COPY_ONLY` zorunludur.
  - `FILE_SNAPSHOT` desteklenmez.
  - Bant seçenekleri: `REWIND`, `NOREWIND`, `UNLOAD` ve `NOUNLOAD` desteklenmez.
  - Günlüğe özel seçenekler: `NORECOVERY`, `STANDBY` ve `NO_TRUNCATE` desteklenmez.

Sınırlamalar: 

- Yönetilen bir örnek ile, yedekleme sıkıştırması kullanılıyorsa 4 TB 'a kadar olan veritabanları için yeterli olan bir örnek veritabanını en fazla 32 şeritli bir yedeklemeye yedekleyebilirsiniz.
- Hizmet tarafından yönetilen Saydam Veri Şifrelemesi (TDE) ile şifrelenen bir veritabanında `BACKUP DATABASE ... WITH COPY_ONLY` ' i çalıştıramazsınız. Hizmet tarafından yönetilen TDE, yedeklemelerin dahili bir TDE anahtarla şifrelenmesini zorlar. Anahtar verilemiyor, bu nedenle yedeklemeyi geri alamazsınız. Otomatik yedeklemeler ve zaman içinde geri yükleme kullanın veya bunun yerine [müşteri tarafından yönetilen (BYOK) TDE](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-azure-sql#customer-managed-transparent-data-encryption---bring-your-own-key) kullanın. Ayrıca, veritabanında şifrelemeyi devre dışı bırakabilirsiniz.
- Yönetilen bir örnekteki `BACKUP` komutu kullanılarak en fazla yedek şerit boyutu 195 GB 'tır ve bu en büyük blob boyutudur. Tek tek Stripe boyutunu azaltmak ve bu sınırın içinde kalmak için yedekleme komutundaki şeritler sayısını artırın.

    > [!TIP]
    > Bu kısıtlamayı geçici olarak çözmek için, bir veritabanını şirket içi bir ortamda veya bir sanal makinede SQL Server yedekleyerek şunları yapabilirsiniz:
    >
    > - @No__t-1 ' e yedeklemek yerine `DISK` ' a yedekleyin.
    > - Yedekleme dosyalarını blob depolamaya yükleyin.
    > - Yönetilen örneğe geri yükleyin.
    >
    > Yönetilen bir örnekteki `Restore` komutu, karşıya yüklenen yedekleme dosyalarının depolanması için farklı bir blob türü kullanıldığından, yedekleme dosyalarındaki daha büyük blob boyutlarını destekler.

T-SQL kullanarak yedeklemeler hakkında daha fazla bilgi için bkz. [yedekleme](https://docs.microsoft.com/sql/t-sql/statements/backup-transact-sql).

## <a name="security"></a>Güvenlik

### <a name="auditing"></a>Denetim

Azure SQL veritabanı ve SQL Server veritabanlarında bulunan veritabanlarında denetim arasındaki temel farklılıklar şunlardır:

- Azure SQL veritabanı 'ndaki yönetilen örnek dağıtım seçeneği ile, denetim sunucu düzeyinde çalışmaktadır. @No__t-0 günlük dosyaları Azure Blob depolama alanında depolanır.
- Azure SQL veritabanı 'nda tek veritabanı ve elastik havuz dağıtım seçenekleriyle denetim, veritabanı düzeyinde çalışmaktadır.
- Şirket içi veya sanal makineler SQL Server, denetim sunucu düzeyinde çalışmaktadır. Olaylar dosya sistemi veya Windows olay günlüklerine depolanır.
 
Yönetilen örnekteki XEvent denetimi Azure Blob depolama hedeflerini destekler. Dosya ve Windows günlükleri desteklenmez.

Azure Blob depolamaya yönelik denetim için `CREATE AUDIT` sözdiziminde önemli farklılıklar şunlardır:

- @No__t-1 dosyalarının yerleştirildiği Azure Blob depolama kapsayıcısının URL 'sini belirtmek için kullanabileceğiniz yeni bir sözdizimi `TO URL` sağlanır.
- Yönetilen bir örnek Windows dosya paylaşımlarına erişemediği için `TO FILE` sözdizimi desteklenmez.

Daha fazla bilgi için bkz.: 

- [SUNUCU DENETIMI OLUŞTUR](https://docs.microsoft.com/sql/t-sql/statements/create-server-audit-transact-sql) 
- [SUNUCU DENETIMINI DEĞIŞTIR](https://docs.microsoft.com/sql/t-sql/statements/alter-server-audit-transact-sql)
- [Denetim](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine)

### <a name="certificates"></a>Sertifikalar

Yönetilen bir örnek dosya paylaşımlarına ve Windows klasörlerine erişemez, bu nedenle aşağıdaki kısıtlamalar geçerlidir:

- @No__t-0 @ no__t-1 @ no__t-2 dosyası sertifikalar için desteklenmez.
- @No__t-3 @ no__t-4 @ no__t-5 ' ten `CREATE` @ no__t-1 @ no__t-2 sertifikası desteklenmez. Özel anahtar dosyaları kullanılamıyor. 

Bkz. SERTIFIKA ve [yedekleme sertifikası](https://docs.microsoft.com/sql/t-sql/statements/backup-certificate-transact-sql) [oluşturma](https://docs.microsoft.com/sql/t-sql/statements/create-certificate-transact-sql) . 
 
**Geçici çözüm**: sertifika veya özel anahtar betiği,. SQL dosyası olarak depola ve ikiliden oluştur:

```sql
CREATE CERTIFICATE  
   FROM BINARY = asn_encoded_certificate
WITH PRIVATE KEY (<private_key_options>)
```

### <a name="credential"></a>Credential

Yalnızca Azure Key Vault ve `SHARED ACCESS SIGNATURE` kimlikleri desteklenir. Windows kullanıcıları desteklenmez.

Bkz. [KIMLIK bilgisi oluşturma](https://docs.microsoft.com/sql/t-sql/statements/create-credential-transact-sql) ve [kimlik bilgisi değiştirme](https://docs.microsoft.com/sql/t-sql/statements/alter-credential-transact-sql).

### <a name="cryptographic-providers"></a>Şifreleme sağlayıcıları

Yönetilen bir örnek dosyalara erişemez, bu nedenle şifreleme sağlayıcıları oluşturulamıyor:

- `CREATE CRYPTOGRAPHIC PROVIDER` desteklenmez. Bkz. [ŞIFRELEME sağlayıcısı oluşturma](https://docs.microsoft.com/sql/t-sql/statements/create-cryptographic-provider-transact-sql).
- `ALTER CRYPTOGRAPHIC PROVIDER` desteklenmez. Bkz. [alter CRYPTOGRAPHIC PROVIDER](https://docs.microsoft.com/sql/t-sql/statements/alter-cryptographic-provider-transact-sql).

### <a name="logins-and-users"></a>Oturum açmalar ve kullanıcılar

- @No__t-0, `FROM ASYMMETRIC KEY` ve `FROM SID` kullanılarak oluşturulan SQL oturum açmaları desteklenir. Bkz. [oturum oluşturma](https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql).
- Azure Active Directory (Azure AD) [create LOGIN](https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current) sözdizimi veya [login from LOGıN [Azure AD Login]](https://docs.microsoft.com/sql/t-sql/statements/create-user-transact-sql?view=azuresqldb-mi-current) sözdizimi kullanılarak oluşturulan sunucu sorumluları (oturum açmalar) desteklenir (Genel Önizleme). Bu oturumlar sunucu düzeyinde oluşturulur.

    Yönetilen örnek `CREATE USER [AADUser/AAD group] FROM EXTERNAL PROVIDER` sözdizimiyle Azure AD veritabanı sorumlularını destekler. Bu özellik, Azure AD içeren veritabanı kullanıcıları olarak da bilinir.

- @No__t-0 sözdizimiyle oluşturulan Windows oturum açmaları desteklenmez. Azure Active Directory oturumlarını ve kullanıcıları kullanın.
- Örneği oluşturan Azure AD kullanıcısının [sınırsız yönetici ayrıcalıkları](sql-database-manage-logins.md#unrestricted-administrative-accounts)vardır.
- Yönetici olmayan Azure AD Veritabanı düzeyi kullanıcılar `CREATE USER ... FROM EXTERNAL PROVIDER` sözdizimi kullanılarak oluşturulabilir. Bkz [. Kullanıcı oluşturma... Dış SAĞLAYıCıDAN](sql-database-manage-logins.md#non-administrator-users).
- Azure AD Server sorumluları (oturum açmalar) yalnızca yönetilen bir örnek içindeki SQL özelliklerini destekler. Aynı Azure AD kiracısı veya farklı kiracılar dahilinde olup olmadıkları fark etmeksizin, Azure AD kullanıcıları için desteklenmez. Bu özelliklere örnek olarak şunlar verilebilir:

  - SQL işlem çoğaltması.
  - Sunucu bağlayın.

- Veritabanı sahibi olarak Azure AD grubuna eşlenmiş bir Azure AD oturum açmanın ayarlanması desteklenmez.
- Diğer Azure AD sorumlularını kullanarak Azure AD sunucu düzeyi sorumluları 'nın kimliğe bürünme işlemi, [execute as](/sql/t-sql/statements/execute-as-transact-sql) yan tümcesi gibi desteklenir. FARKLı ÇALıŞTıR sınırlamaları şunlardır:

  - Ad, oturum açma adından farklı olduğunda Azure AD kullanıcıları için Kullanıcı tarafından desteklenmez. Kullanıcı, [john@contoso.com] oturum açma IŞLEMINDEN Kullanıcı oluşturma [Myaadkullanıcı] sözdizimi aracılığıyla oluşturulmuştur ve EXEC Kullanıcı = _myaaduser_aracılığıyla kimliğe bürünme girişiminde bulunur. Bir Azure AD sunucu sorumlusu 'ndan (oturum açma) bir **Kullanıcı** oluşturduğunuzda, **oturum açma**işleminden aynı login_name için user_name 'yi belirtin.
  - Yalnızca `sysadmin` rolünün parçası olan SQL Server düzeyi sorumlular (oturumlar), Azure AD sorumlularını hedefleyen aşağıdaki işlemleri yürütebilir:

    - KULLANıCı OLARAK YÜRÜT
    - OTURUM AÇMA OLARAK YÜRÜT

- Azure AD Server sorumluları (oturum açmalar) için genel önizleme sınırlamaları:

  - Yönetilen örnek için yönetici sınırlamalarını Active Directory:

    - Yönetilen örneği kurmak için kullanılan Azure AD yöneticisi, yönetilen örnek içinde bir Azure AD sunucu sorumlusu (oturum açma) oluşturmak için kullanılamaz. İlk Azure AD sunucu sorumlusunu (oturum açma), `sysadmin` rolü olan SQL Server bir hesap kullanarak oluşturmanız gerekir. Bu geçici sınırlama, Azure AD Server sorumluları (oturumlar) genel kullanıma sunulduğunda kaldırılacak. Oturum açmayı oluşturmak için bir Azure AD yönetici hesabı kullanmaya çalışırsanız, şu hatayı görürsünüz: `Msg 15247, Level 16, State 1, Line 1 User does not have permission to perform this action.`
      - Şu anda, ana veritabanında oluşturulan ilk Azure AD oturum açma, dış SAĞLAYıCıDAN [oturum oluşturma](/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current) kullanılarak `sysadmin` rolü olan standart SQL Server hesabı (Azure dışı ad) tarafından oluşturulmalıdır. Genel kullanılabilirlik sonrasında, bu sınırlama kaldırılır. Daha sonra, yönetilen örnek için Active Directory Yöneticisi 'ni kullanarak ilk Azure AD oturum açma oluşturabilirsiniz.
    - SQL Server Management Studio veya SqlPackage ile kullanılan DacFx (dışarı aktarma/içeri aktarma), Azure AD oturum açmaları için desteklenmez. Bu sınırlama, Azure AD Server sorumlularını (oturumlar) genel kullanıma sunulduğunda kaldırılacak.
    - SQL Server Management Studio ile Azure AD Server sorumlularını (oturum açma) kullanma:

      - Kimliği doğrulanmış herhangi bir oturum açma kullanan Azure AD oturum açmaları betiği oluşturma desteklenmez.
      - IntelliSense, dış SAĞLAYıCıDAN oturum açma oluştur ekstresini tanımıyor ve kırmızı alt çizgi gösterir.

- Yalnızca yönetilen örnek sağlama işlemi tarafından oluşturulan sunucu düzeyi asıl oturum açma, `securityadmin` veya `sysadmin` gibi sunucu rollerinin üyeleri veya sunucu düzeyinde herhangi bir oturum açma iznini DEĞIŞTIR ile diğer oturumlar Azure AD sunucu sorumlularını oluşturabilir (oturum açma) yönetilen örnek için ana veritabanında.
- Oturum açma bir SQL sorumlusu ise, yalnızca `sysadmin` rolünün parçası olan oturumlar Create komutunu kullanarak bir Azure AD hesabı için oturum açma bilgileri oluşturabilir.
- Azure AD oturum açma, Azure SQL veritabanı yönetilen örneği için kullanılan aynı dizin içinde bir Azure AD 'nin üyesi olmalıdır.
- Azure AD Server sorumluları (oturum açmalar), SQL Server Management Studio 18,0 Preview 5 ' te başlayan Nesne Gezgini görünür.
- Azure AD yönetici hesabıyla çakışan Azure AD Server sorumlularına (oturum açmalar) izin verilir. Sorumluyu çözümlediğinizde ve yönetilen örneğe izinleri uyguladığınızda Azure AD Server sorumluları (oturum açmalar) Azure AD Yöneticisi üzerinden önceliklidir.
- Kimlik doğrulama sırasında, kimliği doğrulanan sorumluyu çözümlemek için aşağıdaki sıra uygulanır:

    1. Azure AD hesabı, "E" türü olarak sys. server_principals ' de bulunan Azure ad sunucu sorumlusu (oturum açma) ile doğrudan eşlenmiş olarak mevcutsa, Azure AD Server sorumlusu (oturum açma) için erişim izni verin ve izinleri uygulayın.
    2. Azure AD hesabı, "X" türü olarak sys. server_principals ' de bulunan Azure ad sunucu sorumlusu (oturum açma) ile eşlenmiş bir Azure AD grubunun üyesiyse, erişim izni verin ve Azure AD grubu oturum açma izinlerini uygulayın.
    3. Azure AD hesabı yönetilen örnek için yönetilen örnek sistem görünümlerinde mevcut olmayan özel bir portal tarafından yapılandırılmış Azure AD yöneticisi ise, yönetilen örnek (eski mod) için Azure AD yöneticisi 'nin özel sabit izinlerini uygulayın.
    4. Azure AD hesabı, bir veritabanında bulunan bir Azure AD kullanıcısına doğrudan eşlenmiş olarak mevcutsa ("E" türü olarak sys. database_principals ' de bulunur), Azure AD veritabanı kullanıcısına erişim izni verin ve izinleri uygulayın.
    5. Azure AD hesabı, bir veritabanında bulunan bir Azure AD kullanıcısına eşlenmiş bir Azure AD grubunun üyesiyse, "X" türü olarak sys. database_principals ' de bulunan ve Azure AD grubu oturum açma izinlerini uygulamaya erişim izni verin.
    6. Kimlik doğrulaması yapan kullanıcıya çözümlenen bir Azure AD kullanıcı hesabıyla veya bir Azure AD grup hesabıyla eşlenmiş bir Azure AD oturum açma bilgileri varsa, bu Azure AD oturum açma bilgilerini tüm izinler uygulanır.

### <a name="service-key-and-service-master-key"></a>Hizmet anahtarı ve hizmet ana anahtarı

- [Ana anahtar yedeklemesi](https://docs.microsoft.com/sql/t-sql/statements/backup-master-key-transact-sql) DESTEKLENMIYOR (SQL veritabanı hizmeti tarafından yönetiliyor).
- [Ana anahtar geri yükleme](https://docs.microsoft.com/sql/t-sql/statements/restore-master-key-transact-sql) desteklenmez (SQL veritabanı hizmeti tarafından yönetiliyor).
- [Hizmet ana anahtarı yedeklemesi](https://docs.microsoft.com/sql/t-sql/statements/backup-service-master-key-transact-sql) DESTEKLENMIYOR (SQL veritabanı hizmeti tarafından yönetiliyor).
- [Hizmet ana anahtarı geri yükleme](https://docs.microsoft.com/sql/t-sql/statements/restore-service-master-key-transact-sql) desteklenmez (SQL veritabanı hizmeti tarafından yönetiliyor).

## <a name="configuration"></a>Yapılandırma

### <a name="buffer-pool-extension"></a>Arabellek havuzu uzantısı

- [Arabellek havuzu uzantısı](https://docs.microsoft.com/sql/database-engine/configure-windows/buffer-pool-extension) desteklenmiyor.
- `ALTER SERVER CONFIGURATION SET BUFFER POOL EXTENSION` desteklenmez. Bkz. [Alter Server CONFIGURATION](https://docs.microsoft.com/sql/t-sql/statements/alter-server-configuration-transact-sql).

### <a name="collation"></a>Harmanlama

Varsayılan örnek harmanlama `SQL_Latin1_General_CP1_CI_AS` ' dır ve bir oluşturma parametresi olarak belirtilebilir. Bkz. [harmanlamalar](https://docs.microsoft.com/sql/t-sql/statements/collations).

### <a name="compatibility-levels"></a>Uyumluluk düzeyleri

- Desteklenen uyumluluk düzeyleri 100, 110, 120, 130, 140 ve 150.
- 100 altındaki uyumluluk düzeyleri desteklenmez.
- Yeni veritabanları için varsayılan uyumluluk düzeyi 140 ' dir. Geri yüklenen veritabanları için, uyumluluk düzeyi 100 ve üzeri olursa değişmeden kalır.

Bkz. [ALTER DATABASE Compatibility Level](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level).

### <a name="database-mirroring"></a>Veritabanı yansıtma

Veritabanı yansıtma desteklenmez.

- `ALTER DATABASE SET PARTNER` ve `SET WITNESS` seçenekleri desteklenmez.
- `CREATE ENDPOINT … FOR DATABASE_MIRRORING` desteklenmez.

Daha fazla bilgi için bkz. [alter database set Partner, TANıK ayarla](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-database-mirroring) ve [uç nokta oluştur... DATABASE_MIRRORING IÇIN](https://docs.microsoft.com/sql/t-sql/statements/create-endpoint-transact-sql).

### <a name="database-options"></a>Veritabanı seçenekleri

- Birden çok günlük dosyası desteklenmez.
- Bellek içi nesneler Genel Amaçlı hizmeti katmanında desteklenmez. 
- Her veritabanı için en fazla 280 dosya Genel Amaçlı örnek başına 280 dosya sınırı vardır. Genel Amaçlı katmanındaki veriler ve günlük dosyaları bu sınıra doğru sayılır. [İş açısından kritik katmanı veritabanı başına 32.767 dosyasını destekler](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-resource-limits#service-tier-characteristics).
- Veritabanı FILESTREAM verileri içeren dosya gruplarını içeremez. . Bak `FILESTREAM` verisi içeriyorsa restore başarısız olur. 
- Her dosya Azure Blob depolama alanına yerleştirilir. Dosya başına GÇ ve aktarım hızı her bir dosyanın boyutuna bağlıdır.

#### <a name="create-database-statement"></a>CREATE DATABASE ekstresi

@No__t-0 için aşağıdaki sınırlamalar geçerlidir:

- Dosyalar ve dosya grupları tanımlanamıyor. 
- @No__t-0 seçeneği desteklenmez. 
- `WITH` seçenekleri desteklenmez. 
   > [!TIP]
   > Geçici bir çözüm olarak, dosya eklemek veya kapsama ayarlamak üzere veritabanı seçeneklerini ayarlamak için `CREATE DATABASE` ' den sonra `ALTER DATABASE` ' ı kullanın. 

- @No__t-0 seçeneği desteklenmez.
- @No__t-0 seçeneği desteklenmez.

Daha fazla bilgi için bkz. [veritabanı oluşturma](https://docs.microsoft.com/sql/t-sql/statements/create-database-sql-server-transact-sql).

#### <a name="alter-database-statement"></a>ALTER DATABASE ifadesi

Bazı dosya özellikleri ayarlanamaz veya değiştirilemez:

- @No__t-0 T-SQL ifadesinde bir dosya yolu belirtilemez. Yönetilen bir örnek dosyaları otomatik olarak yerleştirtiğinden, `FILENAME` ' yı komut dosyasından kaldırın. 
- @No__t-0 açıklaması kullanılarak dosya adı değiştirilemez.

Aşağıdaki seçenekler varsayılan olarak ayarlanır ve değiştirilemez:

- `MULTI_USER`
- `ENABLE_BROKER ON`
- `AUTO_CLOSE OFF`

Aşağıdaki seçenekler değiştirilemez:

- `AUTO_CLOSE`
- `AUTOMATIC_TUNING(CREATE_INDEX=ON|OFF)`
- `AUTOMATIC_TUNING(DROP_INDEX=ON|OFF)`
- `DISABLE_BROKER`
- `EMERGENCY`
- `ENABLE_BROKER`
- `FILESTREAM`
- `HADR`
- `NEW_BROKER`
- `OFFLINE`
- `PAGE_VERIFY`
- `PARTNER`
- `READ_ONLY`
- `RECOVERY BULK_LOGGED`
- `RECOVERY_SIMPLE`
- `REMOTE_DATA_ARCHIVE` 
- `RESTRICTED_USER`
- `SINGLE_USER`
- `WITNESS`

Daha fazla bilgi için bkz. [alter database](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-file-and-filegroup-options).

### <a name="sql-server-agent"></a>SQL Server Aracısı

- SQL Server Agent etkinleştirme ve devre dışı bırakma Şu anda yönetilen örnekte desteklenmiyor. SQL Aracısı her zaman çalışır.
- SQL Server Agent ayarları salt okunurdur. @No__t-0 yordamı yönetilen örnekte desteklenmez. 
- İşler
  - T-SQL iş adımları desteklenir.
  - Aşağıdaki çoğaltma işleri desteklenir:
    - İşlem-günlük okuyucu
    - Anlık Görüntü
    - Dağıtım
  - SSIS iş adımları desteklenir.
  - Diğer iş adımı türleri şu anda desteklenmemektedir:
    - Birleştirme çoğaltması işi adımı desteklenmez. 
    - Sıra okuyucusu desteklenmez. 
    - Komut kabuğu henüz desteklenmiyor.
  - Yönetilen örnekler, örneğin Robocopy aracılığıyla ağ paylaşımları gibi dış kaynaklara erişemez. 
  - SQL Server Analysis Services desteklenmez.
- Bildirimler kısmen desteklenir.
- E-posta bildirimi desteklenir, ancak bir Veritabanı Postası profili yapılandırmanızı gerektirir. SQL Server Agent yalnızca bir Veritabanı Postası profili kullanabilir ve `AzureManagedInstance_dbmail_profile` olarak çağrılmalıdır. 
  - Sayfalayıcı desteklenmiyor.
  - NetSend desteklenmez.
  - Uyarılar henüz desteklenmiyor.
  - Proxy 'ler desteklenmez.
- EventLog desteklenmiyor.

Aşağıdaki SQL Aracısı özellikleri şu anda desteklenmiyor:

- Kullanıldığı
- Boştaki bir CPU 'da iş planlama
- Aracıyı etkinleştirme veya devre dışı bırakma
- Uyarılar

SQL Server Agent hakkında bilgi için bkz. [SQL Server Agent](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent).

### <a name="tables"></a>Takvimleri

Aşağıdaki tablo türleri desteklenmez:

- [AKıŞı](https://docs.microsoft.com/sql/relational-databases/blob/filestream-sql-server)
- [FILETABLE](https://docs.microsoft.com/sql/relational-databases/blob/filetables-sql-server)
- [Dış tablo](https://docs.microsoft.com/sql/t-sql/statements/create-external-table-transact-sql) (PolyBase)
- [MEMORY_OPTIMIZED](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/introduction-to-memory-optimized-tables) (yalnızca genel amaçlı katmanında desteklenmez)

Tablo oluşturma ve değiştirme hakkında daha fazla bilgi için bkz. [Create Table](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql) ve [alter table](https://docs.microsoft.com/sql/t-sql/statements/alter-table-transact-sql).

## <a name="functionalities"></a>İşlevsellikler

### <a name="bulk-insert--openrowset"></a>Toplu ekleme/OPENROWSET

Yönetilen bir örnek dosya paylaşımlarına ve Windows klasörlerine erişemez, bu nedenle dosyalar Azure Blob depolamadan içeri aktarılmalıdır:

- `BULK INSERT` komutunda, Azure Blob depolamadan dosya içeri aktarılırken `DATASOURCE` gereklidir. Bkz. [bulk INSERT](https://docs.microsoft.com/sql/t-sql/statements/bulk-insert-transact-sql).
- Azure Blob depolama alanındaki bir dosyanın içeriğini okurken `OPENROWSET` işlevinde `DATASOURCE` gereklidir. Bkz. [OPENROWSET](https://docs.microsoft.com/sql/t-sql/functions/openrowset-transact-sql).
- `OPENROWSET`, diğer Azure SQL tek veritabanlarından, yönetilen örneklerden veya SQL Server örneklerinden veri okumak için kullanılabilir. Oracle veritabanları veya Excel dosyaları gibi diğer kaynaklar desteklenmez.

### <a name="clr"></a>CLR

Yönetilen bir örnek dosya paylaşımlarına ve Windows klasörlerine erişemez, bu nedenle aşağıdaki kısıtlamalar geçerlidir:

- Yalnızca `CREATE ASSEMBLY FROM BINARY` desteklenir. Bkz. [Assem 'dan](https://docs.microsoft.com/sql/t-sql/statements/create-assembly-transact-sql)bir şekilde oluşturma. 
- `CREATE ASSEMBLY FROM FILE` desteklenmez. Bkz. [dosyadan derleme oluşturma](https://docs.microsoft.com/sql/t-sql/statements/create-assembly-transact-sql).
- `ALTER ASSEMBLY` dosyalara başvuramaz. Bkz. [alter assembly](https://docs.microsoft.com/sql/t-sql/statements/alter-assembly-transact-sql).

### <a name="database-mail-db_mail"></a>Veritabanı Postası (db_mail)
 - `sp_send_dbmail` @file_attachments parametresini kullanarak ekleri gönderemez. Yerel dosya sistemi ve dış paylaşımlar veya Azure Blob depolamaya bu yordamdan erişilemez.
 - @No__t-0 parametresiyle ilgili bilinen sorunları ve kimlik doğrulamasını inceleyin.
 
### <a name="dbcc"></a>DBCC

SQL Server ' de etkin olan belgelenmemiş DBCC deyimleri yönetilen örneklerde desteklenmez.

- Yalnızca sınırlı sayıda genel Izleme bayrağı desteklenir. Oturum düzeyi `Trace flags` desteklenmez. Bkz. [izleme bayrakları](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql).
- [DBCC TRACEOFF](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceoff-transact-sql) ve [DBCC TRACEON](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceon-transact-sql) , sınırlı sayıda Global Trace-Flags ile çalışır.
- REPAIR_ALLOW_DATA_LOSS, REPAIR_FAST ve REPAIR_REBUILD seçeneklerine sahip [DBCC CHECKDB](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-checkdb-transact-sql) kullanılamaz çünkü veritabanı `SINGLE_USER` modunda ayarlanamıyor- [alter database farklılıklarını](#alter-database-statement)inceleyin. Olası veritabanı bozuklukları Azure destek ekibi tarafından işlenir. Yaşıyorsanız veritabanında düzeltilmesi gereken bir veritabanı bozulması varsa Azure desteği 'ne başvurun.

### <a name="distributed-transactions"></a>Dağıtılmış işlemler

Şu anda yönetilen örneklerde MSDTC ve [elastik işlemler](sql-database-elastic-transactions-overview.md) desteklenmez.

### <a name="extended-events"></a>Genişletilmiş olaylar

Genişletilmiş olaylar (XEvents) için Windows 'a özgü bazı hedefler desteklenmez:

- @No__t-0 hedefi desteklenmez. @No__t-0 dosyalarını Azure Blob depolama alanında depolayın. Bkz. [etw_classic_sync Target](https://docs.microsoft.com/sql/relational-databases/extended-events/targets-for-extended-events-in-sql-server#etw_classic_sync_target-target).
- @No__t-0 hedefi desteklenmez. @No__t-0 dosyalarını Azure Blob depolama alanında depolayın. Bkz. [event_file Target](https://docs.microsoft.com/sql/relational-databases/extended-events/targets-for-extended-events-in-sql-server#event_file-target).

### <a name="external-libraries"></a>Dış kitaplıklar

Veritabanı içi R ve Python, dış kitaplıklar henüz desteklenmiyor. Bkz. [SQL Server Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics/r/sql-server-r-services).

### <a name="filestream-and-filetable"></a>FILESTREAM ve FileTable

- FILESTREAM verileri desteklenmez.
- Veritabanı, `FILESTREAM` verilerine sahip dosya grupları içeremez.
- `FILETABLE` desteklenmez.
- Tablolarda `FILESTREAM` tür olamaz.
- Aşağıdaki işlevler desteklenmez:
  - `GetPathLocator()`
  - `GET_FILESTREAM_TRANSACTION_CONTEXT()`
  - `PathName()`
  - `GetFileNamespacePat)`
  - `FileTableRootPath()`

Daha fazla bilgi için bkz. [FILESTREAM](https://docs.microsoft.com/sql/relational-databases/blob/filestream-sql-server) ve [FileTables](https://docs.microsoft.com/sql/relational-databases/blob/filetables-sql-server).

### <a name="full-text-semantic-search"></a>Tam metin anlam arama

[Anlamsal arama](https://docs.microsoft.com/sql/relational-databases/search/semantic-search-sql-server) desteklenmiyor.

### <a name="linked-servers"></a>Bağlı sunucular

Yönetilen örneklerdeki bağlı sunucular sınırlı sayıda hedef destekler:

- Desteklenen hedefler yönetilen örnekler, tek veritabanları ve SQL Server örneklerdir. 
- Bağlı sunucular dağıtılmış yazılabilir işlemleri (MS DTC) desteklemez.
- Desteklenmeyen hedefler dosya, Analysis Services ve diğer RDBMS ' dir. Dosya içeri aktarma için alternatif olarak `BULK INSERT` veya `OPENROWSET` kullanarak Azure Blob depolamadan yerel CSV içeri aktarmayı kullanmayı deneyin.

İşlemler

- Çapraz örnek yazma işlemleri desteklenmez.
- `sp_dropserver`, bağlı bir sunucunun atılması için desteklenir. Bkz. [sp_dropserver](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-dropserver-transact-sql).
- @No__t-0 işlevi yalnızca SQL Server örneklerinde sorgu yürütmek için kullanılabilir. Bunlar yönetilen, şirket içi veya sanal makinelerde olabilir. Bkz. [OPENROWSET](https://docs.microsoft.com/sql/t-sql/functions/openrowset-transact-sql).
- @No__t-0 işlevi yalnızca SQL Server örneklerinde sorgu yürütmek için kullanılabilir. Bunlar yönetilen, şirket içi veya sanal makinelerde olabilir. Sağlayıcı olarak yalnızca `SQLNCLI`, `SQLNCLI11` ve `SQLOLEDB` değerleri desteklenir. Örnek `SELECT * FROM OPENDATASOURCE('SQLNCLI', '...').AdventureWorks2012.HumanResources.Employee` ' dır. Bkz. [opendatasource](https://docs.microsoft.com/sql/t-sql/functions/opendatasource-transact-sql).
- Bağlı sunucular ağ paylaşımlarından dosyaları (Excel, CSV) okumak için kullanılamaz. Azure Blob depolamadan CSV dosyalarını okuyan [bulk INSERT](https://docs.microsoft.com/sql/t-sql/statements/bulk-insert-transact-sql#e-importing-data-from-a-csv-file) veya [OPENROWSET](https://docs.microsoft.com/sql/t-sql/functions/openrowset-transact-sql#g-accessing-data-from-a-csv-file-with-a-format-file) kullanmayı deneyin. @No__t [yönetilen örnek geri bildirim öğesinde](https://feedback.azure.com/forums/915676-sql-managed-instance/suggestions/35657887-linked-server-to-non-sql-sources)bu istekleri izle-1

### <a name="polybase"></a>PolyBase

, Bir veya Azure Blob depolama alanındaki dosyalara başvuran dış tablolar desteklenmez. PolyBase hakkında daha fazla bilgi için bkz. [PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide).

### <a name="replication"></a>Yinelemesi

- Anlık görüntü ve Iki yönlü çoğaltma türleri desteklenir. Birleştirme çoğaltması, eşler arası çoğaltma ve güncelleştirilebilir abonelikler desteklenmez.
- [Işlem çoğaltma](sql-database-managed-instance-transactional-replication.md) , yönetilen örnek üzerinde bazı kısıtlamalarla genel önizleme için kullanılabilir:
    - Tüm çoğaltma katılımcıları türleri (yayımcı, dağıtıcı, çekme abonesi ve anında Iletme abonesi) yönetilen örneklere yerleştirilebilir, ancak yayımcı ve dağıtıcı hem bulutta hem de şirket içinde olmalıdır.
    - Yönetilen örnekler SQL Server son sürümleriyle iletişim kurabilir. Desteklenen sürümlere [buradan](sql-database-managed-instance-transactional-replication.md#supportability-matrix-for-instance-databases-and-on-premises-systems)bakın.
    - İşlemsel çoğaltma bazı [ek ağ gereksinimlerine](sql-database-managed-instance-transactional-replication.md#requirements)sahiptir.

Çoğaltmayı yapılandırma hakkında daha fazla bilgi için bkz. [çoğaltma öğreticisi](replication-with-sql-database-managed-instance.md).


[Yük devretme grubundaki](sql-database-auto-failover-group.md)bir veritabanında çoğaltma etkinleştirilirse, yönetilen örnek yöneticisinin eski birincil üzerindeki tüm yayınları temizlemesi ve yük devretme gerçekleştikten sonra yeni birincil üzerinde yeniden yapılandırması gerekir. Bu senaryoda aşağıdaki etkinlikler gereklidir:

1. Varsa, veritabanında çalışan tüm çoğaltma işlerini durdurun.
2. Yayımcı veritabanında aşağıdaki betiği çalıştırarak, yayımcıdan abonelik meta verilerini bırakın:

   ```sql
   EXEC sp_dropsubscription @publication='<name of publication>', @article='all',@subscriber='<name of subscriber>'
   ```             
 
1. Abonelik meta verilerini aboneden bırakın. Abone örneğindeki abonelik veritabanında aşağıdaki betiği çalıştırın:

   ```sql
   EXEC sp_subscription_cleanup
      @publisher = N'<full DNS of publisher, e.g. example.ac2d23028af5.database.windows.net>', 
      @publisher_db = N'<publisher database>', 
      @publication = N'<name of publication>'; 
   ```                

1. Yayımlanan veritabanında aşağıdaki betiği çalıştırarak tüm çoğaltma nesnelerini yayımcıya zorla bırakın:

   ```sql
   EXEC sp_removedbreplication
   ```

1. Eski dağıtıcıyı orijinal birincil örnekten zorla bırakma (dağıtıcıya sahip olmak için kullanılan eski bir birincili geri yük devreder). Aşağıdaki betiği, eski dağıtımcı yönetilen örneğindeki ana veritabanında çalıştırın:

   ```sql
   EXEC sp_dropdistributor 1,1
   ```

### <a name="restore-statement"></a>RESTORE ekstresi 

- Desteklenen sözdizimi:
  - `RESTORE DATABASE`
  - `RESTORE FILELISTONLY ONLY`
  - `RESTORE HEADER ONLY`
  - `RESTORE LABELONLY ONLY`
  - `RESTORE VERIFYONLY ONLY`
- Desteklenmeyen sözdizimi:
  - `RESTORE LOG ONLY`
  - `RESTORE REWINDONLY ONLY`
- Kaynaktaki 
  - `FROM URL` (Azure Blob depolama) desteklenen tek seçenektir.
  - `FROM DISK` @ no__t-1 @ no__t-2/Backup cihazı desteklenmez.
  - Yedekleme kümeleri desteklenmez.
- `DIFFERENTIAL` veya `STATS` gibi bir @no__t seçeneği desteklenmez.
- `ASYNC RESTORE`: istemci bağlantısı kesilse bile geri yükleme devam eder. Bağlantınız atıldığı takdirde, bir geri yükleme işleminin durumu için `sys.dm_operation_status` görünümünü ve bir oluşturma ve BıRAKMA veritabanı olup olmadığını kontrol edebilirsiniz. Bkz. [sys. DM _operation_status](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database). 

Aşağıdaki veritabanı seçenekleri ayarlanır veya geçersiz kılınır ve daha sonra değiştirilemez: 

- Aracı. bak dosyasında etkinleştirilmemişse, `NEW_BROKER`. 
- Aracı. bak dosyasında etkinleştirilmemişse, `ENABLE_BROKER`. 
- . bak dosyasındaki bir veritabanında `AUTO_CLOSE=ON` varsa-0 @no__t. 
- . bak dosyasındaki bir veritabanının `SIMPLE` veya `BULK_LOGGED` kurtarma modu varsa `RECOVERY FULL`.
- Bellek için iyileştirilmiş bir dosya grubu eklenir ve kaynak. bak dosyasında yoksa XTP çağırılır. 
- Bellek için iyileştirilmiş mevcut dosya grubu XTP olarak yeniden adlandırılır. 
- `SINGLE_USER` ve `RESTRICTED_USER` seçenekleri `MULTI_USER` ' ye dönüştürülür.

Sınırlamalar: 

- Bozulan veritabanlarının yedeklemeleri, bozulmanın türüne bağlı olarak geri yüklenebilir, ancak bozulma düzeltilinceye kadar otomatik yedeklemeler alınmaz. Kaynak örneğinde `DBCC CHECKDB` ' ın çalıştığından emin olun ve bu sorunu engellemek için yedekleme `WITH CHECKSUM` ' i kullanın.
- Bu belgede açıklanan (örneğin, `FILESTREAM` veya `FILETABLE` nesneleri) herhangi bir sınırlama içeren bir veritabanının `.BAK` dosyasını geri yükleme, yönetilen örnek üzerinde geri yüklenemez.
- birden çok yedekleme kümesi içeren `.BAK` dosya geri yüklenemez. 
- birden çok günlük dosyası içeren `.BAK` dosyaları geri yüklenemez.
- 8 TB 'den büyük veritabanları, etkin bellek içi OLTP nesneleri veya örnek başına 280 dosya aşılacak dosya sayısı bir Genel Amaçlı örneğine geri yüklenemez. 
- 4 TB 'den büyük veya bellek içi OLTP nesnelerinden daha büyük olan veritabanları içeren yedeklemeler, İş Açısından Kritik örneğine [geri yüklenemez.](sql-database-managed-instance-resource-limits.md)
Restore deyimleri hakkında daha fazla bilgi için bkz. [restore deyimleri](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-transact-sql).

 > [!IMPORTANT]
 > Aynı sınırlamalar, yerleşik bir noktadan sonra geri yükleme işlemi için geçerlidir. Örnek olarak, 4 TB 'den büyük Genel Amaçlı veritabanı İş Açısından Kritik örneğine geri yüklenemez. Bellek içi OLTP dosyaları veya 280 'den fazla dosya içeren İş Açısından Kritik veritabanı Genel Amaçlı örneğine geri yüklenemez.

### <a name="service-broker"></a>Hizmet Aracısı

Çapraz örnek hizmet Aracısı desteklenmez:

- `sys.routes`: bir önkoşul olarak sys. Routes adresini seçmeniz gerekir. Adresin her rotada yerel olması gerekir. Bkz. [sys. Routes](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-routes-transact-sql).
- `CREATE ROUTE`: `LOCAL` dışında `ADDRESS` ile `CREATE ROUTE` kullanamazsınız. Bkz. [yol oluşturma](https://docs.microsoft.com/sql/t-sql/statements/create-route-transact-sql).
- `ALTER ROUTE`: `LOCAL` dışında `ADDRESS` ile `ALTER ROUTE` kullanamazsınız. Bkz. [yol değiştirme](https://docs.microsoft.com/sql/t-sql/statements/alter-route-transact-sql). 

### <a name="stored-procedures-functions-and-triggers"></a>Saklı yordamlar, işlevler ve Tetikleyiciler

- `NATIVE_COMPILATION` Genel Amaçlı katmanında desteklenmez.
- Aşağıdaki [sp_configure](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-configure-transact-sql) seçenekleri desteklenmez: 
  - `allow polybase export`
  - `allow updates`
  - `filestream_access_level`
  - `remote data archive`
  - `remote proc trans`
- `sp_execute_external_scripts` desteklenmez. Bkz. [sp_execute_external_scripts](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql#examples).
- `xp_cmdshell` desteklenmez. Bkz. [xp_cmdshell](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/xp-cmdshell-transact-sql).
- `sp_addextendedproc` @ no__t-2 ve `sp_dropextendedproc` içeren `Extended stored procedures` desteklenmez. Bkz. [genişletilmiş saklı yordamlar](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/general-extended-stored-procedures-transact-sql).
- `sp_attach_db`, `sp_attach_single_file_db` ve `sp_detach_db` desteklenmez. Bkz. [sp_attach_db](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-attach-db-transact-sql), [sp_attach_single_file_db](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-attach-single-file-db-transact-sql)ve [sp_detach_db](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-detach-db-transact-sql).

### <a name="system-functions-and-variables"></a>Sistem işlevleri ve değişkenler

Aşağıdaki değişkenler, işlevler ve görünümler farklı sonuçlar döndürüyor:

- `SERVERPROPERTY('EngineEdition')`, 8 değerini döndürür. Bu özellik yönetilen bir örneği benzersiz bir şekilde tanımlar. Bkz. [ServerProperty](https://docs.microsoft.com/sql/t-sql/functions/serverproperty-transact-sql).
- `SERVERPROPERTY('InstanceName')`, SQL Server için mevcut olduğu gibi örnek kavramı yönetilen bir örneğe uygulanamadığından NULL değerini döndürür. Bkz. [ServerProperty (' ÖrnekAdı ')](https://docs.microsoft.com/sql/t-sql/functions/serverproperty-transact-sql).
- `@@SERVERNAME`, tam DNS "bağlanılabilir" adı (örneğin, my-managed-instance.wcus17662feb9ce98.database.windows.net) döndürür. Bkz. [@ @SERVERNAME](https://docs.microsoft.com/sql/t-sql/functions/servername-transact-sql). 
- `SYS.SERVERS`, "Name" ve "data_source" özellikleri için `myinstance.domain.database.windows.net` gibi tam DNS "bağlanılabilir" adı döndürür. Bkz [. sys. Sunucu](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-servers-transact-sql).
- `@@SERVICENAME`, SQL Server için mevcut olduğu gibi hizmet kavramı yönetilen bir örneğe uygulanamadığından NULL değerini döndürür. Bkz. [@ @SERVICENAME](https://docs.microsoft.com/sql/t-sql/functions/servicename-transact-sql).
- `SUSER_ID` desteklenir. Azure AD oturum açma, sys. syslogins içinde değilse NULL değerini döndürür. Bkz. [SUSER_ID](https://docs.microsoft.com/sql/t-sql/functions/suser-id-transact-sql). 
- `SUSER_SID` desteklenmez. Yanlış veriler döndürülür, bu geçici olarak bilinen bir sorundur. Bkz. [SUSER_SID](https://docs.microsoft.com/sql/t-sql/functions/suser-sid-transact-sql). 

## <a name="Environment"></a>Ortam kısıtlamaları

### <a name="subnet"></a>Alt ağ
-  Yönetilen örneğinizi dağıttığınız alt ağa başka herhangi bir kaynak (örneğin, sanal makineler) yerleştirebilirsiniz. Bu kaynakları farklı bir alt ağ kullanarak dağıtın.
- Alt ağda yeterli sayıda kullanılabilir [IP adresi](sql-database-managed-instance-connectivity-architecture.md#network-requirements)olmalıdır. En az 16, ancak öneri alt ağda en az 32 IP adresine sahip olur.
- [Hizmet uç noktaları, yönetilen örneğin alt ağıyla ilişkilendirilemez](sql-database-managed-instance-connectivity-architecture.md#network-requirements). Sanal ağı oluştururken hizmet uç noktaları seçeneğinin devre dışı olduğundan emin olun.
- Bir bölgede dağıtabileceğiniz sanal çekirdek sayısı ve örnek türleri bazı [kısıtlamalar ve sınırlara](sql-database-managed-instance-resource-limits.md#regional-resource-limitations)sahiptir.
- [Alt ağda uygulanması gereken bazı güvenlik kuralları](sql-database-managed-instance-connectivity-architecture.md#network-requirements)vardır.

### <a name="vnet"></a>ADLı
- VNet, kaynak modeli kullanılarak dağıtılabilir-sanal ağ için klasik model desteklenmez.
- Yönetilen bir örnek oluşturulduktan sonra, yönetilen örneği veya VNet 'i başka bir kaynak grubuna veya aboneliğe taşımak desteklenmez.
- App Service ortamları, Logic Apps ve yönetilen örnekler (coğrafi çoğaltma, Işlemsel çoğaltma veya bağlı sunucular aracılığıyla kullanılan) gibi bazı hizmetler, sanal ağları küresel olarak bağlandığında farklı bölgelerdeki yönetilen örneklere erişemez [ eşleme](../virtual-network/virtual-networks-faq.md#what-are-the-constraints-related-to-global-vnet-peering-and-load-balancers). Sanal ağ geçitleri aracılığıyla ExpressRoute veya VNet-VNet aracılığıyla bu kaynaklara bağlanabilirsiniz.

### <a name="tempdb"></a>'Nın

@No__t-0 ' ın en büyük dosya boyutu, bir Genel Amaçlı katmanında çekirdek başına 24 GB 'den büyük olamaz. Bir İş Açısından Kritik katmanındaki en fazla `tempdb` boyutu örnek depolama boyutuyla sınırlıdır. `Tempdb` günlük dosyası boyutu, Genel Amaçlı ve İş Açısından Kritik katmanlarında 120 GB ile sınırlıdır. Bazı sorgular, `tempdb` ' da çekirdek başına 24 GB 'den fazla gereksinim duyduklarında veya 120 GB 'den fazla günlük verisi ürettiklerinde bir hata döndürebilir.

### <a name="error-logs"></a>Hata günlükleri

Yönetilen bir örnek, hata günlüklerinde ayrıntılı bilgileri koyar. Hata günlüğünde günlüğe kaydedilen çok sayıda iç sistem olayı vardır. İlgisiz bazı girdilerin filtrelediğini belirten hata günlüklerini okumak için özel bir yordam kullanın. Daha fazla bilgi için bkz. [yönetilen örnek – sp_readmierrorlog](https://blogs.msdn.microsoft.com/sqlcat/2018/05/04/azure-sql-db-managed-instance-sp_readmierrorlog/).

## <a name="Issues"></a> Bilinen sorunlar

### <a name="wrong-error-returned-while-trying-to-remove-a-file-that-is-not-empty"></a>Boş olmayan bir dosya kaldırılmaya çalışılırken hatalı hata döndürüldü

**Tarih:** Eki 2019

SQL Server/yönetilen örnek [, kullanıcının boş olmayan bir dosyayı bırakmaya izin vermez](https://docs.microsoft.com/sql/relational-databases/databases/delete-data-or-log-files-from-a-database#Prerequisites). Boş olmayan bir veri dosyasını `ALTER DATABASE REMOVE FILE` ifadesini kullanarak kaldırmaya çalışırsanız, `Msg 5042 – The file '<file_name>' cannot be removed because it is not empty` hatası hemen döndürülmeyecektir. Yönetilen örnek dosyayı bırakmaya devam eder ve `Internal server error` ile 30 dakika sonra işlem başarısız olur.

**Geçici çözüm**: `DBCC SHRINKFILE (N'<file_name>', EMPTYFILE)` komutunu kullanarak dosyanın içeriğini kaldırın. Dosya grubunda tek dosya varsa, dosyayı daralmadan önce bu dosya grubuyla ilişkili tablodan veya bölümden verileri silmeniz ve isteğe bağlı olarak bu verileri başka bir tabloya/bölüme yüklemeniz gerekir.

### <a name="change-service-tier-and-create-instance-operations-are-blocked-by-ongoing-database-restore"></a>Hizmet katmanını değiştirme ve örnek oluşturma işlemleri sürekli veritabanı geri yükleme tarafından engelleniyor

**Tarih:** Eyl 2019

Devam eden `RESTORE` ekstresi, veri geçiş hizmeti geçiş işlemi ve yerleşik nokta geri yükleme işlemi, hizmet katmanını güncelleştirmeyi veya mevcut örneğin yeniden boyutlandırılmasını engeller ve geri yükleme işlemi bitene kadar yeni örnekler oluşturur. Geri yükleme işlemi, geri yükleme işleminin çalıştırıldığı aynı alt ağdaki yönetilen örneklerde ve örnek havuzlardaki bu işlemleri engeller. Örnek havuzlardaki örnekler etkilenmez. Hizmet katmanı işlemleri oluşturma veya değiştirme başarısız olmayacak veya zaman aşımı-geri yükleme işlemi tamamlandıktan veya iptal edildikten sonra devam eder.

**Geçici çözüm**: geri yükleme işlemi tamamlanana kadar bekleyin veya hizmet katmanı oluşturma veya güncelleştirme işlemi daha yüksek önceliğe sahipse geri yükleme işlemini iptal edin.

### <a name="missing-validations-in-restore-process"></a>Restore işleminde eksik doğrulamalar

**Tarih:** Eyl 2019

`RESTORE` açıklaması ve yerleşik bir nokta geri yükleme, geri yüklenen veritabanında bazı nessecary denetimleri gerçekleştirmez:
- **DBCC CHECKDB** -  @ no__t-2 ifadesinde geri yüklenen veritabanında `DBCC CHECKDB` gerçekleştirilemiyor. Özgün bir veritabanı bozuksa veya Azure Blob depolama alanına kopyalanırken yedekleme dosyası bozuksa, otomatik yedeklemeler alınmaz ve Azure desteği müşteriyle iletişim kuracaktır. 
- Yerleşik bir zaman noktası geri yükleme işlemi, İş Açısından Kritik örneğinden otomatik yedeklemenin [bellek ıçı OLTP nesnelerini](sql-database-in-memory.md#in-memory-oltp)içermediğini denetlemez. 

**Geçici çözüm**: yedekleme yapmadan önce kaynak veritabanında `DBCC CHECKDB` ' i Yürütdiğinizden ve yönetilen örnek üzerinde geri yüklenelebilecek olası bozulmaları önlemek için yedeklemede `WITH CHECKSUM` seçeneğini kullanmaktan emin olun. Kaynak veritabanınızın Genel Amaçlı katmana geri yüklüyorsanız [bellek ıçı OLTP nesneleri](sql-database-in-memory.md#in-memory-oltp) içermediğinden emin olun.

### <a name="resource-governor-on-business-critical-service-tier-might-need-to-be-reconfigured-after-failover"></a>İş Açısından Kritik hizmet katmanındaki Resource Governor yük devretmeden sonra yeniden yapılandırılması gerekebilir

**Tarih:** Eyl 2019

Kullanıcı iş yüküne atanan kaynakları sınırlandırmanızı sağlayan [Resource Governor](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor) özelliği, yük devretmeden sonra bazı Kullanıcı iş yükünü yanlış sınıflandırabilir veya hizmet katmanının Kullanıcı tarafından başlatılan değişikliğini (örneğin, en fazla sanal çekirdek veya en büyük örnek değişikliği) depolama boyutu).

**Geçici çözüm**: `ALTER RESOURCE GOVERNOR RECONFIGURE` ' i düzenli aralıklarla veya [Resource Governor](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor)kullanıyorsanız örnek başladığında SQL Agent Işi çalıştıran SQL Aracısı işinin bir parçası olarak çalıştırın.

### <a name="cannot-authenticate-to-external-mail-servers-using-secure-connection-ssl"></a>Güvenli bağlantı (SSL) kullanılarak dış posta sunucularında kimlik doğrulaması yapılamıyor

**Tarih:** Ağu 2019

[Güvenli bağlantı (SSL) kullanılarak yapılandırılan](https://docs.microsoft.com/sql/relational-databases/database-mail/configure-database-mail) veritabanı postası, Azure dışındaki bazı e-posta sunucularında kimlik doğrulaması yapamaz. Bu, yakında çözümlenecek olan güvenlik yapılandırması sorununa neden olur.

**Geçici çözüm:** Güvenli bağlantı (SSL), sorun çözülene kadar veritabanı posta yapılandırmasından geçici kaldırma. 

### <a name="cross-database-service-broker-dialogs-must-be-re-initialized-after-service-tier-upgrade"></a>Çapraz veritabanı Hizmet Aracısı iletişim kutuları, hizmet katmanı yükseltmesinden sonra yeniden başlatılmalıdır

**Tarih:** Ağu 2019

Çapraz veritabanı Hizmet Aracısı iletişim kutuları, hizmet katmanı işlemini değiştirdikten sonra iletileri diğer veritabanlarındaki hizmetlere teslim eder. İletiler **kaybolmaz** ve gönderici kuyruğunda bulunabilir. Yönetilen örnekteki sanal çekirdeklerin veya örnek depolama boyutunun herhangi bir değişikliği, [sys. databases](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-databases-transact-sql) görünümünde tüm veritabanları için değiştirilecek `service_broke_guid` değeri oluşmasına neden olur. Diğer veritabanındaki hizmet aracılarına başvuran [BEGIN iletişim kutusu](https://docs.microsoft.com/en-us/sql/t-sql/statements/begin-dialog-conversation-transact-sql) ifadesiyle oluşturulan `DIALOG`, hedef hizmete ileti teslim etme işlemini durdurur.

**Geçici çözüm:** Hizmet katmanını güncelleştirmeden önce veritabanları arası Hizmet Aracısı iletişim konuşmaları kullanan tüm etkinlikleri durdurun ve sonra yeniden başlatın. Hizmet katmanı değişikliğinden sonra teslim edilmemiş kalan iletiler varsa, kaynak kuyruktaki iletileri okuyun ve hedef sıraya yeniden gönderin.

### <a name="impersonification-of-aad-login-types-is-not-supported"></a>AAD oturum açma türlerinin ımpersonbirleşme işlemi desteklenmiyor

**Tarih:** 2019 Temmuz

Aşağıdaki AAD sorumlularının `EXECUTE AS USER` veya `EXECUTE AS LOGIN` kullanarak kimliğe bürünme desteklenmez:
-   Diğer ad AAD kullanıcıları. Bu durumda `15517` ' a aşağıdaki hata döndürülür.
- Aad uygulamaları ve hizmet sorumlularına göre AAD oturum açmaları ve kullanıcılar. Bu durumda `15517` ve `15406` durumunda aşağıdaki hatalar döndürülür.

### <a name="query-parameter-not-supported-in-sp_send_db_mail"></a>@query parametresi sp_send_db_mail içinde desteklenmez

**Tarih:** 2019 Nisan

[Sp_send_db_mail](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-send-dbmail-transact-sql) yordamındaki `@query` parametresi çalışmıyor.

### <a name="transactional-replication-must-be-reconfigured-after-geo-failover"></a>İşlemsel çoğaltmanın coğrafi Yük devretme sonrasında yeniden yapılandırılması gerekir

**Tarih:** Mar 2019

Bir otomatik yük devretme grubundaki bir veritabanında Işlem çoğaltması etkinleştirilmişse, yönetilen örnek yöneticisinin eski birincil üzerindeki tüm yayınları temizlemesi ve başka bir bölgeye yük devretme gerçekleştikten sonra yeni birincil üzerinde yeniden yapılandırması gerekir. Daha fazla ayrıntı için bkz. [çoğaltma](#replication) .

### <a name="aad-logins-and-users-are-not-supported-in-tools"></a>AAD oturum açmaları ve kullanıcılar araçlar 'da desteklenmez

**Tarih:** Ocak 2019

SQL Server Management Studio ve SQL Server Veri Araçları Azure Active Directory oturumlarını ve kullanıcılarını tam olarak desteklemez.
- SQL Server Veri Araçları şu anda Azure AD Server sorumlularını (oturum açma) ve kullanıcıları (Genel Önizleme) kullanma desteklenmiyor.
- Azure AD Server sorumluları (oturumlar) ve kullanıcıları (Genel Önizleme) için betik oluşturma SQL Server Management Studio desteklenmez.

### <a name="temporary-database-is-used-during-restore-operation"></a>GERI yükleme işlemi sırasında geçici veritabanı kullanılıyor

Bir veritabanı yönetilen örneğe geri yüklenirken, geri yükleme hizmeti öncelikle adı örnek üzerinde ayırmak için istenen ada sahip boş bir veritabanı oluşturur. Bir süre sonra, bu veritabanı bırakılır ve gerçek veritabanının geri yüklenmesi başlatılır. *Geri yükleme* durumundaki veritabanı geçici olarak ad yerine rastgele bir GUID değeri olur. Geri yükleme işlemi tamamlandıktan sonra, geçici ad `RESTORE` ifadesinde belirtilen istenen adla değiştirilecektir. İlk aşamada, Kullanıcı boş veritabanına erişebilir ve hatta tablo oluşturabilir veya bu veritabanında veri yükleyebilir. Bu geçici veritabanı, geri yükleme hizmeti ikinci aşamayı başlattığında bırakılacak.

**Geçici çözüm**: geri yükleme işleminin tamamlandığını görene kadar geri yüklemekte olduğunuz veritabanına erişmeyin.

### <a name="tempdb-structure-and-content-is-re-created"></a>TEMPDB yapısı ve içerik yeniden oluşturuluyor

@No__t-0 veritabanı her zaman 12 veri dosyasına bölünür ve dosya yapısı değiştirilemez. Dosya başına en büyük boyut değiştirilemez ve yeni dosyalar `tempdb` ' a eklenemez. `Tempdb`, örnek başladığında veya başarısız olduğunda boş bir veritabanı olarak her zaman yeniden oluşturulur ve `tempdb` ' de yapılan değişiklikler korunmaz.

### <a name="exceeding-storage-space-with-small-database-files"></a>Küçük veritabanı dosyalarıyla depolama alanını aşma

örnek Azure depolama sınırına ulaşabileceğinden `CREATE DATABASE`, `ALTER DATABASE ADD FILE` ve `RESTORE DATABASE` deyimleri başarısız olabilir.

Her Genel Amaçlı yönetilen örnek, Azure Premium disk alanı için ayrılan 35 TB 'a kadar depolama alanı içerir. Her veritabanı dosyası ayrı bir fiziksel diske yerleştirilir. Disk boyutları 128 GB, 256 GB, 512 GB, 1 TB veya 4 TB olabilir. Diskteki kullanılmayan alan ücretlendirilmez, ancak Azure Premium disk boyutlarının toplam toplamı 35 TB 'yi aşamaz. Bazı durumlarda, toplam olarak 8 TB 'lık bir yönetilen örnek, iç parçalanma nedeniyle depolama boyutu 35 TB Azure sınırını aşabilir.

Örneğin, Genel Amaçlı yönetilen bir örnek, 4 TB 'lık bir diske yerleştirilmiş boyutu 1,2 TB olan bir büyük dosyaya sahip olabilir. Ayrıca, her biri ayrı 128 GB disklere yerleştirilmiş 1 GB boyutundaki 248 dosya olabilir. Bu örnekte:

- Ayrılan toplam disk depolama boyutu 1 x 4 TB + 248 x 128 GB = 35 TB 'tır.
- Örnekteki veritabanları için ayrılan toplam alan 1 x 1,2 TB + 248 x 1 GB = 1,4 TB 'tır.

Bu örnekte, belirli bir dosya dağıtımı nedeniyle yönetilen bir örnek, bir bağlı Azure Premium diski için ayrılan 35 TB sınırına ulaşamadığınızda, bu örnek için bir yönetilen örnek olduğunu gösterir.

Bu örnekte, mevcut veritabanları çalışmaya devam eder ve yeni dosyalar eklenmedikçe herhangi bir sorun olmadan büyüyebilir. Tüm veritabanlarının toplam boyutu örnek boyut sınırına ulaşmasa bile yeni disk sürücüleri için yeterli alan olmadığından yeni veritabanları oluşturulamaz veya geri yüklenemez. Bu durumda döndürülen hata net değildir.

[Kalan dosyaların sayısını](https://medium.com/azure-sqldb-managed-instance/how-many-files-you-can-create-in-general-purpose-azure-sql-managed-instance-e1c7c32886c1) sistem görünümlerini kullanarak belirleyebilirsiniz. Bu sınıra ulaştıysanız, [DBCC SHRINKFILE ifadesini kullanarak daha küçük bir dosyayı boş ve silmeyi](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-shrinkfile-transact-sql#d-emptying-a-file) deneyin veya [Bu sınıra sahip olmayan iş açısından kritik katmanına](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-resource-limits#service-tier-characteristics)geçin.

### <a name="guid-values-shown-instead-of-database-names"></a>Veritabanı adları yerine gösterilen GUID değerleri

Çeşitli sistem görünümleri, performans sayaçları, hata iletileri, XEvents ve hata günlüğü girdileri, gerçek veritabanı adları yerine GUID veritabanı tanımlayıcılarını görüntüler. Gelecekte gerçek veritabanı adlarıyla değiştirildiklerinden, bu GUID tanımlayıcılarına güvenmeyin.

### <a name="error-logs-arent-persisted"></a>Hata günlükleri kalıcı değil

Yönetilen örnekte kullanılabilen hata günlükleri kalıcı değil ve boyutu en fazla depolama sınırına dahil değildir. Yük devretme gerçekleşirse hata günlükleri otomatik olarak silinebilir. Yönetilen örnek birkaç sanal makinede birkaç kez taşındığından, hata günlüğü geçmişinde boşluklar olabilir.

### <a name="transaction-scope-on-two-databases-within-the-same-instance-isnt-supported"></a>Aynı örnek içindeki iki veritabanında işlem kapsamı desteklenmiyor

Aynı işlem kapsamında aynı örnek içindeki iki veritabanına iki sorgu gönderilirse, .NET 'teki `TransactionScope` sınıfı çalışmaz:

```csharp
using (var scope = new TransactionScope())
{
    using (var conn1 = new SqlConnection("Server=quickstartbmi.neu15011648751ff.database.windows.net;Database=b;User ID=myuser;Password=mypassword;Encrypt=true"))
    {
        conn1.Open();
        SqlCommand cmd1 = conn1.CreateCommand();
        cmd1.CommandText = string.Format("insert into T1 values(1)");
        cmd1.ExecuteNonQuery();
    }

    using (var conn2 = new SqlConnection("Server=quickstartbmi.neu15011648751ff.database.windows.net;Database=b;User ID=myuser;Password=mypassword;Encrypt=true"))
    {
        conn2.Open();
        var cmd2 = conn2.CreateCommand();
        cmd2.CommandText = string.Format("insert into b.dbo.T2 values(2)");        cmd2.ExecuteNonQuery();
    }

    scope.Complete();
}

```

Bu kod aynı örnekteki verilerle çalışabilse de, MSDTC 'yi gerektirir.

**Geçici çözüm:** İki bağlantı kullanmak yerine, başka bir veritabanını bir bağlantı bağlamında kullanmak için [SqlConnection. ChangeDatabase (dize)](https://docs.microsoft.com/dotnet/api/system.data.sqlclient.sqlconnection.changedatabase) kullanın.

### <a name="clr-modules-and-linked-servers-sometimes-cant-reference-a-local-ip-address"></a>CLR modülleri ve bağlı sunucular bazen yerel bir IP adresine başvuramaz

Yönetilen bir örneğe yerleştirilmiş olan CLR modülleri veya geçerli örneğe başvuran dağıtılmış sorgular bazen yerel bir örneğin IP 'sini çözümleyemeyebilir. Bu hata, geçici bir sorundur.

**Geçici çözüm:** Mümkünse, bir CLR modülünde bağlam bağlantılarını kullanın.

## <a name="next-steps"></a>Sonraki adımlar

- Yönetilen örnekler hakkında daha fazla bilgi için bkz. [yönetilen örnek nedir?](sql-database-managed-instance.md)
- Özellikler ve karşılaştırma listesi için bkz. [Azure SQL veritabanı özelliği karşılaştırması](sql-database-features.md).
- Yeni bir yönetilen örnek oluşturmayı gösteren bir hızlı başlangıç için bkz. [yönetilen örnek oluşturma](sql-database-managed-instance-get-started.md).
