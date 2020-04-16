---
title: SQL Kimlik Doğrulaması
description: Azure Synapse Analytics'te SQL kimlik doğrulaması hakkında bilgi edinin.
services: synapse-analytics
author: vvasic-msft
ms.service: synapse-analytics
ms.topic: overview
ms.date: 04/15/2020
ms.author: vvasic
ms.reviewer: jrasnick
ms.openlocfilehash: 2b80efa30ac7e04b9eb21dd6f8a39ab4ee90adf6
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81424856"
---
# <a name="sql-authentication"></a>SQL Kimlik Doğrulaması

Azure Synapse Analytics'te kaynak tüketiminizi kontrol etmenizi sağlayan iki SQL form faktörü vardır. Bu makalede, iki form faktörünün kullanıcı kimlik doğrulamasını nasıl denetlenen açıkladığı açıklanmaktadır.

Synapse SQL'e yetki vermek için iki yetkilendirme türü kullanabilirsiniz:

- AAD yetkilendirmesi
- SQL yetkilendirme

AAD yetkilendirmesi Azure Active Directory'ye dayanır ve kullanıcı yönetimi için tek bir yere sahip olduğunuzu sağlar. SQL yetkilendirmesi, eski uygulamaların Synapse SQL'i iyi bilinen bir şekilde kullanmasını sağlar.

## <a name="administrative-accounts"></a>İdari hesaplar

Yönetici işlevlerine sahip iki yönetici hesabı (**Sunucu yöneticisi** ve **Active Directory yöneticisi**) vardır. SQL sunucunuz için bu yönetici hesaplarını tanımlamak için Azure portalını açın ve Synapse SQL'inizin Özellikler sekmesine gidin.

![SQL Server Yöneticileri](./media/sql-authentication/sql-admins.png)

- **Sunucu yöneticisi**

  Bir Azure Synapse Analytics oluşturduğunuzda, bir **Sunucu yöneticisi girişi**belirlemeniz gerekir. SQL Server, bu hesabı ana veritabanında oturum açma bilgileri olarak oluşturur. Bu hesap SQL Server kimlik doğrulaması (kullanıcı adı ve parola) kullanarak bağlanır. Bu hesaplardan yalnızca biri mevcut olabilir.

- **Azure Active Directory yöneticisi**

  Ayrıca, Azure Active Directory’deki bir adet kişi veya güvenlik grubu hesabı da yönetici olarak yapılandırılabilir. Bir Azure AD yöneticisini yapılandırmak isteğe bağlıdır, ancak Synapse SQL'e bağlanmak için Azure AD hesaplarını kullanmak istiyorsanız Azure AD yöneticisinin yapılandırılması **gerekir.**

**Sunucu yöneticisi** ve Azure **AD yöneticisi** hesapları aşağıdaki özelliklere sahiptir:

- Sunucudaki herhangi bir SQL Veritabanına otomatik olarak bağlanabilen tek hesaplardır. (Diğer hesapların, bir kullanıcı veritabanına bağlanabilmek için veritabanının sahibi olmaları veya kullanıcı veritabanında kullanıcı hesabına sahip olmaları gerekir.)
- Bu hesaplar kullanıcı veritabanlarına `dbo` kullanıcısı olarak girer ve kullanıcı veritabanlarında tüm izinlere sahip olur. (Kullanıcı veritabanının sahibi de veritabanına `dbo` kullanıcısı olarak girer.)
- Veritabanına `master` kullanıcı olarak `dbo` girmeyin ve ana olarak sınırlı izinlere sahip olun.
- SQL veritabanında bulunmayan standart `sysadmin` SQL Server sabit sunucu rolünün üyeleri **değildir.**  
- Veritabanlarını, girişleri, ana alandaki kullanıcıları ve sunucu düzeyindeKI IP güvenlik duvarı kurallarını oluşturabilir, değiştirebilir ve bırakabilir.
- Üyeler ekime ve `dbmanager` `loginmanager` rollere kaldırabilir.
- Sistem tablosunu `sys.sql_logins` görüntüleyebilirsiniz.

