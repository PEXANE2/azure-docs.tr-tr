---
title: Yönetilen örnek T-SQL farkları
description: Bu makalede, Azure SQL Veritabanı ve SQL Server'da yönetilen bir örnek arasındaki T-SQL farkları anlatılmaktadır
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
ms.sourcegitcommit: 07d62796de0d1f9c0fa14bfcc425f852fdb08fb1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80365493"
---
# <a name="managed-instance-t-sql-differences-and-limitations"></a>Yönetilen örnek T-SQL farklılıkları ve sınırlamaları

Bu makalede, sözdizimi ve davranış arasındaki farklar Azure SQL Veritabanı yönetilen örnek ve şirket içi SQL Server Veritabanı Altyapısı arasında açıklar. Yönetilen örnek dağıtım seçeneği, şirket içi SQL Server Database Engine ile yüksek uyumluluk sağlar. SQL Server veritabanı altyapısı özelliklerinin çoğu yönetilen bir örnekte desteklenir.

![Geçiş](./media/sql-database-managed-instance/migration.png)

Yönetilen Örnek'te tanıtılan bazı PaaS sınırlamaları ve SQL Server ile karşılaştırıldığında bazı davranış değişiklikleri vardır. Farklar aşağıdaki kategorilere ayrılır:<a name="Differences"></a>

