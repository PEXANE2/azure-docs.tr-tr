---
title: Yönetilen örnek T-SQL farkları
description: Bu makalede, Azure SQL veritabanı ve SQL Server yönetilen bir örnek arasındaki T-SQL farklılıkları ele alınmaktadır
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein, carlrab, bonova, danil
ms.date: 03/11/2020
ms.custom: seoapril2019
ms.openlocfilehash: e01c61ca4f415ffbb46c86034d4b7441bc2617d9
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80365493"
---
# <a name="managed-instance-t-sql-differences-and-limitations"></a>Yönetilen örnek T-SQL farkları ve sınırlamaları

Bu makalede, Azure SQL veritabanı yönetilen örneği ve şirket içi SQL Server veritabanı altyapısı arasındaki söz dizimi ve davranış farklılıkları özetlenmektedir ve açıklanmaktadır. Yönetilen örnek dağıtım seçeneği, şirket içi SQL Server veritabanı altyapısı ile yüksek uyumluluk sağlar. SQL Server veritabanı altyapısı özelliklerinin çoğu yönetilen bir örnekte desteklenir.

![Geçiş](./media/sql-database-managed-instance/migration.png)

Yönetilen örnekte tanıtılan bazı PaaS sınırlamaları ve SQL Server karşılaştırıldığında bazı davranış değişiklikleri vardır. Farklar aşağıdaki kategorilere ayrılmıştır:<a name="Differences"></a>