## <a name="sql-on-demand-preview"></a>İsteğe bağlı SQL (önizleme)

İsteğe bağlı OLARAK SQL erişimi olan kullanıcıları yönetmek için aşağıdaki yönergeleri kullanabilirsiniz.

İsteğe bağlı SQL'e giriş oluşturmak için aşağıdaki sözdizimini kullanın:

```sql
CREATE LOGIN Mary WITH PASSWORD = '<strong_password>';
-- or
CREATE LOGIN Mary@domainname.net FROM EXTERNAL PROVIDER;
```
Oturum açma var olduktan sonra, SQL on-demand bitiş noktası nın içindeki tek tek veritabanlarında kullanıcılar oluşturabilir ve bu kullanıcılara gerekli izinleri verebilirsiniz. Bir kullanım oluşturmak için aşağıdaki sözdizimini kullanabilirsiniz:
```sql
CREATE USER Mary FROM LOGIN Mary;
-- or
CREATE USER Mary FROM LOGIN Mary@domainname.net;
-- or
CREATE USER [mike@contoso.com] FROM EXTERNAL PROVIDER;
```

Oturum açma ve kullanıcı oluşturulduktan sonra, hakları vermek için normal SQL Server sözdizimini kullanabilirsiniz.

## <a name="sql-pool"></a>SQL Havuzu

### <a name="administrator-access-path"></a>Yönetici erişim yolu

Sunucu düzeyi güvenlik duvarı doğru şekilde yapılandırıldığında **SQL sunucu yöneticisi** ve **Azure Active Directory yöneticisi**, SQL Server Management Studio veya SQL Server Veri Araçları gibi istemci araçlarını kullanarak bağlantı kurabilir. Tüm özellikler ve yetenekler yalnızca en güncel araçlar tarafından sunulur. 

Aşağıdaki diyagram, iki yönetici hesabı için tipik bir yapılandırmayı gösterir:
 
![iki yönetim hesaplarının yapılandırması](./media/sql-authentication/1sql-db-administrator-access.png)

Yöneticiler, sunucu düzeyi güvenlik duvarındaki açık bağlantı noktalarından birini kullanarak tüm SQL Veritabanlarına bağlanabilir.

### <a name="database-creators"></a>Veritabanı oluşturucuları

Bu idari rollerden biri **dbmanager** rolüdür. Bu rolün üyeleri yeni veritabanları oluşturabilir. Bu rolü kullanmak için `master` veritabanında bir kullanıcı oluşturmanız ve bu kullanıcıyı **dbmanager** veritabanı rolüne eklemeniz gerekir. 

Bir veritabanı oluşturmak için, kullanıcı `master` veritabanında bir SQL Server girişine dayalı bir kullanıcı veya bir Azure Active Directory kullanıcısını temel alan veritabanı kullanıcısı olmalıdır.

1. Yönetici hesabı kullanarak veritabanına `master` bağlanın.
2. [CREATE Gİrİş](/sql/t-sql/statements/create-login-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) ekstresini kullanarak bir SQL Server kimlik doğrulama girişi oluşturun. Örnek deyim:

   ```sql
   CREATE LOGIN Mary WITH PASSWORD = '<strong_password>';
   ```

   > [!NOTE]
   > Oturum açma bilgileri veya bağımsız veritabanı kullanıcısı oluştururken güçlü bir parola kullanın. Daha fazla bilgi için bkz. [Güçlü Parolalar](/sql/relational-databases/security/strong-passwords?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest).

   Performansı artırmak için oturum açma bilgileri (sunucu düzeyi asıl hesaplar) veritabanı düzeyinde geçici olarak önbelleğe alınır. Kimlik doğrulaması önbelleğini yenilemek için bkz. [DBCC FLUSHAUTHCACHE](/sql/t-sql/database-console-commands/dbcc-flushauthcache-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest).

