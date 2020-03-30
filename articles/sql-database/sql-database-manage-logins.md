---
title: Oturum açma ve kullanıcı hesaplarını kullanarak sunucu ve veritabanı erişimini yetkilendirme
description: Azure SQL Veritabanı ve Azure Synapse Analytics'in oturum açma ve kullanıcı hesaplarını kullanarak kullanıcılara erişim için nasıl kimlik doğrulaması olduğunu öğrenin. Ayrıca, oturum açma ve kullanıcıları eylemleri ve sorgu verilerini gerçekleştirmeleri için yetkilendirmek için veritabanı rollerini ve açık izinleri nasıl gerçekleştireceklerini de öğrenin.
keywords: sql veritabanı güvenliği,veritabanı güvenliği yönetimi,oturum açma güvenliği,veritabanı güvenliği,veritabanı erişimi
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: VanMSFT
ms.author: vanto
ms.reviewer: carlrab
ms.date: 03/23/2020
ms.openlocfilehash: 98c15fe11b64e8c177e60a2ea1eb7c50eaf69353
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80124789"
---
# <a name="authorizing-database-access-to-authenticated-users-to-sql-database-and-azure-synapse-analytics-using-logins-and-user-accounts"></a>Oturum açma ve kullanıcı hesaplarını kullanarak kimlik doğrulaması verilen kullanıcılara SQL Veritabanı ve Azure Synapse Analytics veritabanı erişiminin yetkilendirilmesi

Bu makalede, hakkında bilgi edinin:

- Kullanıcıların yönetim görevlerini gerçekleştirmesini ve bu veritabanlarında depolanan verilere erişmesini sağlamak için Azure SQL Veritabanı ve Azure Synapse Analytics'i (eski adıyla Azure SQL Veri Ambarı) yapılandırma seçenekleri.
- Başlangıçta yeni bir Azure SQL Veritabanı oluşturduktan sonra erişim ve yetkilendirme yapılandırması
- Ana veritabanına ve kullanıcı hesaplarına giriş ve kullanıcı hesapları ekleme ve ardından bu hesaplara yönetim izinleri verme
- Oturum açma larla ilişkili veya içerdiği kullanıcı hesapları olarak kullanıcı veritabanlarına kullanıcı hesapları ekleme
- Veritabanı rollerini ve açık izinleri kullanarak kullanıcı veritabanlarında izinlerle kullanıcı hesaplarını yapılandırma

> [!IMPORTANT]
> Azure SQL Veritabanı ve Azure Synapse Analytics'teki (eski adıyla Azure SQL Veri Ambarı) veritabanları, bu makalenin geri kalanında toplu olarak veritabanları veya Azure SQL (basitlik için) olarak adlandırılır.

## <a name="authentication-and-authorization"></a>Kimlik doğrulama ve yetkilendirme

