---
title: Azure Active Directory
description: SQL Veritabanı, Yönetilen Örnek ve SQL Veri Ambarı ile kimlik doğrulama için Azure Active Directory'yi nasıl kullanacağınızı öğrenin
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: data warehouse
ms.devlang: ''
ms.topic: conceptual
author: GithubMirek
ms.author: mireks
ms.reviewer: vanto, carlrab
ms.date: 02/20/2019
ms.openlocfilehash: ef20c5b8babdf378b88997ae2fd7b98350c31319
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80124916"
---
# <a name="use-azure-active-directory-authentication-for-authentication-with-sql"></a>SQL ile kimlik doğrulama için Azure Active Directory Authentication'ı kullanma

Azure Active Directory kimlik doğrulaması, Azure Active Directory (Azure AD) kimliklerini kullanarak Azure [SQL Veritabanı](sql-database-technical-overview.md), [Yönetilen Örnek](sql-database-managed-instance.md)ve SQL [Veri Ambarı'na](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md) bağlanma mekanizmasıdır. 

> [!NOTE]
> Bu konu başlığı, Azure SQL sunucusunun yanı sıra Azure SQL sunucusu üzerinde oluşturulmuş olan SQL Veritabanı ve SQL Veri Ambarı veritabanları için de geçerlidir. Kolaylık açısından, hem SQL Veritabanı hem de SQL Veri Ambarı için SQL Veritabanı terimi kullanılmaktadır.

Azure AD kimlik doğrulamasıyla, veritabanı kullanıcılarının ve diğer Microsoft hizmetlerinin kimliklerini tek bir konumda merkezi olarak yönetebilirsiniz. Merkezi kimlik yönetimi, tüm veritabanı kullanıcılarını aynı yerden yönetmenizi sağlar ve izin yönetimini kolaylaştırır. Avantajları şunlardır:

- SQL Server kimlik doğrulamasına alternatif sağlar.
- Veritabanı sunucuları arasında kullanıcı kimliklerinin çoğalmasını durdurmaya yardımcı olur.
- Parola döndürmeyi tek bir yerde sağlar.
- Müşteriler veritabanı izinlerini harici (Azure AD) gruplarını kullanarak yönetebilir.
- Azure Active Directory tarafından desteklenen tümleşik Windows kimlik doğrulaması ve diğer kimlik doğrulama biçimlerini etkinleştirerek parola depolamayı ortadan kaldırabilir.
- Azure AD kimlik doğrulaması, veritabanı düzeyinde kimlikleri doğrulamak için veritabanı kullanıcılarını kullanır.
- Azure AD, SQL Veritabanı'na bağlanan uygulamalar için belirteç tabanlı kimlik doğrulamasını destekler.
- Azure AD kimlik doğrulaması, etki alanı eşitlemesi olmadan yerel bir Azure Etkin Dizini için ADFS (etki alanı federasyonu) veya yerel kullanıcı/parola kimlik doğrulamasını destekler.
- Azure AD, Multi-Factor Authentication (MFA) içeren Active Directory Evrensel Kimlik Doğrulaması'nın kullanıldığı SQL Server Management Studio'dan gelen bağlantıları destekler.  MFA telefon görüşmesi, SMS mesajı, pin ile akıllı kart veya mobil uygulama bildirimi gibi çok sayıda kolay doğrulama seçeneğine sahiptir. Daha fazla bilgi için bkz. [SQL Veritabanı ve SQL Veri Ambarı ile Azure AD MFA için SSMS desteği](sql-database-ssms-mfa-authentication.md).
- Azure AD, Active Directory Etkileşimli Kimlik Doğrulaması'nın kullanıldığı SQL Server Veri Araçları'ndan (SSDT) gelen benzer bağlantıları destekler. Daha fazla bilgi için bkz. [SQL Server Veri Araçları'na (SSDT) Azure Active Directory desteği](/sql/ssdt/azure-active-directory).

> [!NOTE]  
> Azure VM üzerinde çalışan SQL Server'a bağlanma, Azure Active Directory hesabı kullanılarak desteklenmez. Bunun yerine etki alanı Active Directory hesabı kullanın.  

Yapılandırma adımları, Azure Active Directory kimlik doğrulamasını yapılandırmak ve kullanmak için aşağıdaki yordamları içerir.

1. Azure AD oluşturun ve doldurun.
2. İsteğe bağlı: Şu anda Azure Aboneliğinizle ilişkili olan etkin dizinini ilişkilendirin veya değiştirin.
3. Azure SQL Veritabanı sunucusu, Yönetilen Örnek veya [Azure SQL Veri Ambarı](https://azure.microsoft.com/services/sql-data-warehouse/)için bir Azure Etkin Dizin yöneticisi oluşturun.
4. İstemci bilgisayarlarınızı yapılandırın.
5. Veritabanınızda Azure AD kimlikkimliklerine eşlenen veritabanında bulunan veritabanı kullanıcıları oluşturun.
6. Azure AD kimliklerini kullanarak veritabanınıza bağlanın.

> [!NOTE]
> Azure AD'yi nasıl oluşturup doldurarak doldurup Azure AD'yi azure SQL Veritabanı, Yönetilen Örnek ve SQL Veri Ambarı ile yapılandırmayı öğrenmek için [bkz.](sql-database-aad-authentication-configure.md)

## <a name="trust-architecture"></a>Güven mimarisi

Aşağıdaki üst düzey diyagram, Azure AD kimlik doğrulamasını Azure SQL Veritabanı ile kullanmanın çözüm mimarisini özetler. Aynı kavramlar SQL Veri Ambarı için de geçerlidir. Azure AD yerel kullanıcı parolasını desteklemek için yalnızca Bulut bölümü ve Azure AD/Azure SQL Veritabanı dikkate alınır. Federe kimlik doğrulamasını (veya Windows kimlik bilgileri için kullanıcı/parolayı) desteklemek için ADFS bloğu ile iletişim gereklidir. Oklar iletişim yollarını gösteriyor.

![aad auth diyagramı][1]

Aşağıdaki diyagram, istemcinin bir belirteç göndererek veritabanına bağlanmasına izin veren federasyon, güven ve barındırma ilişkilerini gösterir. Belirteç bir Azure REKLAMı tarafından doğrulanır ve veritabanı tarafından güvenilir. Müşteri 1, yerel kullanıcılarla bir Azure Etkin Dizinini veya federe kullanıcılarla bir Azure REKLAM'ı temsil edebilir. Müşteri 2, içe aktarılan kullanıcılar da dahil olmak üzere olası bir çözümü temsil eder; Bu örnekte, ADFS'nin Azure Active Directory ile senkronize edildiği federe bir Azure Active Directory'sinden gelir. Azure AD kimlik doğrulaması kullanarak bir veritabanına erişimin barındırma aboneliğinin Azure AD ile ilişkilendirilmesi gerektiğini anlamak önemlidir. Aynı abonelik, Azure SQL Veritabanı'nı veya SQL Veri Ambarını barındıran SQL Server'ı oluşturmak için kullanılmalıdır.

![abonelik ilişkisi][2]

## <a name="administrator-structure"></a>Yönetici yapısı

Azure AD kimlik doğrulaması kullanırken, SQL Veritabanı sunucusu ve Yönetilen Örnek için iki Yönetici hesabı vardır; özgün SQL Server yöneticisi ve Azure AD yöneticisi. Aynı kavramlar SQL Veri Ambarı için de geçerlidir. Yalnızca bir Azure AD hesabına dayalı yönetici, bir kullanıcı veritabanında ilk Azure AD içerdiği veritabanı kullanıcısını oluşturabilir. Azure AD yöneticisi girişi bir Azure AD kullanıcısı veya Azure AD grubu olabilir. Yönetici bir grup hesabı olduğunda, sql server örneği için birden çok Azure AD yöneticisine olanak sağlayarak herhangi bir grup üyesi tarafından kullanılabilir. Grup hesabını yönetici olarak kullanmak, SQL Veritabanı'ndaki kullanıcıları veya izinleri değiştirmeden Azure AD'deki grup üyelerini merkezi olarak eklemenize ve kaldırmanıza izin vererek yönetilebilirliği artırır. İstediğiniz zaman yalnızca bir Azure AD yöneticisi (kullanıcı veya grup) yapılandırılabilir.

![yönetici yapısı][3]

## <a name="permissions"></a>İzinler

Yeni kullanıcılar oluşturmak için veritabanında izne `ALTER ANY USER` sahip olmalısınız. İzin `ALTER ANY USER` herhangi bir veritabanı kullanıcısına verilebilir. `ALTER ANY USER` İzin, sunucu yöneticisi hesapları ve veritabanı kullanıcıları tarafından `CONTROL ON DATABASE` `ALTER ON DATABASE` bu veritabanı için izin li `db_owner` veya veritabanı rolü üyeleri tarafından da tutulur.

Azure SQL Veritabanı, Yönetilen Örnek veya SQL Veri Ambarı'nda bulunan bir veritabanı kullanıcısı oluşturmak için, azure AD kimliğini kullanarak veritabanına veya örneğe bağlanmanız gerekir. İlk bulunan veritabanı kullanıcısını oluşturmak için, bir Azure AD yöneticisi (veritabanının sahibi) kullanarak veritabanına bağlanmanız gerekir. Bu, Azure [Active Directory kimlik doğrulamasını SQL Veritabanı veya SQL Veri Ambarı ile Yapılandırma ve yönetme](sql-database-aad-authentication-configure.md)de gösterilmiştir. Azure AD kimlik doğrulaması yalnızca Azure AD yöneticisi Azure SQL Veritabanı veya SQL Veri Ambarı sunucusu için oluşturulduğunda mümkündür. Azure Active Directory yöneticisi sunucudan kaldırıldıysa, daha önce SQL Server içinde oluşturulan mevcut Azure Active Directory kullanıcıları, Azure Active Directory kimlik bilgilerini kullanarak veritabanına artık bağlanabilir.

## <a name="azure-ad-features-and-limitations"></a>Azure AD özellikleri ve sınırlamaları

- Azure AD'nin aşağıdaki üyeleri Azure SQL sunucusunda veya SQL Veri Ambarı'nda kullanılabilir:

  - Yerel üyeler: Azure AD'de yönetilen etki alanında veya müşteri etki alanında oluşturulan üye. Daha fazla bilgi için [bkz.](../active-directory/active-directory-domains-add-azure-portal.md)
  - Federe etki alanı üyeleri: Azure AD'de federe etki alanı yla oluşturulmuş bir üye. Daha fazla bilgi için Bkz. [Microsoft Azure şimdi Windows Server Active Directory ile federasyonu destekler.](https://azure.microsoft.com/blog/20../../windows-azure-now-supports-federation-with-windows-server-active-directory/)
  - Diğer Azure AD'lerinden, yerel veya federe etki alanı üyesi olan üyeler ithal edilir.
  - Etkin Dizin grupları güvenlik grupları olarak oluşturuldu.

- Sunucu rolü olan bir grubun parçası olan Azure AD kullanıcıları, Azure SQL Veritabanı ve Azure SQL Veri Ambarı'na karşı **[CREATE DATABASE SCOPED INTIAL](/sql/t-sql/statements/create-database-scoped-credential-transact-sql)** sözdizimini kullanamaz. `db_owner` Aşağıdaki hatayı görürsünüz:

    `SQL Error [2760] [S0001]: The specified schema name 'user@mydomain.com' either does not exist or you do not have permission to use it.`

    CREATE `db_owner` **DATABASE SCOPED CREDIAL** sorununu azaltmak için rolü doğrudan tek bir Azure AD kullanıcısına ver.

- Bu sistem işlevleri Azure AD ilkeleri altında yürütüldüğünde NULL değerlerini döndürür:

  - `SUSER_ID()`
  - `SUSER_NAME(<admin ID>)`
  - `SUSER_SNAME(<admin SID>)`
  - `SUSER_ID(<admin name>)`
  - `SUSER_SID(<admin name>)`

### <a name="managed-instances"></a>Yönetilen Örnekler

- Azure AD sunucu ilkeleri (oturum açmalar) ve kullanıcılar [Yönetilen Örnekler](sql-database-managed-instance.md)için önizleme özelliği olarak desteklenir.
- Veritabanı sahibi olarak Azure AD grubuna eşlenen Azure AD sunucu ilkelerini (oturum açmaları) Oluşturma Yönetilen [Örneklerde](sql-database-managed-instance.md)desteklenmez.
    - Bunun bir uzantısı, sunucu rolünün `dbcreator` bir parçası olarak bir grup eklendiğinde, bu gruptaki kullanıcıların Yönetilen Örnek'e bağlanıp yeni veritabanları oluşturabilmeleri, ancak veritabanına erişemeyecekleridir. Bunun nedeni, yeni veritabanı sahibinin Azure AD kullanıcısı değil, SA olmasıdır. Tek tek kullanıcı `dbcreator` sunucu rolüne eklenirse, bu sorun ortaya çıkar.
- SQL Agent yönetimi ve iş yürütme Azure AD sunucu ilkeleri (oturum açma) için desteklenir.
- Veritabanı yedekleme ve geri yükleme işlemleri Azure AD sunucu ilkeleri (oturum açmalar) tarafından yürütülebilir.
- Azure AD sunucu ilkeleri (oturum açmalar) ve kimlik doğrulama olayları ile ilgili tüm deyimlerin denetlenmesi desteklenir.
- Sysadmin sunucu rolünün üyesi olan Azure AD sunucu ilkeleri (oturum açmalar) için özel yönetici bağlantısı desteklenir.
    - SQLCMD Utility ve SQL Server Management Studio aracılığıyla desteklenir.
- Oturum açma tetikleyicileri, Azure AD sunucu ilkelerinden (oturum açma) gelen oturum açma etkinlikleri için desteklenir.
- Service Broker ve DB postası, Azure AD sunucu ilkesi (oturum açma) kullanılarak ayarlanabilir.


## <a name="connecting-using-azure-ad-identities"></a>Azure AD kimliklerini kullanarak bağlanma

Azure Etkin Dizin kimlik doğrulaması, Azure AD kimliklerini kullanarak bir veritabanına bağlanma nın aşağıdaki yöntemlerini destekler:

- Azure Active Directory Password
- Azure Active Directory Entegre
- MFA ile Azure Active Directory Universal
- Uygulama belirteç kimlik doğrulamasını kullanma

Aşağıdaki kimlik doğrulama yöntemleri Azure AD sunucu ilkeleri (oturum açmalar)**(genel önizleme)** için desteklenir:

- Azure Active Directory Password
- Azure Active Directory Entegre
- MFA ile Azure Active Directory Universal


### <a name="additional-considerations"></a>Diğer konular

- Yönetilebilirliği artırmak için, yönetici olarak özel bir Azure REKLAM grubu sağlamanızı öneririz.   
- Yalnızca bir Azure AD yöneticisi (kullanıcı veya grup) herhangi bir zamanda bir Azure SQL Veritabanı sunucusu veya Azure SQL Veri Ambarı için yapılandırılabilir.
  - Yönetilen Örnekler için Azure AD sunucu ilkelerinin **(oturum**açmalar) eklenmesi, `sysadmin` role eklenebilecek birden çok Azure AD sunucu ilkesi (oturum açma) oluşturma olanağı sağlar.
- Yalnızca SQL Server için bir Azure AD yöneticisi, azure etkin dizin hesabı kullanarak başlangıçta Azure SQL Veritabanı sunucusuna, Yönetilen Örnek'e veya Azure SQL Veri Ambarına bağlanabilir. Etkin Dizin yöneticisi sonraki Azure AD veritabanı kullanıcılarını yapılandırabilir.   
- Bağlantı zaman larını 30 saniyeye ayarlamanızı öneririz.   
- VISUAL Studio 2015 için SQL Server 2016 Management Studio ve SQL Server Data Tools (sürüm 14.0.60311.1Nisan 2016 veya sonrası) Azure Active Directory kimlik doğrulamasını destekler. (Azure AD kimlik **doğrulaması, SqlServer için .NET Framework Data Provider**tarafından desteklenir ; en azından sürüm .NET Framework 4.6). Bu nedenle bu araçların en yeni sürümleri ve veri katmanı uygulamaları (DAC ve . BACPAC) Azure AD kimlik doğrulamasını kullanabilir.   
- Sürüm 15.0.1 ile başlayarak, [sqlcmd yardımcı programı](/sql/tools/sqlcmd-utility) ve [bcp yardımcı programı](/sql/tools/bcp-utility) MFA ile Active Directory Interactive kimlik doğrulamasını destekler.
- Visual Studio 2015 için SQL Server Veri Araçları, Veri Araçları'nın en az Nisan 2016 sürümünü (sürüm 14.0.60311.1) gerektirir. Şu anda Azure AD kullanıcıları SSDT Object Explorer'da gösterilmez. Geçici çözüm olarak, [sys.database_principals'deki](https://msdn.microsoft.com/library/ms187328.aspx)kullanıcıları görüntüleyin.   
- [SQL Server için Microsoft JDBC Driver 6.0](https://www.microsoft.com/download/details.aspx?id=11774) Azure AD kimlik doğrulamasını destekler. Ayrıca, bkz. [Bağlantı Özelliklerini Ayarlama.](https://msdn.microsoft.com/library/ms378988.aspx)   
- PolyBase, Azure AD kimlik doğrulamasını kullanarak kimlik doğrulaması yapamaz.   
- Azure AD kimlik doğrulaması, SQL Veritabanı için Azure portalı **Alma Veritabanı** ve Veritabanı Nı **Dışa Aktarma** bıçakları tarafından desteklenir. Azure AD kimlik doğrulaması kullanarak alma ve dışa aktarma da PowerShell komutundan desteklenir.   
- Azure AD kimlik doğrulaması, CLI kullanımıyla SQL Veritabanı, Yönetilen Örnek ve SQL Veri Ambarı için desteklenir. Daha fazla bilgi için, SQL Veritabanı veya SQL Veri Ambarı ve [SQL Server - az sql server](https://docs.microsoft.com/cli/azure/sql/server)ile Azure Active [Directory kimlik doğrulamasını yapılandırın ve yönetin.](sql-database-aad-authentication-configure.md)

## <a name="next-steps"></a>Sonraki adımlar

- Azure AD'yi nasıl oluşturup doldurarak doldurup azure AD'yi Azure SQL Veritabanı veya Azure SQL Veri Ambarı ile yapılandırmak için [Bkz.](sql-database-aad-authentication-configure.md)
- Yönetilen Örneklerle Azure AD sunucu ilkelerini (oturum açma) kullanma eğitimi için, [Yönetilen Örneklerle Azure AD sunucu ilkelerine (oturum açma)](sql-database-managed-instance-aad-security-tutorial.md) bakın
- SQL Veritabanı'ndaki oturum açma, kullanıcılar, veritabanı rolleri ve izinlere genel bakış [için](sql-database-manage-logins.md)bkz.
- Veritabanı sorumluları hakkında daha fazla bilgi için bkz. [Sorumlular](https://msdn.microsoft.com/library/ms181127.aspx).
- Veritabanı rolleri hakkında daha fazla bilgi için bkz. [Veritabanı rolleri](https://msdn.microsoft.com/library/ms189121.aspx).
- Yönetilen Örnekler için Azure AD sunucu ilkeleri (oturum açma) oluşturma sözdizimi için CREATE [LOGIN'e](/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current)bakın.
- SQL Veritabanındaki güvenlik duvarı kuralları hakkında daha fazla bilgi için bkz. [SQL Veritabanı güvenlik duvarı kuralları](sql-database-firewall-configure.md).

<!--Image references-->

[1]: ./media/sql-database-aad-authentication/1aad-auth-diagram.png
[2]: ./media/sql-database-aad-authentication/2subscription-relationship.png
[3]: ./media/sql-database-aad-authentication/3admin-structure.png
[4]: ./media/sql-database-aad-authentication/4select-subscription.png
[5]: ./media/sql-database-aad-authentication/5ad-settings-portal.png
[6]: ./media/sql-database-aad-authentication/6edit-directory-select.png
[7]: ./media/sql-database-aad-authentication/7edit-directory-confirm.png
[8]: ./media/sql-database-aad-authentication/8choose-ad.png
[9]: ./media/sql-database-aad-authentication/9ad-settings.png
[10]: ./media/sql-database-aad-authentication/10choose-admin.png
[11]: ./media/sql-database-aad-authentication/11connect-using-int-auth.png
[12]: ./media/sql-database-aad-authentication/12connect-using-pw-auth.png
[13]: ./media/sql-database-aad-authentication/13connect-to-db.png