- [Kullanılabilirlik,](#availability) Her [Zaman Kullanılabilirlik Grupları](#always-on-availability-groups) ve [yedeklerarasındaki](#backup)farklılıkları içerir.
- [Güvenlik](#security) [denetim](#auditing)farklılıkları içerir, [sertifikalar,](#certificates) [kimlik bilgileri,](#credential) [şifreleme sağlayıcıları,](#cryptographic-providers) [giriş ve kullanıcılar,](#logins-and-users)ve [hizmet anahtarı ve hizmet ana anahtarı](#service-key-and-service-master-key).
- [Yapılandırma](#configuration) [arabellek havuzu uzantısı,](#buffer-pool-extension) [harmanlama,](#collation) [uyumluluk düzeyleri,](#compatibility-levels) [veritabanı yansıtma,](#database-mirroring) [veritabanı seçenekleri,](#database-options) [SQL Server Agent](#sql-server-agent)ve [tablo seçenekleri](#tables)farklılıkları içerir.
- [İşlevler](#functionalities) [toplu insert / OPENROWSET](#bulk-insert--openrowset), [CLR](#clr), [DBCC](#dbcc), [dağıtılmış işlemler](#distributed-transactions), [genişletilmiş olaylar](#extended-events), dış [kütüphaneler](#external-libraries), [filestream ve FileTable](#filestream-and-filetable), [tam metin Semantik Arama](#full-text-semantic-search), bağlantılı [sunucular](#linked-servers), [PolyBase](#polybase), [Çoğaltma](#replication), [RESTORE](#restore-statement), [Servis Broker](#service-broker), [saklanan yordamlar, fonksiyonlar ve tetikleyiciler](#stored-procedures-functions-and-triggers)içerir.
- VNet ve alt ağ yapılandırmaları gibi [ortam ayarları.](#Environment)

Bu özelliklerin çoğu mimari kısıtlamalardır ve hizmet özelliklerini temsil eder.

Yönetilen örnekte keşfedilen ve gelecekte çözülecek geçici bilinen sorunlar sürüm [notları sayfasında](sql-database-release-notes.md)açıklanmıştır.

## <a name="availability"></a>Kullanılabilirlik

### <a name="always-on-availability-groups"></a><a name="always-on-availability-groups"></a>Her Zaman Kullanılabilirlik Gruplarında

[Yüksek kullanılabilirlik](sql-database-high-availability.md) yönetilen örnekte yerleşiktir ve kullanıcılar tarafından denetlenir. Aşağıdaki ifadeler desteklenmez:

- [BITIŞ Noktası OLUŞTURUN ... DATABASE_MIRRORING İçİn](/sql/t-sql/statements/create-endpoint-transact-sql)
- [KULLANıLABILIRLIK GRUBU OLUŞTUR](/sql/t-sql/statements/create-availability-group-transact-sql)
- [KULLANıLABILIRLIK GRUBUNU DEĞIŞTIR](/sql/t-sql/statements/alter-availability-group-transact-sql)
- [DROP KULLANıLABILIRLIK GRUBU](/sql/t-sql/statements/drop-availability-group-transact-sql)
- [ALTER DATABASE](/sql/t-sql/statements/alter-database-transact-sql) deyiminin [SET HADR](/sql/t-sql/statements/alter-database-transact-sql-set-hadr) maddesi

### <a name="backup"></a>Backup

Yönetilen örneklerde otomatik yedeklemeler vardır, böylece `COPY_ONLY` kullanıcılar tam veritabanı yedeklemeleri oluşturabilir. Diferansiyel, günlük ve dosya anlık görüntü yedekleri desteklenmez.

- Yönetilen bir örnekle, örnek veritabanını yalnızca bir Azure Blob depolama hesabına yedekleyebilirsiniz:
  - Yalnızca `BACKUP TO URL` desteklenir.
  - `FILE`, `TAPE`ve yedekleme aygıtları desteklenmez.
- Genel `WITH` seçeneklerin çoğu desteklenir.
  - `COPY_ONLY`zorunludur.
  - `FILE_SNAPSHOT`desteklenmez.
  - Teyp `REWIND` `NOREWIND`seçenekleri: , , `UNLOAD`, ve `NOUNLOAD` desteklenmez.
  - Günlüğe özgü `NORECOVERY`seçenekler: , , `STANDBY`ve `NO_TRUNCATE` desteklenmez.

Sınırlamalar: 

- Yönetilen bir örnekle, bir örnek veritabanını en fazla 32 çizgili bir yedeklemeye yedekolarak yedekleyebilirsiniz ve bu da yedekleme sıkıştırma kullanıldığında 4 TB'a kadar veritabanları için yeterlidir.
- Hizmet tarafından yönetilen `BACKUP DATABASE ... WITH COPY_ONLY` Saydam Veri Şifreleme (TDE) ile şifrelenmiş bir veritabanında yürütemezsiniz. Servis tarafından yönetilen TDE, yedeklemeleri dahili bir TDE anahtarıyla şifrelenecek şekilde zorlar. Anahtar dışa aktarılamaz, bu nedenle yedeklemeyi geri yükleyemezsiniz. Otomatik yedeklemeleri ve zamanında geri yüklemeyi kullanın veya bunun yerine [müşteri tarafından yönetilen (BYOK) TDE'yi](transparent-data-encryption-azure-sql.md#customer-managed-transparent-data-encryption---bring-your-own-key) kullanın. Veritabanındaki şifrelemeyi de devre dışı bekleyebilirsiniz.
- Yönetilen bir örnekte komutu `BACKUP` kullanarak maksimum yedek şerit boyutu 195 GB'dır, bu da maksimum blob boyutudur. Tek tek şerit boyutunu azaltmak ve bu sınır içinde kalmak için yedekleme komutundaki şerit sayısını artırın.

    > [!TIP]
    > Bu sınırlamayı aşmak için, sql server'dan bir veritabanını şirket içi ortamda veya sanal bir makinede yedeklediğinizde şunları yapabilirsiniz:
    >
    > - Yedekleme `DISK` yerine yedeklemeye `URL`geri dön.
    > - Yedekleme dosyalarını Blob depolama alanına yükleyin.
    > - Yönetilen örneğin içine geri yükleyin.
    >
    > Yönetilen `Restore` bir örnekteki komut, yüklenen yedekleme dosyalarının depolanması için farklı bir blob türü kullanıldığından yedekleme dosyalarındaki daha büyük blob boyutlarını destekler.

T-SQL kullanan yedeklemeler hakkında daha fazla bilgi için [BACKUP'a](/sql/t-sql/statements/backup-transact-sql)bakın.

## <a name="security"></a>Güvenlik

### <a name="auditing"></a>Denetim

Azure SQL Veritabanı'ndaki veritabanlarında denetim ile SQL Server'daki veritabanları arasındaki temel farklar şunlardır:

- Azure SQL Veritabanı'nda yönetilen örnek dağıtım seçeneğiyle, denetim sunucu düzeyinde çalışır. `.xel` Günlük dosyaları Azure Blob depolama alanında depolanır.
- Azure SQL Veritabanı'ndaki tek veritabanı ve esnek havuz dağıtım seçenekleriyle, denetim veritabanı düzeyinde çalışır.
- SQL Server şirket içi veya sanal makinelerde, denetim sunucu düzeyinde çalışır. Olaylar dosya sisteminde veya Windows olay günlüklerinde depolanır.
 
Yönetilen örnekte XEvent denetimi Azure Blob depolama hedeflerini destekler. Dosya ve Windows günlükleri desteklenmez.

Azure Blob depolamasına denetim için sözdiziminde `CREATE AUDIT` önemli farklar şunlardır:

- Dosyaların yerleştirildiği `TO URL` Azure Blob depolama kapsayıcısının URL'sini belirtmek için kullanabileceğiniz yeni bir sözdizimi sağlanır. `.xel`
- Yönetilen bir `TO FILE` örnek Windows dosya paylaşımlarına erişemediğiiçin sözdizimi desteklenmez.

Daha fazla bilgi için bkz. 

- [SUNUCU DENETIMI OLUŞTUR](/sql/t-sql/statements/create-server-audit-transact-sql) 
- [SUNUCU DENETIMINI DEĞIŞTIR](/sql/t-sql/statements/alter-server-audit-transact-sql)
- [Denetim](/sql/relational-databases/security/auditing/sql-server-audit-database-engine)

### <a name="certificates"></a>Sertifikalar

Yönetilen bir örnek dosya paylaşımlarına ve Windows klasörlerine erişemez, bu nedenle aşağıdaki kısıtlamalar geçerlidir:

- `CREATE FROM` / Dosya `BACKUP TO` sertifikalar için desteklenmez.
- `CREATE` / `BACKUP` Sertifika `FILE` / `ASSEMBLY` Özel anahtar dosyaları kullanılamaz. 

Bkz. [CREATE CERTIFICATE](/sql/t-sql/statements/create-certificate-transact-sql) ve [BACKUP CERTIFICATE](/sql/t-sql/statements/backup-certificate-transact-sql). 
 
**Geçici Çözüm**: Sertifika yedeklemesini oluşturmak ve yedeklemeyi geri almak yerine, [sertifika ikili içeriğini ve özel anahtarı nı alın, .sql dosyası olarak saklayın ve ikili den oluşturun:](/sql/t-sql/functions/certencoded-transact-sql#b-copying-a-certificate-to-another-database)

```sql
CREATE CERTIFICATE  
   FROM BINARY = asn_encoded_certificate
WITH PRIVATE KEY (<private_key_options>)
```

### <a name="credential"></a>Kimlik Bilgisi

Yalnızca Azure Anahtar `SHARED ACCESS SIGNATURE` Kasası ve kimlikleri desteklenir. Windows kullanıcıları desteklenmez.

Bkz. [CREATE CRETOAL](/sql/t-sql/statements/create-credential-transact-sql) ve ALTER [CREDENTIAL](/sql/t-sql/statements/alter-credential-transact-sql).

### <a name="cryptographic-providers"></a>Şifreleme sağlayıcıları

Yönetilen bir örnek dosyalara erişemez, bu nedenle şifreleme sağlayıcıları oluşturulamaz:

- `CREATE CRYPTOGRAPHIC PROVIDER`desteklenmez. Bkz. [ŞIFRELEME Sağlayıcı OLUŞTUR.](/sql/t-sql/statements/create-cryptographic-provider-transact-sql)
- `ALTER CRYPTOGRAPHIC PROVIDER`desteklenmez. Bkz. [ALTER CRYPTOGRAPHIC Provider](/sql/t-sql/statements/alter-cryptographic-provider-transact-sql).

### <a name="logins-and-users"></a>Oturum açma bilgileri ve kullanıcılar

- SQL girişleri kullanılarak `FROM CERTIFICATE` `FROM ASYMMETRIC KEY`oluşturulur ve `FROM SID` desteklenir. Bkz. [Gİrİş YAP.](/sql/t-sql/statements/create-login-transact-sql)
- [CREATE LOGIN](/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current) sözdizimi veya [Gİrİş 'TEN CREATE USER [Azure AD Girişi]](/sql/t-sql/statements/create-user-transact-sql?view=azuresqldb-mi-current) sözdizimi ile oluşturulan Azure Active Directory (Azure AD) sunucu ilkeleri (oturum açmalar) desteklenir. Bu oturum açmalar sunucu düzeyinde oluşturulur.

    Yönetilen örnek sözdizimi `CREATE USER [AADUser/AAD group] FROM EXTERNAL PROVIDER`ile Azure AD veritabanı ilkelerini destekler. Bu özellik, Azure AD içerdiği veritabanı kullanıcıları olarak da bilinir.

- `CREATE LOGIN ... FROM WINDOWS` Sözdizimi yle oluşturulan Windows girişleri desteklenmez. Azure Active Directory girişlerini ve kullanıcılarını kullanın.
- Örneği oluşturan Azure AD kullanıcısısınırsız [yönetici ayrıcalıklarına sahiptir.](sql-database-manage-logins.md)
- Yönetici olmayan Azure AD veritabanı düzeyindeki kullanıcılar `CREATE USER ... FROM EXTERNAL PROVIDER` sözdizimi kullanılarak oluşturulabilir. Bkz. [CREATE USER ... DıŞ SAĞLAYıCıDAN](sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities).
- Azure AD sunucu ilkeleri (oturum açmalar) SQL özelliklerini yalnızca yönetilen tek bir örnekte destekler. Aynı Azure AD kiracısı veya farklı kiracılar içinde olsunlar, durumlar arası etkileşim gerektiren özellikler, Azure AD kullanıcıları için desteklenmez. Bu özelliklere örnek olarak şunlar verilebilir:

  - SQL işlem çoğaltma.
  - Bağlantı sunucusu.

- Veritabanı sahibi desteklenmezken, Azure AD oturum açmanın bir Azure REKLAM grubuna eşlenmesini ayarlama.
- Diğer Azure AD ilkelerini kullanarak Azure AD sunucu düzeyindeki ilkelerin kimliğe bürünme,EXECUTE [AS](/sql/t-sql/statements/execute-as-transact-sql) yan tümcesi gibi desteklenir. Execute as sınırlamaları şunlardır:

  - Kullanıcı olarak execute, oturum açma adından farklı olduğunda Azure AD kullanıcıları için desteklenmez. Bir örnek kullanıcı sözdizimi CREATE USER [myAadUser] FROM LOGIN [ ]john@contoso.comve kimliğe bürünme EXEC AS USER = _myAadUser_üzerinden denenir oluşturulur. Bir Azure AD sunucu sorumlusundan (oturum açma) bir **KULLANICI** oluşturduğunuzda, **Gİrİş'ten**aynı login_name olarak user_name belirtin.
  - Yalnızca `sysadmin` rolün bir parçası olan SQL Server düzeyindeki ilkeler (oturum açmalar), Azure AD ilkelerini hedefleyen aşağıdaki işlemleri gerçekleştirebilir:

    - KULLANıCı OLARAK ÇALıŞTıRın
    - Gİrİş OLARAK YÜRÜT

- Bacpac dosyalarını kullanarak veritabanı dışa aktarma/alma, yönetilen örnekte Azure AD kullanıcıları için [SSMS V18.4 veya daha sonra](/sql/ssms/download-sql-server-management-studio-ssms)veya [SQLPackage.exe](/sql/tools/sqlpackage-download)kullanılarak desteklenir.
  - Aşağıdaki yapılandırmalar veritabanı bacpac dosyası kullanılarak desteklenir: 
    - Aynı Azure AD etki alanındaki farklı yönetme örnekleri arasında bir veritabanı dışa aktarma/alma.
    - Yönetilen örnekten bir veritabanı dışa aktarın ve aynı Azure AD etki alanı içinde SQL Veritabanına aktarın. 
    - Veritabanını SQL Veritabanı'ndan dışa aktarın ve aynı Azure AD etki alanı içinde yönetilen örne aktarın.
    - Yönetilen örnekten veritabanını dışa aktarın ve SQL Server'a (sürüm 2012 veya sonraki sürüm) aktarın.
      - Bu yapılandırmada tüm Azure AD kullanıcıları oturum açmadan SQL veritabanı ilkeleri (kullanıcıları) olarak oluşturulur. Kullanıcı türü SQL olarak listelenir (sys.database_principals'da SQL_USER olarak görülebilir). İzinleri ve rolleri SQL Server veritabanı meta verilerinde kalır ve kimliğe bürünme için kullanılabilir. Ancak, kimlik bilgilerini kullanarak SQL Server'a erişmek ve oturum açmak için kullanılamazlar.

- Yalnızca yönetilen örnek sağlama işlemi tarafından oluşturulan sunucu düzeyindeki ana oturum açma işlemi, `securityadmin` sunucu `sysadmin`rolleri gibi sunucu rollerinin üyeleri veya sunucu düzeyinde ALTER ANY LOGIN izni olan diğer oturum açmalar, yönetilen örnek için ana veritabanında Azure AD sunucu ilkeleri (oturum açmalar) oluşturabilir.
- Oturum açma bir SQL sorumlusuysa, yalnızca rolün `sysadmin` bir parçası olan oturum açmalar, bir Azure REKLAM hesabı için oturum açma oluşturmak için oluştur komutunu kullanabilir.
- Azure AD girişi, Azure SQL Veritabanı yönetilen örneği için kullanılan aynı dizin içinde bir Azure AD üyesi olmalıdır.
- Azure AD sunucu ilkeleri (oturum açmalar) OBJECT Explorer'da SQL Server Management Studio 18.0 önizleme 5 ile başlar.
- Azure AD sunucu ilkelerinin (oturum açmalar) azure AD yöneticisi hesabıyla çakışmasına izin verilir. Azure AD sunucu ilkeleri (oturum açmalar), ana parayı çözdüğünüzde ve izinleri yönetilen örne uyguladığınızda Azure AD yöneticisinden önce gelir.
- Kimlik doğrulama sırasında, kimlik doğrulaması asılını çözmek için aşağıdaki sıra uygulanır:

    1. Azure AD hesabı, sys.server_principals'da "E" türü olarak bulunan Azure AD sunucu ilkesine (oturum açma) doğrudan eşlenen olarak varsa, Azure AD sunucu sorumlusunun (oturum açma) izinlerine erişim tanıyın ve izinleri uygulayın.
    2. Azure AD hesabı, sys.server_principals'da "X" türü olarak bulunan Azure AD sunucu ilkesine (oturum açma) eşlenen bir Azure REKLAM grubunun üyesiyse, Azure AD grubu oturum açma izinlerine erişim tanıyın ve izinleri uygulayın.
    3. Azure AD hesabı yönetilen örnek sistem görünümlerinde bulunmayan yönetilen örnek için özel bir portal yapılandırılmış Azure AD yöneticisiyse, yönetilen örnek için Azure AD yöneticisinin özel sabit izinlerini uygulayın (eski mod).
    4. Azure AD hesabı, sys.database_principals'da "E" türü olarak bulunan bir veritabanındaki Azure AD kullanıcısına doğrudan eşlenen bir Azure REKLAM hesabı varsa, Azure AD veritabanı kullanıcısına erişim izni verir ve azure AD veritabanı kullanıcısının izinlerini uygulayın.
    5. Azure AD hesabı, sys.database_principals'da "X" türü olarak bulunan bir veritabanındaki Azure AD kullanıcısına eşlenen bir Azure REKLAM grubunun üyesiyse, Azure AD grubu girişine erişim izni tanıyın ve izinleri uygulayın.
    6. Bir Azure AD kullanıcı hesabına veya bir Azure AD grup hesabına eşlenen ve kimlik doğrulaması yapan kullanıcıya yönelik olarak eşlenen bir Azure AD girişi varsa, bu Azure AD oturum açma izinleri uygulanır.

### <a name="service-key-and-service-master-key"></a>Servis anahtarı ve servis ana anahtarı

- [Ana anahtar yedeklemesi](/sql/t-sql/statements/backup-master-key-transact-sql) desteklenmez (SQL Veritabanı hizmeti tarafından yönetilir).
- [Ana anahtar geri yüklemesi](/sql/t-sql/statements/restore-master-key-transact-sql) desteklenmez (SQL Veritabanı hizmeti tarafından yönetilir).
- [Servis ana anahtar yedeklemesi](/sql/t-sql/statements/backup-service-master-key-transact-sql) desteklenmez (SQL Veritabanı hizmeti tarafından yönetilir).
- [Hizmet ana anahtar geri yüklemesi](/sql/t-sql/statements/restore-service-master-key-transact-sql) desteklenmez (SQL Veritabanı hizmeti tarafından yönetilir).

## <a name="configuration"></a>Yapılandırma

### <a name="buffer-pool-extension"></a>Arabellek havuzu uzantısı

- [Arabellek havuzu uzantısı](/sql/database-engine/configure-windows/buffer-pool-extension) desteklenmez.
- `ALTER SERVER CONFIGURATION SET BUFFER POOL EXTENSION`desteklenmez. Bkz. [ALTER SERVER YAPıLANDıRMAsI](/sql/t-sql/statements/alter-server-configuration-transact-sql).

### <a name="collation"></a>Harmanlama

Varsayılan örnek harmanlama `SQL_Latin1_General_CP1_CI_AS` ve oluşturma parametresi olarak belirtilebilir. [Bkz. Collations](/sql/t-sql/statements/collations).

### <a name="compatibility-levels"></a>Uyumluluk düzeyleri

- Desteklenen uyumluluk düzeyleri 100, 110, 120, 130, 140 ve 150'dir.
- 100'ün altındaki uyumluluk düzeyleri desteklenmez.
- Yeni veritabanları için varsayılan uyumluluk düzeyi 140'tır. Geri yüklenen veritabanları için uyumluluk düzeyi 100 ve üzeri yse değişmeden kalır.

Alter [DATABASE Uyumluluk Düzeyi'ne](/sql/t-sql/statements/alter-database-transact-sql-compatibility-level)bakın.

### <a name="database-mirroring"></a>Veritabanı yansıtma

Veritabanı yansıtma sı desteklenmez.

- `ALTER DATABASE SET PARTNER`ve `SET WITNESS` seçenekler desteklenmez.
- `CREATE ENDPOINT … FOR DATABASE_MIRRORING`desteklenmez.

Daha fazla bilgi için [ALTER DATABASE SET PARTNER ve SET WITNESS](/sql/t-sql/statements/alter-database-transact-sql-database-mirroring) ve CREATE [ENDPOINT 'e bakın... DATABASE_MIRRORING Için.](/sql/t-sql/statements/create-endpoint-transact-sql)

### <a name="database-options"></a>Veritabanı seçenekleri

- Birden çok günlük dosyası desteklenmez.
- Bellek nesneleri Genel Amaç hizmet katmanında desteklenmez. 
- Genel Amaçlı örnek başına 280 dosya sınırı vardır ve bu da veritabanı başına en fazla 280 dosya anlamına gelir. Genel Amaç katmanındaki hem veri hem de günlük dosyaları bu sınıra doğru sayılır. [Business Critical katmanı veritabanı başına 32.767 dosyayı destekler.](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-resource-limits#service-tier-characteristics)
- Veritabanı, dosya akışı verileri içeren dosya grupları içeremez. .bak veri içeriyorsa `FILESTREAM` geri yükleme başarısız olur. 
- Her dosya Azure Blob depolama alanına yerleştirilir. IO ve dosya başına iş başı her dosyanın boyutuna bağlıdır.

#### <a name="create-database-statement"></a>CREATE DATABASE deyimi

Aşağıdaki sınırlamalar `CREATE DATABASE`geçerlidir:

- Dosyalar ve dosya grupları tanımlanamaz. 
- Seçenek `CONTAINMENT` desteklenmiyor. 
- `WITH`seçenekler desteklenmez. 
   > [!TIP]
   > Geçici çözüm olarak, `ALTER DATABASE` `CREATE DATABASE` dosya eklemek veya çevreleme ayarlamak için veritabanı seçeneklerini ayarlamak için sonra kullanın. 

- Seçenek `FOR ATTACH` desteklenmiyor.
- Seçenek `AS SNAPSHOT OF` desteklenmiyor.

Daha fazla bilgi için [CREATE DATABASE'e](/sql/t-sql/statements/create-database-sql-server-transact-sql)bakın.

#### <a name="alter-database-statement"></a>ALTER DATABASE deyimi

Bazı dosya özellikleri ayarlanamıyor veya değiştirilemez:

- T-SQL deyiminde `ALTER DATABASE ADD FILE (FILENAME='path')` bir dosya yolu belirtilenemez. Yönetilen `FILENAME` bir örnek dosyaları otomatik olarak yerleştirdığından komut dosyasından kaldırın. 
- `ALTER DATABASE` İfade kullanılarak dosya adı değiştirilemez.

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

Daha fazla bilgi için [ALTER DATABASE'e](/sql/t-sql/statements/alter-database-transact-sql-file-and-filegroup-options)bakın.

### <a name="sql-server-agent"></a>SQL Server Agent

- SQL Server Agent'ı etkinleştirmek ve devre dışı bırakmak şu anda yönetilen örnekte desteklenmez. SQL Agent her zaman çalışıyor.
- SQL Server Agent ayarları yalnızca okunur. Yordam `sp_set_agent_properties` yönetilen örnekte desteklenmez. 
- İşler
  - T-SQL iş adımları desteklenir.
  - Aşağıdaki çoğaltma işleri desteklenir:
    - İşlem günlüğü okuyucu
    - Anlık Görüntü
    - Dağıtımcı
  - SSIS iş adımları desteklenir.
  - Diğer iş adımları türleri şu anda desteklenmez:
    - Birleştirme çoğaltma iş adımı desteklenmez. 
    - Queue Reader desteklenmiyor. 
    - Komuta mermisi henüz desteklenmedi.
  - Yönetilen örnekler dış kaynaklara erişemez, örneğin, ağ paylaşımları robocopy üzerinden. 
  - SQL Server Analysis Services desteklenmez.
- Bildirimler kısmen desteklenir.
- Bir Veritabanı Posta profili yapılandırmanızı gerektirmese de, e-posta bildirimi desteklenir. SQL Server Agent yalnızca bir Veritabanı Posta profili `AzureManagedInstance_dbmail_profile`kullanabilir ve çağrılmalıdır. 
  - Çağrı cihazı desteklenmiyor.
  - NetSend desteklenmez.
  - Uyarılar henüz desteklenmedi.
  - Yakınlıklar desteklenmez.
- EventLog desteklenmez.

Aşağıdaki SQL Agent özellikleri şu anda desteklenmez:

- Proxy 'leri
- Boşta kalan bir CPU'da işleri zamanlama
- Aracıyı etkinleştirme veya devre dışı bırakma
- Uyarılar

SQL Server Agent hakkında daha fazla bilgi için [SQL Server Agent'a](/sql/ssms/agent/sql-server-agent)bakın.

### <a name="tables"></a>Tablolar

Aşağıdaki tablo türleri desteklenmez:

- [Fılestream](/sql/relational-databases/blob/filestream-sql-server)
- [Filetable](/sql/relational-databases/blob/filetables-sql-server)
- [HARICI TABLO](/sql/t-sql/statements/create-external-table-transact-sql) (Polybase)
- [MEMORY_OPTIMIZED](/sql/relational-databases/in-memory-oltp/introduction-to-memory-optimized-tables) (yalnızca Genel Amaçlı katmanda desteklenmez)

Tabloların nasıl oluşturulup değiştirilen hakkında bilgi için [CREATE TABLE](/sql/t-sql/statements/create-table-transact-sql) ve [ALTER TABLE'a](/sql/t-sql/statements/alter-table-transact-sql)bakın.

## <a name="functionalities"></a>İşlevler

### <a name="bulk-insert--openrowset"></a>Toplu kesici uç / OPENROWSET

Yönetilen bir örnek dosya paylaşımlarına ve Windows klasörlerine erişemez, bu nedenle dosyaların Azure Blob depolama sundan alınması gerekir:

- `DATASOURCE`Azure Blob `BULK INSERT` depolama alanından dosya içe aktarırken komutta gereklidir. Bkz. [TOPLU INSERT](/sql/t-sql/statements/bulk-insert-transact-sql).
- `DATASOURCE`Azure Blob `OPENROWSET` depolama alanından bir dosyanın içeriğini okuduğunuz da işlevde gereklidir. Bkz. [OPENROWSET](/sql/t-sql/functions/openrowset-transact-sql).
- `OPENROWSET`diğer Azure SQL tek veritabanlarından, yönetilen örneklerden veya SQL Server örneklerinden gelen verileri okumak için kullanılabilir. Oracle veritabanları veya Excel dosyaları gibi diğer kaynaklar desteklenmez.

### <a name="clr"></a>CLR

Yönetilen bir örnek dosya paylaşımlarına ve Windows klasörlerine erişemez, bu nedenle aşağıdaki kısıtlamalar geçerlidir:

- Yalnızca `CREATE ASSEMBLY FROM BINARY` desteklenir. Bkz. [İkİl DEN ASSEM BLY OLUŞTUR](/sql/t-sql/statements/create-assembly-transact-sql). 
- `CREATE ASSEMBLY FROM FILE`desteklenmez. Bkz. [DOSYADAN MONTAJ OLUŞTUR](/sql/t-sql/statements/create-assembly-transact-sql).
- `ALTER ASSEMBLY`dosyalara başvuru yapamaz. Bkz. [ALTER ASSEMBLY](/sql/t-sql/statements/alter-assembly-transact-sql).

### <a name="database-mail-db_mail"></a>Veritabanı Postası (db_mail)
 - `sp_send_dbmail`parametre kullanarak @file_attachments ekleri gönderemez. Yerel dosya sistemi ve harici paylaşımlar veya Azure Blob Depolama bu yordamdan erişilemez.
 - Parametre ve kimlik `@query` doğrulamayla ilgili bilinen sorunlara bakın.
 
### <a name="dbcc"></a>Dbcc

SQL Server'da etkinleştirilmiş belgesiz DBCC deyimleri yönetilen örneklerde desteklenmez.

- Yalnızca sınırlı sayıda Global Trace bayrakları desteklenir. Oturum düzeyi `Trace flags` desteklenmez. Bkz. [İz bayrakları.](/sql/t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql)
- [DBCC TRACEOFF](/sql/t-sql/database-console-commands/dbcc-traceoff-transact-sql) ve [DBCC TRACEON](/sql/t-sql/database-console-commands/dbcc-traceon-transact-sql) sınırlı sayıda küresel izleme bayrakları ile çalışır.
- [DBCC CHECKDB](/sql/t-sql/database-console-commands/dbcc-checkdb-transact-sql) seçenekleri REPAIR_ALLOW_DATA_LOSS, REPAIR_FAST ve REPAIR_REBUILD veritabanı `SINGLE_USER` modunda ayarlanamadığından kullanılamaz - ALTER DATABASE [farklılıklarına](#alter-database-statement)bakın. Olası veritabanı bozulmaları Azure destek ekibi tarafından işlenir. Düzeltilmesi gereken veritabanı bozulmasını fark ediyorsanız Azure desteğine başvurun.

### <a name="distributed-transactions"></a>Dağıtılmış işlemler

MSDTC ve [esnek işlemler](sql-database-elastic-transactions-overview.md) şu anda yönetilen örneklerde desteklenmez.

### <a name="extended-events"></a>Genişletilmiş Olaylar

Genişletilmiş Etkinlikler (XEvents) için Windows'a özgü bazı hedefler desteklenmez:

- Hedef `etw_classic_sync` desteklenmiyor. Dosyaları `.xel` Azure Blob depolama alanında saklayın. [Etw_classic_sync hedefe](/sql/relational-databases/extended-events/targets-for-extended-events-in-sql-server#etw_classic_sync_target-target)bakın.
- Hedef `event_file` desteklenmiyor. Dosyaları `.xel` Azure Blob depolama alanında saklayın. [Event_file hedefe](/sql/relational-databases/extended-events/targets-for-extended-events-in-sql-server#event_file-target)bakın.

### <a name="external-libraries"></a>Dış kitaplıklar

Veritabanı nda R ve Python, dış kitaplıklar henüz desteklenmez. Bkz. [SQL Server Machine Learning Services](/sql/advanced-analytics/r/sql-server-r-services).

### <a name="filestream-and-filetable"></a>Filestream ve FileTable

- Filestream verileri desteklenmez.
- Veritabanı veri içeren `FILESTREAM` dosya grupları içeremez.
- `FILETABLE`desteklenmez.
- Tabloların türleri `FILESTREAM` olamaz.
- Aşağıdaki işlevler desteklenmez:
  - `GetPathLocator()`
  - `GET_FILESTREAM_TRANSACTION_CONTEXT()`
  - `PathName()`
  - `GetFileNamespacePat)`
  - `FileTableRootPath()`

Daha fazla bilgi için [FILESTREAM](/sql/relational-databases/blob/filestream-sql-server) ve [FileTables'a](/sql/relational-databases/blob/filetables-sql-server)bakın.

### <a name="full-text-semantic-search"></a>Tam metin Anlamsal Arama

[Anlamsal Arama](/sql/relational-databases/search/semantic-search-sql-server) desteklenmez.

### <a name="linked-servers"></a>Bağlı sunucular

Yönetilen örneklerdeki bağlantılı sunucular sınırlı sayıda hedefi destekler:

- Desteklenen hedefler Yönetilen Örnekler, Tek Veritabanları ve SQL Server örnekleridir. 
- Bağlantılı sunucular dağıtılabilir işlemleri (MS DTC) desteklemez.
- Desteklenmeyen hedefler dosyalar, Çözümleme Hizmetleri ve diğer RDBMS'dir. Azure Blob Depolama'dan yerel CSV `BULK INSERT` `OPENROWSET` alma işlemi'ni dosya alma için alternatif olarak kullanmayı deneyin.

İşlemler

- Çapraz örnek yazma hareketleri desteklenmez.
- `sp_dropserver`bağlantılı bir sunucu bırakmak için desteklenir. [Bkz. sp_dropserver.](/sql/relational-databases/system-stored-procedures/sp-dropserver-transact-sql)
- İşlev, `OPENROWSET` sorguları yalnızca SQL Server örneklerinde yürütmek için kullanılabilir. Bunlar, şirket içinde veya sanal makinelerde yönetilebilir. Bkz. [OPENROWSET](/sql/t-sql/functions/openrowset-transact-sql).
- İşlev, `OPENDATASOURCE` sorguları yalnızca SQL Server örneklerinde yürütmek için kullanılabilir. Bunlar, şirket içinde veya sanal makinelerde yönetilebilir. Sağlayıcı `SQLNCLI`olarak `SQLNCLI11`yalnızca `SQLOLEDB` , ve değerler desteklenir. `SELECT * FROM OPENDATASOURCE('SQLNCLI', '...').AdventureWorks2012.HumanResources.Employee` bunun bir örneğidir. Bkz. [OPENDATASOURCE](/sql/t-sql/functions/opendatasource-transact-sql).
- Bağlantılı sunucular ağ paylaşımlarından dosyaları (Excel, CSV) okumak için kullanılamaz. Azure Blob Depolama'dan CSV dosyalarını okuyan [TOPLU INSERT](/sql/t-sql/statements/bulk-insert-transact-sql#e-importing-data-from-a-csv-file) veya [OPENROWSET'i](/sql/t-sql/functions/openrowset-transact-sql#g-accessing-data-from-a-csv-file-with-a-format-file) kullanmayı deneyin. Yönetilen örnek [Geri bildirim öğesinde](https://feedback.azure.com/forums/915676-sql-managed-instance/suggestions/35657887-linked-server-to-non-sql-sources) bu istekleri izleme|

### <a name="polybase"></a>PolyBase

HDFS veya Azure Blob depolama sındaki dosyalara başvuran harici tablolar desteklenmez. PolyBase hakkında daha fazla bilgi için [Bkz. PolyBase.](/sql/relational-databases/polybase/polybase-guide)

### <a name="replication"></a>Çoğaltma

- Anlık görüntü ve çift yönlü çoğaltma türleri desteklenir. Birleştirme çoğaltma, eşler arası çoğaltma ve güncellenebilir abonelikler desteklenmez.
- [İşlemsel Çoğaltma,](sql-database-managed-instance-transactional-replication.md) bazı kısıtlamalarla yönetilen örnekte genel önizleme için kullanılabilir:
    - Her tür çoğaltma katılımcısı (Publisher, Distributor, Pull Subscriber ve Push Subscriber) yönetilen örneklere yerleştirilebilir, ancak yayımcı ve dağıtıcı hem bulutta hem de şirket içinde olmalıdır.
    - Yönetilen örnekler, SQL Server'ın son sürümleriyle iletişim kurabilir. Daha fazla bilgi için [desteklenen sürümler matrisine](sql-database-managed-instance-transactional-replication.md#supportability-matrix-for-instance-databases-and-on-premises-systems) bakın.
    - İşlem çoğaltma bazı [ek ağ gereksinimleri](sql-database-managed-instance-transactional-replication.md#requirements)vardır.

İşlem çoğaltma yapılandırma hakkında daha fazla bilgi için aşağıdaki öğreticilere bakın:
- [MI yayımcısı ve abone arasında çoğaltma](replication-with-sql-database-managed-instance.md)
- [MI yayımcısı, MI dağıtıcısı ve SQL Server abonesi arasında çoğaltma](sql-database-managed-instance-configure-replication-tutorial.md)

### <a name="restore-statement"></a>RESTORE deyimi 

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
  - `FROM DISK`/`TAPE`/yedekleme aygıtı desteklenmez.
  - Yedekleme kümeleri desteklenmez.
- `WITH`seçenekler desteklenmez, örneğin hayır `DIFFERENTIAL` `STATS`veya .
- `ASYNC RESTORE`: İstemci bağlantısı kesişse bile geri yükleme devam ediyor. Bağlantınız bırakılırsa, `sys.dm_operation_status` görünümü geri yükleme işleminin durumunu ve CREATE ve DROP veritabanını denetleyebilirsiniz. Bkz. [sys.dm_operation_status](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database). 

Aşağıdaki veritabanı seçenekleri ayarlanır veya geçersiz kılındı ve daha sonra değiştirilemez: 

- `NEW_BROKER`broker .bak dosyasında etkin değilse. 
- `ENABLE_BROKER`broker .bak dosyasında etkin değilse. 
- `AUTO_CLOSE=OFF`.bak dosyasındaki bir veritabanında `AUTO_CLOSE=ON`. 
- `RECOVERY FULL`.bak dosyasındaki bir veritabanında `BULK_LOGGED` varsa veya kurtarma moduna sahiptir. `SIMPLE`
- Bellek için en iyi duruma getirilmiş bir dosya grubu eklenir ve kaynak .bak dosyasında yoksa XTP olarak adlandırılır. 
- Varolan herhangi bir bellek için en iyi duruma getirilmiş dosya grubu XTP olarak yeniden adlandırılır. 
- `SINGLE_USER`ve `RESTRICTED_USER` seçenekler ' `MULTI_USER`e dönüştürülür.

Sınırlamalar: 

- Bozuk veritabanlarının yedekleri bozulmanın türüne bağlı olarak geri yüklenebilir, ancak bozulma giderilene kadar otomatik yedeklemeler alınmaz. Kaynak örnekte `DBCC CHECKDB` çalıştırdığınızdan ve bu `WITH CHECKSUM` sorunu önlemek için yedekleme kullandığınızdan emin olun.
- Bu `.BAK` belgede açıklanan herhangi bir sınırlamayı (örneğin, `FILESTREAM` veya `FILETABLE` nesneler) içeren bir veritabanının dosyasının geri yüklenir, Yönetilen Örnek'te geri yüklenemez.
- `.BAK`birden çok yedekleme kümesi içeren dosyalar geri yüklenebilir. 
- `.BAK`birden çok günlük dosyası içeren dosyalar geri yüklenebilir.
- 8 TB'den büyük veritabanları, etkin bellek içi OLTP nesneleri veya örnek başına 280 dosyayı aşan dosya sayısı içeren yedeklemeler Genel Amaçlı örneğinde geri yüklenmez. 
- 4 TB'den büyük veritabanları veya [kaynak sınırlarında](sql-database-managed-instance-resource-limits.md) açıklanan boyuttan daha büyük toplam boyutu olan bellek içi OLTP nesneleri içeren yedeklemeler İş Açısından Kritik örneğinde geri yüklenemez.
Geri yükleme deyimleri hakkında bilgi için geri [yükleme deyimleri'ne](/sql/t-sql/statements/restore-statements-transact-sql)bakın.

 > [!IMPORTANT]
 > Aynı sınırlamalar yerleşik nokta-in-time geri yükleme işlemi için de geçerlidir. Örnek olarak, İş Açısından Kritik örnekte 4 TB'den büyük Genel Amaç veritabanı geri yüklenemez. In-memory OLTP dosyaları veya 280'den fazla dosya ile İş Kritik veritabanı Genel Amaçlı örneğinde geri yüklenemez.

### <a name="service-broker"></a>Servis komisyoncusu

Örnekler arası hizmet aracısı desteklenmez:

- `sys.routes`: Ön koşul olarak adresi sys.routes adresinden seçmeniz gerekmektedir. Adres her rotada YEREL olmalıdır. Bkz. [sys.routes](/sql/relational-databases/system-catalog-views/sys-routes-transact-sql).
- `CREATE ROUTE`: Başka bir `CREATE ROUTE` şeyle `ADDRESS` `LOCAL`kullanamazsınız. Bkz. [CREATE ROUTE](/sql/t-sql/statements/create-route-transact-sql).
- `ALTER ROUTE`: Başka bir `ALTER ROUTE` şeyle `ADDRESS` `LOCAL`kullanamazsınız. Bkz. [ALTER ROUTE](/sql/t-sql/statements/alter-route-transact-sql). 

### <a name="stored-procedures-functions-and-triggers"></a>Depolanan yordamlar, işlevler ve tetikleyiciler

- `NATIVE_COMPILATION`Genel Amaç katmanında desteklenmez.
- Aşağıdaki [sp_configure](/sql/relational-databases/system-stored-procedures/sp-configure-transact-sql) seçenekleri desteklenmez: 
  - `allow polybase export`
  - `allow updates`
  - `filestream_access_level`
  - `remote access`
  - `remote data archive`
  - `remote proc trans`
- `sp_execute_external_scripts`desteklenmez. [Bkz. sp_execute_external_scripts.](/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql#examples)
- `xp_cmdshell`desteklenmez. [Bkz. xp_cmdshell](/sql/relational-databases/system-stored-procedures/xp-cmdshell-transact-sql).
- `Extended stored procedures`desteklenmez, içerir `sp_addextendedproc`  ve `sp_dropextendedproc`. Bkz. [Genişletilmiş depolanmış yordamlar.](/sql/relational-databases/system-stored-procedures/general-extended-stored-procedures-transact-sql)
- `sp_attach_db`, `sp_attach_single_file_db`ve `sp_detach_db` desteklenmez. [bkz sp_attach_db](/sql/relational-databases/system-stored-procedures/sp-attach-db-transact-sql), [sp_attach_single_file_db](/sql/relational-databases/system-stored-procedures/sp-attach-single-file-db-transact-sql), ve [sp_detach_db](/sql/relational-databases/system-stored-procedures/sp-detach-db-transact-sql).

### <a name="system-functions-and-variables"></a>Sistem fonksiyonları ve değişkenler

Aşağıdaki değişkenler, işlevler ve görünümler farklı sonuçlar verir:

- `SERVERPROPERTY('EngineEdition')`değerini 8 döndürür. Bu özellik, yönetilen bir örneği benzersiz olarak tanımlar. Bkz. [SERVERPROPERTY](/sql/t-sql/functions/serverproperty-transact-sql).
- `SERVERPROPERTY('InstanceName')`SQL Server için var olduğu gibi örnek kavramı yönetilen bir örneğe geçerli olmadığından NULL döndürür. Bkz. [SERVERPROPERTY('InstanceName')](/sql/t-sql/functions/serverproperty-transact-sql).
- `@@SERVERNAME`tam bir DNS "bağlanabilir" adı döndürür, örneğin, my-managed-instance.wcus17662feb9ce98.database.windows.net. @ bakın . [@SERVERNAME](/sql/t-sql/functions/servername-transact-sql) 
- `SYS.SERVERS`özellikleri "ad" ve "data_source" gibi `myinstance.domain.database.windows.net` tam bir DNS "bağlanabilir" adı döndürür. [Bkz. Sys. SUNUCULAR](/sql/relational-databases/system-catalog-views/sys-servers-transact-sql).
- `@@SERVICENAME`SQL Server için var olan hizmet kavramı yönetilen bir örneğe uygulanmadığından NULL döndürür. @ bakın . [@SERVICENAME](/sql/t-sql/functions/servicename-transact-sql)
- `SUSER_ID`desteklenir. Azure AD girişi sys.syslogins'de değilse NULL döndürür. [Bkz. SUSER_ID](/sql/t-sql/functions/suser-id-transact-sql). 
- `SUSER_SID`desteklenmez. Yanlış veriler döndürülür, bu da bilinen geçici bir sorundur. [Bkz. SUSER_SID](/sql/t-sql/functions/suser-sid-transact-sql). 

## <a name="environment-constraints"></a><a name="Environment"></a>Çevre kısıtlamaları

### <a name="subnet"></a>Alt ağ
-  Yönetilen örneğini dağıttığınız alt ağa başka kaynaklar (örneğin sanal makineler) koyamazsınız. Bu kaynakları farklı bir alt ağ kullanarak dağıtın.
- Subnet yeterli sayıda kullanılabilir [IP adresine](sql-database-managed-instance-connectivity-architecture.md#network-requirements)sahip olmalıdır. En az 16, tavsiye ise alt ağda en az 32 IP adresi olmasıdır.
- [Hizmet bitiş noktaları yönetilen örneğin alt ağıyla ilişkilendirilemez.](sql-database-managed-instance-connectivity-architecture.md#network-requirements) Sanal ağ oluştururken hizmet uç noktaları seçeneğinin devre dışı bırakıldığından emin olun.
- VCores sayısı ve bir bölgede dağıtabilirsiniz örnek türleri bazı [kısıtlamalar ve sınırlar](sql-database-managed-instance-resource-limits.md#regional-resource-limitations)vardır.
- [Alt ağüzerinde uygulanması gereken](sql-database-managed-instance-connectivity-architecture.md#network-requirements)bazı güvenlik kuralları vardır.

### <a name="vnet"></a>VNET
- VNet Kaynak Modeli kullanılarak dağıtılabilir - VNet için Klasik Model desteklenmez.
- Yönetilen bir örnek oluşturulduktan sonra, yönetilen örneğin veya VNet'in başka bir kaynak grubuna taşınması veya aboneliği desteklenmez.
- Uygulama Hizmet Ortamları, Mantık uygulamaları ve yönetilen örnekler (Coğrafi çoğaltma, İşlem çoğaltma veya bağlantılı sunucular aracılığıyla kullanılan) gibi bazı hizmetler, VNet'leri [genel bakış kullanarak](../virtual-network/virtual-networks-faq.md#what-are-the-constraints-related-to-global-vnet-peering-and-load-balancers)bağlıysa farklı bölgelerdeyönetilen örneklere erişemez. Bu kaynaklara ExpressRoute veya VNet-to-VNet üzerinden VNet Ağ Geçitleri üzerinden bağlanabilirsiniz.

### <a name="tempdb"></a>Tempdb

Genel Amaçlı bir `tempdb` katmanda çekirdek başına en fazla 24 GB'dan büyük dosya boyutu olamaz. İş `tempdb` Açısından Kritik katmanındaki maksimum boyut, örnek depolama boyutuyla sınırlıdır. `Tempdb`günlük dosya boyutu Genel Amaçlı katmanında 120 GB ile sınırlıdır. Bazı sorgular, çekirdek başına 24 GB'tan fazla `tempdb` sınagereksinim duyduklarında veya 120 GB'dan fazla günlük verisi üretirlerse hata döndürebilir.

### <a name="msdb"></a>Msdb

Yönetilen örnekte aşağıdaki MSDB şemalarının kendi önceden tanımlanmış rollerine sahip olması gerekir:

- Genel roller
  - TargetServersRole
- [Veritabanı rolleri düzeltildi](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent-fixed-database-roles?view=sql-server-ver15)
  - SQLAgentUserRole
  - SQLAgentReaderRole
  - SQLAgentOperatorRole
- [DatabaseMail rolleri:](https://docs.microsoft.com/sql/relational-databases/database-mail/database-mail-configuration-objects?view=sql-server-ver15#DBProfile)
  - Databasemailuserrole
- [Entegrasyon hizmetleri rolleri:](https://docs.microsoft.com/sql/integration-services/security/integration-services-roles-ssis-service?view=sql-server-ver15)
  - Db_ssisadmin
  - Db_ssisltduser
  - db_ssisoperator
  
> [!IMPORTANT]
> Önceden tanımlanmış rol adlarının, şema adlarının ve şema sahiplerinin müşteriler tarafından değiştirilmesi, hizmetin normal çalışmasını etkileyecektir. Bunlarda yapılan değişiklikler, normal hizmet çalışmasını sağlamak için önceden tanımlanmış değerlere veya en geç bir sonraki hizmet güncelleştirmesine geri döndürülür.

### <a name="error-logs"></a>Hata günlükleri

Yönetilen bir örnek, ayrıntılı bilgileri hata günlüklerine yerleştirir. Hata günlüğünde günlüğe kaydedilmiş birçok iç sistem olayı vardır. Bazı alakasız girişleri filtreleyen hata günlüklerini okumak için özel bir yordam kullanın. Daha fazla bilgi için, Azure Veri Stüdyosu için yönetilen örnek sp_readmierrorlog veya [yönetilen örnek uzantısı (önizleme)](/sql/azure-data-studio/azure-sql-managed-instance-extension#logs) gibi yönetilen [örneğe](https://blogs.msdn.microsoft.com/sqlcat/2018/05/04/azure-sql-db-managed-instance-sp_readmierrorlog/) bakın.

## <a name="next-steps"></a>Sonraki adımlar

- Yönetilen örnekler hakkında daha fazla bilgi için [bkz.](sql-database-managed-instance.md)
- Özellikler ve karşılaştırma listesi için [Azure SQL Veritabanı özellik karşılaştırması'na](sql-database-features.md)bakın.
- Sürüm güncelleştirmeleri ve bilinen sorunlar durumu için [SQL Veritabanı sürüm notlarına](sql-database-release-notes.md) bakın
- Yeni yönetilen bir örnek oluşturmayı gösteren hızlı [bir](sql-database-managed-instance-get-started.md)başlangıç için bkz.