- [Kullanılabilirlik](#availability) , [her zaman açık kullanılabilirlik grupları](#always-on-availability-groups) ve [yedeklemelerdeki](#backup)farkları içerir.
- [Güvenlik](#security) , [Denetim](#auditing), [sertifika](#certificates), [kimlik bilgileri](#credential), [şifreleme sağlayıcıları](#cryptographic-providers), [oturum açmalar ve kullanıcılar](#logins-and-users)ve [hizmet anahtarı ile hizmet ana anahtarı](#service-key-and-service-master-key)arasındaki farkları içerir.
- [Yapılandırma](#configuration) , [arabellek havuzu genişletme](#buffer-pool-extension), [harmanlama](#collation), [Uyumluluk düzeyleri](#compatibility-levels), [veritabanı yansıtma](#database-mirroring), [veritabanı seçenekleri](#database-options), [SQL Server Agent](#sql-server-agent)ve [tablo seçeneklerindeki](#tables)farklılıkları içerir.
- [İşlevler](#functionalities) şunlardır [bulk INSERT/OPENROWSET](#bulk-insert--openrowset), [clr](#clr), [DBCC](#dbcc), [Dağıtılmış işlemler](#distributed-transactions), [genişletilmiş olaylar](#extended-events), [dış kitaplıklar](#external-libraries), [FILESTREAM ve FileTable](#filestream-and-filetable), [tam metin anlam arama](#full-text-semantic-search), [bağlı sunucular](#linked-servers), [PolyBase](#polybase), [çoğaltma](#replication), [geri yükleme](#restore-statement), [Hizmet Aracısı](#service-broker), [saklı yordamlar, işlevler ve Tetikleyiciler](#stored-procedures-functions-and-triggers).
- Sanal ağlar ve alt ağ yapılandırması gibi [ortam ayarları](#Environment) .

Bu özelliklerin çoğu mimari kısıtlamalardır ve hizmet özelliklerini temsil eder.

Yönetilen örnekte bulunan ve gelecekte çözümlenecek olan geçici bilinen sorunlar, [sürüm notları sayfasında](sql-database-release-notes.md)açıklanmaktadır.

## <a name="availability"></a>Kullanılabilirlik

### <a name="always-on-availability-groups"></a><a name="always-on-availability-groups"></a>Always on kullanılabilirlik grupları

[Yüksek kullanılabilirlik](sql-database-high-availability.md) yönetilen örnekte yerleşiktir ve kullanıcılar tarafından denetlenemez. Aşağıdaki deyimler desteklenmez:

- [UÇ NOKTA OLUŞTUR... DATABASE_MIRRORING IÇIN](/sql/t-sql/statements/create-endpoint-transact-sql)
- [KULLANıLABILIRLIK GRUBU OLUŞTUR](/sql/t-sql/statements/create-availability-group-transact-sql)
- [KULLANıLABILIRLIK GRUBUNU DEĞIŞTIR](/sql/t-sql/statements/alter-availability-group-transact-sql)
- [KULLANıLABILIRLIK GRUBU BıRAKMA](/sql/t-sql/statements/drop-availability-group-transact-sql)
- [Alter database](/sql/t-sql/statements/alter-database-transact-sql) ifadesinin [set HADR](/sql/t-sql/statements/alter-database-transact-sql-set-hadr) yan tümcesi

### <a name="backup"></a>Backup

Yönetilen örneklerin otomatik yedeklemeleri vardır, böylece kullanıcılar tam veritabanı `COPY_ONLY` yedeklemeleri oluşturabilir. Fark, günlük ve dosya anlık görüntüsü yedeklemeleri desteklenmez.

- Yönetilen bir örnek ile, bir örnek veritabanını yalnızca bir Azure Blob depolama hesabına yedekleyebilirsiniz:
  - Yalnızca `BACKUP TO URL` desteklenir.
  - `FILE`, `TAPE`, ve yedekleme cihazları desteklenmez.
- Genel `WITH` seçeneklerin çoğu desteklenir.
  - `COPY_ONLY`zorunludur.
  - `FILE_SNAPSHOT`desteklenmez.
  - Bant seçenekleri: `REWIND`, `NOREWIND`, `UNLOAD`, ve `NOUNLOAD` desteklenmez.
  - Günlüğe özel seçenekler: `NORECOVERY`, `STANDBY`ve `NO_TRUNCATE` desteklenmez.

Sınırlamalar: 

- Yönetilen bir örnek ile, yedekleme sıkıştırması kullanılıyorsa 4 TB 'a kadar olan veritabanları için yeterli olan bir örnek veritabanını en fazla 32 şeritli bir yedeklemeye yedekleyebilirsiniz.
- Hizmet tarafından yönetilen `BACKUP DATABASE ... WITH COPY_ONLY` saydam veri şifrelemesi (tde) ile şifrelenmiş bir veritabanında yürütemezsiniz. Hizmet tarafından yönetilen TDE, yedeklemelerin dahili bir TDE anahtarla şifrelenmesini zorlar. Anahtar verilemiyor, bu nedenle yedeklemeyi geri alamazsınız. Otomatik yedeklemeler ve zaman içinde geri yükleme kullanın veya bunun yerine [müşteri tarafından yönetilen (BYOK) TDE](transparent-data-encryption-azure-sql.md#customer-managed-transparent-data-encryption---bring-your-own-key) kullanın. Ayrıca, veritabanında şifrelemeyi devre dışı bırakabilirsiniz.
- Yönetilen bir örnekteki `BACKUP` komutu kullanılarak en fazla yedekleme şeridi boyutu, en fazla BLOB boyutu olan 195 GB 'dir. Tek tek Stripe boyutunu azaltmak ve bu sınırın içinde kalmak için yedekleme komutundaki şeritler sayısını artırın.

    > [!TIP]
    > Bu kısıtlamayı geçici olarak çözmek için, bir veritabanını şirket içi bir ortamda veya bir sanal makinede SQL Server yedekleyerek şunları yapabilirsiniz:
    >
    > - ' A yedeklemek `DISK` yerine uygulamasına yedekleyin `URL`.
    > - Yedekleme dosyalarını blob depolamaya yükleyin.
    > - Yönetilen örneğe geri yükleyin.
    >
    > Yönetilen `Restore` bir örnekteki komut, karşıya yüklenen yedekleme dosyalarının depolanması için farklı bir blob türü kullanıldığından, yedekleme dosyalarındaki daha büyük blob boyutlarını destekler.

T-SQL kullanarak yedeklemeler hakkında daha fazla bilgi için bkz. [yedekleme](/sql/t-sql/statements/backup-transact-sql).

## <a name="security"></a>Güvenlik

### <a name="auditing"></a>Denetim

Azure SQL veritabanı ve SQL Server veritabanlarında bulunan veritabanlarında denetim arasındaki temel farklılıklar şunlardır:

- Azure SQL veritabanı 'ndaki yönetilen örnek dağıtım seçeneği ile, denetim sunucu düzeyinde çalışmaktadır. `.xel` Günlük dosyaları Azure Blob depolama alanında depolanır.
- Azure SQL veritabanı 'nda tek veritabanı ve elastik havuz dağıtım seçenekleriyle denetim, veritabanı düzeyinde çalışmaktadır.
- Şirket içi veya sanal makineler SQL Server, denetim sunucu düzeyinde çalışmaktadır. Olaylar dosya sistemi veya Windows olay günlüklerine depolanır.
 
Yönetilen örnekteki XEvent denetimi Azure Blob depolama hedeflerini destekler. Dosya ve Windows günlükleri desteklenmez.

Azure Blob depolamaya denetim için `CREATE AUDIT` söz diziminde önemli farklılıklar şunlardır:

- Dosyaların yerleştirildiği Azure `TO URL` BLOB depolama kapsayıcısının URL 'sini belirtmek için kullanabileceğiniz yeni bir sözdizimi sağlanır. `.xel`
- Yönetilen bir `TO FILE` örnek Windows dosya paylaşımlarına erişemediği için sözdizimi desteklenmiyor.

Daha fazla bilgi için bkz. 

- [SUNUCU DENETIMI OLUŞTUR](/sql/t-sql/statements/create-server-audit-transact-sql) 
- [SUNUCU DENETIMINI DEĞIŞTIR](/sql/t-sql/statements/alter-server-audit-transact-sql)
- [Denetim](/sql/relational-databases/security/auditing/sql-server-audit-database-engine)

### <a name="certificates"></a>Sertifikalar

Yönetilen bir örnek dosya paylaşımlarına ve Windows klasörlerine erişemez, bu nedenle aşağıdaki kısıtlamalar geçerlidir:

- `CREATE FROM` / `BACKUP TO`
- `CREATE` / Sertifika `ASSEMBLY` desteklenmiyor. `FILE` `BACKUP` / Özel anahtar dosyaları kullanılamıyor. 

Bkz. SERTIFIKA ve [yedekleme sertifikası](/sql/t-sql/statements/backup-certificate-transact-sql) [oluşturma](/sql/t-sql/statements/create-certificate-transact-sql) . 
 
**Geçici çözüm**: sertifika yedeklemesi oluşturmak ve yedeklemeyi geri yüklemek yerine [sertifika ikili içeriğini ve özel anahtarı alın, bunu. SQL dosyası olarak depolayın ve ikiliden oluşturun](/sql/t-sql/functions/certencoded-transact-sql#b-copying-a-certificate-to-another-database):

```sql
CREATE CERTIFICATE  
   FROM BINARY = asn_encoded_certificate
WITH PRIVATE KEY (<private_key_options>)
```

### <a name="credential"></a>Kimlik Bilgisi

Yalnızca Azure Key Vault ve `SHARED ACCESS SIGNATURE` kimlikleri desteklenir. Windows kullanıcıları desteklenmez.

Bkz. [KIMLIK bilgisi oluşturma](/sql/t-sql/statements/create-credential-transact-sql) ve [kimlik bilgisi değiştirme](/sql/t-sql/statements/alter-credential-transact-sql).

### <a name="cryptographic-providers"></a>Şifreleme sağlayıcıları

Yönetilen bir örnek dosyalara erişemez, bu nedenle şifreleme sağlayıcıları oluşturulamıyor:

- `CREATE CRYPTOGRAPHIC PROVIDER`desteklenmez. Bkz. [ŞIFRELEME sağlayıcısı oluşturma](/sql/t-sql/statements/create-cryptographic-provider-transact-sql).
- `ALTER CRYPTOGRAPHIC PROVIDER`desteklenmez. Bkz. [alter CRYPTOGRAPHIC PROVIDER](/sql/t-sql/statements/alter-cryptographic-provider-transact-sql).

### <a name="logins-and-users"></a>Oturum açma bilgileri ve kullanıcılar

- , `FROM CERTIFICATE` `FROM ASYMMETRIC KEY`Ve `FROM SID` kullanılarak oluşturulan SQL oturum açmaları desteklenir. Bkz. [oturum oluşturma](/sql/t-sql/statements/create-login-transact-sql).
- Azure Active Directory (Azure AD), [oturum açma sözdizimi oluştur](/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current) veya [oturum açma işleminden Kullanıcı oluşturma [Azure AD oturum açma]](/sql/t-sql/statements/create-user-transact-sql?view=azuresqldb-mi-current) sözdizimi kullanılarak oluşturulan sunucu sorumluları (oturumlar) desteklenir. Bu oturumlar sunucu düzeyinde oluşturulur.

    Yönetilen örnek, söz dizimi `CREATE USER [AADUser/AAD group] FROM EXTERNAL PROVIDER`Ile Azure AD veritabanı sorumlularını destekler. Bu özellik, Azure AD içeren veritabanı kullanıcıları olarak da bilinir.

- `CREATE LOGIN ... FROM WINDOWS` Sözdizimi ile oluşturulan Windows oturum açmaları desteklenmez. Azure Active Directory oturumlarını ve kullanıcıları kullanın.
- Örneği oluşturan Azure AD kullanıcısının [sınırsız yönetici ayrıcalıkları](sql-database-manage-logins.md)vardır.
- Yönetici olmayan Azure AD Veritabanı düzeyi kullanıcılar `CREATE USER ... FROM EXTERNAL PROVIDER` söz dizimi kullanılarak oluşturulabilir. Bkz [. Kullanıcı oluşturma... Dış SAĞLAYıCıDAN](sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities).
- Azure AD Server sorumluları (oturum açmalar) yalnızca yönetilen bir örnek içindeki SQL özelliklerini destekler. Aynı Azure AD kiracısı veya farklı kiracılar dahilinde olup olmadıkları fark etmeksizin, Azure AD kullanıcıları için desteklenmez. Bu özelliklere örnek olarak şunlar verilebilir:

  - SQL işlem çoğaltması.
  - Sunucu bağlayın.

- Veritabanı sahibi olarak Azure AD grubuna eşlenmiş bir Azure AD oturum açmanın ayarlanması desteklenmez.
- Diğer Azure AD sorumlularını kullanarak Azure AD sunucu düzeyi sorumluları 'nın kimliğe bürünme işlemi, [execute as](/sql/t-sql/statements/execute-as-transact-sql) yan tümcesi gibi desteklenir. FARKLı ÇALıŞTıR sınırlamaları şunlardır:

  - Ad, oturum açma adından farklı olduğunda Azure AD kullanıcıları için Kullanıcı tarafından desteklenmez. Örneğin, kullanıcının LOGIN [john@contoso.com] ADLı Kullanıcı oluşturma [myaaduser] sözdizimi aracılığıyla oluşturulduğu ve kimliğe bürünme ÖZELLIĞI, exec as user = _myaaduser_aracılığıyla denendiğinde bir örnektir. Bir Azure AD sunucu sorumlusu 'ndan (oturum açma) bir **Kullanıcı** oluşturduğunuzda, user_name **oturum açma**işleminden aynı login_name olarak belirtin.
  - Yalnızca `sysadmin` rolün parçası olan SQL Server düzeyi sorumlular (oturumlar), Azure AD sorumlularını hedefleyen aşağıdaki işlemleri yürütebilir:

    - KULLANıCı OLARAK YÜRÜT
    - OTURUM AÇMA OLARAK YÜRÜT

- Bacpac dosyalarını kullanarak veritabanı dışarı aktarma/içeri aktarma, [SMS v 18.4 veya üzeri](/sql/ssms/download-sql-server-management-studio-ssms)ya da [SqlPackage. exe](/sql/tools/sqlpackage-download)KULLANıLARAK yönetilen örnekteki Azure AD kullanıcıları için desteklenir.
  - Aşağıdaki konfigürasyonlar veritabanı bacpac dosyası kullanılarak desteklenir: 
    - Aynı Azure AD etki alanı içindeki farklı yönetme örnekleri arasında bir veritabanını içeri/dışarı aktarın.
    - Yönetilen örnekten bir veritabanını dışarı aktarın ve aynı Azure AD etki alanı içindeki SQL veritabanı 'na içeri aktarın. 
    - SQL veritabanından bir veritabanını dışarı aktarın ve aynı Azure AD etki alanı içindeki yönetilen örneğe içeri aktarın.
    - Yönetilen örnekten bir veritabanını dışarı aktarın ve SQL Server (sürüm 2012 veya üzeri) içeri aktarın.
      - Bu yapılandırmada tüm Azure AD kullanıcıları, oturum açma bilgileri olmayan SQL veritabanı sorumluları (kullanıcılar) olarak oluşturulur. Kullanıcıların türü SQL olarak listelenir (sys. database_principals içinde SQL_USER olarak görünür). İzinleri ve rolleri SQL Server veritabanı meta verilerinde kalır ve kimliğe bürünme için kullanılabilir. Ancak, kimlik bilgilerini kullanarak SQL Server erişmek ve oturum açmak için kullanılamaz.

- Yalnızca yönetilen örnek sağlama işlemi tarafından oluşturulan sunucu düzeyi asıl oturum açma, `securityadmin` veya `sysadmin`gibi sunucu rollerinin üyeleri veya sunucu düzeyinde herhangi BIR oturum açma iznini Değiştir ile diğer oturumlar, yönetilen örnek IÇIN ana veritabanında Azure AD Server sorumlularını (oturum açma) oluşturabilir.
- Oturum açma bir SQL sorumlusu ise, yalnızca `sysadmin` rolün parçası olan oturum açma Işlemleri BIR Azure AD hesabı için oturum açma bilgileri oluşturmak üzere Oluştur komutunu kullanabilir.
- Azure AD oturum açma, Azure SQL veritabanı yönetilen örneği için kullanılan aynı dizin içinde bir Azure AD 'nin üyesi olmalıdır.
- Azure AD Server sorumluları (oturum açmalar), SQL Server Management Studio 18,0 Preview 5 ' te başlayan Nesne Gezgini görünür.
- Azure AD yönetici hesabıyla çakışan Azure AD Server sorumlularına (oturum açmalar) izin verilir. Sorumluyu çözümlediğinizde ve yönetilen örneğe izinleri uyguladığınızda Azure AD Server sorumluları (oturum açmalar) Azure AD Yöneticisi üzerinden önceliklidir.
- Kimlik doğrulama sırasında, kimliği doğrulanan sorumluyu çözümlemek için aşağıdaki sıra uygulanır:

    1. Azure AD hesabı, sys. server_principals ' de bulunan Azure AD Server sorumlusu (oturum açma) ile doğrudan eşlenmiş olarak mevcutsa, Azure AD Server sorumlusu (oturum açma) için erişim izni verin ve izinleri uygulayın.
    2. Azure AD hesabı, server_principals sys 'de bulunan Azure AD Server sorumlusu (oturum açma) ile eşlenmiş bir Azure AD grubunun üyesiyse, "X" türü olarak erişim izni verin ve Azure AD grubu oturum açma izinlerini uygulayın.
    3. Azure AD hesabı yönetilen örnek için yönetilen örnek sistem görünümlerinde mevcut olmayan özel bir portal tarafından yapılandırılmış Azure AD yöneticisi ise, yönetilen örnek (eski mod) için Azure AD yöneticisi 'nin özel sabit izinlerini uygulayın.
    4. Azure AD hesabı bir veritabanında bulunan bir Azure AD kullanıcısına doğrudan eşlenmiş olarak varsa ("E" türü olarak, Azure AD veritabanı kullanıcısına erişim izni verin ve izinleri uygulayın. database_principals.
    5. Azure AD hesabı, sys. database_principals "X" türü olarak bir veritabanında bulunan bir Azure AD kullanıcısına eşlenmiş bir Azure AD grubunun üyesiyse, erişim izni verin ve Azure AD grubu oturum açma izinlerini uygulayın.
    6. Kimlik doğrulaması yapan kullanıcıya çözümlenen bir Azure AD kullanıcı hesabıyla veya bir Azure AD grup hesabıyla eşlenmiş bir Azure AD oturum açma bilgileri varsa, bu Azure AD oturum açma bilgilerini tüm izinler uygulanır.

### <a name="service-key-and-service-master-key"></a>Hizmet anahtarı ve hizmet ana anahtarı

- [Ana anahtar yedeklemesi](/sql/t-sql/statements/backup-master-key-transact-sql) DESTEKLENMIYOR (SQL veritabanı hizmeti tarafından yönetiliyor).
- [Ana anahtar geri yükleme](/sql/t-sql/statements/restore-master-key-transact-sql) desteklenmez (SQL veritabanı hizmeti tarafından yönetiliyor).
- [Hizmet ana anahtarı yedeklemesi](/sql/t-sql/statements/backup-service-master-key-transact-sql) DESTEKLENMIYOR (SQL veritabanı hizmeti tarafından yönetiliyor).
- [Hizmet ana anahtarı geri yükleme](/sql/t-sql/statements/restore-service-master-key-transact-sql) desteklenmez (SQL veritabanı hizmeti tarafından yönetiliyor).

## <a name="configuration"></a>Yapılandırma

### <a name="buffer-pool-extension"></a>Arabellek havuzu uzantısı

- [Arabellek havuzu uzantısı](/sql/database-engine/configure-windows/buffer-pool-extension) desteklenmiyor.
- `ALTER SERVER CONFIGURATION SET BUFFER POOL EXTENSION`desteklenmez. Bkz. [Alter Server CONFIGURATION](/sql/t-sql/statements/alter-server-configuration-transact-sql).

### <a name="collation"></a>Harmanlama

Varsayılan örnek harmanlaması, `SQL_Latin1_General_CP1_CI_AS` ve oluşturma parametresi olarak belirtilebilir. Bkz. [harmanlamalar](/sql/t-sql/statements/collations).

### <a name="compatibility-levels"></a>Uyumluluk düzeyleri

- Desteklenen uyumluluk düzeyleri 100, 110, 120, 130, 140 ve 150.
- 100 altındaki uyumluluk düzeyleri desteklenmez.
- Yeni veritabanları için varsayılan uyumluluk düzeyi 140 ' dir. Geri yüklenen veritabanları için, uyumluluk düzeyi 100 ve üzeri olursa değişmeden kalır.

Bkz. [ALTER DATABASE Compatibility Level](/sql/t-sql/statements/alter-database-transact-sql-compatibility-level).

### <a name="database-mirroring"></a>Veritabanı yansıtma

Veritabanı yansıtma desteklenmez.

- `ALTER DATABASE SET PARTNER`ve `SET WITNESS` seçenekleri desteklenmez.
- `CREATE ENDPOINT … FOR DATABASE_MIRRORING`desteklenmez.

Daha fazla bilgi için bkz. [alter database set Partner, TANıK ayarla](/sql/t-sql/statements/alter-database-transact-sql-database-mirroring) ve [uç nokta oluştur... DATABASE_MIRRORING IÇIN](/sql/t-sql/statements/create-endpoint-transact-sql).

### <a name="database-options"></a>Veritabanı seçenekleri

- Birden çok günlük dosyası desteklenmez.
- Bellek içi nesneler Genel Amaçlı hizmeti katmanında desteklenmez. 
- Her veritabanı için en fazla 280 dosya Genel Amaçlı örnek başına 280 dosya sınırı vardır. Genel Amaçlı katmanındaki veriler ve günlük dosyaları bu sınıra doğru sayılır. [İş açısından kritik katmanı veritabanı başına 32.767 dosyasını destekler](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-resource-limits#service-tier-characteristics).
- Veritabanı FILESTREAM verileri içeren dosya gruplarını içeremez. . Bak veri içeriyorsa `FILESTREAM` geri yükleme başarısız olur. 
- Her dosya Azure Blob depolama alanına yerleştirilir. Dosya başına GÇ ve aktarım hızı her bir dosyanın boyutuna bağlıdır.

#### <a name="create-database-statement"></a>CREATE DATABASE ekstresi

Aşağıdaki sınırlamalar şunlar için `CREATE DATABASE`geçerlidir:

- Dosyalar ve dosya grupları tanımlanamıyor. 
- `CONTAINMENT` Seçenek desteklenmiyor. 
- `WITH`Seçenekler desteklenmez. 
   > [!TIP]
   > Geçici bir çözüm olarak, `ALTER DATABASE` dosya `CREATE DATABASE` eklemek veya kapsama ayarlamak için veritabanı seçeneklerini ayarlamak üzere sonra ' yi kullanın. 

- `FOR ATTACH` Seçenek desteklenmiyor.
- `AS SNAPSHOT OF` Seçenek desteklenmiyor.

Daha fazla bilgi için bkz. [veritabanı oluşturma](/sql/t-sql/statements/create-database-sql-server-transact-sql).

#### <a name="alter-database-statement"></a>ALTER DATABASE ifadesi

Bazı dosya özellikleri ayarlanamaz veya değiştirilemez:

- `ALTER DATABASE ADD FILE (FILENAME='path')` T-SQL ifadesinde bir dosya yolu belirtilemez. Yönetilen `FILENAME` bir örnek dosyaları otomatik olarak yerleştirtiğinden, betikten kaldır. 
- Bir dosya adı, `ALTER DATABASE` ifadesini kullanarak değiştirilemez.

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

Daha fazla bilgi için bkz. [alter database](/sql/t-sql/statements/alter-database-transact-sql-file-and-filegroup-options).

### <a name="sql-server-agent"></a>SQL Server Agent

- SQL Server Agent etkinleştirme ve devre dışı bırakma Şu anda yönetilen örnekte desteklenmiyor. SQL Aracısı her zaman çalışır.
- SQL Server Agent ayarları salt okunurdur. Yordam `sp_set_agent_properties` , yönetilen örnekte desteklenmez. 
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
- E-posta bildirimi desteklenir, ancak bir Veritabanı Postası profili yapılandırmanızı gerektirir. SQL Server Agent yalnızca bir Veritabanı Postası profili kullanabilir ve çağrılması `AzureManagedInstance_dbmail_profile`gerekir. 
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

SQL Server Agent hakkında bilgi için bkz. [SQL Server Agent](/sql/ssms/agent/sql-server-agent).

### <a name="tables"></a>Tablolar

Aşağıdaki tablo türleri desteklenmez:

- [AKıŞı](/sql/relational-databases/blob/filestream-sql-server)
- [FILETABLE](/sql/relational-databases/blob/filetables-sql-server)
- [Dış tablo](/sql/t-sql/statements/create-external-table-transact-sql) (PolyBase)
- [MEMORY_OPTIMIZED](/sql/relational-databases/in-memory-oltp/introduction-to-memory-optimized-tables) (yalnızca genel amaçlı katmanında desteklenmez)

Tablo oluşturma ve değiştirme hakkında daha fazla bilgi için bkz. [Create Table](/sql/t-sql/statements/create-table-transact-sql) ve [alter table](/sql/t-sql/statements/alter-table-transact-sql).

## <a name="functionalities"></a>İşlevler

### <a name="bulk-insert--openrowset"></a>Toplu ekleme/OPENROWSET

Yönetilen bir örnek dosya paylaşımlarına ve Windows klasörlerine erişemez, bu nedenle dosyalar Azure Blob depolamadan içeri aktarılmalıdır:

- `DATASOURCE`, `BULK INSERT` Azure Blob depolamadan dosyaları içeri aktarırken komutunda gereklidir. Bkz. [bulk INSERT](/sql/t-sql/statements/bulk-insert-transact-sql).
- `DATASOURCE`, Azure Blob depolama `OPENROWSET` alanındaki bir dosyanın içeriğini okurken işlevinde gereklidir. Bkz. [OPENROWSET](/sql/t-sql/functions/openrowset-transact-sql).
- `OPENROWSET`diğer Azure SQL tek veritabanlarından, yönetilen örneklerden veya SQL Server örneklerinden veri okumak için kullanılabilir. Oracle veritabanları veya Excel dosyaları gibi diğer kaynaklar desteklenmez.

### <a name="clr"></a>CLR

Yönetilen bir örnek dosya paylaşımlarına ve Windows klasörlerine erişemez, bu nedenle aşağıdaki kısıtlamalar geçerlidir:

- Yalnızca `CREATE ASSEMBLY FROM BINARY` desteklenir. Bkz. [Assem 'dan](/sql/t-sql/statements/create-assembly-transact-sql)bir şekilde oluşturma. 
- `CREATE ASSEMBLY FROM FILE`desteklenmez. Bkz. [dosyadan derleme oluşturma](/sql/t-sql/statements/create-assembly-transact-sql).
- `ALTER ASSEMBLY`dosyalara başvurulamıyor. Bkz. [alter assembly](/sql/t-sql/statements/alter-assembly-transact-sql).

### <a name="database-mail-db_mail"></a>Veritabanı Postası (db_mail)
 - `sp_send_dbmail`parametre kullanılarak @file_attachments ekler gönderilemez. Yerel dosya sistemi ve dış paylaşımlar veya Azure Blob depolamaya bu yordamdan erişilemez.
 - `@query` Parametreli ve kimlik doğrulamasıyla ilgili bilinen sorunlara bakın.
 
### <a name="dbcc"></a>DBCC

SQL Server ' de etkin olan belgelenmemiş DBCC deyimleri yönetilen örneklerde desteklenmez.

- Yalnızca sınırlı sayıda genel Izleme bayrağı desteklenir. Oturum düzeyi `Trace flags` desteklenmiyor. Bkz. [izleme bayrakları](/sql/t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql).
- [DBCC TRACEOFF](/sql/t-sql/database-console-commands/dbcc-traceoff-transact-sql) ve [DBCC TRACEON](/sql/t-sql/database-console-commands/dbcc-traceon-transact-sql) , sınırlı sayıda Global Trace-Flags ile çalışır.
- Veritabanı `SINGLE_USER` modda ayarlanamadığı için REPAIR_ALLOW_DATA_LOSS, REPAIR_FAST ve REPAIR_REBUILD SEÇENEKLERIYLE [DBCC CHECKDB](/sql/t-sql/database-console-commands/dbcc-checkdb-transact-sql) kullanılamıyor-bkz. [alter database farkları](#alter-database-statement). Olası veritabanı bozuklukları Azure destek ekibi tarafından işlenir. Yaşıyorsanız veritabanında düzeltilmesi gereken bir veritabanı bozulması varsa Azure desteği 'ne başvurun.

### <a name="distributed-transactions"></a>Dağıtılmış işlemler

Şu anda yönetilen örneklerde MSDTC ve [elastik işlemler](sql-database-elastic-transactions-overview.md) desteklenmez.

### <a name="extended-events"></a>Genişletilmiş Olaylar

Genişletilmiş olaylar (XEvents) için Windows 'a özgü bazı hedefler desteklenmez:

- `etw_classic_sync` Hedef desteklenmiyor. Azure `.xel` blob depolamada dosyaları depolayın. [Etw_classic_sync hedefi](/sql/relational-databases/extended-events/targets-for-extended-events-in-sql-server#etw_classic_sync_target-target)' ne bakın.
- `event_file` Hedef desteklenmiyor. Azure `.xel` blob depolamada dosyaları depolayın. [Event_file hedefi](/sql/relational-databases/extended-events/targets-for-extended-events-in-sql-server#event_file-target)' ne bakın.

### <a name="external-libraries"></a>Dış kitaplıklar

Veritabanı içi R ve Python, dış kitaplıklar henüz desteklenmiyor. Bkz. [SQL Server Machine Learning Services](/sql/advanced-analytics/r/sql-server-r-services).

### <a name="filestream-and-filetable"></a>FILESTREAM ve FileTable

- FILESTREAM verileri desteklenmez.
- Veritabanı, verileri içeren `FILESTREAM` dosya grupları içeremez.
- `FILETABLE`desteklenmez.
- Tablolarda tür olamaz `FILESTREAM` .
- Aşağıdaki işlevler desteklenmez:
  - `GetPathLocator()`
  - `GET_FILESTREAM_TRANSACTION_CONTEXT()`
  - `PathName()`
  - `GetFileNamespacePat)`
  - `FileTableRootPath()`

Daha fazla bilgi için bkz. [FILESTREAM](/sql/relational-databases/blob/filestream-sql-server) ve [FileTables](/sql/relational-databases/blob/filetables-sql-server).

### <a name="full-text-semantic-search"></a>Tam metin anlam arama

[Anlamsal arama](/sql/relational-databases/search/semantic-search-sql-server) desteklenmiyor.

### <a name="linked-servers"></a>Bağlı sunucular

Yönetilen örneklerdeki bağlı sunucular sınırlı sayıda hedef destekler:

- Desteklenen hedefler yönetilen örnekler, tek veritabanları ve SQL Server örneklerdir. 
- Bağlı sunucular dağıtılmış yazılabilir işlemleri (MS DTC) desteklemez.
- Desteklenmeyen hedefler dosya, Analysis Services ve diğer RDBMS ' dir. Dosya içeri aktarma için alternatif olarak veya `BULK INSERT` `OPENROWSET` kullanarak Azure Blob depolama 'dan yerel CSV içeri aktarmayı kullanmayı deneyin.

İşlemler

- Çapraz örnek yazma işlemleri desteklenmez.
- `sp_dropserver`, bağlı bir sunucunun atılması için desteklenir. Bkz. [sp_dropserver](/sql/relational-databases/system-stored-procedures/sp-dropserver-transact-sql).
- `OPENROWSET` İşlevi yalnızca SQL Server örneklerinde sorgu yürütmek için kullanılabilir. Bunlar yönetilen, şirket içi veya sanal makinelerde olabilir. Bkz. [OPENROWSET](/sql/t-sql/functions/openrowset-transact-sql).
- `OPENDATASOURCE` İşlevi yalnızca SQL Server örneklerinde sorgu yürütmek için kullanılabilir. Bunlar yönetilen, şirket içi veya sanal makinelerde olabilir. Yalnızca `SQLNCLI`, `SQLNCLI11`ve `SQLOLEDB` değerleri sağlayıcı olarak desteklenir. `SELECT * FROM OPENDATASOURCE('SQLNCLI', '...').AdventureWorks2012.HumanResources.Employee` bunun bir örneğidir. Bkz. [opendatasource](/sql/t-sql/functions/opendatasource-transact-sql).
- Bağlı sunucular ağ paylaşımlarından dosyaları (Excel, CSV) okumak için kullanılamaz. Azure Blob depolamadan CSV dosyalarını okuyan [bulk INSERT](/sql/t-sql/statements/bulk-insert-transact-sql#e-importing-data-from-a-csv-file) veya [OPENROWSET](/sql/t-sql/functions/openrowset-transact-sql#g-accessing-data-from-a-csv-file-with-a-format-file) kullanmayı deneyin. Bu istekleri [yönetilen örnek geri bildirim öğesinde](https://feedback.azure.com/forums/915676-sql-managed-instance/suggestions/35657887-linked-server-to-non-sql-sources) izle|

### <a name="polybase"></a>PolyBase

, Bir veya Azure Blob depolama alanındaki dosyalara başvuran dış tablolar desteklenmez. PolyBase hakkında daha fazla bilgi için bkz. [PolyBase](/sql/relational-databases/polybase/polybase-guide).

### <a name="replication"></a>Çoğaltma

- Anlık görüntü ve Iki yönlü çoğaltma türleri desteklenir. Birleştirme çoğaltması, eşler arası çoğaltma ve güncelleştirilebilir abonelikler desteklenmez.
- [Işlem çoğaltma](sql-database-managed-instance-transactional-replication.md) , yönetilen örnek üzerinde bazı kısıtlamalarla genel önizleme için kullanılabilir:
    - Tüm çoğaltma katılımcıları türleri (yayımcı, dağıtıcı, çekme abonesi ve anında Iletme abonesi) yönetilen örneklere yerleştirilebilir, ancak yayımcı ve dağıtıcı hem bulutta hem de şirket içinde olmalıdır.
    - Yönetilen örnekler SQL Server son sürümleriyle iletişim kurabilir. Daha fazla bilgi için [Desteklenen sürümler matrisine](sql-database-managed-instance-transactional-replication.md#supportability-matrix-for-instance-databases-and-on-premises-systems) bakın.
    - İşlemsel çoğaltma bazı [ek ağ gereksinimlerine](sql-database-managed-instance-transactional-replication.md#requirements)sahiptir.

İşlem çoğaltmasını yapılandırma hakkında daha fazla bilgi için aşağıdaki öğreticilere bakın:
- [Mı yayımcısı ile abone arasında çoğaltma](replication-with-sql-database-managed-instance.md)
- [Mı yayımcısı, mı dağıtıcısı ve SQL Server abonesi arasında çoğaltma](sql-database-managed-instance-configure-replication-tutorial.md)

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
- Kaynak: 
  - `FROM URL`(Azure Blob depolama) desteklenen tek seçenektir.
  - `FROM DISK`/`TAPE`/Backup cihazı desteklenmiyor.
  - Yedekleme kümeleri desteklenmez.
- `WITH`Seçenekler, `DIFFERENTIAL` veya `STATS`gibi desteklenmez.
- `ASYNC RESTORE`: Geri yükleme, istemci bağlantısı kesilse bile devam eder. Bağlantınız atıldıysanız, geri yükleme işleminin durumunun `sys.dm_operation_status` görünümünü ve bir oluşturma ve bırakma veritabanı için görünümü kontrol edebilirsiniz. Bkz. [sys. dm_operation_status](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database). 

Aşağıdaki veritabanı seçenekleri ayarlanır veya geçersiz kılınır ve daha sonra değiştirilemez: 

- `NEW_BROKER`Aracı. bak dosyasında etkinleştirilmemişse. 
- `ENABLE_BROKER`Aracı. bak dosyasında etkinleştirilmemişse. 
- `AUTO_CLOSE=OFF`. bak dosyasındaki bir veritabanı varsa `AUTO_CLOSE=ON`. 
- `RECOVERY FULL`. bak dosyasındaki bir veritabanının `SIMPLE` veya `BULK_LOGGED` kurtarma modu varsa.
- Bellek için iyileştirilmiş bir dosya grubu eklenir ve kaynak. bak dosyasında yoksa XTP çağırılır. 
- Bellek için iyileştirilmiş mevcut dosya grubu XTP olarak yeniden adlandırılır. 
- `SINGLE_USER`ve `RESTRICTED_USER` seçenekleri öğesine `MULTI_USER`dönüştürülür.

Sınırlamalar: 

- Bozulan veritabanlarının yedeklemeleri, bozulmanın türüne bağlı olarak geri yüklenebilir, ancak bozulma düzeltilinceye kadar otomatik yedeklemeler alınmaz. Bu sorunu engellemek için kaynak `DBCC CHECKDB` örneği üzerinde çalıştırdığınızdan ve yedeklemeyi `WITH CHECKSUM` kullandığınızdan emin olun.
- Bu belgede `.BAK` (örneğin, `FILESTREAM` veya `FILETABLE` nesneler) açıklanan herhangi bir kısıtlamayı Içeren bir veritabanının dosyasının geri yüklenmesi yönetilen örnek üzerinde geri yüklenemez.
- `.BAK`birden çok yedekleme kümesi içeren dosyalar geri yüklenemez. 
- `.BAK`birden çok günlük dosyası içeren dosyalar geri yüklenemez.
- 8 TB 'den büyük veritabanları, etkin bellek içi OLTP nesneleri veya örnek başına 280 dosya aşılacak dosya sayısı bir Genel Amaçlı örneğine geri yüklenemez. 
- 4 TB 'den büyük veya bellek içi OLTP nesnelerinden daha büyük olan veritabanları içeren yedeklemeler, İş Açısından Kritik örneğine [geri yüklenemez.](sql-database-managed-instance-resource-limits.md)
Restore deyimleri hakkında daha fazla bilgi için bkz. [restore deyimleri](/sql/t-sql/statements/restore-statements-transact-sql).

 > [!IMPORTANT]
 > Aynı sınırlamalar, yerleşik bir noktadan sonra geri yükleme işlemi için geçerlidir. Örnek olarak, 4 TB 'den büyük Genel Amaçlı veritabanı İş Açısından Kritik örneğine geri yüklenemez. Bellek içi OLTP dosyaları veya 280 'den fazla dosya içeren İş Açısından Kritik veritabanı Genel Amaçlı örneğine geri yüklenemez.

### <a name="service-broker"></a>Hizmet Aracısı

Çapraz örnek hizmet Aracısı desteklenmez:

- `sys.routes`: Bir önkoşul olarak sys. Routes adresini seçmeniz gerekir. Adresin her rotada yerel olması gerekir. Bkz. [sys. Routes](/sql/relational-databases/system-catalog-views/sys-routes-transact-sql).
- `CREATE ROUTE`: İle `CREATE ROUTE` `ADDRESS` kullanamazsınız `LOCAL`. Bkz. [yol oluşturma](/sql/t-sql/statements/create-route-transact-sql).
- `ALTER ROUTE`: İle `ALTER ROUTE` `ADDRESS` kullanamazsınız `LOCAL`. Bkz. [yol değiştirme](/sql/t-sql/statements/alter-route-transact-sql). 

### <a name="stored-procedures-functions-and-triggers"></a>Saklı yordamlar, işlevler ve Tetikleyiciler

- `NATIVE_COMPILATION`Genel Amaçlı katmanında desteklenmez.
- Aşağıdaki [sp_configure](/sql/relational-databases/system-stored-procedures/sp-configure-transact-sql) seçenekleri desteklenmez: 
  - `allow polybase export`
  - `allow updates`
  - `filestream_access_level`
  - `remote access`
  - `remote data archive`
  - `remote proc trans`
- `sp_execute_external_scripts`desteklenmez. Bkz. [sp_execute_external_scripts](/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql#examples).
- `xp_cmdshell`desteklenmez. Bkz. [xp_cmdshell](/sql/relational-databases/system-stored-procedures/xp-cmdshell-transact-sql).
- `Extended stored procedures`, ve `sp_addextendedproc`   `sp_dropextendedproc`içeren desteklenmez. Bkz. [genişletilmiş saklı yordamlar](/sql/relational-databases/system-stored-procedures/general-extended-stored-procedures-transact-sql).
- `sp_attach_db`, `sp_attach_single_file_db`ve `sp_detach_db` desteklenmez. Bkz. [sp_attach_db](/sql/relational-databases/system-stored-procedures/sp-attach-db-transact-sql), [sp_attach_single_file_db](/sql/relational-databases/system-stored-procedures/sp-attach-single-file-db-transact-sql)ve [sp_detach_db](/sql/relational-databases/system-stored-procedures/sp-detach-db-transact-sql).

### <a name="system-functions-and-variables"></a>Sistem işlevleri ve değişkenler

Aşağıdaki değişkenler, işlevler ve görünümler farklı sonuçlar döndürüyor:

- `SERVERPROPERTY('EngineEdition')`8 değerini döndürür. Bu özellik yönetilen bir örneği benzersiz bir şekilde tanımlar. Bkz. [ServerProperty](/sql/t-sql/functions/serverproperty-transact-sql).
- `SERVERPROPERTY('InstanceName')`SQL Server için var olduğu gibi örnek kavramı yönetilen bir örneğe uygulanamadığından NULL değerini döndürür. Bkz. [ServerProperty (' ÖrnekAdı ')](/sql/t-sql/functions/serverproperty-transact-sql).
- `@@SERVERNAME`tam DNS "bağlanılabilir" adı döndürür, örneğin, my-managed-instance.wcus17662feb9ce98.database.windows.net. Bkz [.@SERVERNAME@](/sql/t-sql/functions/servername-transact-sql). 
- `SYS.SERVERS`"ad" ve "data_source" özellikleri gibi bir tam `myinstance.domain.database.windows.net` DNS "bağlanılabilir" adı döndürür. Bkz [. sys. Sunucu](/sql/relational-databases/system-catalog-views/sys-servers-transact-sql).
- `@@SERVICENAME`SQL Server için mevcut hizmet kavramı yönetilen bir örneğe uygulanamadığından NULL değerini döndürür. Bkz [.@SERVICENAME@](/sql/t-sql/functions/servicename-transact-sql).
- `SUSER_ID`desteklenir. Azure AD oturum açma, sys. syslogins içinde değilse NULL değerini döndürür. Bkz. [SUSER_ID](/sql/t-sql/functions/suser-id-transact-sql). 
- `SUSER_SID`desteklenmez. Yanlış veriler döndürülür, bu geçici olarak bilinen bir sorundur. Bkz. [SUSER_SID](/sql/t-sql/functions/suser-sid-transact-sql). 

## <a name="environment-constraints"></a><a name="Environment"></a>Ortam kısıtlamaları

### <a name="subnet"></a>Alt ağ
-  Yönetilen örneğinizi dağıttığınız alt ağa başka herhangi bir kaynak (örneğin, sanal makineler) yerleştirebilirsiniz. Bu kaynakları farklı bir alt ağ kullanarak dağıtın.
- Alt ağda yeterli sayıda kullanılabilir [IP adresi](sql-database-managed-instance-connectivity-architecture.md#network-requirements)olmalıdır. En az 16, ancak öneri alt ağda en az 32 IP adresine sahip olur.
- [Hizmet uç noktaları, yönetilen örneğin alt ağıyla ilişkilendirilemez](sql-database-managed-instance-connectivity-architecture.md#network-requirements). Sanal ağı oluştururken hizmet uç noktaları seçeneğinin devre dışı olduğundan emin olun.
- Bir bölgede dağıtabileceğiniz sanal çekirdek sayısı ve örnek türleri bazı [kısıtlamalar ve sınırlara](sql-database-managed-instance-resource-limits.md#regional-resource-limitations)sahiptir.
- [Alt ağda uygulanması gereken bazı güvenlik kuralları](sql-database-managed-instance-connectivity-architecture.md#network-requirements)vardır.

### <a name="vnet"></a>ADLı
- VNet, kaynak modeli kullanılarak dağıtılabilir-sanal ağ için klasik model desteklenmez.
- Yönetilen bir örnek oluşturulduktan sonra, yönetilen örneği veya VNet 'i başka bir kaynak grubuna veya aboneliğe taşımak desteklenmez.
- App Service ortamları, mantıksal uygulamalar ve yönetilen örnekler (coğrafi çoğaltma, Işlemsel çoğaltma veya bağlı sunucular aracılığıyla kullanılan) gibi bazı hizmetler, sanal ağları [genel eşleme](../virtual-network/virtual-networks-faq.md#what-are-the-constraints-related-to-global-vnet-peering-and-load-balancers)kullanılarak bağlanmışsa farklı bölgelerdeki yönetilen örneklere erişemez. Sanal ağ geçitleri aracılığıyla ExpressRoute veya VNet-VNet aracılığıyla bu kaynaklara bağlanabilirsiniz.

### <a name="tempdb"></a>'Nın

Genel Amaçlı katmanındaki en büyük dosya `tempdb` boyutu, çekirdek BAŞıNA 24 GB 'den büyük olamaz. İş Açısından Kritik katmanındaki `tempdb` en büyük boyut, örnek depolama boyutuyla sınırlıdır. `Tempdb`günlük dosyası boyutu Genel Amaçlı katmanında 120 GB ile sınırlıdır. Bazı sorgular, üzerinde `tempdb` çekirdek BAŞıNA 24 GB 'den fazla gereksinim duyduklarında veya 120 GB 'den fazla günlük verisi ürettiklerinde bir hata döndürebilir.

### <a name="msdb"></a>MSDB

Yönetilen örnekteki aşağıdaki MSDB şemaları, kendi önceden tanımlanmış rollerine ait olmalıdır:

- Genel roller
  - TargetServersRole
- [Düzeltilen veritabanı rolleri](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent-fixed-database-roles?view=sql-server-ver15)
  - SQLAgentUserRole
  - SQLAgentReaderRole
  - SQLAgentOperatorRole
- [DatabaseMail rolleri](https://docs.microsoft.com/sql/relational-databases/database-mail/database-mail-configuration-objects?view=sql-server-ver15#DBProfile):
  - DatabaseMailUserRole
- [Tümleştirme Hizmetleri rolleri](https://docs.microsoft.com/sql/integration-services/security/integration-services-roles-ssis-service?view=sql-server-ver15):
  - db_ssisadmin
  - db_ssisltduser
  - db_ssisoperator
  
> [!IMPORTANT]
> Önceden tanımlanmış rol adlarını, şema adlarını ve şema sahiplerini müşterilere göre değiştirmek, hizmetin normal işlemlerini etkiler. Bunlar üzerinde yapılan tüm değişiklikler, algılanan en kısa sürede önceden tanımlanmış değerlere geri döndürülecek veya normal hizmet işlemini sağlamak için en son bir sonraki hizmet güncelleştirmesinde olacaktır.

### <a name="error-logs"></a>Hata günlükleri

Yönetilen bir örnek, hata günlüklerinde ayrıntılı bilgileri koyar. Hata günlüğünde günlüğe kaydedilen çok sayıda iç sistem olayı vardır. İlgisiz bazı girdilerin filtrelediğini belirten hata günlüklerini okumak için özel bir yordam kullanın. Daha fazla bilgi için bkz. Azure Data Studio için [yönetilen örnek – sp_readmierrorlog](https://blogs.msdn.microsoft.com/sqlcat/2018/05/04/azure-sql-db-managed-instance-sp_readmierrorlog/) veya [yönetilen örnek uzantısı (Önizleme)](/sql/azure-data-studio/azure-sql-managed-instance-extension#logs) .

## <a name="next-steps"></a>Sonraki adımlar

- Yönetilen örnekler hakkında daha fazla bilgi için bkz. [yönetilen örnek nedir?](sql-database-managed-instance.md)
- Özellikler ve karşılaştırma listesi için bkz. [Azure SQL veritabanı özelliği karşılaştırması](sql-database-features.md).
- Yayın güncelleştirmeleri ve bilinen sorunlar durumu için bkz. [SQL veritabanı sürüm notları](sql-database-release-notes.md)
- Yeni bir yönetilen örnek oluşturmayı gösteren bir hızlı başlangıç için bkz. [yönetilen örnek oluşturma](sql-database-managed-instance-get-started.md).
