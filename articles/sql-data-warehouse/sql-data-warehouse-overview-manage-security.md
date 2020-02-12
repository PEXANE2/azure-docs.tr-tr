---
title: Veritabanını güvenli hale getirme
description: SQL Analytics 'in SQL havuzu kaynağında bir veritabanını güvenli hale getirme ve çözüm geliştirme ipuçları.
services: sql-data-warehouse
author: julieMSFT
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: security
ms.date: 04/17/2018
ms.author: jrasnick
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 26cdbb1fc2899d1b03fea6199074467623706c63
ms.sourcegitcommit: 812bc3c318f513cefc5b767de8754a6da888befc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77153290"
---
# <a name="secure-a-database-in-sql-data-warehouse"></a>SQL veri ambarı 'nda bir veritabanının güvenliğini sağlama
> [!div class="op_single_selector"]
> * [Güvenliğe genel bakış](sql-data-warehouse-overview-manage-security.md)
> * [Kimlik doğrulaması](sql-data-warehouse-authentication.md)
> * [Şifreleme (portal)](sql-data-warehouse-encryption-tde.md)
> * [Şifreleme (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

Bu makalede, SQL Analytics 'te SQL havuzunuzu güvenli hale getirmenin temelleri adım adım açıklanmaktadır. Özellikle bu makale, SQL havuzu kullanılarak sağlanan bir veritabanında erişimi sınırlandırma, verileri koruma ve etkinlikleri izlemeye yönelik kaynakları kullanmaya başlamanızı temin ediyor.

## <a name="connection-security"></a>Bağlantı güvenliği
Bağlantı Güvenliği, veritabanı bağlantılarını güvenlik duvarı kuralları ve bağlantı şifrelemesi kullanarak kısıtlamayı ve bu bağlantıların güvenliğini sağlamayı kapsar.

Güvenlik duvarı kuralları, açıkça bir şekilde listelenmiş olmayan IP adreslerinden gelen bağlantı girişimlerini reddetmek için hem sunucu hem de veritabanı tarafından kullanılır. Uygulamanızın veya istemci makinenizin ortak IP adresinden gelen bağlantılara izin vermek için, önce Azure portal, REST API veya PowerShell 'i kullanarak sunucu düzeyinde bir güvenlik duvarı kuralı oluşturmanız gerekir. 

En iyi uygulama olarak sunucu güvenlik duvarınızdan geçmesine izin verilen IP adresi aralıklarını mümkün olduğunca sınırlı tutmanız gerekir.  Yerel bilgisayarınızdan SQL havuzuna erişmek için, ağınızdaki ve yerel bilgisayarınızdaki güvenlik duvarının TCP bağlantı noktası 1433 ' de giden iletişime izin verdiğinden emin olun.  

Azure SYNAPSE Analytics, sunucu düzeyinde IP güvenlik duvarı kuralları kullanır. Veritabanı düzeyinde IP güvenlik duvarı kurallarını desteklemez. Daha fazla bilgi için bkz. [Azure SQL veritabanı güvenlik duvarı kuralları](../sql-database/sql-database-firewall-configure.md)

SQL havuzunuza bağlantılar varsayılan olarak şifrelenir.  Şifrelemeyi devre dışı bırakmak için bağlantı ayarlarını değiştirme yok sayılır.

## <a name="authentication"></a>Kimlik Doğrulaması
Kimlik doğrulaması, veritabanına bağlanırken kimliğinizi nasıl kanıtlayacağınızı belirtir. SQL havuzu şu anda Kullanıcı adı ve parolayla SQL Server kimlik doğrulamasını destekler ve Azure Active Directory. 

Veritabanınıza ait mantıksal sunucuyu oluşturduktan sonra kullanıcı adı ve parola belirleyerek "sunucu yöneticisi" oturum açma bilgisi oluşturdunuz. Bu kimlik bilgilerini kullanarak, bu sunucuda veritabanı sahibi olarak veya SQL Server kimlik doğrulaması aracılığıyla "dbo" olarak herhangi bir veritabanında kimlik doğrulaması yapabilirsiniz.

Ancak, en iyi uygulama olarak, kuruluşunuzun kullanıcıları kimlik doğrulaması için farklı bir hesap kullanmalıdır. Bu şekilde, uygulamaya verilen izinleri sınırlayabilir ve uygulama kodunuzun bir SQL ekleme saldırısından etkilenmesi durumunda kötü amaçlı etkinliğin riskleri azaltabilirsiniz. 

SQL Server kimliği doğrulanmış bir kullanıcı oluşturmak için Sunucu Yöneticisi oturum açma bilgileriyle sunucunuzdaki **ana** veritabanına bağlanın ve yeni bir sunucu oturumu oluşturun.  Ana veritabanı Azure SYNAPSE kullanıcıları 'nda da bir kullanıcı oluşturmak iyi bir fikirdir. Ana öğe içinde Kullanıcı oluşturmak, bir kullanıcının bir veritabanı adı belirtmeden SSMS gibi araçları kullanarak oturum açmasına olanak tanır.  Ayrıca, bir SQL Server üzerindeki tüm veritabanlarını görüntülemek için Nesne Gezgini 'ni kullanmalarına izin verir.

```sql
-- Connect to master database and create a login
CREATE LOGIN ApplicationLogin WITH PASSWORD = 'Str0ng_password';
CREATE USER ApplicationUser FOR LOGIN ApplicationLogin;
```

Ardından, Sunucu Yöneticisi oturum açma bilgilerinizi kullanarak **SQL havuzu veritabanınıza** bağlanın ve oluşturduğunuz sunucu oturumuna göre bir veritabanı kullanıcısı oluşturun.

```sql
-- Connect to SQL DW database and create a database user
CREATE USER ApplicationUser FOR LOGIN ApplicationLogin;
```

Bir kullanıcıya oturum açma veya yeni veritabanları oluşturma gibi ek işlemler gerçekleştirme izni vermek için, kullanıcıyı ana veritabanındaki `Loginmanager` ve `dbmanager` rollere atayın. 

Bu ek roller ve bir SQL veritabanında kimlik doğrulama hakkında daha fazla bilgi için bkz. [Azure SQL veritabanı 'nda veritabanlarını ve oturum açma Işlemlerini yönetme](../sql-database/sql-database-manage-logins.md).  Azure Active Directory kullanarak bağlanma hakkında daha fazla bilgi için bkz. [Azure Active Directory kimlik doğrulaması kullanarak bağlanma](sql-data-warehouse-authentication.md).

## <a name="authorization"></a>Yetkilendirme
Yetkilendirme, kimlik doğrulamasından ve bağlandıktan sonra bir veritabanı içinde yapabileceklerinizi ifade eder. Yetkilendirme ayrıcalıkları, rol üyelikleri ve izinleri tarafından belirlenir. En iyi uygulama olarak, kullanıcılarınıza gerekli olan en düşük ayrıcalıkları tanımanız gerekir. Rolleri yönetmek için aşağıdaki saklı yordamları kullanabilirsiniz:

```sql
EXEC sp_addrolemember 'db_datareader', 'ApplicationUser'; -- allows ApplicationUser to read data
EXEC sp_addrolemember 'db_datawriter', 'ApplicationUser'; -- allows ApplicationUser to write data
```

Bağlantı kurmak için kullandığınız sunucu yöneticisi hesabı, veritabanında tüm işlemleri gerçekleştirme yetkisi olan db_owner rolünün üyesidir. Bu hesabı şema yükseltmeleri ve diğer yönetimsel işlemlerde kullanmak üzere saklayın. Uygulamanızdan veritabanına, uygulamanızın ihtiyaç duyduğu en düşük ayrıcalıklarla bağlanmak için daha sınırlı izinlere sahip olan "ApplicationUser" hesabını kullanın.

Bir kullanıcının veri ambarı içinde neler yapabileceğini daha fazla sınırlandırmak için yollar vardır:

* Ayrıntılı [izinler](https://docs.microsoft.com/sql/relational-databases/security/permissions-database-engine?view=sql-server-ver15) , veritabanındaki tek tek sütunlarda, tablolarda, görünümlerde, şemalarda, yordamlarda ve diğer nesnelerde hangi işlemleri yapabilmeniz gerektiğini denetlemenize olanak tanır. En fazla denetime sahip olmak ve gereken en düşük izinleri vermek için ayrıntılı izinler kullanın. 
* Db_datareader ve db_datawriter dışındaki [veritabanı rolleri](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/database-level-roles?view=sql-server-ver15) , daha güçlü uygulama kullanıcı hesapları veya daha az güçlü yönetim hesapları oluşturmak için kullanılabilir. Yerleşik sabit veritabanı rolleri, izin vermek için kolay bir yol sağlar, ancak gerekenden daha fazla izin verilmesine neden olabilir.
* [Saklı yordamlar](https://docs.microsoft.com/sql/relational-databases/stored-procedures/stored-procedures-database-engine?redirectedfrom=MSDN&view=sql-server-ver15) ile veritabanında gerçekleştirilebilecek eylemler sınırlandırılabilir.

Aşağıdaki örnek, Kullanıcı tanımlı bir şemaya okuma erişimi verir.
```sql
--CREATE SCHEMA Test
GRANT SELECT ON SCHEMA::Test to ApplicationUser
```

Veritabanlarını ve mantıksal sunucuları Azure portal yönetme veya Azure Resource Manager API 'sini kullanma, Portal Kullanıcı hesabınızın rol atamalarınız tarafından denetlenir. Daha fazla bilgi için, bkz. [Azure Portal rol tabanlı erişim denetimi](https://azure.microsoft.com/documentation/articles/role-based-access-control-configure).

## <a name="encryption"></a>Şifreleme
Saydam Veri Şifrelemesi (TDE), bekleyen verilerinizi şifreleyerek ve şifresini çözerek kötü amaçlı etkinlik tehditlerine karşı korunmaya yardımcı olur. Veritabanınızı şifrelerken, ilişkili yedeklemeler ve işlem günlük dosyaları, uygulamalarınızda herhangi bir değişiklik yapılmadan şifrelenir. TDE, veritabanı şifreleme anahtarı adlı bir simetrik anahtar kullanarak veritabanının tamamının depolanmasını şifreler. 

SQL veritabanında, veritabanı şifreleme anahtarı yerleşik bir sunucu sertifikası tarafından korunur. Yerleşik sunucu sertifikası her SQL veritabanı sunucusu için benzersizdir. Microsoft bu sertifikaları en az 90 günde bir otomatik olarak döndürür. Kullanılan şifreleme algoritması AES-256 ' dir. TDE genel bir açıklaması için bkz. [Saydam veri şifrelemesi](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-ver15).

[Azure Portal](sql-data-warehouse-encryption-tde.md) veya [T-SQL](sql-data-warehouse-encryption-tde-tsql.md)' i kullanarak veritabanınızı şifreleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Farklı protokollerle ambarınıza bağlanma hakkındaki ayrıntılar ve örnekler için bkz. [SQL Pool 'A bağlanma](sql-data-warehouse-connect-overview.md).