3. Veritabanında, CREATE USER deyimini kullanarak bir kullanıcı oluşturun. [CREATE USER](/sql/t-sql/statements/create-user-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) `master` Kullanıcı, içerdiği bir Azure Active Directory kimlik doğrulaması veritabanı kullanıcısı (Azure AD kimlik doğrulaması için ortamınızı yapılandırmışsanız) veya veritabanı kullanıcısı içeren bir SQL Server kimlik doğrulaması veya SQL Server kimlik doğrulama sını temel alan bir SQL Server kimlik doğrulama kullanıcısı (önceki adımda oluşturulmuştur.) olabilir. Örnek ifadeler:

   ```sql
   CREATE USER [mike@contoso.com] FROM EXTERNAL PROVIDER; -- To create a user with Azure Active Directory
   CREATE USER Ann WITH PASSWORD = '<strong_password>'; -- To create a SQL Database contained database user
   CREATE USER Mary FROM LOGIN Mary;  -- To create a SQL Server user based on a SQL Server authentication login
   ```

4. [ALTER ROLE](/sql/t-sql/statements/alter-role-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) deyimini kullanırken `master` **dbmanager** veritabanı rolüne yeni kullanıcıyı ekleyin. Örnek deyimler:

   ```sql
   ALTER ROLE dbmanager ADD MEMBER Mary;
   ALTER ROLE dbmanager ADD MEMBER [mike@contoso.com];
   ```

   > [!NOTE]
   > dbmanager, ana veritabanındaki bir veritabanı rolü olduğu için bu role yalnızca veritabanı kullanıcısı ekleyebilirsiniz. Veritabanı düzeyindeki role sunucu düzeyinde oturum açma bilgisi ekleyemezsiniz.

5. Gerekirse, yeni kullanıcının bağlantı kurabilmesi için bir güvenlik duvarı kuralı yapılandırın. (Yeni kullanıcı, mevcut bir güvenlik duvarı kuralı kapsamında olabilir.)

Artık kullanıcı `master` veritabanına bağlanabilir ve yeni veritabanları oluşturabilir. Veritabanını oluşturan hesap, veritabanının sahibi olur.

### <a name="login-managers"></a>Oturum açma yöneticileri

Diğer yönetim rolü ise oturum açma yöneticisi rolüdür. Bu rolün üyeleri ana veritabanında yeni oturum açma bilgileri oluşturabilir. İsterseniz, aynı adımları uygulayarak (oturum açma bilgileri oluşturmak, kullanıcı oluşturmak ve **loginmanager** rolüne kullanıcı eklemek) bir kullanıcının ana veritabanında yeni oturum açma bilgileri oluşturmasını sağlayabilirsiniz. Microsoft, oturum açma bilgisi tabanlı kullanıcılar yerine, veritabanı düzeyinde kimlik doğrulaması yapan bağımsız veritabanı kullanıcılarından yararlanılmasını önerir. Dolayısıyla, oturum açma bilgileri genellikle gerekli değildir. Daha fazla bilgi için bkz. [Bağımsız Veritabanı Kullanıcıları - Veritabanınızı Taşınabilir Hale Getirme](/sql/relational-databases/security/contained-database-users-making-your-database-portable?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest).

## <a name="non-administrator-users"></a>Yönetici olmayan kullanıcılar

