---
title: Oturum açma bilgilerini ve Kullanıcı hesaplarını kullanarak sunucu ve veritabanı erişimini yetkilendir
description: Azure SQL veritabanı ve Azure SYNAPSE Analytics 'in, oturum açma bilgilerini ve Kullanıcı hesaplarını kullanarak erişim için kimlik doğrulama hakkında bilgi edinin. Ayrıca, oturum açma bilgileri ve kullanıcıların eylemleri gerçekleştirmesini ve sorgu verilerini yetkilendirmek için veritabanı rollerini ve açık izinleri nasıl kullanacağınızı öğrenin.
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
ms.openlocfilehash: 0f1611e6d3524cc78fc20fed9d1aac6f3fd453fa
ms.sourcegitcommit: 354a302d67a499c36c11cca99cce79a257fe44b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82106449"
---
# <a name="authorizing-database-access-to-authenticated-users-to-sql-database-and-azure-synapse-analytics-using-logins-and-user-accounts"></a>Oturum açma ve Kullanıcı hesaplarını kullanarak SQL veritabanı ve Azure SYNAPSE Analytics 'e veritabanı erişimi verme

Bu makalede şunları öğreneceksiniz:

- Kullanıcıların yönetim görevleri gerçekleştirmesini ve bu veritabanlarında depolanan verilere erişimini sağlamak için Azure SQL veritabanı ve Azure SYNAPSE Analytics (eskiden Azure SQL veri ambarı) yapılandırma seçenekleri.
- İlk olarak yeni bir Azure SQL veritabanı oluşturulduktan sonra erişim ve yetkilendirme yapılandırması
- Ana veritabanına ve Kullanıcı hesaplarına oturum açma bilgileri ve Kullanıcı hesapları ekleme ve ardından bu hesaplara yönetici izinleri verme
- Kullanıcı veritabanlarına, oturum açmalar veya dahil edilen kullanıcı hesaplarıyla ilişkili kullanıcı hesapları ekleme
- Veritabanı rollerini ve açık izinleri kullanarak Kullanıcı veritabanlarında izinleri olan kullanıcı hesaplarını yapılandırma

> [!IMPORTANT]
> Azure SQL veritabanı ve Azure SYNAPSE Analytics (eski adıyla Azure SQL veri ambarı) veritabanları, bu makalenin geri kalanında veritabanları veya Azure SQL (basitlik için) olarak adlandırılır.

## <a name="authentication-and-authorization"></a>Kimlik doğrulaması ve yetkilendirme

