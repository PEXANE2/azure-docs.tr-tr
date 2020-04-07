---
title: Veritabanını güvenli hale
description: Synapse SQL havuz kaynağında veritabanını güvence altına almak ve çözümler geliştirmek için ipuçları.
author: julieMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/17/2018
ms.author: jrasnick
ms.reviewer: igorstan
ms.custom: seo-lt-2019
tags: azure-synapse
ms.openlocfilehash: 0c30294f2ca139a602074a980810e7c6737c4e2d
ms.sourcegitcommit: bd5fee5c56f2cbe74aa8569a1a5bce12a3b3efa6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80742993"
---
# <a name="secure-a-database-in-azure-synapse"></a>Azure Synapse'de veritabanını güvenli hale

> [!div class="op_single_selector"]
>
> * [Güvenliğe Genel Bakış](sql-data-warehouse-overview-manage-security.md)
> * [Kimlik Doğrulaması](sql-data-warehouse-authentication.md)
> * [Şifreleme (Portal)](sql-data-warehouse-encryption-tde.md)
> * [Şifreleme (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)

Bu makalede, Synapse SQL havuzu güvenliğini temelleri size yol verecektir. Özellikle, bu makalede, erişim sınırlama, verileri koruma ve SQL havuzu kullanılarak sağlanan bir veritabanında etkinlikleri izleme kaynakları ile başlar.

## <a name="connection-security"></a>Bağlantı güvenliği

Bağlantı Güvenliği, veritabanı bağlantılarını güvenlik duvarı kuralları ve bağlantı şifrelemesi kullanarak kısıtlamayı ve bu bağlantıların güvenliğini sağlamayı kapsar.

Güvenlik duvarı kuralları, açıkça beyaz listeye alınmamış IP adreslerinden gelen bağlantı girişimlerini reddetmek için hem sunucu hem de veritabanı tarafından kullanılır. Uygulamanızın veya istemci makinenizin genel IP adresinden bağlantılara izin vermek için, öncelikle Azure portalı, REST API veya PowerShell'i kullanarak sunucu düzeyinde bir güvenlik duvarı kuralı oluşturmanız gerekir.

En iyi uygulama olarak sunucu güvenlik duvarınızdan geçmesine izin verilen IP adresi aralıklarını mümkün olduğunca sınırlı tutmanız gerekir.  Yerel bilgisayarınızdan SQL havuzuna erişmek için, ağınızdaki güvenlik duvarının ve yerel bilgisayarın TCP bağlantı noktası 1433'te giden iletişime izin verdiğinden emin olun.  

Azure Synapse Analytics sunucu düzeyinde IP güvenlik duvarı kurallarını kullanır. Veritabanı düzeyindeIP güvenlik duvarı kurallarını desteklemez. Daha fazla bilgi için [bkz.](../../sql-database/sql-database-firewall-configure.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)

SQL havuzunuza bağlantılar varsayılan olarak şifrelenir.  Şifrelemeyi devre dışı kakacak bağlantı ayarlarını değiştirmek göz ardı edilir.

## <a name="authentication"></a>Kimlik Doğrulaması

Kimlik doğrulaması, veritabanına bağlanırken kimliğinizi nasıl kanıtlayacağınızı belirtir. SQL havuzu şu anda bir kullanıcı adı ve parola yla ve Azure Active Directory ile SQL Server Authentication'ı destekler.

Veritabanınıza ait mantıksal sunucuyu oluşturduktan sonra kullanıcı adı ve parola belirleyerek "sunucu yöneticisi" oturum açma bilgisi oluşturdunuz. Bu kimlik bilgilerini kullanarak, veritabanı sahibi olarak o sunucudaki herhangi bir veritabanına veya SQL Server Authentication aracılığıyla "dbo" kimlik doğrulaması yapabilirsiniz.

Ancak, en iyi yöntem olarak, kuruluşunuzun kullanıcıları kimlik doğrulaması yapmak için farklı bir hesap kullanmalıdır. Bu şekilde, uygulamaya verilen izinleri sınırlayabilir ve uygulama kodunuz bir SQL enjeksiyon saldırısına karşı savunmasız durumda kötü amaçlı etkinlik risklerini azaltabilirsiniz.

BIR SQL Server Authenticated kullanıcısı oluşturmak için sunucu yönetici girişi ile sunucunuzdaki **ana** veritabanına bağlanın ve yeni bir sunucu girişi oluşturun.  Ana veritabanında da bir kullanıcı oluşturmak iyi bir fikirdir. Ana bir kullanıcı oluşturmak, bir veritabanı adı belirtmeden kullanıcının SSMS gibi araçları kullanarak oturum açmasına olanak tanır.  Ayrıca, bir SQL sunucusundaki tüm veritabanlarını görüntülemek için nesne gezginini kullanmalarına da olanak tanır.

```sql
-- Connect to master database and create a login
CREATE LOGIN ApplicationLogin WITH PASSWORD = 'Str0ng_password';
CREATE USER ApplicationUser FOR LOGIN ApplicationLogin;
```

Ardından, sunucu yönetici girişi ile **SQL havuz veritabanınıza** bağlanın ve oluşturduğunuz sunucu girişine dayalı bir veritabanı kullanıcısı oluşturun.

```sql
-- Connect to the database and create a database user
CREATE USER ApplicationUser FOR LOGIN ApplicationLogin;
```

Kullanıcıya oturum açma veya yeni veritabanları oluşturma gibi ek işlemler gerçekleştirme izni vermek `Loginmanager` için, kullanıcıyı ana veritabanındaki ve `dbmanager` rollere atayın.

Bu ek roller ve bir SQL Veritabanına kimlik doğrulama hakkında daha fazla bilgi için Azure [SQL Veritabanı'nda veritabanlarını ve girişleri yönetme'ye](../../sql-database/sql-database-manage-logins.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)bakın.  Azure Etkin Dizin'i kullanarak bağlanma hakkında daha fazla bilgi için Azure [Etkin Dizin Kimlik Doğrulaması'nı kullanarak Bağlanma'ya](sql-data-warehouse-authentication.md)bakın.

## <a name="authorization"></a>Yetkilendirme

Yetkilendirme, kimlik doğrulaması alındıktan ve bağlandıktan sonra veritabanı içinde neler yapabileceğinizi ifade eder. Yetkilendirme ayrıcalıkları rol üyelikleri ve izinlerle belirlenir. En iyi uygulama olarak, kullanıcılarınıza gerekli olan en düşük ayrıcalıkları tanımanız gerekir. Rolleri yönetmek için aşağıdaki saklı yordamları kullanabilirsiniz:

```sql
EXEC sp_addrolemember 'db_datareader', 'ApplicationUser'; -- allows ApplicationUser to read data
EXEC sp_addrolemember 'db_datawriter', 'ApplicationUser'; -- allows ApplicationUser to write data
```

Bağlantı kurmak için kullandığınız sunucu yöneticisi hesabı, veritabanında tüm işlemleri gerçekleştirme yetkisi olan db_owner rolünün üyesidir. Bu hesabı şema yükseltmeleri ve diğer yönetimsel işlemlerde kullanmak üzere saklayın. Uygulamanızdan veritabanına, uygulamanızın ihtiyaç duyduğu en düşük ayrıcalıklarla bağlanmak için daha sınırlı izinlere sahip olan "ApplicationUser" hesabını kullanın.

Bir kullanıcının veritabanı içinde yapabileceklerini daha da sınırlamanın yolları vardır:

* Parçalı [İzinler,](/sql/relational-databases/security/permissions-database-engine?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) veritabanındaki tek tek sütunlar, tablolar, görünümler, şemalar, yordamlar ve diğer nesneler üzerinde hangi işlemleri yapabileceğinizi denetlemenize izin verir. En fazla denetime sahip olmak ve gerekli minimum izinleri vermek için parçalı izinleri kullanın.
* db_datareader ve db_datawriter dışındaki [veritabanı rolleri,](/sql/relational-databases/security/authentication-access/database-level-roles?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) daha güçlü uygulama kullanıcı hesapları veya daha az güçlü yönetim hesapları oluşturmak için kullanılabilir. Yerleşik sabit veritabanı rolleri izin vermek için kolay bir yol sağlar, ancak gerekenden daha fazla izin verilmesine neden olabilir.
* [Saklı yordamlar](/sql/relational-databases/stored-procedures/stored-procedures-database-engine?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) ile veritabanında gerçekleştirilebilecek eylemler sınırlandırılabilir.

Aşağıdaki örnek, kullanıcı tanımlı bir şemaya okuma erişimi sağlar.

```sql
--CREATE SCHEMA Test
GRANT SELECT ON SCHEMA::Test to ApplicationUser
```

Azure portalından veritabanlarını ve mantıksal sunucuları yönetmek veya Azure Kaynak Yöneticisi API'sini kullanmak portal kullanıcı hesabınızın rol atamaları tarafından denetlenir. Daha fazla bilgi için [Azure portalında Rol tabanlı erişim denetimine](../../role-based-access-control/role-assignments-portal.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)bakın.

## <a name="encryption"></a>Şifreleme

Saydam Veri Şifreleme (TDE), verilerinizi şifreleyerek ve şifresini çözerek kötü amaçlı etkinlik tehdidine karşı korumaya yardımcı olur. Veritabanınızı şifrelediğinizde, ilişkili yedeklemeler ve işlem günlüğü dosyaları uygulamalarınızda herhangi bir değişiklik gerektirmeden şifrelenir. TDE, veritabanı şifreleme anahtarı olarak adlandırılan bir simetrik anahtarı kullanarak veritabanı depolama alanının tamamını şifreler.

SQL Veritabanı'nda veritabanı şifreleme anahtarı yerleşik bir sunucu sertifikası tarafından korunur. Yerleşik sunucu sertifikası her SQL Veritabanı sunucusu için benzersizdir. Microsoft bu sertifikaları en az 90 günde bir otomatik olarak döndürür. Kullanılan şifreleme algoritması AES-256. TDE'nin genel açıklaması için [Bkz. Saydam Veri Şifreleme.](/sql/relational-databases/security/encryption/transparent-data-encryption?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)

[Azure portalını](sql-data-warehouse-encryption-tde.md) veya [T-SQL'i](sql-data-warehouse-encryption-tde-tsql.md)kullanarak veritabanınızı şifreleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Ambarınıza farklı protokollerle bağlanmayla ilgili ayrıntılar ve örnekler için [bkz.](sql-data-warehouse-connect-overview.md)