[**Kimlik doğrulama,**](sql-database-security-overview.md#authentication) kullanıcının iddia ettikleri kişi olduğunu kanıtlama işlemidir. Kullanıcı, bir kullanıcı hesabı kullanarak veritabanına bağlanır.
Bir kullanıcı veritabanına bağlanmaya çalıştığında, bir kullanıcı hesabı ve kimlik doğrulama bilgileri sağlar. Kullanıcı, aşağıdaki iki kimlik doğrulama yönteminden biri kullanılarak kimlik doğrulanır:

- [SQL kimlik doğrulaması.](https://docs.microsoft.com/sql/relational-databases/security/choose-an-authentication-mode#connecting-through-sql-server-authentication)

  Bu kimlik doğrulama yöntemiyle, kullanıcı bağlantı kurmak için bir kullanıcı hesabı adı ve ilişkili parola gönderir. Bu parola, girişe bağlı olan veya girişe bağlı olmayan kullanıcı hesaplarının kullanıcı hesabını içeren veritabanında depolanan kullanıcı hesapları için ana veritabanında depolanır.
- [Azure Active Directory Kimlik Doğrulaması](sql-database-aad-authentication.md)

  Bu kimlik doğrulama yöntemiyle, kullanıcı bir kullanıcı hesabı adı gönderir ve hizmetin Azure Etkin Dizini'nde depolanan kimlik bilgilerini kullanmasını ister.

**Oturum açmalar ve kullanıcılar**: Azure SQL'de, veritabanındaki bir kullanıcı hesabı ana veritabanında depolanan bir oturum açma yla ilişkilendirilebilir veya tek bir veritabanında depolanan bir kullanıcı adı olabilir.

- **Oturum açma,** bir veya daha fazla veritabanlarındaki bir kullanıcı hesabının bağlanabildiği ana veritabanındaki tek bir hesaptır. Giriş ile, kullanıcı hesabının kimlik bilgileri girişle birlikte depolanır.
- **Kullanıcı hesabı,** herhangi bir veritabanında oturum açma yla bağlantılı olabilecek ancak olması gerekmeyen tek bir hesaptır. Oturum açmayla bağlantılı olmayan bir kullanıcı hesabıyla, kimlik bilgileri kullanıcı hesabıyla birlikte depolanır.

Verilere erişim ve çeşitli eylemleri gerçekleştirme [**yetkisi**](sql-database-security-overview.md#authorization) veritabanı rolleri ve açık izinler kullanılarak yönetilir. Yetkilendirme, bir kullanıcıya atanan izinleri ifade eder ve bu kullanıcının ne yapmasına izin verilip verilmediğini belirler. Yetkilendirme, kullanıcı hesabınızın veritabanı [rol üyelikleri](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/database-level-roles) ve [nesne düzeyindeizinler](https://docs.microsoft.com/sql/relational-databases/security/permissions-database-engine)tarafından denetlenir. En iyi uygulama olarak, kullanıcılarınıza gerekli olan en düşük ayrıcalıkları tanımanız gerekir.

## <a name="existing-logins-and-user-accounts-after-creating-a-new-database"></a>Yeni bir veritabanı oluşturduktan sonra varolan oturum açmalar ve kullanıcı hesapları

İlk Azure SQL dağıtımınızı oluşturduğunuzda, bu giriş için bir yönetici girişi ve ilişkili bir parola belirtirsiniz. Bu yönetim hesabı **sunucu admin**denir. Ana ve kullanıcı veritabanlarındaki oturum açmaların ve kullanıcıların aşağıdaki yapılandırması dağıtım sırasında gerçekleşir:

- Belirttiğiniz giriş adı kullanılarak yönetim ayrıcalıklarına sahip bir SQL girişi oluşturulur. [Oturum açma,](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/principals-database-engine#sa-login) SQL Veritabanı'nda oturum açmak için ayrı bir kullanıcı hesabıdır.
- Bu giriş, [sunucu düzeyinde bir anapara](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/principals-database-engine)olarak tüm veritabanlarında tam yönetim izinleri verilir. Bu giriş, SQL Veritabanı'nda kullanılabilen tüm izinlere sahiptir ve sınırlandırılamaz. Yönetilen bir durumda, bu giriş [sysadmin sabit sunucu rolüne](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/server-level-roles) eklenir (bu rol tek veya havuzlu veritabanlarında bulunmaz).
- Her [kullanıcı](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/getting-started-with-database-engine-permissions#database-users) `dbo` veritabanında bu giriş için adverilen bir kullanıcı hesabı oluşturulur. [DBO](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/principals-database-engine) kullanıcısı veritabanındaki tüm veritabanı izinlerine sahiptir ve `db_owner` sabit veritabanı rolüne eşlenir. Ek sabit veritabanı rolleri bu makalede daha sonra ele alınmıştır.

Bir veritabanının yönetici hesaplarını tanımlamak için Azure portalını açın ve sunucunuzun veya yönetilen örneğinizin **Özellikler** sekmesine gidin.

![SQL Server Yöneticileri](media/sql-database-manage-logins/sql-admins.png)

![SQL Server Yöneticileri](media/sql-database-manage-logins/sql-admins2.png)

> [!IMPORTANT]
> Yönetici giriş adı oluşturulduktan sonra değiştirilemez. Mantıksal sunucu yöneticisinin parolasını sıfırlamak için [Azure portalına](https://portal.azure.com)gidin , **SQL Sunucuları'nı**tıklatın, listeden sunucuyu seçin ve ardından **Parolayı Sıfırla'yı**tıklatın. Yönetilen bir örnek sunucunun parolasını sıfırlamak için Azure portalına gidin, örneği tıklatın ve **parolayı sıfırla'yı**tıklatın. PowerShell'i veya Azure CLI'yi de kullanabilirsiniz.

## <a name="create-additional-logins-and-users-having-administrative-permissions"></a>Yönetim izinlerine sahip ek oturum açma lar ve kullanıcılar oluşturma

Bu noktada, Azure SQL örneğiniz yalnızca tek bir SQL girişi ve kullanıcı hesabı kullanılarak erişim için yapılandırılır. Tam veya kısmi yönetim izinleri içeren ek oturum açmalar oluşturmak için aşağıdaki seçeneklere sahipsiniz (dağıtım moduna bağlı olarak):

- **Tam yönetim izinlerine sahip bir Azure Active Directory yöneticisi hesabı oluşturma**

  Azure Etkin Dizin kimlik doğrulamasını etkinleştirin ve bir Azure AD yöneticisi girişi oluşturun. Bir Azure Active Directory hesabı, tam yönetim izinleriyle SQL Veritabanı dağıtımının yöneticisi olarak yapılandırılabilir. Bu hesap bir bireysel veya güvenlik grubu hesabı olabilir. SQL Veritabanı'na bağlanmak için Azure AD hesaplarını kullanmak istiyorsanız, bir Azure REKLAM yöneticisinin yapılandırılması **gerekir.** Tüm SQL Veritabanı dağıtım türleri için Azure AD kimlik doğrulamasını etkinleştirme hakkında ayrıntılı bilgi için aşağıdaki makalelere bakın:

  - [SQL ile kimlik doğrulama için Azure Active Directory Authentication'ı kullanma](sql-database-aad-authentication.md)
  - [Azure Active Directory kimlik doğrulamasını SQL ile yapılandırma ve yönetme](sql-database-aad-authentication-configure.md)

- **Yönetilen bir örnek dağıtımda, tam yönetim izinlerine sahip SQL girişleri oluşturun**

  - Yönetilen örnekte ek bir SQL Server girişi oluşturma
  - [ALTER SERVER ROLE](https://docs.microsoft.com/sql/t-sql/statements/alter-server-role-transact-sql) deyimini kullanarak [sysadmin sabit sunucu rolüne](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/server-level-roles) giriş ekleyin. Bu giriş tam yönetim izinlerine sahip olacaktır.
  - Alternatif olarak, <a href="/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current">CREATE Gİrİş</a> sözdizimini kullanarak bir [Azure AD girişi](sql-database-aad-authentication-configure.md?tabs=azure-powershell#new-azure-ad-admin-functionality-for-mi) oluşturun.

- **Tek veya havuzlu dağıtımda, sınırlı yönetim izinlerine sahip SQL girişleri oluşturun**

  - Tek veya havuza alınan veritabanı dağıtımı veya yönetilen örnek dağıtımı için ana veritabanında ek bir SQL girişi oluşturma
  - Bu yeni girişle ilişkili ana veritabanında bir kullanıcı hesabı oluşturma
  - [ALTER SERVER ROLE](https://docs.microsoft.com/sql/t-sql/statements/alter-server-role-transact-sql) deyimini `loginmanager` kullanarak kullanıcı `master` hesabını, `dbmanager`rolü veya veritabanındaki her ikisini de veritabanına ekleyin (Azure Synapse Analytics için [sp_addrolemember](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql) deyimini kullanın).

  > [!NOTE]
  > `dbmanager`ve `loginmanager` roller yönetilen örnek dağıtımları ile ilgili **değildir.**

  Tek veya havuzlu veritabanları için bu [özel ana veritabanı rollerinin](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/database-level-roles#special-roles-for--and-) üyeleri, kullanıcıların veritabanları oluşturma ve yönetme veya oturum açma ve yönetme yetkisine sahip olmasını sağlar. `dbmanager` Rolün bir üyesi olan bir kullanıcı tarafından oluşturulan veritabanlarında, üye `db_owner` sabit veritabanı rolüne eşlenir ve kullanıcı `dbo` hesabını kullanarak bu veritabanında oturum açabilir ve yönetebilir. Bu rollerin ana veritabanı dışında açık izinleri yoktur.

  > [!IMPORTANT]
  > Tek veya havuzlu bir veritabanında tam yönetim izinleri içeren ek bir SQL girişi oluşturamazsınız.

## <a name="create-accounts-for-non-administrator-users"></a>Yönetici olmayan kullanıcılar için hesap oluşturma

İki yöntemden birini kullanarak yönetim dışı kullanıcılar için hesaplar oluşturabilirsiniz:

- **Oturum oluşturur**

  Ana veritabanında bir SQL girişi oluşturun. Ardından, her veritabanında, kullanıcının erişmesi gereken bir kullanıcı hesabı oluşturun ve kullanıcı hesabını bu girişle ilişkilendirin. Bu yaklaşım, kullanıcının birden çok veritabanına erişmesi gerektiğinde ve parolaları eşitlenmiş tutmak istediğinizde tercih edilir. Ancak, giriş hem birincil sunucu da hem de ikincil sunucu(lar) üzerinde oluşturulması gerektiğinden, bu yaklaşım coğrafi çoğaltma ile kullanıldığında karmaşıklıklara sahiptir. Daha fazla bilgi için, coğrafi geri yükleme veya başarısız olmak [için Azure SQL Veritabanı güvenliğini yapılandırVe yönetin.](sql-database-geo-replication-security-config.md)
- **Kullanıcı hesabı oluşturma**

  Bir kullanıcının erişmesi gereken veritabanında [(dahil edilen kullanıcı](https://docs.microsoft.com/sql/relational-databases/security/contained-database-users-making-your-database-portable)olarak da adlandırılır) bir kullanıcı hesabı oluşturun.

  - Tek veya havuzlu bir veritabanıyla, bu tür bir kullanıcı hesabı her zaman oluşturabilirsiniz.
  - [Azure AD sunucu ilkelerini](sql-database-aad-authentication-configure.md?tabs=azure-powershell#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities)desteklemeyen yönetilen bir örnek veritabanıyla, yalnızca bu tür bir kullanıcı hesabını yalnızca bulunan bir [veritabanında](https://docs.microsoft.com/sql/relational-databases/databases/contained-databases)oluşturabilirsiniz. Yönetilen örnek, [Azure AD sunucu ilkelerini](sql-database-aad-authentication-configure.md?tabs=azure-powershell#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities)destekleyerek, veritabanı kullanıcılarının dahili bir veritabanı kullanıcısı olarak oluşturulmasına gerek kalmadan yönetilen örneğin kimliğini doğrulamak için kullanıcı hesapları oluşturabilirsiniz.

  Bu yaklaşımla, kullanıcı kimlik doğrulama bilgileri her veritabanında depolanır ve coğrafi olarak çoğaltılan veritabanlarına otomatik olarak çoğaltılır. Ancak, aynı hesap birden çok veritabanında varsa ve SQL Authentication kullanıyorsanız, parolaları el ile eşitlenmiş tutmanız gerekir. Ayrıca, bir kullanıcının farklı veritabanlarında farklı parolalara sahip bir hesabı varsa, bu parolaları hatırlamak sorun haline gelebilir.

> [!IMPORTANT]
> Azure AD kimliklerine eşlenen içerdiği kullanıcıları oluşturmak için, SQL Veritabanı'nda yönetici olan bir Azure AD hesabı nı kullanarak oturum açmış olmanız gerekir. Yönetilen örnekte, izinleri `sysadmin` olan bir SQL girişi de bir Azure AD girişi veya kullanıcı oluşturabilir.

Oturum açma ve kullanıcıların nasıl oluşturulabildiğini gösteren örnekler için bkz:

- [Tek veya havuzlu veritabanları için oturum açma oluşturma](https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-current#examples-1)
- [Yönetilen örnek veritabanı için oturum açma oluşturma](https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current#examples-2)
- [Azure Synapse Analytics veritabanı için oturum açma oluşturma](https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql?view=azure-sqldw-latest#examples-3)
- [Kullanıcı oluştur](https://docs.microsoft.com/sql/t-sql/statements/create-user-transact-sql#examples)
- [Azure AD oluşturma kullanıcıları içeriyordu](sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities)

> [!TIP]
> SQL Server'ı tek veya havuza edilmiş bir veritabanında bulunan bir kullanıcı oluşturmayı içeren bir güvenlik öğreticisi için [Bkz.](sql-database-security-tutorial.md)

## <a name="using-fixed-and-custom-database-roles"></a>Sabit ve özel veritabanı rollerini kullanma

Bir veritabanında, oturum açma ya da içerdiği kullanıcı olarak bir kullanıcı hesabı oluşturduktan sonra, bu kullanıcıya çeşitli eylemler gerçekleştirmesi ve belirli bir veritabanındaki verilere erişmesi için yetki verebilirsiniz. Erişimi yetkilendirmek için aşağıdaki yöntemleri kullanabilirsiniz:

- **Veritabanı rolleri düzeltildi**

  Kullanıcı hesabını sabit bir [veritabanı rolüne](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/database-level-roles)ekleyin. Her biri tanımlı izin kümesine sahip 9 sabit veritabanı rolü vardır. En yaygın sabit veritabanı rolleri şunlardır: **db_owner**, **db_ddladmin**, **db_datawriter**, **db_datareader**, **db_denydatawriter**, ve **db_denydatareader**. Birkaç kullanıcıya tam izin vermek için genelde **db_owner** kullanılır. Diğer sabit veritabanı rolleri, geliştirme aşamasında basit bir veritabanını hızlı bir şekilde kullanıma almak için kullanışlıdır ancak çoğu üretim veritabanı için önerilmez. Örneğin, **db_datareader** sabit veritabanı rolü, veritabanındaki her tabloya okuma erişimi sağlar ve bu da kesinlikle gerekli olandan daha fazladır.

  - Kullanıcıyı sabit bir veritabanı rolüne eklemek için:

    - Azure SQL Veritabanı'nda [ALTER ROLE](https://docs.microsoft.com/sql/t-sql/statements/alter-role-transact-sql) deyimini kullanın. Örnekler için [ALTER ROLE örneklerine](https://docs.microsoft.com/sql/t-sql/statements/alter-role-transact-sql#examples) bakın
    - Azure Synapse Analytics, [sp_addrolemember](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql) ifadesini kullanın. Örnekler için sp_addrolemember [örneklere](https://docs.microsoft.com/sql/t-sql/statements/alter-role-transact-sql)bakın.

- **Özel veritabanı rolü**

  CREATE ROLE deyimini kullanarak özel bir veritabanı rolü [oluşturun.](https://docs.microsoft.com/sql/t-sql/statements/create-role-transact-sql) Özel bir rol, kendi kullanıcı tanımlı veritabanı rollerinizi oluşturmanıza ve her role iş gereksinimi için gereken en az izinleri dikkatle vermenize olanak tanır. Daha sonra kullanıcıları özel role ekleyebilirsiniz. Birden fazla rolün üyesi olan kullanıcılar, tüm rollerin izinlerine sahip olur.
- **İzinleri doğrudan verme**

  Kullanıcı hesabı [izinlerini](https://docs.microsoft.com/sql/relational-databases/security/permissions-database-engine) doğrudan ver. SQL Veritabanında ayrı ayrı verilebilen veya reddedilebilen 100'den fazla izin vardır. Bu izinlerin çoğu iç içe geçmiş haldedir. Örneğin, bir şemada için verilen `UPDATE` izni, o şema içindeki tüm tablolar için `UPDATE` iznini de içerir. Çoğu izin sisteminde olduğu gibi bir iznin reddedilmesi, aynı iznin verilme durumunu geçersiz kılar. İç içe geçmiş yapısı ve izin sayısı nedeniyle, veritabanınızı doğru şekilde korumak için uygun bir izin sistemi tasarlamak uzun ve dikkatli bir çalışma gerektirebilir. [İzinler (Veritabanı Altyapısı)](https://docs.microsoft.com/sql/relational-databases/security/permissions-database-engine) ile başlayın ve izinlerin [poster boyutundaki tablosunu](https://docs.microsoft.com/sql/relational-databases/security/media/database-engine-permissions.png) inceleyin.

## <a name="using-groups"></a>Grupları kullanma

Verimli erişim yönetimi, tek tek kullanıcılar yerine Etkin Dizin güvenlik gruplarına atanan izinleri ve sabit veya özel rolleri kullanır.

- Azure Active Directory kimlik doğrulamasını kullanırken, Azure Etkin Dizin kullanıcılarının azure etkin dizin güvenlik grubuna yerleştirin. Grup için bir bağımsız veritabanı kullanıcısı oluşturun. Bir veya daha fazla veritabanı kullanıcısını, bu kullanıcı grubuna uygun belirli izinlerle özel bir veritabanı rolüne yerleştirin.

- SQL kimlik doğrulaması kullanırken, veritabanında bulunan veritabanı kullanıcıları oluşturun. Bir veya daha fazla veritabanı kullanıcısını, bu kullanıcı grubuna uygun belirli izinlerle özel bir veritabanı rolüne yerleştirin.

  > [!NOTE]
  > İçermeyen veritabanı kullanıcıları için grupları da kullanabilirsiniz.

İzinleri sınırlamak veya yükseltmek için kullanılabilen aşağıdaki özellikleri tanımanız gerekir:

- [Kimliğe Bürünme](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/customizing-permissions-with-impersonation-in-sql-server) ve [modül imzalama](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/signing-stored-procedures-in-sql-server) ile izinler geçici olarak ve güvenli bir şekilde artırılabilir.
- [Satır Düzeyinde Güvenlik](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) ile bir kullanıcının erişebileceği satırlar sınırlandırılabilir.
- [Veri Maskeleme](sql-database-dynamic-data-masking-get-started.md) ile hassas verilerin kapsamı sınırlandırılabilir.
- [Saklı yordamlar](https://docs.microsoft.com/sql/relational-databases/stored-procedures/stored-procedures-database-engine) ile veritabanında gerçekleştirilebilecek eylemler sınırlandırılabilir.

## <a name="next-steps"></a>Sonraki adımlar

SQL Veritabanı güvenlik özelliklerinin tümüne genel bakış için bkz. [SQL Veritabanı güvenliğine genel bakış](sql-database-security-overview.md).