[**Kimlik doğrulama**](sql-database-security-overview.md#authentication) , kullanıcının talep ettikleri kim olduğunu kanıtlama işlemidir. Kullanıcı bir kullanıcı hesabı kullanarak bir veritabanına bağlanır.
Bir Kullanıcı bir veritabanına bağlanmaya çalıştığında, bir kullanıcı hesabı ve kimlik doğrulama bilgileri sağlarlar. Kullanıcının kimliği, aşağıdaki iki kimlik doğrulama yönteminden birini kullanarak doğrulanır:

- [SQL kimlik doğrulaması](https://docs.microsoft.com/sql/relational-databases/security/choose-an-authentication-mode#connecting-through-sql-server-authentication).

  Bu kimlik doğrulama yöntemiyle kullanıcı bir bağlantı kurmak için bir kullanıcı hesabı adı ve ilişkili parola gönderir. Bu parola, bir oturum açma ile bağlantılı veya bir oturum açma ile bağlantılı *olmayan* Kullanıcı hesaplarını içeren veritabanında depolanan kullanıcı hesapları için ana veritabanında depolanır.
- [Azure Active Directory Kimlik Doğrulaması](sql-database-aad-authentication.md)

  Bu kimlik doğrulama yöntemiyle kullanıcı, bir kullanıcı hesabı adı ve hizmetin Azure Active Directory depolanan kimlik bilgisi bilgilerini kullandığı istekleri gönderir.

**Oturum açmalar ve kullanıcılar**: Azure SQL 'de, veritabanındaki bir kullanıcı hesabı, ana veritabanında depolanan bir oturum ile ilişkilendirilebilir veya ayrı bir veritabanında depolanan bir Kullanıcı adı olabilir.

- **Oturum açma** , ana veritabanında bir veya daha fazla veritabanında bulunan bir kullanıcı hesabının bağlanacağı bireysel bir hesaptır. Oturum açma ile, Kullanıcı hesabı için kimlik bilgisi bilgileri oturum açmayla birlikte depolanır.
- **Kullanıcı hesabı** , herhangi bir veritabanında bulunan ancak bir oturum açma ile bağlantılı olması gereken tek bir hesaptır. Bir oturum açmayla bağlantılı olmayan bir kullanıcı hesabı ile, kimlik bilgisi bilgileri kullanıcı hesabıyla birlikte depolanır.

Veri erişimi ve çeşitli eylemleri gerçekleştirme [**yetkilendirmesi**](sql-database-security-overview.md#authorization) , veritabanı rolleri ve açık izinler kullanılarak yönetilir. Yetkilendirme, bir kullanıcıya atanan izinleri ifade eder ve kullanıcının ne yapmasına izin verileceğini belirler. Yetkilendirme, Kullanıcı hesabınızın veritabanı [rolü üyelikleri](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/database-level-roles) ve [nesne düzeyi izinleri](https://docs.microsoft.com/sql/relational-databases/security/permissions-database-engine)tarafından denetlenir. En iyi uygulama olarak, kullanıcılarınıza gerekli olan en düşük ayrıcalıkları tanımanız gerekir.

## <a name="existing-logins-and-user-accounts-after-creating-a-new-database"></a>Yeni bir veritabanı oluşturduktan sonra mevcut oturumlar ve Kullanıcı hesapları

İlk Azure SQL dağıtımınızı oluşturduğunuzda, bu oturum açma için yönetici oturumu ve ilişkili bir parola belirtirsiniz. Bu yönetim hesabına **Sunucu Yöneticisi**adı verilir. Dağıtım sırasında, ana ve Kullanıcı veritabanlarındaki oturum açma ve kullanıcıların aşağıdaki yapılandırması oluşur:

- Yönetim ayrıcalıklarına sahip bir SQL oturumu açma, belirttiğiniz oturum açma adı kullanılarak oluşturulur. [Oturum açma](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/principals-database-engine#sa-login) , SQL veritabanı 'nda oturum açmak için bireysel bir kullanıcı hesaplarıdır.
- Bu oturum açma, tüm veritabanlarında [sunucu düzeyi sorumlusu](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/principals-database-engine)olarak tam yönetim izinleri verilmesini ister. Bu oturum açma, SQL veritabanı 'nda kullanılabilir tüm izinlere sahip ve bunlarla sınırlı olamaz. Yönetilen bir örnekte, bu oturum açma [sysadmin sabit sunucu rolüne](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/server-level-roles) eklenir (Bu rol, tek veya havuza alınmış veritabanlarında bulunmaz).
- Bu oturum için her `dbo` Kullanıcı veritabanında çağrılan bir [Kullanıcı hesabı](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/getting-started-with-database-engine-permissions#database-users) oluşturulur. [Dbo](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/principals-database-engine) kullanıcısı veritabanında tüm veritabanı izinlerine sahiptir ve `db_owner` sabit veritabanı rolüne eşlenir. Ek sabit veritabanı rolleri bu makalenin ilerleyen kısımlarında ele alınmıştır.

Bir veritabanının yönetici hesaplarını belirlemek için, Azure portal açın ve sunucunuzun veya yönetilen örneğinizin **Özellikler** sekmesine gidin.

![SQL Server Yöneticileri](media/sql-database-manage-logins/sql-admins.png)

![SQL Server Yöneticileri](media/sql-database-manage-logins/sql-admins2.png)

> [!IMPORTANT]
> Yönetici oturum açma adı oluşturulduktan sonra değiştirilemez. Mantıksal sunucu yöneticisinin parolasını sıfırlamak için [Azure Portal](https://portal.azure.com)gidin, **SQL sunucuları**' na tıklayın, listeden sunucuyu seçin ve ardından **Parolayı Sıfırla**' ya tıklayın. Yönetilen bir örnek sunucusunun parolasını sıfırlamak için Azure portal gidin, örneğe tıklayın ve **Parolayı Sıfırla**' ya tıklayın. PowerShell veya Azure CLı 'yi de kullanabilirsiniz.

## <a name="create-additional-logins-and-users-having-administrative-permissions"></a>Yönetici izinlerine sahip ek oturumlar ve kullanıcılar oluşturun

Bu noktada, Azure SQL örneğiniz yalnızca tek bir SQL oturumu ve Kullanıcı hesabı kullanılarak erişim için yapılandırılır. Tam veya kısmi yönetim izinleriyle ek oturumlar oluşturmak için aşağıdaki seçeneklere (dağıtım modunuza bağlı olarak) sahipsiniz:

- **Tam yönetim izinlerine sahip bir Azure Active Directory yönetici hesabı oluşturun**

  Azure Active Directory kimlik doğrulamasını etkinleştirin ve bir Azure AD yönetici oturumu oluşturun. Bir Azure Active Directory hesap, tam yönetici izinleriyle SQL veritabanı dağıtımının Yöneticisi olarak yapılandırılabilir. Bu hesap, bir bireysel veya güvenlik grubu hesabı olabilir. SQL veritabanına bağlanmak için Azure AD hesapları kullanmak istiyorsanız bir Azure **ad Yöneticisi yapılandırılmalıdır** . Tüm SQL veritabanı dağıtım türleri için Azure AD kimlik doğrulamasını etkinleştirme hakkında ayrıntılı bilgi için aşağıdaki makalelere bakın:

  - [SQL ile kimlik doğrulaması için Azure Active Directory kimlik doğrulaması kullanma](sql-database-aad-authentication.md)
  - [SQL ile Azure Active Directory kimlik doğrulamasını yapılandırma ve yönetme](sql-database-aad-authentication-configure.md)

- **Yönetilen bir örnek dağıtımında, tam yönetim izinlerine sahip SQL oturum açmaları oluşturun**

  - Yönetilen örnekte ek SQL Server oturum açma oluşturma
  - [Alter Server role](https://docs.microsoft.com/sql/t-sql/statements/alter-server-role-transact-sql) deyimini kullanarak [sysadmin sabit sunucu rolüne](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/server-level-roles) oturum açma ekleyin. Bu oturum açma tam yönetim izinlerine sahip olacaktır.
  - Alternatif olarak, <a href="/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current">create LOGIN</a> sözdizimini kullanarak BIR [Azure AD oturum açma bilgisi](sql-database-aad-authentication-configure.md?tabs=azure-powershell#new-azure-ad-admin-functionality-for-mi) oluşturun.

- **Tek veya havuza alınmış bir dağıtımda, sınırlı yönetim izinlerine sahip SQL oturum açmaları oluşturun**

  - Tek veya havuza alınmış bir veritabanı dağıtımı veya yönetilen örnek dağıtımı için ana veritabanında ek bir SQL oturumu oluşturun
  - Ana veritabanında bu yeni oturum açmayla ilişkili bir kullanıcı hesabı oluşturun
  - [Alter Server role](https://docs.microsoft.com/sql/t-sql/statements/alter-server-role-transact-sql) deyimini kullanarak `master` veritabanında `dbmanager`, `loginmanager` role veya her Ikisine de kullanıcı hesabını ekleyin (Azure SYNAPSE Analytics için [sp_addrolemember](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql) deyimini kullanın).

  > [!NOTE]
  > `dbmanager`ve `loginmanager` rolleri yönetilen örnek dağıtımlarıyla ilgili **değildir** .

  Tek veya havuza alınmış veritabanları için bu [özel ana veritabanı rollerinin](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/database-level-roles#special-roles-for--and-) üyeleri, kullanıcıların veritabanları oluşturma ve yönetme veya oturum açma işlemleri oluşturma ve Yönetme yetkisine sahip olmasını sağlar. `dbmanager` Rolün üyesi olan bir kullanıcı tarafından oluşturulan veritabanlarında, üye `db_owner` sabit veritabanı rolüne eşlenir ve bu veritabanını oturum açıp `dbo` Kullanıcı hesabını kullanarak yönetebilir. Bu rollerin, ana veritabanı dışında açık izinleri yoktur.

  > [!IMPORTANT]
  > Tek veya havuza alınmış bir veritabanında tam yönetim izinleriyle ek bir SQL oturumu oluşturamazsınız.

## <a name="create-accounts-for-non-administrator-users"></a>Yönetici olmayan kullanıcılar için hesap oluşturma

Yönetici olmayan kullanıcılar için, iki yöntemden birini kullanarak hesap oluşturabilirsiniz:

- **Oturum oluşturur**

  Ana veritabanında bir SQL oturum açma oluşturun. Ardından, kullanıcının erişmesi gereken her veritabanında bir kullanıcı hesabı oluşturun ve Kullanıcı hesabını bu oturum açmayla ilişkilendirin. Bu yaklaşım, kullanıcının birden çok veritabanına erişmesi gerektiğinde ve parolaları eşitlenmiş halde tutmak istediğinizde tercih edilir. Ancak, bu yaklaşım, hem birincil sunucuda hem de ikincil sunucularda oturum açmanın oluşturulması için coğrafi çoğaltma ile birlikte kullanıldığında karmaşıklıkları vardır. Daha fazla bilgi için bkz. [coğrafi geri yükleme veya yük devretme Için Azure SQL veritabanı güvenliğini yapılandırma ve yönetme](sql-database-geo-replication-security-config.md).
- **Kullanıcı hesabı oluşturma**

  Bir kullanıcının erişim ihtiyacı olan veritabanında ( [Kapsanan kullanıcı](https://docs.microsoft.com/sql/relational-databases/security/contained-database-users-making-your-database-portable)olarak da adlandırılır) bir kullanıcı hesabı oluşturun.

  - Tek veya havuza alınmış bir veritabanı ile, her zaman bu tür bir kullanıcı hesabı oluşturabilirsiniz.
  - [Azure AD Server sorumlularını](sql-database-aad-authentication-configure.md?tabs=azure-powershell#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities)desteklemeyen bir yönetilen örnek veritabanı ile, bu tür bir kullanıcı hesabını yalnızca [Kapsanan bir veritabanında](https://docs.microsoft.com/sql/relational-databases/databases/contained-databases)oluşturabilirsiniz. [Azure AD Server sorumlularını](sql-database-aad-authentication-configure.md?tabs=azure-powershell#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities)destekleyen yönetilen örnek sayesinde, yönetilen örnekte kimlik doğrulaması yapmak için Kullanıcı hesapları oluşturabilirsiniz. Bu, veritabanı kullanıcılarının kapsanan veritabanı kullanıcısı olarak oluşturulmasını gerektirmeksizin.

  Bu yaklaşımda, Kullanıcı kimlik doğrulama bilgileri her bir veritabanında depolanır ve coğrafi olarak çoğaltılan veritabanlarına otomatik olarak çoğaltılır. Ancak, birden çok veritabanında aynı hesap varsa ve SQL kimlik doğrulaması kullanıyorsanız, parolaları el ile eşitlenmiş tutmanız gerekir. Ayrıca, bir kullanıcının farklı parolalara sahip farklı veritabanlarında bir hesabı varsa, bu Parolaları hatırlama bir sorun haline gelebilir.

> [!IMPORTANT]
> Azure AD kimlikleriyle eşleştirilmiş kapsanan kullanıcıları oluşturmak için, SQL veritabanında yönetici olan bir Azure AD hesabı kullanarak oturum açmış olmanız gerekir. Yönetilen örnekte, `sysadmin` IZINLERLE bir SQL oturumu açma da Azure AD oturum açma veya Kullanıcı oluşturabilir.

Oturum açma ve Kullanıcı oluşturma işlemlerini gösteren örnekler için bkz.:

- [Tek veya havuza alınmış veritabanları için oturum açma oluşturma](https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-current#examples-1)
- [Yönetilen örnek veritabanı için oturum açma oluştur](https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current#examples-2)
- [Azure SYNAPSE Analytics veritabanı için oturum açma oluşturma](https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql?view=azure-sqldw-latest#examples-3)
- [Kullanıcı oluştur](https://docs.microsoft.com/sql/t-sql/statements/create-user-transact-sql#examples)
- [Azure AD kapsanan kullanıcılar oluşturuluyor](sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities)

> [!TIP]
> Tek veya havuza alınmış bir veritabanında kapsanan kullanıcılar SQL Server oluşturmayı içeren bir güvenlik öğreticisi için bkz. [öğretici: tek veya havuza alınmış bir veritabanının güvenliğini sağlama](sql-database-security-tutorial.md).

## <a name="using-fixed-and-custom-database-roles"></a>Sabit ve özel veritabanı rollerini kullanma

Bir veritabanında bir oturum açma ya da kapsanan Kullanıcı olarak bir kullanıcı hesabı oluşturduktan sonra, söz konusu kullanıcıya çeşitli eylemler gerçekleştirme ve belirli bir veritabanındaki verilere erişme yetkisi verebilirsiniz. Erişimi yetkilendirmek için aşağıdaki yöntemleri kullanabilirsiniz:

- **Düzeltilen veritabanı rolleri**

  Kullanıcı hesabını bir [sabit veritabanı rolüne](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/database-level-roles)ekleyin. Her biri tanımlanmış bir izin kümesine sahip 9 sabit veritabanı rolü vardır. En yaygın sabit veritabanı rolleri şunlardır: **db_owner**, **db_ddladmin**, **db_datawriter**, **db_datareader**, **db_denydatawriter**ve **db_denydatareader**. Birkaç kullanıcıya tam izin vermek için genelde **db_owner** kullanılır. Diğer sabit veritabanı rolleri, geliştirme aşamasında basit bir veritabanını hızlı bir şekilde kullanıma almak için kullanışlıdır ancak çoğu üretim veritabanı için önerilmez. Örneğin, **db_datareader** sabit veritabanı rolü, veritabanı içindeki her tabloya okuma erişimi verir ve bu, kesinlikle gereklidir.

  - Sabit bir veritabanı rolüne kullanıcı eklemek için:

    - Azure SQL veritabanı 'nda [alter role](https://docs.microsoft.com/sql/t-sql/statements/alter-role-transact-sql) deyimini kullanın. Örnekler için bkz. [alter role örnekleri](https://docs.microsoft.com/sql/t-sql/statements/alter-role-transact-sql#examples)
    - Azure SYNAPSE Analytics, [sp_addrolemember](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql) ifadesini kullanın. Örnekler için bkz. [sp_addrolemember örnekleri](https://docs.microsoft.com/sql/t-sql/statements/alter-role-transact-sql).

- **Özel veritabanı rolü**

  [Create role](https://docs.microsoft.com/sql/t-sql/statements/create-role-transact-sql) ifadesini kullanarak özel bir veritabanı rolü oluşturun. Özel bir rol, kendi Kullanıcı tanımlı veritabanı rollerinizi oluşturmanıza ve her bir role iş ihtiyacı için gereken en az izni sağlamanıza olanak sağlar. Ardından, özel Role Kullanıcı ekleyebilirsiniz. Birden fazla rolün üyesi olan kullanıcılar, tüm rollerin izinlerine sahip olur.
- **İzinleri doğrudan verme**

  Kullanıcı hesabı [izinlerini](https://docs.microsoft.com/sql/relational-databases/security/permissions-database-engine) doğrudan verin. SQL Veritabanında ayrı ayrı verilebilen veya reddedilebilen 100'den fazla izin vardır. Bu izinlerin çoğu iç içe geçmiş haldedir. Örneğin, bir şemada için verilen `UPDATE` izni, o şema içindeki tüm tablolar için `UPDATE` iznini de içerir. Çoğu izin sisteminde olduğu gibi bir iznin reddedilmesi, aynı iznin verilme durumunu geçersiz kılar. İç içe geçmiş yapısı ve izin sayısı nedeniyle, veritabanınızı doğru şekilde korumak için uygun bir izin sistemi tasarlamak uzun ve dikkatli bir çalışma gerektirebilir. [İzinler (Veritabanı Altyapısı)](https://docs.microsoft.com/sql/relational-databases/security/permissions-database-engine) ile başlayın ve izinlerin [poster boyutundaki tablosunu](https://docs.microsoft.com/sql/relational-databases/security/media/database-engine-permissions.png) inceleyin.

## <a name="using-groups"></a>Grupları kullanma

Verimli erişim yönetimi, bireysel kullanıcılar yerine Active Directory güvenlik gruplarına ve sabit veya özel rollere atanan izinleri kullanır.

- Azure Active Directory kimlik doğrulaması kullanırken, Azure Active Directory Kullanıcıları Azure Active Directory güvenlik grubuna yerleştirin. Grup için bir bağımsız veritabanı kullanıcısı oluşturun. Bir veya daha fazla veritabanı kullanıcıyı, bu kullanıcı grubuna uygun belirli izinlerle özel bir veritabanı rolüne yerleştirin.

- SQL kimlik doğrulaması kullanırken, veritabanında kapsanan veritabanı kullanıcıları oluşturun. Bir veya daha fazla veritabanı kullanıcıyı, bu kullanıcı grubuna uygun belirli izinlerle özel bir veritabanı rolüne yerleştirin.

  > [!NOTE]
  > Ayrıca, içerilmeyen veritabanı kullanıcıları için grupları kullanabilirsiniz.

İzinleri sınırlamak veya yükseltmek için kullanılabilen aşağıdaki özellikleri tanımanız gerekir:

- [Kimliğe Bürünme](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/customizing-permissions-with-impersonation-in-sql-server) ve [modül imzalama](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/signing-stored-procedures-in-sql-server) ile izinler geçici olarak ve güvenli bir şekilde artırılabilir.
- [Satır Düzeyinde Güvenlik](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) ile bir kullanıcının erişebileceği satırlar sınırlandırılabilir.
- [Veri Maskeleme](sql-database-dynamic-data-masking-get-started.md) ile hassas verilerin kapsamı sınırlandırılabilir.
- [Saklı yordamlar](https://docs.microsoft.com/sql/relational-databases/stored-procedures/stored-procedures-database-engine) ile veritabanında gerçekleştirilebilecek eylemler sınırlandırılabilir.

## <a name="next-steps"></a>Sonraki adımlar

SQL Veritabanı güvenlik özelliklerinin tümüne genel bakış için bkz. [SQL Veritabanı güvenliğine genel bakış](sql-database-security-overview.md).