Çoğu durumda, yönetici olmayan kullanıcıların ana veritabanına erişmesi gerekmez. [CREATE USER (Transact-SQL)](https://msdn.microsoft.com/library/ms173463.aspx) deyimini kullanarak veritabanı düzeyinde bağımsız veritabanı kullanıcıları oluşturun. 

Kullanıcı Azure Active Directory kimlik doğrulaması bağımsız veritabanı kullanıcısı (ortamınızı Azure AD kimlik doğrulaması için yapılandırdıysanız) veya SQL Server kimlik doğrulaması bağımsız veritabanı kullanıcısı ya da SQL Server kimlik doğrulaması oturum açma bilgilerini kullanan SQL Server kimlik doğrulaması kullanıcısı (önceki adımda oluşturulan) olabilir.  

Kullanıcı oluşturmak için veritabanına bağlanın ve aşağıdaki örneklere benzer deyimleri çalıştırın:

```sql
CREATE USER Mary FROM LOGIN Mary;
CREATE USER [mike@contoso.com] FROM EXTERNAL PROVIDER;
```

Başlangıçta veritabanı yöneticilerinden yalnızca biri veya veritabanının sahibi kullanıcı oluşturabilir. Daha fazla kullanıcıya yeni kullanıcı oluşturma yetkisi vermek için şunun gibi bir deyim kullanarak `ALTER ANY USER` izni verin:

```sql
GRANT ALTER ANY USER TO Mary;
```

Ek kullanıcılara veritabanının tam denetimini sağlamak için, onları **db_owner** sabit veritabanı rolünün bir üyesi yapın.

Azure SQL Veritabanı'nda `ALTER ROLE` deyimi kullanın.

```sql
ALTER ROLE db_owner ADD MEMBER Mary;
```

SQL havuzunda [EXEC sp_addrolemember](/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)kullanın.

```sql
EXEC sp_addrolemember 'db_owner', 'Mary';
```

> [!NOTE]
> SQL Database sunucu girişine dayalı bir veritabanı kullanıcısı oluşturmanın yaygın nedenlerinden biri, birden çok veritabanına erişmesi gereken kullanıcılardır. İçerdiği veritabanı kullanıcıları ayrı varlıklar olduğundan, her veritabanı kendi kullanıcısını ve kendi parolasını korur. Kullanıcının her veritabanı için her parolayı hatırlaması gerektiğinden, bu durum genel kullanıma neden olabilir ve birçok veritabanı için birden çok parolayı değiştirmek zorunda kaldığında savunulamaz hale gelebilir. Ancak, SQL Server Girişleri ve yüksek kullanılabilirlik (etkin coğrafi çoğaltma ve başarısız gruplar) kullanırken, SQL Server girişleri her sunucuda el ile ayarlanmalıdır. Aksi takdirde, veritabanı kullanıcısı artık bir hata oluştuktan sonra sunucu girişine eşlenir ve veritabanı sonrası failover'a erişemez. 

Coğrafi çoğaltma için oturum açma yapılandırma hakkında daha fazla bilgi için, lütfen azure SQL Veritabanı güvenliğini coğrafi geri yükleme veya başarısız lık [için Yapılandır ve yönetin.](../../sql-database/sql-database-geo-replication-security-config.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)

### <a name="configuring-the-database-level-firewall"></a>Veritabanı düzeyinde güvenlik duvarını yapılandırma

En iyi uygulamalardan biri, yönetici olmayan kullanıcıların kullandıkları veritabanlarına yalnızca güvenlik duvarı aracılığıyla erişim sağlamasıdır. Bu kullanıcıların IP adreslerini sunucu düzeyinde güvenlik duvarından yetkilendirip tüm veritabanlarına erişim vermek yerine [sp_set_database_firewall_rule](/sql/relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) deyimini kullanarak veritabanı düzeyinde güvenlik duvarını yapılandırabilirsiniz. Veritabanı düzeyinde güvenlik duvarı portal kullanılarak yapılandırılamaz.

### <a name="non-administrator-access-path"></a>Yönetici olmayan kullanıcılar için erişim yolu

Veritabanı düzeyinde güvenlik duvarı doğru şekilde yapılandırıldığında veritabanı kullanıcıları SQL Server Management Studio veya SQL Server Veri Araçları gibi istemci araçlarını kullanarak bağlantı kurabilir. Tüm özellikler ve yetenekler yalnızca en güncel araçlar tarafından sunulur. Aşağıdaki şema, yönetici olmayan kullanıcılar için tipik erişim yolunu göstermektedir.

![Yönetici olmayan kullanıcılar için erişim yolu](./media/sql-authentication/2sql-db-nonadmin-access.png)

## <a name="groups-and-roles"></a>Gruplar ve roller

Verimli erişim yönetimi için bireysel kullanıcılar yerine gruplara ve rollere atanan izinler kullanılır.

- Azure Active Directory kimlik doğrulaması kullandığınızda Azure Active Directory kullanıcılarını bir Azure Active Directory grubuna ekleyin. Grup için bir bağımsız veritabanı kullanıcısı oluşturun. Bir veya daha fazla veritabanı kullanıcılarını bir [veritabanı rolüne](/sql/relational-databases/security/authentication-access/database-level-roles?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) ekleyin ve [izinleri](/sql/relational-databases/security/permissions-database-engine?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) veritabanı rolüne atayın.

- SQL Server kimlik doğrulamasını kullanırken veritabanında bağımsız veritabanı kullanıcılarını oluşturun. Bir veya daha fazla veritabanı kullanıcılarını bir [veritabanı rolüne](/sql/relational-databases/security/authentication-access/database-level-roles?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) ekleyin ve [izinleri](/sql/relational-databases/security/permissions-database-engine?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) veritabanı rolüne atayın.

Veritabanı rolleri **db_owner**, **db_ddladmin**, **db_datawriter**, **db_datareader**, **db_denydatawriter** ve **db_denydatareader** gibi yerleşik roller olabilir. Birkaç kullanıcıya tam izin vermek için genelde **db_owner** kullanılır. Diğer sabit veritabanı rolleri, geliştirme aşamasında basit bir veritabanını hızlı bir şekilde kullanıma almak için kullanışlıdır ancak çoğu üretim veritabanı için önerilmez. 

Örneğin **db_datareader** sabit veritabanı rolü, veritabanındaki tüm tablolara okuma izni verir ve bu durum genelde ihtiyaç duyulandan fazlasıdır. 

[CREATE ROLE](https://msdn.microsoft.com/library/ms187936.aspx) deyimini kullanarak kendi kullanıcı tanımlı veritabanı rollerinizi oluşturmak ve dikkatli bir şekilde her role ihtiyaç duyulan en alt düzeyde izinleri vermek çok daha iyidir. Birden fazla rolün üyesi olan kullanıcılar, tüm rollerin izinlerine sahip olur.

## <a name="permissions"></a>İzinler

SQL Veritabanında ayrı ayrı verilebilen veya reddedilebilen 100'den fazla izin vardır. Bu izinlerin çoğu iç içe geçmiş haldedir. Örneğin, bir şemada için verilen `UPDATE` izni, o şema içindeki tüm tablolar için `UPDATE` iznini de içerir. Çoğu izin sisteminde olduğu gibi bir iznin reddedilmesi, aynı iznin verilme durumunu geçersiz kılar. 

İç içe geçmiş yapısı ve izin sayısı nedeniyle, veritabanınızı doğru şekilde korumak için uygun bir izin sistemi tasarlamak uzun ve dikkatli bir çalışma gerektirebilir. 

[İzinler (Veritabanı Altyapısı)](https://docs.microsoft.com/sql/relational-databases/security/permissions-database-engine) ile başlayın ve izinlerin [poster boyutundaki tablosunu](https://docs.microsoft.com/sql/relational-databases/security/media/database-engine-permissions.png) inceleyin.

### <a name="considerations-and-restrictions"></a>Dikkat edilmesi gerekenler ve kısıtlamalar

SQL Veritabanı'nda oturum açma ları ve kullanıcıları yönetirken aşağıdaki noktaları göz önünde bulundurun:

- İfadeleri yürüterken **ana** veritabanına `CREATE/ALTER/DROP DATABASE` bağlı olmalısınız.
- **Sunucu yöneticisi** oturum açma bilgilerine karşılık gelen veritabanı kullanıcısı değiştirilemez veya kaldırılamaz.
- **Sunucu yöneticisi** oturum açma bilgilerinin varsayılan dili ABD-İngilizce olarak belirlenmiştir.
- Yalnızca yöneticiler (**Sunucu yöneticisi** oturum açma bilgileri veya Azure AD yöneticisi) ve **ana** veritabanındaki **dbmanager** veritabanı rolünün üyeleri `CREATE DATABASE` ve `DROP DATABASE` deyimlerini yürütme iznine sahiptir.
- `CREATE/ALTER/DROP LOGIN` deyimlerini yürütürken ana veritabanına bağlanmış olmanız gerekir. Ancak oturum açma bilgilerinin kullanılması önerilmez. Bunun yerine bağımsız veritabanı kullanıcılarını kullanmanız önerilir.
- Bir kullanıcı veritabanına bağlanmak için bağlantı dizesinde veritabanının adını belirtmeniz gerekir.
- Yalnızca sunucu düzeyi asıl oturum açma bilgisi ve **ana** veritabanındaki **loginmanager** veritabanı rolünün üyeleri `CREATE LOGIN`,`ALTER LOGIN` ve `DROP LOGIN` deyimlerini yürütme iznine sahiptir.
- Bir ADO.NET uygulamasında `CREATE/ALTER/DROP LOGIN` ve `CREATE/ALTER/DROP DATABASE` deyimlerini yürütürken parametreli komutların kullanılmasına izin verilmez. Daha fazla bilgi için bkz. [Komutlar ve Parametreler](/dotnet/framework/data/adonet/commands-and-parameters?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json).
- `CREATE/ALTER/DROP DATABASE` ve `CREATE/ALTER/DROP LOGIN` deyimlerini yürütürken bu deyimlerin her birinin bir Transact-SQL toplu işindeki tek deyim olması gerekir. Aksi takdirde bir hata oluşur. Örneğin, aşağıdaki Transact-SQL veritabanının mevcut olup olmadığını kontrol eder. Mevcutsa, veritabanını kaldırmak için bir `DROP DATABASE` deyimi çağrılır. `DROP DATABASE` deyimi toplu işteki tek deyim olmadığından aşağıdaki Transact-SQL deyiminin yürütülmesi hatayla sonuçlanacaktır.

  ```sql
  IF EXISTS (SELECT [name]
           FROM   [sys].[databases]
           WHERE  [name] = N'database_name')
  DROP DATABASE [database_name];
  GO
  ```
  
  Bunun yerine, aşağıdaki Transact-SQL deyimini kullanın:
  
  ```sql
  DROP DATABASE IF EXISTS [database_name]
  ```

- `CREATE USER` deyimini `FOR/FROM LOGIN` seçeneğiyle yürütürken bunun bir Transact-SQL toplu işindeki tek deyim olması gerekir.
- `ALTER USER` deyimini `WITH LOGIN` seçeneğiyle yürütürken bunun bir Transact-SQL toplu işindeki tek deyim olması gerekir.
- Bir kullanıcıda `CREATE/ALTER/DROP` işlemini gerçekleştirmek için veritabanında `ALTER ANY USER` izni gerekir.
- Bir veritabanı rolünün sahibi başka bir kullanıcıyı ilgili veritabanı rolüne eklemeye veya bu rolden kaldırmaya çalıştığında şu hata ortaya çıkabilir: **Kullanıcı veya rol "Ad" bu veritabanında mevcut değil.** Bu hatanın nedeni, kullanıcının rol sahibine görünür olmamasıdır. Bu sorunu çözmek için rol sahibine kullanıcı için `VIEW DEFINITION` iznini verin. 

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz. [Bağımsız Veritabanı Kullanıcıları - Veritabanınızı Taşınabilir Hale Getirme](https://msdn.microsoft.com/library/ff929188.aspx).
 
